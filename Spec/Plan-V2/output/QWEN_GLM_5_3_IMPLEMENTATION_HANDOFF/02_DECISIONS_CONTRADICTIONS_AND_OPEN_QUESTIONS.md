# 02 — DECISIONS, CONTRADICTIONS AND OPEN QUESTIONS

Fonte canónica para decisões e resolução de contradições. Os restantes ficheiros do pacote
**referenciam** este documento em vez de duplicar redações (regra de não-duplicação do handoff §5.2).

Estados usados: `CONFIRMED` (decisão explícita do utilizador) · `TECHNICAL DECISION` (resolvido
pelo planning agent com evidência) · `SOURCE VERIFICATION REQUIRED` · `BUSINESS DECISION REQUIRED` ·
`SAFE TO DEFER`.

---

## 1. Contrato transversal do sistema (GLM-CORE-01)

O BA DMO é um programa de registo de factos operacionais, rastreabilidade e histórico — definição
completa em `00_START_HERE.md` §2. Este contrato aplica-se a **todos** os módulos e sobrepõe-se a
qualquer comportamento legacy que o contrarie.

## 2. Decisões explícitas atuais do utilizador (CONFIRMED)

| ID | Decisão | Fonte |
|---|---|---|
| UD-01 | Aplicação modular por domínio funcional; módulos novos registáveis sem reescrever Shell/módulos | handoff §1.1 |
| UD-02 | Atribuição de módulos/capabilities por **templates de acesso** editáveis na Administração; título/função do perfil é apenas texto visual | handoff §1.2 |
| UD-03 | Tabs da Shell derivadas dos módulos autorizados; tab escondida não é segurança; revalidação server-side de tudo | handoff §1.2/§1.3 |
| UD-04 | Sem segundo login, sem launcher central; landing determinístico: Boquilhas quando autorizada; `admin.gerir` sem Boquilhas → Administração | handoff §1.3 |
| UD-05 | `Controlo` é área funcional separada; Peso e Pegamentos pertencem a Controlo sem fusão de lógicas | handoff §1.4 |
| UD-06 | Peso com experiências exclusivas Operador/Responsável determinadas por `peso.aprovar`; sem selector manual; Responsável não recebe página do Operador nem vice-versa | handoff §1.5 |
| UD-07 | Natureza de registo/rastreabilidade/histórico; sem previsão, recomendação, decisão ou julgamento operacional; sem bloqueios por heurística | handoff §1.6 |
| UD-08 | Boquilhas 20→25: retorno completo aceite e persistido, sem bloqueio nem autorização especial; ver `modules/01_BOQUILHAS_SPEC.md` §6 | handoff §1.7 |
| UD-09 | `BQ.RETURN_UNMATCHED_NOT_ALLOWED` / `AllowUnmatched` não são transportados como hard block nem autorização manual | handoff §1.7.6 |
| UD-10 | Proteção do último Admin (self-lockout); utilizadores/templates desativados, nunca apagados | `Spec/04_ADMIN_COMPLETE_SPEC.md` §10–11 + handoff §1.2.8 |
| UD-11 | Cálculos do Peso são determinísticos e autoritativos (C#); apresentação permitida, conversão em decisão proibida | handoff §1.6.7 |
| UD-12 | Utilização manual confirma-se por pessoa; informa, nunca bloqueia | merged §3/§15c |
| UD-13 | `reparacao_externa` é **um único módulo atribuível**; dentro dele: Boquilhas, Contra Moldes, Moldes Finais, Envios, Histórico, Definições; BQ por referência/lote/quantidade; CM e MF por referência/lote/número individual, com o mesmo fluxo de preparação e envio antes da produção, separados internamente por tipo de entidade/apresentação; Reparação Interna é outro módulo | instrução de sincronização §4 (design atual confirmado por CODER_HANDOFF §14 + REPARACAO_EXTERNA brief) |
| UD-14 | Controlo é **área/domínio funcional** (não um simples agrupador visual); Peso e Pegamentos pertencem a Controlo, são **atribuíveis separadamente**, apenas filhos autorizados aparecem e as lógicas nunca são fundidas | instrução de sincronização §4 + handoff master §1.4 |
| UD-15 | Peso é **um módulo** com duas **experiências mutuamente exclusivas** (Operador sem `peso.aprovar`; Responsável com `peso.aprovar`); sem seletor manual; sem acesso cruzado às páginas. Operador/Responsável não são submódulos nem módulos separados | instrução de sincronização §4 + handoff master §1.5 |
| UD-16 | A landing page global é **Job On para todos os utilizadores autenticados** (incl. Administrador); não é configurável por utilizador; todos consultam, apenas o papel/template técnico Responsável edita (`jobon.edit`/`jobon.configure`); Administração acede-se pela navegação | design atual consolidado (HANDOFF_INDEX, PORTAL_LOGIN_ADMIN §"Landing page", CODER_HANDOFF §8/§11, README) — o handoff master §1.3.5 prevê explicitamente esta substituição "salvo evidência atual e decisão explícita em contrário"; regista-se aqui a substituição de UD-04 quanto ao landing |
| UD-17 | Auditoria global: todas as ações de negócio relevantes de todos os utilizadores geram eventos factuais append-only (utilizador/módulo/ação/entidade/data-hora/resultado); consulta anual no Admin (tab Auditoria) com `audit.view`/`audit.export`; sem pontuação/ranking/avaliação automática | AUDITORIA_GLOBAL_HANDOFF.md (novo) + HANDOFF_INDEX |

Nota sobre UD-04: a parte "Boquilhas como landing" fica substituída por UD-16. As restantes regras de
UD-04 mantêm-se: um único login, sem launcher central, abertura determinística (agora sempre Job On).

## 3. Decisões técnicas do planning agent (TECHNICAL DECISION)

### 3.1 BT-01 — Baseline
Usar **ambas** as famílias como evidência: `Spec/` (production-cloud) para comportamento e
backend de BQ/Peso/Admin/Auth; `ba-dmo-v2` para o modelo alargado (ferramentas, Job On, Controlo,
repair, warehouse, tampões). A aplicação nova é uma **fresh build** planeada; nenhum ramo é
continuado cegamente (merged §15d).

### 3.2 BT-02 — Estrutura de dados de ferramentas
UUID PK + business keys. `tool_references` (identidade mestre por tipo) + `tool_lotes` (lote =
**dado** repetível; UNIQUE por referência+lote para BQ e CM/MF quando a fonte o exige). Para CM/MF,
`physical_pieces` com número individual e ID imutável. Rejeitado o modelo README de identidade
quádrupla como PK (C3/C18) — ver AB-01 abaixo.

### 3.3 BT-03 — Autoridade de histórico
Histórico de negócio **por domínio** (append-only próprio: movimentos/correções/voids BQ; approval_log
Peso; repair_events; warehouse_movements; tampao_movements; job_on_audit_event). As ações de
utilizador/administrador são a camada de auditoria global única `audit_events` (TD-19) — não uma
tabela de histórico de negócio. O módulo transversal História/Auditoria é uma **vista de leitura**
(união por consulta) que nunca escreve numa tabela universal de negócio (08_SUPABASE §12; merged §5.15).

### 3.4 BT-04 — jsonb vs colunas
Colunas relacionais para factos consultáveis/filtráveis; jsonb apenas para setup/facts
variáveis por role, snapshots e secções de controlo (MODELO_LOCKED princípios).

### 3.5 BT-05 — Stock de Armazém
Ocupação materializada (`warehouse_stock` UNIQUE(location, tool_lote)), escrita atómica
posição+movimento; `fora` = calculado (sem posição). A VIEW `tool_status` partida do v2 (C21)
não é transportada.

### 3.6 BT-06 — Concorrência
Optimistic concurrency com `updated_at` (BACKEND-DECISION-09); append-only isento; sem `xmin`,
sem coluna `version` (a família v2 `revision` não é transportada).

### 3.7 BT-07 — Fronteira de software da reparação
Um bounded context **Repair** com três fluxos separados (AB-03): BQ por quantidade, CM/MF por
número individual (externa), interna como evento de turno. Tabelas partilhadas apenas para
reparadores e listas de saída programada; entidades e máquinas de estado separadas. Rejeitada a
fusão num único agregado.

### 3.8 BT-08 — Estratégia de migração
Fresh-build schema numa **nova família de migrations** da aplicação nova. As famílias existentes
(`Spec/database/migrations` 001–006 e `ba-dmo-v2/migrations` 000–008) permanecem em disco como
provenance; nenhuma é executada nem continuada. Importação de dados legacy tratada em
`08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md` (OPEN-03 mantido).

### 3.9 TD-09 — Auth provider
**Supabase Auth** + cookie session ASP.NET Core (07_C_SHARP §9; BACKEND-DECISION). `firebase_uid`
e `entry_capability` do v2 (C20) são rejeitados. Identidade interna: `internal_users(auth_user_id
uuid, actor_id, display_name, profile_title, template_id, active)` + `access_templates.modules`.

### 3.10 TD-10 — Catálogo de módulos e capabilities
Catálogo **controlado em código** (fonte de verdade) com espelho em DB para ordenação/visualização
administrativa. A Administração atribui apenas entradas existentes; identificadores arbitrários são
rejeitados server-side (ADMIN-DECISION-02 + handoff §1.2.6). Catálogo canónico em
`modules/00_MODULE_CATALOG.md` (resolve C14, C19, C25).

### 3.11 TD-11 — Linhas canónicas
`B1, B2, B3, C1, C2, C3` (todas as fontes atuais e o design usam B1–C3). `B4` apenas existe no
default v2 — `SAFE TO DEFER` como extensão de catálogo (C12).

### 3.12 TD-12 — Constantes do Peso (C6)
Fonte única C# (`WeightCalculator`), valores confirmados NNPB `2.4027` / PS `2.4231`, editáveis
via `peso_settings` (`constant_nnpb`, `constant_ps`). A duplicação JS `peso.js` não é transportada.
Nenhuma cópia de fórmulas em componentes visuais.

### 3.13 TD-13 — Comparação anterior do Peso (C5)
Manter **ambos** os mecanismos conforme decisão do utilizador: tabela `peso_comparacao_anterior`
(completar o **read path**) + snapshot `previous_control` jsonb.

### 3.14 TD-14 — start_line NOT NULL (C29)
Mantido para traces de produção; a criação de trace passa a incluir sempre movimento START +
leitura inicial numa transação (resolve GAP-BACKEND-01), eliminando a divergência RPC/constraint.

### 3.15 TD-15 — Repairer canónico (C30)
`repairers(id, name, active)` + `line_repairer_defaults(line, tool_type, repairer_id)`. As quatro
formas legacy colapsam nesta; `shop` em texto livre deixa de ser mecanismo primário (permitido como
snapshot histórico no movimento). Reparadores desativados, nunca apagados.

### 3.16 TD-16 — Criação de utilizadores Auth (C13)
A Administração pode (a) associar utilizador Auth existente e (b) criar utilizador Auth via Admin
API server-side (BACKEND-DECISION-18), porque o utilizador autorizou a criação de utilizadores.
`service_role` nunca exposta ao browser.

### 3.17 TD-17 — Processo NNPB/PS e lote do Peso (design sync)
O processo `NNPB/PS` é definido **na criação do lote no módulo Peso** (não na referência mestre).
O lote do Peso inclui ainda máquinas permitidas (cartões B1–C3, mínimo uma) e `Subpasta dos
relatórios` (nome relativo resolvido sob o diretório principal configurado). Job On, Novo controlo e
Comparação herdam o processo do lote do Peso associado; nunca é pedido novamente no controlo.
Substitui a redação anterior “processo na referência/template” (PESO_INTERFACE_HANDOFF novo §"Origem
do processo"; FERRAMENTAS brief §2/§4; CODER_HANDOFF §10/§17).

### 3.18 TD-18 — Persistência do Job On (design sync)
Contrato canónico = `JOB_ON_DATA_MODEL.md`: `job_on`, `job_on_revision` (imutáveis),
`job_on_component`, `job_on_component_field` (tipado), `job_on_component_row` (CAL),
`job_on_verification_occurrence`, `job_on_audit_event`, `job_on_field_option`. Revisão guardada nunca
é atualizada destrutivamente; duplicação copia o snapshot completo; consumidores (Peso, Pegamentos,
PDFs, aprovações) guardam o `job_on_revision_id` exato. Substitui o esquema anterior do pacote
(`productions`/`production_lines`/`jobon`/`jobon_tools` da proposta v2). Unicidade: usar a identidade
real do negócio; não impor unicidade que elimine múltiplos Job Ons para a mesma Produção/Referência/Máquina
se o programa o permitir (JOB_ON_DATA_MODEL §2).

### 3.19 TD-19 — Auditoria global canónica (design sync)
Existe **uma única tabela canónica de eventos de auditoria** (`audit_events`), append-only, para
todos os módulos e utilizadores, com os campos de `AUDITORIA_GLOBAL_HANDOFF.md` §3 (incl. `year`,
`actionCode`, `correlationId`, `jobOnId`, `revisionId`, `before/afterSummary`), índice/filtro anual.
As ações administrativas são eventos desta mesma tabela (`moduleId=admin`); não existe tabela de
auditoria separada por módulo nem por ano. O histórico de negócio próprio de cada domínio
(movimentos, correções, snapshots, approval_log) mantém-se como registo domain; a auditoria global é
a camada transversal de “quem fez o quê”. Backend autoritativo; mesma transação que o domínio sempre
que possível (outbox/correlação se assíncrono). Substitui o desenho anterior `admin_audit_log` isolado.

### 3.20 TD-20 — Capabilities Job On (design sync)
`jobon.view` (todos os utilizadores ativos), `jobon.edit` e `jobon.configure` (apenas papel/template
técnico Responsável). A confirmação de verificações usa capability própria que não concede edição
estrutural da folha (CODER_HANDOFF §11); as fontes declaram a nomenclatura final por fechar
(JOB_ON_VERIFICACOES §16), pelo que se adota o identificador estável `jobon.confirmar` como decisão
técnica de catálogo (renomeável sem alteração de comportamento).

### 3.21 TD-21 — Nomes de ficheiros PDF (design sync, P2.9)
Convenção derivada do snapshot do Job On/controlo aprovado: inclui pelo menos Produção, Referência,
tipo de documento (`Peso`/`Pegamentos`) e revisão/data; nunca texto livre do operador
(PESO_INTERFACE_HANDOFF §"Persistência"; CODER_HANDOFF §17). A convenção legada do Peso
`{mold}{neckring}__{periodo}__{line}__L{lote}.pdf` permanece como instanciação válida para
documentos Peso; documentos Pegamentos acrescentam o tipo `Pegamentos`.

### 3.22 TD-22 — repair_exit_items e BQ (design sync; resolve inconsistência interna #3)
`repair_exits`/`repair_exit_items` cobrem os três tipos do ciclo externo: **BQ (quantidade)**,
CM e MF (número individual), conforme o modelo v2 (`repair_type BQ|CM|MF`) e o brief de Reparação
externa (itens BQ = Referência, lote, quantidade). A semântica de retorno excedente em BQ continua
governada pelo modelo de discrepances de Boquilhas (C27/UD-08); as duas camadas coexistem sem
contradição: a lista programada regista o ciclo físico; os movimentos/discrepancies BQ registam a
reconciliação de quantidades. Corrigida a redação anterior “repair_exit_items apenas CM/MF”.

### 3.23 TD-23 — Ownership da imagem do artigo (design sync; resolve open question anterior)
A imagem é guardada por **revisão do Job On** (`job_on_revision.image_asset_id`), com substituição/
remoção confirmadas e auditadas; não é copiada entre produções sem regra explícita
(JOB_ON_DATA_MODEL §2; JOB_ON_DESIGN_BRIEF §"Imagem do artigo").

### 3.24 TD-24 — História e autorização de origem (design sync; resolve questão conhecida #6)
As vistas transversais de História/Auditoria aplicam a autorização dos **módulos de origem**: um
utilizador apenas vê eventos/registos de módulos que o seu template autoriza; a tab Auditoria do
Admin exige `audit.view`/`audit.export`. Nenhum dado é apresentado fora do âmbito autorizado.

### 3.25 TD-25 — Tabela canónica de densidades do Peso (LEGACY RECOVERY; GAP-002)
Recuperada da implementação real autoritativa `Spec/src/BA.Dmo.Domain/Modules/Peso/Services/
WeightCalculator.cs` (fonte única; exposta ao preview JS por injeção server-side, sem duplicação) e
confirmada ponto a ponto por `WeightCalculatorTests.cs` (31 valores + fronteiras). Tabela de
densidades da água (g/cm³), chave = temperatura inteira °C:

| °C | ρ | °C | ρ | °C | ρ | °C | ρ |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 5 | 0.99888 | 13 | 0.99832 | 21 | 0.99696 | 29 | 0.99494 |
| 6 | 0.99885 | 14 | 0.99819 | 22 | 0.99674 | 30 | 0.99485 |
| 7 | 0.99882 | 15 | 0.99805 | 23 | 0.99652 | 31 | 0.99435 |
| 8 | 0.99877 | 16 | 0.99789 | 24 | 0.99628 | 32 | 0.99403 |
| 9 | 0.99871 | 17 | 0.99773 | 25 | 0.99603 | 33 | 0.99371 |
| 10 | 0.99863 | 18 | 0.99765 | 26 | 0.99577 | 34 | 0.99339 |
| 11 | 0.99854 | 19 | 0.99737 | 27 | 0.99551 | 35 | 0.99305 |
| 12 | 0.99844 | 20 | 0.99717 | 28 | 0.99523 | | |

Regras de lookup (provenance: código + testes): temperatura decimal é arredondada ao inteiro mais
próximo (`MidpointRounding.AwayFromZero`; 4.50→5, 35.49→35); abaixo de 5 ou acima de 35 → erro
(`ArgumentOutOfRangeException` → erro de domínio apresentado ao utilizador); sem interpolação;
precisão da tabela: 5 casas decimais. Unidade: g/cm³. Versões divergentes: **nenhuma encontrada**
(peso.js já não duplica a tabela; recebe-a do servidor). Não substituir por fórmula externa.

### 3.26 TD-26 — Identidade do lote CM (LEGACY RECOVERY; GAP-001)
Resolução conceptual com provenance (peso_templates/Template.cs/migration 003; design atual;
Ferramentas brief):
1. **Uma identidade conceptual de lote CM** (o lote de um CM), com **dois registos de domínio com
   responsabilidades distintas** — nunca duas identidades paralelas silenciosas:
   - `peso_references` + `peso_lotes` (módulo **Peso**): a referência do Peso é o par
     molde+neckring (UNIQUE mold_number+neckring_number — legacy migration 003); o **lote do Peso**
     detém `processo NNPB/PS`, `allowed_lines`, `report_subfolder`, peso nominal/volumes herdados da
     referência. É a identidade de controlo consumida por Job On e Pegamentos como “CM/lote”.
   - `tool_references`/`tool_lotes`/`physical_pieces` (módulo **Ferramentas**): registo físico da
     ferramenta CM/MF (desenho, estado, peças individuais) — identidade física usada em seleção,
     Armazém e Reparação.
2. Relação: o lote do Peso e o tool_lote CM correspondem ao mesmo CM físico pelo par legível
   (código do molde + lote); o Job On guarda ambos (IDs Ferramentas + referência/lote do Peso).
   Sem FK cruzada obrigatória inventada; a ligação é o par de códigos estável registado no Job On.
3. Legacy note: `peso_lotes` não existia na geração production-cloud (apenas `default_lote` no
   template e `lote` por controlo) — a entidade lote do Peso é introduzida pelo design atual
   (criação/duplicação de lotes no Peso); a duplicação anterior é apenas dívida técnica legacy, não
   uma segunda identidade de negócio.
4. Onde vivem os atributos: NNPB/PS → lote do Peso; allowed lines → lote do Peso (linhas do controlo);
   report subfolder → lote do Peso; peso nominal e volumes (punção/marisa/tampão) → referência do
   Peso (herdados pelo controlo via snapshot).

### 3.27 TD-27 — Job On ativo e lifecycle (LEGACY RECOVERY; GAP-003)
- **Lifecycle canónico** (design atual JOB_ON_DATA_MODEL): `rascunho → planeado → em fabrico →
  fechado`; `cancelado` a partir de rascunho/planeado. Transições: rascunho→planeado por Gravar do
  Responsável (`jobon.edit`) com datas planeadas; planeado→em fabrico por início confirmado/edição
  autorizada (timestamp); →fechado por fecho autorizado (regista `fechado_em`); cancelar regista
  motivo e ator. Efeitos: `rascunho` nunca é resolvido como ativo; `fechado`/`cancelado` excluídos
  de resolução e de novos consumos; edições em `fechado` exigem `change_reason` (revisão nova).
  Provenance parcial legacy: v2 `jobon` com `fechado` boolean + `fechado_em` + readiness
  (planned/active/…); os estados readiness legacy não transitam como estados de domínio.
- **Resolve(line, at)** — lookup canónico (preserva o conceito do pacote; definição fechada agora):
  candidatos = Job Ons com `machine = line`, estado ∈ {planeado, em fabrico}, e `at` dentro de
  `[planned_start_at, planned_end_at]` (se `planned_end_at` nulo, o limite superior é o próximo
  `planned_start_at` do mesmo `machine` — provenance legacy: v2 derivava `data_saida` como a próxima
  `data_entrada` da mesma linha). 1 candidato → devolvido; múltiplos → **escolha explícita** (nunca
  auto-selection); 0 → “Sem Job On ativo para esta Linha/data” (consumidores bloqueiam de forma
  acionável). Consumidores: Reparação Interna, Boquilhas (painel lateral), Peso, Pegamentos.

### 3.28 TD-28 — Mapping de volumes do Peso (LEGACY RECOVERY; DG-02)
Confirmado no cálculo real (WeightCalculator.cs + doc comments + Template.cs + peso.js):
`glass = (capacity + volumeNeck − volumePu) × constante[tipo]` onde **`volumePu` = volume do punção
(subtraído)** e **`volumeNeck` = volume da marisa (adicionado)**; `volume_tampao` = volume do tampão
calculado pela fórmula da calote `π·s²·(3r−s)/3` (sagitta/raio do baffle), não entra na fórmula do
peso em vidro. A asserção “punção = volumePu, marisa = volumeNeck” está confirmada — não é assunção.

### 3.29 TD-29 — Base da comparação (LEGACY RECOVERY; DG-03)
Quando existem vários controlos aprovados, a base da Comparação é o **Novo controlo aprovado do
mesmo Job On** (design atual: o utilizador seleciona o controlo aprovado e ativa `Comparar`). Sem
Job On/controlo aprovado associado, a Comparação não é criada (mensagem acionável). Provenance
legacy: o legacy escolhia a base por picker de controlo aprovado; a regra atual fixa-a no contexto
do Job On.

### 3.30 TD-30 — Comparação anterior / delta vs anterior (LEGACY RECOVERY; DG-04)
`peso_comparacao_anterior` mantém-se (TD-13). Para o delta “vs anterior” de um Novo controlo, a
resolução é **automática**: último controlo aprovado do mesmo molde+neckring (referência/CM) com
produção/data estritamente anteriores — **a linha NÃO é condição de matching** (comparação
cross-line; provenance: PesoService `GetPreviousApprovedControlAsync` + clarification §4; o spec
antigo referia “mesma linha”, substituído pela clarificação de segunda passagem). Não é seleção
manual. `peso_comparacao_anterior` guarda os dados SAP da produção anterior (período, datas, dias,
peso médio) introduzidos pelo operador.

### 3.31 TD-31 — Convenção de filename dos PDFs (LEGACY RECOVERY)
- **Peso**: `{mold}{neckring}__{periodo}__{line}__L{lote}.pdf` (separadores duplos `__`; prefixo
  `L` no lote; extensão minúscula). Provenance: gerador real `PesoFilenameBuilder.Build` + ficheiro
  de referência `9262T288__202604__C3__L16.pdf`. Substitui redações anteriores sem provenance.
- **Pegamentos**: `Pegamentos_{produção6}_{referência}_{jobOnId}_relatorio.pdf` (nomes sanitizados;
  provenance: mockup v1.9 congelado como design baseline; sem fonte funcional legacy no workspace).
- Subpasta: `diretório principal / subpasta do lote` (TD-21/DS-08 mantém-se).

### 3.32 TD-32 — Regras dos Pegamentos (LEGACY RECOVERY)
Não existe implementação funcional legacy dos Pegamentos no workspace (apenas hooks de schema v2
`control_type='pegamento'` e o mockup v1.9). As regras operacionais promovidas vêm do design
baseline congelado (`pegamentos.html` v1.9 + brief), com provenance:
- medições por componente CM/BQ/MF: `Costura` e `Contra costura` (nome interno legado `noventa`);
- **ovalização = Costura − Contra costura**; **média = (Costura + Contra costura)/2** (ou o valor
  único quando só existe um);
- tolerância nominal: **nominal ± 0,20 mm** por componente (default configurável por registo);
  verificação sobre a média;
- gaps entre componentes (boundary map): CM→Boquilha e Boquilha→Molde final, tolerância default
  **0,05** (configurável);
- componentes com contagens de secções por omissão CM=2, BQ=8, MF=14 (demonstrativo);
- filename ver TD-31. Classificação: valores do mockup = design baseline (não “legacy funcional”);
  sem fonte legacy real que os contradiga, mantêm-se; se surgir fonte funcional comprovada, esta
  prevalece sobre o mockup.

### 3.33 TD-33 — Capability `ferramentas.configure` (clarificação)
A gestão das regras de verificação na ficha da ferramenta/lote exige `ferramentas.configure`
(Chefe/Responsável autorizado); a confirmação de ocorrências no Job On usa `jobon.confirmar`
(operador autorizado). Enforcement server-side. Sem colisão semântica com as capabilities existentes
(`{moduleId}.{ação}` verificado no catálogo: jobon.view/edit/configure/confirmar, peso.aprovar,
admin.gerir, audit.view, audit.export, reparacao_interna.corrigir, ferramentas.configure).

### 3.34 Rejeições legacy desta passagem (classificação D/C)
| Descoberta legacy | Fonte | Classificação | Destino |
|---|---|---|---|
| `BQ.RETURN_UNMATCHED_NOT_ALLOWED` + flag `AllowUnmatched` + checkbox “Entrada excecional” como autorização | `MovementValidator.cs` L169–183; `Movement.cs`; `bq_movements.allow_unmatched`; UI Index.cshtml | **D. CONTRADICTS CURRENT USER/DESIGN DECISION** (UD-08/UD-09) | Não transportar; o retorno excedente regista-se sempre com aviso + observação + discrepancia |
| Cálculo `matched/unmatched/exceptionalReceived` do `InventoryCalculator` | `InventoryCalculator.cs` | **A. CONFIRMED CURRENT DOMAIN KNOWLEDGE** | Mantido (conforme 20→25) |
| `peso.js` sem tabela própria (injeção server-side) | `peso.js` header | **A** | Mantido — fonte única C# |
| Job On readiness states v2 (attention_required/blocked/…) como estados de domínio | v2 002_productions_jobon.sql | **C. TECHNICAL LEGACY — DO NOT CARRY FORWARD** | Estados de domínio = rascunho/planeado/em fabrico/fechado/cancelado; readiness só como avisos |
| `data_saida` derivada por sequência da linha | v2 JobOn.cs | **B. SUPPORTING LEGACY EVIDENCE** | Suporta o limite superior do Resolve(line, at) |
| `pegamentosDB` localStorage | pegamentos.html | **C** | Não transportar |
| Atalhos de teclado (Enter/Espaço/Ctrl+Enter) em listas | dmo-interactions.js; DMO §13; CODER §4 | **D/C** | Não transportar como requisito funcional (NC-01 resolvido) |


## 4. Resolução de AB-01, AB-02, AB-03

### AB-01 — Identidade operacional da ferramenta (TECHNICAL DECISION + evidência do utilizador)
Reconciliação por **camadas de identidade** (vocabulário do próprio MAP §1):
1. `tool/asset`: registo mestre = `(tool_type, referência de ferramenta, lote)` — entidade estável;
2. `set/toolSet`: `(ToolType · Reference · Lot · Line)` — camada **operacional** usada nos factos
   de produção (Job On, produção por linha). A afirmação do utilizador (“5447 lote 3 b1 é uma
   ferramenta, b3 outra”) aplica-se a esta camada: conjuntos operacionais distintos por linha;
3. factos (movimentos, utilização, reparações) ligam-se ao registo estável **com contexto de linha**,
   nunca reescritos.
Consequência: o registo mestre não é duplicado por linha; os factos por linha são eventos distintos.

### AB-02 — Mudança temporária de linha (TECHNICAL DECISION)
Mudança de linha é um **evento de estado/localização** do mesmo lote (movimento `linha` em BQ;
alteração de contexto em CM/MF), não uma nova identidade. O toolSet anterior permanece no histórico;
o novo contexto é registado. Nenhuma reescrita silenciosa (confirma o teste do merged §15a STEP 7).

### AB-03 — Semântica de registo da reparação (CONFIRMED pelo utilizador, estrutura BT-07)
- BQ: diária, alta frequência, por **quantidade**, reparadores por linha;
- CM/MF: externa, ciclo de dias, por **número individual**, dentro/fora;
- Interna: intervenção de turno, ferramenta continua na linha, registo rápido.
O *quê registar* está fechado; as tabelas/agregados são decisão técnica (BT-07).

## 5. Inventário e classificação de hard blocks (GLM-CORE-02)

| Bloqueio existente/proposto | Fonte | Classificação | Destino |
|---|---|---|---|
| Autenticação obrigatória | global | SECURITY | Mantido |
| Autorização por módulo/capability server-side | global | SECURITY | Mantido (estendido a todos os módulos) |
| BQ module guard em falta | 03_AUTH §9 | SECURITY (gap) | **Implementar** na app nova |
| RLS como defesa em profundidade | 08_SUPABASE §14 | SECURITY | Mantido |
| Self-lockout do último Admin | 04_ADMIN §11 | CONFIRMED BUSINESS RULE | Mantido |
| Saída BQ qty ≤ produção atual (BQ-RULE-003) | 01_BQ §6 | CONFIRMED BUSINESS RULE | Mantido |
| Não reparáveis ≤ reparação (BQ-RULE-005) | 01_BQ §6 | CONFIRMED BUSINESS RULE | Mantido |
| Correção não cria saldo negativo (BQ-RULE-006) | 01_BQ §6 | CONFIRMED BUSINESS RULE | Mantido |
| Reopen só do último trace fechado (BQ-RULE-007) | 01_BQ §6 | CONFIRMED BUSINESS RULE | Mantido |
| Lote com trace ativo não arquivável (BQ-RULE-008) | 01_BQ §6 | CONFIRMED BUSINESS RULE | Mantido |
| `BQ.RETURN_UNMATCHED_NOT_ALLOWED` / `AllowUnmatched` | código legacy | **UNSUPPORTED HEURISTIC** | **Removido** (UD-08/UD-09); retorno > saldo é WARNING ONLY + registo de exceção |
| Limite de retorno ao saldo esperado | heurística | UNSUPPORTED HEURISTIC | Não transportar |
| Rejeitar nota em falta na rejeição Peso | 02_PESO §4 | CONFIRMED BUSINESS RULE | Mantido |
| Estado editável só rascunho/nao_aprovado | 02_PESO §4 | CONFIRMED BUSINESS RULE | Mantido |
| Delete Peso: proibido pendente/aprovado | 08_SUPABASE §9 | CONFIRMED BUSINESS RULE | Mantido |
| Duplicado de controlo (mold+neckring+produção+linha+lote+data) | 02_PESO §4 | TECHNICAL INTEGRITY | Mantido |
| Item já incluído noutra saída aberta | RE_EXT §8 | TECHNICAL INTEGRITY | Mantido (duplicação técnica) |
| Retorno sem saída correspondente | RE_EXT §8 | **WARNING ONLY + registo** | Regista facto, pede observação; não inventar bloqueio operacional |
| Falha de CM/BQ/MF obrigatórios ou lote inválido no contexto Job On (Peso/Comparação/Pegamentos) | PESO_INTERFACE novo §Referências; PEGAMENTOS novo §Integração | CONFIRMED BUSINESS RULE (design atual) | Bloqueia abertura da folha e encaminha `Corrigir ferramentas no Job On`; nunca escolhe outra ferramenta automaticamente |
| Entrada de Armazém em posição ocupada | CODER_HANDOFF §12 | TECHNICAL INTEGRITY | Bloqueada pelo estado registado (ocupação 1:1); sem fluxo preventivo UI nem `Substituir` no fluxo normal |
| Operações de edição/duplicação/configuração do Job On sem `jobon.edit`/`jobon.configure` | CODER_HANDOFF §11 | SECURITY | Bloqueio server-side; ocultação UI não é autorização |
| Consulta/exportação da Auditoria sem `audit.view`/`audit.export` | AUDITORIA_GLOBAL §8 | SECURITY | Bloqueio server-side |
| Saldo negativo de tampões | TAMPOES §5 | TECHNICAL INTEGRITY | Mantido (impossibilidade aritmética) |
| Transferência de tampões parcial | TAMPOES §6 | TECHNICAL INTEGRITY | Mantido (atomicidade) |
| Job On “ readiness blocked” (falta CM/MF/BQ) | MODELO_LOCKED 6b | **WARNING ONLY** | Avisos descritivos; nunca impedem registo |
| Utilização perto do limite | MODELO_LOCKED 6b | **WARNING ONLY** | Informa, nunca bloqueia (UD-12) |
| Previsões de data de saída por sequência | legacy | UNSUPPORTED HEURISTIC | Não transportar como facto; apenas sugestão visual identificada |
| `window.PCVAuth.role==='administrador'` | 03_AUTH §10 | UNSUPPORTED HEURISTIC | Substituído por `admin.gerir` |

## 6. Contradições C1–C32 — estado final

| ID | Estado | Resolução |
|---|---|---|
| C1 | RESOLVED | BT-01 (ambas evidência; fresh build) |
| C2 | RESOLVED | BT-02: lote é **chave** em BQ (`UNIQUE(reference, batch_code)` — comportamento atual forte) e **dado** em tool_lotes CM/MF (máquina/linha desambigua) |
| C3 | RESOLVED | Rebaixado; identidade quádrupla nunca foi decisão do utilizador; camadas em AB-01 |
| C4/C18 | RESOLVED | Camadas de identidade AB-01/AB-02; README v2 marcado contraditório |
| C5 | RESOLVED | TD-13 (keep + read path) |
| C6 | RESOLVED | TD-12 |
| C7 | RESOLVED | Ownership por módulo no catálogo; CM/MF donos dos seus dados; Job On apenas consulta (03_ARCH §6) |
| C8 | RESOLVED | Utilização manual com before/added/after/cumulative como **facto registado** (usage_records); sem cálculo automático |
| C9 | RESOLVED | PDF derivado real (binário), output local configurável, apenas `filename` persistido |
| C10 | RESOLVED | Distinção saldo transacional UI vs inventário físico preservada (modules/01 §7) |
| C11 | RESOLVED | BT-08 (nova família; antigas = provenance) |
| C12 | RESOLVED | TD-11 |
| C13 | RESOLVED | TD-16 |
| C14 | RESOLVED | TD-10 |
| C15 | RESOLVED | Paleta V1 `#3C73A8` + tokens do design system; azuis legacy não transportados |
| C16 | SAFE TO DEFER | `admin.consultar` não existe na V1 |
| C17 | RESOLVED | `sap_start`/`sap_end` mantidos como leituras de utilização (não remover; não ler como integração SAP) |
| C19 | RESOLVED | Catálogo canónico (modules/00) — 3 módulos atuais + módulos novos; `admin` incluído |
| C20 | RESOLVED | TD-09 (Supabase; firebase rejeitado) |
| C21 | RESOLVED | VIEW partida não transportada (BT-05) |
| C22 | RESOLVED | Estados canónicos definidos em 06_DATA §5; divergência MODELO_LOCKED/000_tools fechada |
| C23 | RESOLVED | README v2 = histórico |
| C24 | RESOLVED | Três noções nomeadas explicitamente: `ref_code` ARTIGOS (MODELO+MARISA), `tool_references.ref_code` (ferramenta), referência Peso (mold+neckring); sem fusão |
| C25 | RESOLVED | `admin` no catálogo canónico (TD-10) |
| C26 | RESOLVED | BT-07 + modelo único de repair no pacote (modules/08/09) |
| C27 | RESOLVED | Ver §7 abaixo |
| C28 | RESOLVED | Storage local legacy não transportado (DO NOT CARRY FORWARD) |
| C29 | RESOLVED | TD-14 |
| C30 | RESOLVED | TD-15 |
| C31 | RESOLVED | Shell reconstruída em `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md` |
| C32 | RESOLVED | Roadmap novo substitui o índice de 65 steps |

### C27 — reconciliação técnica do excesso de retorno
Três representações (`exceptionalReceived`, `Discrepancy`, `repair_exit_items`) reconciliadas **sem
reabrir UD-08**:
1. o movimento de retorno guarda a quantidade real (25); o cálculo `matched/unmatched` deriva
   `exceptional_received` (campo do cálculo e do facto de retorno, não autorização);
2. o excesso gera um registo **discrepancy** de primeira classe (`bq_discrepancies`: expected,
   actual, excess, status open/under_review/resolved, resolution auditável) — é o facto auditável
   da resolução posterior;
3. `repair_exit_items` regista o ciclo externo por item — BQ por quantidade, CM/MF por número
   individual (TD-22); a reconciliação do excesso em BQ é a discrepancia do domínio Boquilhas.
`allow_unmatched` **não existe** no schema novo.

## 7. Novas contradições encontradas nesta sessão

| ID | Fontes | Resolução |
|---|---|---|
| NC-01 | Design System (Enter seleciona/Ctrl+Enter abre) vs Coder Handoff (Enter abre/Espaço seleciona) | `RESOLVED — DECISÃO ATUAL`: o BA DMO **não tem shortcuts funcionais próprios**. Interação canónica: **1 clique seleciona; 2 cliques abrem/entram no registo**. As variantes de teclado do design/legacy são `LEGACY / DEMONSTRATIVE IMPLEMENTATION DETAIL — DO NOT CARRY FORWARD`; nenhuma é promovida a requisito. Acessibilidade web standard (foco visível, navegação por tab, operação de dropdowns/calendários por teclado) permanece válida como acessibilidade, não como shortcut funcional do produto |
| NC-02 | Botão 34/36/40 divergente | compact 34, padrão 36 (ações), form/filter 40 — um token por size (contrato §18) |
| NC-03 | Catálogo v2 `repair` único vs handoff exige Reparacão interna/externa separadas | Catálogo canónico com dois módulos `reparacao_interna` e `reparacao_externa`; partilham reparadores e saída programada (BT-07) |
| NC-04 | 06_UI_UX tab `Fabrico` vs BOQUILHAS_INTERFACE_BEHAVIOR “tab Fabrico não existe” | Design final prevalece (mais recente e confirmado): **sem tab Fabrico**; lotes em produção vistos no painel lateral |
| NC-05 | `peso_templates` UNIQUE(mold,neckring,line) [08] vs UNIQUE(mold,neckring)+allowed_lines[] [99_CURRENT_STATE] | Segunda passagem (99_CURRENT_STATE) é a mais recente: UNIQUE(mold_number, neckring_number) + `allowed_lines text[]` + `tipo` |

### 7.1 Delta da sincronização de design (DESIGN-SYNC — 2026-08-16)

O design consolidado mudou de versão/localização (`portal-dmo-design-final/` na raiz). Delta factual
(provenance e hashes em `01_SOURCE_AUTHORITY_REGISTER.md` §2):

| Delta | Descrição factual | Tratamento no pacote |
|---|---|---|
| DS-01 | Landing page global passa a ser **Job On** para todos os utilizadores; Administração deixa de ser landing | UD-16; 05_SHL §4; modules/05 |
| DS-02 | Capabilities Job On: `jobon.view`/`jobon.edit`/`jobon.configure` + capability própria para confirmar verificações | TD-20; modules/00; 04_ACC |
| DS-03 | **Auditoria global** append-only por ação, consulta anual no Admin (`audit.view`/`audit.export`), sem pontuações | UD-17/TD-19; 06_DATA; modules/11; 04_ACC |
| DS-04 | Peso: Processo NNPB/PS + máquinas permitidas + `Subpasta dos relatórios` no **lote do Peso**; Novo controlo e Comparação iniciam no contexto do **Job On** (CM/lote herdados, sem segunda seleção; bloqueio com `Corrigir ferramentas no Job On` quando falta) | TD-17; modules/03; 06_DATA |
| DS-05 | Pegamentos: contexto **Job On obrigatório**; CM/BQ/MF herdados como instâncias/lotes exatos; sem fallback silencioso; PDF local com caminho resolvido `diretório principal / subpasta do lote` | modules/04; 06_DATA |
| DS-06 | Job On: Modo consulta (não editável, sem live) vs Modo edição (todos os campos editáveis; substituição com lista live: posição Armazém + estado/% do domínio); snapshot completo por revisão (`JOB_ON_DATA_MODEL.md`); histórico em dois níveis (Produções da Referência → Revisões da Produção) | TD-18/TD-23; modules/05; 06_DATA |
| DS-07 | Ferramentas: Processo movido da referência para o **lote** (fluxo Peso); verificações configuráveis por lote (regra ≠ ocorrência; reset; duplicação copia configuração sem checks) | modules/06; modules/05 |
| DS-08 | Fronteira servidor/local dos relatórios: dados estruturados no servidor; PDFs no computador configurado; falha local não desfaz aprovação; estados `Dados guardados no servidor` vs `PDF guardado localmente` | modules/03/04; 06_DATA §9 |
| DS-09 | Tokens visuais atualizados (DMO_DESIGN_SYSTEM v2.7 §4): semânticas dessaturadas `#527c72`/`#a97943`/`#9a625d`, página `#f6f9fc`, contorno `#d9e6f2`; novos componentes Tool Availability Picker, Local Directory Selector, Resolved Report Path | 07_DESIGN §2/§4 |
| DS-10 | Gaps P2 novos: P2.9 nomes de PDF; P2.10 estados/recuperação de permissão do File System Access (sem representar ficheiro local como dado garantido no servidor) | TD-21; modules/03 §documentos |
| DS-11 | Contradição de teclado no design (DMO_DESIGN_SYSTEM §13 vs CODER_HANDOFF §4) | `RESOLVED — DECISÃO ATUAL`: nenhum shortcut funcional BA DMO é reintroduzido (nem Enter, nem Espaço, nem Ctrl+Enter); 1 clique seleciona, 2 cliques abrem; as variantes do design ficam classificadas como demonstrativas e não transitam |
| DS-12 | Admin com tab `Auditoria` (Utilizadores/Aplicações/Auditoria) e ação “Voltar ao Job On” | 04_ACC §9; 05_SHL |
| DS-13 | `job_on_field_option`: dropdowns de negócio evolutivos data-driven, geridos em Definições por Família/Campo; desativar preserva snapshots antigos | modules/05; 06_DATA |
| DS-14 | Reparação externa: tab **Boquilhas** presente no módulo (quantidade por referência/lote), omitida na versão anterior do pacote; CM e MF com o mesmo fluxo de preparação/envio pré-produção | UD-13; modules/09; modules/00 |

Regra aplicada: onde o design atual contradiz uma decisão explícita anterior, a divergência foi
registada (UD-16 vs UD-04) e a substituição feita apenas onde o próprio handoff master a condiciona
a evidência atual (“salvo evidência atual e decisão explícita em contrário”); nenhuma substituição
silenciosa foi feita.

## 8. MUST PRESERVE (não pode desaparecer na fresh build)

1. Filosofia de registo passivo (regista e filtra; nunca infere) — UD-07;
2. Separação Operador/Responsável por `peso.aprovar` (UD-06);
3. Cálculos do Peso (density table 5–35 °C, fórmulas, filename `{mold}{neckring}__{periodo}__{line}__L{lote}.pdf`) — TD-12;
4. `peso_comparacao_anterior` com read path — TD-13;
5. Documentos de produção derivados: dados estruturados no servidor; PDFs no diretório local
   configurado (`diretório principal / subpasta do lote`); falha local não desfaz aprovação (DS-08);
6. Modelo de movimentos BQ: calculateTrace, saldo transacional vs inventário, movimentos imutáveis, correções/voids append-only, reopen (último), lifecycle history;
7. Múltiplos lotes da mesma referência na mesma linha = válido; referências diferentes na mesma linha = aviso (não erro);
8. Autoria server-side (`actor_id`), timestamps UTC, snapshots before/after;
9. Templates/capabilities como fonte de autorização; título visual sem poder;
10. Job On agrega “informação de todo lado” sem criar registos mestres; snapshot completo e imutável por revisão; consumidores guardam `job_on_revision_id` (TD-18);
11. Utilização manual informa, nunca bloqueia;
12. Três fluxos de reparação distintos (AB-03); Reparação externa = um módulo com BQ/CM/MF (UD-13);
13. Tampões apenas quantidade, sem associação a ferramentas/referências;
14. Auditoria global append-only por ação de negócio, anual no Admin, sem pontuações/rankings (UD-17/TD-19);
15. Landing Job On para todos os utilizadores; consulta universal, edição só do Responsável técnico (UD-16);
16. Peso/Pegamentos consomem as escolhas CM/BQ/MF do Job On sem segunda seleção; ausência/invalidade encaminha `Corrigir ferramentas no Job On` (DS-04/DS-05).

## 9. DO NOT CARRY FORWARD

1. Persistência local legacy: localStorage como BD, IndexedDB handles, pasta partilhada como
   autoridade de dados, dual-write, polling (C28). Nota: o File System Access API **é** usado no
   design atual apenas para o diretório local de relatórios (DS-08/DS-10), nunca como base de dados;
2. `firebase_uid`, `entry_capability`, envelopes `revision` do v2 (C20);
3. VIEW `tool_status` partida (C21);
4. Cálculos duplicados em `peso.js`;
5. HTML a fingir de PDF; PDF guardado no servidor como única fonte do histórico;
6. Bugs atuais: “Entrada excecional” mostrada em tipos errados; gaps de pesquisa BQ;
7. `05_SHELL_COMPLETE_SPEC.md` como autoridade de Shell (C31);
8. As duas famílias de migrations antigas como schema da app nova;
9. `BQ.RETURN_UNMATCHED_NOT_ALLOWED` / `AllowUnmatched` como bloqueio/autorização;
10. CSS local de mockups, `<style>` por página, inline styles de design, paleta antiga;
11. Globais `window.BA_IDENTITY`, `window.PCV_ACTIVE_USER`, `window.APP_ROLE`;
12. Redirect pós-login fixo para BQ; landing Boquilhas/Admin (substituídas por UD-16 — Job On);
13. Role legacy `administrador`; título/função visual como fonte de `jobon.edit`;
14. Qualquer previsão/recomendação/bloqueio heurístico do §5; pontuações/rankings na auditoria;
15. Processo NNPB/PS guardado na referência mestre (movido para o lote do Peso — TD-17);
16. Segunda seleção de CM/lote dentro de Peso/Comparação/Pegamentos (contexto vem do Job On — DS-04/DS-05);
17. Tabela de auditoria separada por módulo ou por ano (tabela única canónica — TD-19).

## 10. Fila humana final

**NONE.** AB-01/AB-02 foram reconciliados por camadas de identidade com evidência do próprio
MAP/vocabulário do utilizador; AB-03 estava confirmado. OPEN-03 (importação de dados legacy) é
decisão de projeto tratada em `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md` com default seguro
(começar vazio + importação faseada opcional) e não bloqueia a construção.
