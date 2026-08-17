# 06 — DATA, BACKEND AND SECURITY SPEC

Autoridade canónica para dados/backend. Consolida BACKEND-DECISION-01..19 (08_SUPABASE) com as
decisões BT-02..BT-08 e TD-09..TD-16 deste pacote. Nenhum SQL daqui é executado nesta sessão; a
aplicação live Supabase exige verificação posterior (secção 12).

## 1. Estratégia geral (GLM-DATA-01)

- PostgreSQL (Supabase), schema `public`; mecanismo primário **Npgsql + Dapper** a partir do C#.
- Supabase RPC/Data API **não** usadas pelos módulos; browser **nunca** acede às tabelas.
- Roles DB: `ba_dmo_app` (runtime, least privilege) e `ba_dmo_migrate` (DDL), com
  `ALTER DEFAULT PRIVILEGES` para objetos futuros. `postgres`/`service_role` não são credenciais da app.
- PDF/documentos são derivados (secção 9); nada binário na base. PDFs são gerados em memória no
  backend e entregues como artefacto de exportação (secção 16).

## 2. Convenções (GLM-DATA-02)

- Timestamps: `timestamptz` UTC (`now()`); datas de negócio `date`.
- Autoria: `actor_id` de `internal_users`, resolvida **server-side** (nunca aceite do cliente).
- IDs: UUID `gen_random_uuid()` + business keys onde definido; IDs imutáveis nunca reutilizados.
- Concorrência: optimistic `updated_at` (BT-06); append-only isento.
- Soft delete preferido (active flags/lifecycle); delete físico apenas onde confirmado (Peso rascunho/nao_aprovado).
- Migrations: scripts SQL versionados, idempotentes, forward-only, na **nova família** fresh-build
  (`database/migrations/` da app nova: `N01_identity.sql`, `N02_catalog.sql`, `N03_bq.sql`, …).
- Migrations geridas por **executor Npgsql custom** + tabela `schema_migrations`; CLI only
  (secções 12–13).

## 3. Schema alvo por dono (GLM-DATA-03)

### 3.1 Shell/Identity/Admin
| Tabela | Chaves/invariantes principais |
|---|---|
| `internal_users` | PK `actor_id` text; `auth_user_id uuid NULL` (ref lógica); `template_id` FK; `active`; `profile_title`; UNIQUE(actor_id); índices auth_user_id/active/template_id |
| `access_templates` | PK `template_id` text; `modules jsonb NOT NULL '[]'`; `active`; validação de catálogo na Application |
| `audit_events` | auditoria global canónica (TD-19): append-only; `occurred_at_utc`, `year` (derivado, índice), `actor_user_id`, `actor_name_snapshot`, `module_id`, `action_code` (catálogo versionado por módulo), `entity_type`, `entity_id`, `entity_label_snapshot`, `result` (succeeded/failed/denied/corrected), `reason`, `correlation_id`, `job_on_id` (quando aplicável), `revision_id`, `before_summary`/`after_summary`; sem UPDATE/DELETE; sem segredos/binários; tabela única (não partir por módulo/ano) |
| `module_catalog_mirror` | espelho do catálogo (moduleId, name, order, active) — só para UI Admin; nunca concede acesso |

### 3.2 Boquilhas
| Tabela | Chaves/invariantes principais |
|---|---|
| `bq_lotes` | UNIQUE(`reference`,`batch_code`); CHECK referência `^[A-Z][0-9]{3}$`; `allowed_lines text[]`; `lifecycle_state` IN (available, archived, scrapped) — estado ativo/preparing derivado |
| `bq_traces` | FK lote; `status` active/closed; `purpose` production/repair; `start_line NOT NULL` (TD-14); `sap_start/sap_end` 0–100; `reopen_history`, `deleted_movements` jsonb |
| `bq_movements` | append-only; `type` IN (inicio, saida, entrada, irreparavel, linha, contagem, fim); `qty NULL` só para `linha`; `exceptional_received_qty` derivável/registado no retorno; **sem coluna `allow_unmatched`** (UD-09) |
| `bq_discrepancies` | excesso de retorno: `expected, actual, excess, status` open/under_review/resolved, `resolution_note`, `resolved_by/at` (C27) |
| `bq_lifecycle_history` | archived/scrapped/restored/retired + reason + ator |
| `bq_utilisation_readings` | initial/final, valor manual 0–100 |

### 3.3 Peso
`peso_references` (identidade mestre: mold_number + neckring_number UNIQUE; counter_mold; volumes;
`calote_tp numeric`; `change_log jsonb` — edição de referência aprovada exige justificação, retira a
aprovação e cria nova revisão — PESO_INTERFACE novo), `peso_lotes` (FK referência; lote; **processo
NNPB/PS no lote** (TD-17); `allowed_lines text[]` (mínimo uma); `report_subfolder` (nome relativo);
peso nominal; UNIQUE(referência, lote)), `peso_controlos` (UNIQUE mold+neckring+produção+linha+lote+
date; `record_type` novo_controlo/comparacao; **`job_on_id` + `job_on_revision_id` obrigatórios**;
CM/lote herdados do Job On (sem seleção própria); estados rascunho/pendente/aprovado/nao_aprovado;
snapshots; `approval_log jsonb`; `previous_control jsonb`; decisões por CM em comparações),
`peso_leituras` (UNIQUE controlo+cm_number; CASCADE), `peso_comparacao_anterior` (read path completo —
TD-13), `peso_day_approvals` (UNIQUE mold+neckring+linha+approval_date), `peso_settings` (key PK;
inclui `constant_nnpb/ps`, destinatários por grupo de linha, `main_output_folder_name`).

### 3.4 Pegamentos
`pegamento_controlos` (**`job_on_id` + `job_on_revision_id` obrigatórios**; referência, produção,
máquina e instâncias/lotes de CM, BQ e MF herdados do Job On — sem seleção alternativa nem fallback;
MF proveniente do respetivo domínio através do Job On; medições Costura/Contra costura; tolerância
±0.20 como **dado configurável**, não hard block; estado/revisão; auditoria), e `pegamento_medicoes`
(append por medição; valores preservados em registos antigos). Sem base local (handoff Pegamentos).

### 3.5 Ferramentas (CM/MF + registo)
`tool_references` (tool_type CM/MF, ref_code, technical_name, owner_plant — **sem processo**; o
processo pertence ao lote no fluxo Peso, TD-17), `tool_lotes` (FK referência; lote; qty;
allowed_lines; desenho+revisão; processo quando aplicável ao fluxo; UNIQUE(tool_reference_id, lote)),
`physical_pieces` (FK tool_lote; sequence; number; ID imutável; status operacional),
`tool_check_rules` (configuradas na ficha do lote: texto, frequência `uma_vez_no_lote`/`por_fabrico`,
estado, origem quando copiadas; edições aplicam-se ao futuro) / `tool_check_occurrences` (materializadas
no Job On; estado pendente/confirmada/reposta/desativada; `completion_source=manual_job_on`; operador
e data/hora só após persistência; reset preserva confirmações anteriores).

### 3.6 Job On (snapshot editável — TD-18)
Família canónica definida em `JOB_ON_DATA_MODEL.md` (design):
- `job_on` (id estável; `production_code` indexado; `article_reference_id` FK anulável +
  `article_reference_snapshot`; `machine_code`; `planned_start_at/planned_end_at` — fonte única do
  calendário; `status` rascunho/planeado/em fabrico/fechado/cancelado; `current_revision_id`;
  `copied_from_job_on_id`; autoria de criação);
- `job_on_revision` (imutável; `revision_number`; snapshots de produção/referência/máquina/datas;
  `sections`, `drop_count`, `type_snapshot`, `stop_snapshot`, `weight_snapshot`, `process_snapshot`
  (recebido do lote do Peso, corrigível apenas nesta revisão), `general_notes`, `image_asset_id`,
  `change_reason` (obrigatório quando se altera revisão fechada/aprovada), `saved_by/saved_at`);
- `job_on_component` (um por família MP/CM, MF, BQ, PU, CAL, AN, ARR, PI, CS, TP, FO por revisão;
  `source_tool_id`/`source_lot_id` + snapshots de referência/lote/nome técnico; `planned_quantity`,
  `stock_snapshot`, `usage_snapshot`, `notes`, `display_order`);
- `job_on_component_field` (campos tipados `value_type` text/integer/decimal/boolean/date/select com
  colunas dedicadas; nada de números apenas em JSON/texto quando usados em cálculo/filtro);
- `job_on_component_row` (linhas repetíveis CAL: element_label, valores, unidade, quantidade em máquina);
- `job_on_verification_occurrence` (regra de origem, componente, estado, `completed_by/at`,
  `completion_source=manual_job_on`);
- `job_on_audit_event` (criação, duplicação, abertura de edição, gravação, substituição de ferramenta,
  alteração de datas, checks; before/after apenas para auditoria — não substitui os snapshots);
- `job_on_field_option` (catálogos data-driven por família/campo para dropdowns de negócio evolutivos;
  gestão em Definições; desativar preserva valores em revisões antigas).
Regras: revisões guardadas nunca sofrem UPDATE destrutivo; `Guardar alterações` insere nova revisão;
duplicação copia o snapshot completo (nova Produção/datas/`copied_from`); unicidade segue a identidade
real do negócio sem excluir múltiplos Job Ons para a mesma Produção/Referência/Máquina se permitido.

### 3.7 Repair
`repairers` (id, name, active), `line_repairer_defaults` (PK(line, tool_type) — TD-15),
`repair_exits` (repair_type BQ/CM/MF; reparador snapshot; data prevista; status preparacao→
a_retirar→enviado→retorno_parcial→concluido|cancelado), `repair_exit_items` (qty para BQ; número
individual para CM/MF; picked; out_at/operator; in_at/operator; estado por item),
`repair_events` (interna/externa; cancelados não contam; repair_count derivado),
`internal_repair_records` (linha, contexto JobOn resolvido, tipo CM/MF, número individual, operador,
data/hora; correções before/after auditáveis — brief interna).

### 3.8 Armazém
`warehouse_locations` (code UNIQUE, kind), `warehouse_stock` (UNIQUE(location_id, tool_lote_ref) —
ocupação 1:1), `warehouse_movements` (direction in/out; destino; ligação a saída programada; ator;
occured_at). `fora` = calculado. Escrita posição+movimento atómica.

### 3.9 Tampões
`tampao_field_defs`, `tampao_field_values` (normalizados), `tampao_configurations` (values jsonb
UNIQUE), `tampao_saldos` (enchidos/por_encher ≥0), `tampao_movements` (Adicionar/Remover/Alterar
estado/Alterar configuração; antes/depois; atómico), `tampao_planos` (planear ≠ reservar).

### 3.10 Partilhado
`app_settings` (key, value jsonb) — ex.: destinatários por grupo de linha (B1–B3→Linha B, C1–C3→Linha C).

## 4. Invariantes transversais (GLM-DATA-04)

1. Factos imutáveis: movimentos, leituras, eventos, auditoria — correções são registos novos.
2. Snapshot ≠ live em toda a cópia inter-módulos.
3. Autoria/timestamp em toda a escrita; nunca do cliente.
4. Saldos calculados a partir de factos; nenhum saldo é “acreditado” sem movimento.
5. Nenhum estado inventado: estados derivam de registos existentes.

## 5. Transações e idempotência (GLM-DATA-05)

- Toda a escrita multi-tabela é transacional na Application (ex.: lote+trace+START; fecho de saída
  programada cria N saídas e liberta posições **atomicamente**; transformação de tampões origem+destino).
- Idempotência: submissão repetida de comando já aplicado não duplica factos (token de submissão ou
  verificação de estado na mesma transação).
- Funções PostgreSQL: nenhuma exigida na V1; permitir apenas se uma operação exigir atomicidade não
  obtível por transação C# (decisão registada).

## 6. RLS e segurança (GLM-DATA-06)

1. RLS ENABLED em todas as tabelas BA DMO; `anon`/`authenticated` sem acesso direto.
2. `ba_dmo_app` com CRUD técnico; autorização funcional **sempre** na Application C#.
3. Sem policies por utilizador/módulo na V1; capabilities não entram em RLS.
4. `ba_dmo_app` sem acesso ao schema `auth`; Admin API server-side para listar utilizadores Auth.
5. Credenciais em user secrets/environment; nunca em repositório ou browser.
6. CSRF (AntiForgeryToken), XSS (encoding Razor), SQL injection (queries parametrizadas), secure headers.

## 7. Auditoria e histórico (GLM-DATA-07)

- Auditoria global: `audit_events` (TD-19; GLM-ACC-11) — camada transversal “quem fez o quê”,
  escrita pelo backend na transação do domínio; consultas anuais no Admin com `audit.view`/`audit.export`.
- Domínio: histórico próprio por módulo (BT-03); toda a correção guarda before/after + autor + motivo + data.
- Nunca apagar histórico; “eliminar” movimento = void com registo (`deleted_movements`/voids).

## 8. Concorrência (GLM-DATA-08)

`UPDATE … WHERE id=@id AND updated_at=@expected`; 0 rows → `ConcurrencyConflict` com mensagem de
recarregamento. Obrigatório em edições (Admin, editLote, controlos, templates); append-only isento.

## 9. Documentos e storage (GLM-DATA-09)

Fronteira servidor/local (DS-08; PESO_INTERFACE §Persistência; CODER_HANDOFF §17):

- O **servidor** guarda os dados estruturados de Peso e Pegamentos (números, resultados, estado,
  revisão, auditoria, `job_on_id`/`job_on_revision_id`). O PDF não substitui estes dados.
- Os **PDFs de Produção** são artefactos derivados gerados a partir do snapshot aprovado e escritos
  no computador/local configurado: `diretório principal / subpasta do lote` (ex.: `Capacidades /
  5447T173`). Diretório principal em Definições (`main_output_folder_name`); subpasta relativa
  definida na criação do lote do Peso (nunca caminho absoluto livre). Peso e Pegamentos do mesmo
  Job On/lote resolvem a mesma subpasta.
- Nome do ficheiro derivado do snapshot (Produção, Referência, tipo de documento `Peso`/`Pegamentos`,
  revisão/data — TD-21); nunca identificação paralela introduzida pelo operador.
- A UI distingue `Dados guardados no servidor` de `PDF guardado localmente`; falha local não desfaz
  aprovação nem apaga dados; é possível repetir apenas a geração/gravação do PDF.
- Permissão do diretório é local ao browser/computador (File System Access): estados unconfigured/
  requesting/authorized/permission-lost/unavailable/error (componentes Local Directory Selector e
  Resolved Report Path); nunca apresentar a pasta como disponível antes de confirmar a permissão;
  o ficheiro local não é dado garantidamente disponível no servidor (DS-10/P2.10).
- Imagem do artigo (Job On): guardada por revisão do Job On (`image_asset_id` — TD-23); storage fora
  da base; substituição/remoção confirmadas e auditadas; sem cópia entre produções sem regra explícita.
- Impressões (listas de saída, folhas) são derivadas; imprimir não muda estado.
- Sem bucket/tabela blob para PDFs.
- **Fronteira browser-only (Plano-V3):** o File System Access é cliente/browser-only, para
  **exportação de PDFs** exclusivamente; nunca para domain persistence, offline database, business
  datastore ou source of truth (03_ARCH §17). Secure context **HTTPS** (Render fornece em deployment
  normal); fallback = download padrão do browser quando API não suportada / autorização recusada /
  handle inválido / permission lost. **Exceção aprovada pelo owner:** IndexedDB pode persistir apenas
  o `FileSystemDirectoryHandle` (permission/state técnico da seleção do diretório local); nunca
  dados de domínio (GLM-DATA-16/03_ARCH §17).

## 10. Integrações (GLM-DATA-10)

| Integração | Estado V1 |
|---|---|
| Supabase Auth + Postgres | Ativa |
| Email produção (Peso) | Preparação + confirmação explícita; destinatários por grupo de linha; envio bloqueado se configuração em falta (aprovação mantém-se) |
| SAP (`sap_start/sap_end`) | Campos mantidos como leituras de utilização; sem leitura/escrita automática |
| mCaliper | Desconhecido — não implementar |
| Ficheiros locais (PDF, imagens) | Secção 9 |

## 11. Seeds e bootstrap (GLM-DATA-11)

Sem dados operacionais fictícios. Migrations contêm apenas estrutura técnica + espelho de catálogo.
Primeiro admin por bootstrap operacional (GLM-ACC-13).

## 12. Migrations — implementação (GLM-DATA-12)

Implementação futura via **Npgsql** com um **custom migration runner mínimo**. Cada migration `.sql`
é: (1) lida integralmente; (2) submetida a SHA-256; (3) comparada com `schema_migrations`; (4) enviada
integralmente ao PostgreSQL; (5) executada **sem parser SQL C# artesanal**; (6) **registada apenas
após sucesso** (sem `split(';')`, sem parser próprio, sem EF Core Migrations).

Tabela `schema_migrations` (contrato mínimo):

| Coluna | Tipo/regra |
|---|---|
| `version`/`id` | PK da migration |
| `filename` | nome do ficheiro executado |
| `sha256` | checksum do ficheiro |
| `applied_at` | timestamp de aplicação |

Podem manter-se outros campos já aprovados (ex.: execution time) se não criarem contradição.

## 13. Migrations — execução e lifecycle de produção (GLM-DATA-13)

Execução de migrations: **CLI ONLY**.

```text
dotnet BA.Dmo.Web.dll migrate
dotnet run --project src/BA.Dmo.Web -- migrate      (development)
```

Proibido: `/admin/migrations`, HTTP migration endpoint, correr migrations automaticamente no startup
normal de Production. Lifecycle de produção (Render):

```text
GitHub push → Render build → pre-deploy migration command →
  success (exit 0) = continua deployment / failure (exit ≠0) = aborta deployment → start Web service
```

Sem container registry como requisito; o modelo normal é GitHub → Render builds Dockerfile.

## 14. Supabase Auth boundary e least privilege (GLM-DATA-14)

- Application/Web **não** dependem diretamente de `supabase-csharp`, REST implementation details nem
  service role. Contrato: `ISupabaseAuthAdapter` + `IAdminProvisioningAdapter` (ou nomes equivalentes
  coerentes com 03_ARCH §14). Implementação concreta (community `supabase-csharp` OU REST direto) é
  `SAFE IMPLEMENTER CHOICE BEHIND ADAPTER`; `supabase-csharp` nunca descrito como SDK oficial
  first-party.
- **Service Role** = least privilege: apenas operações administrativas privilegiadas
  (provisioning/bootstrap); **nunca** no request pipeline normal; **nunca** no browser. Runtime
  normal usa credenciais apropriadas + identidade do utilizador/JWT/RLS conforme contrato.
- Credenciais em user secrets/environment; nunca em repositório ou browser.

## 15. Initial Admin bootstrap (GLM-DATA-15)

Primeiro Admin = **ONE-SHOT CLI** privilegiado e auditável:

```text
dotnet BA.Dmo.Web.dll bootstrap-admin
```

Não usar: HostedService automatic bootstrap, setup page pública, anonymous admin, startup privileged
bootstrap. O comando é explícito, idempotente, auditável, executado apenas quando necessário; pode
usar environment variables temporárias (Render operational environment). Service Role existe apenas
durante a operação privilegiada apropriada (GLM-ARCH-14). Sem seeds operacionais fictícios (§11).

## 16. PDF storage boundary (GLM-DATA-16)

Arquitetura fechada (03_ARCH §16): PDF gerado **em memória** no backend → HTTP binary/FileResult →
Browser Blob → File System Access API → filesystem do utilizador. Backend Render **não** escreve no
filesystem do workstation; Render filesystem **não** é storage dos PDFs dos utilizadores; DB é a
source of truth dos dados estruturados; PDF é artefacto de exportação derivado. Contrato
`IPdfRenderer`; library concreta = IMPLEMENTATION DECISION (QuestPDF **não** obrigatório).

## 17. Itens que exigem verificação live posterior (SOURCE VERIFICATION REQUIRED)

1. Existência/estado das roles `ba_dmo_app`/`ba_dmo_migrate` na instância Supabase real.
2. Configuração RLS efetiva da instância (não assumir que migrations em disco estão aplicadas).
3. Credenciais Admin API para listagem de utilizadores Auth.
4. Diretório principal de relatórios e permissão local em cada posto de trabalho (renovável).
5. Eventual estado de produção da BD atual (importação — 08_MIGRATION).
