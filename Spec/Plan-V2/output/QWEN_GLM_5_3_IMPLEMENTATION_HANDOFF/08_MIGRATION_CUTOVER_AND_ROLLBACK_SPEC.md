# 08 — MIGRATION, CUTOVER AND ROLLBACK SPEC

Autoridade para transição de dados e coexistência. Base: BACKEND-DECISION-12 (legacy/cutover),
OPEN-03 (importação), BT-08 (nova família de migrations), `07_C_SHARP §25`.

## 1. Inventário de dados existentes (GLM-MIG-01)

| Origem | Conteúdo | Estado |
|---|---|---|
| Legacy JS BQ (runtime.js) | localStorage `ba.boquilhas.v2.database`, pasta partilhada `estado_atual.json`, backups JSON | Não está no workspace (evidência indireta via specs); localização real a confirmar com o owner antes de qualquer importação |
| Supabase atual (se existir) | Tabelas da família A (bq_*, peso_*, internal_users, access_templates, admin_audit_log) | `SOURCE VERIFICATION REQUIRED` — não assumir aplicado/live apenas por existir SQL em disco |
| Excel `ARTIGOS-MG_03-09-2025.xlsx` | Referências MODELO+MARISA, códigos MF/MP/BQ | Evidência para bootstrap de referências (não migração automática) |
| `Exemplo5.xlsx` | Estrutura Job On | Evidência apenas |

## 2. Estratégia base (GLM-MIG-02)

1. A BD da aplicação nova **começa vazia** (sem seeds operacionais). Provenance desta decisão:
   BACKEND-DECISION-19 (APPROVED/CONFIRMED em `Spec/08_SUPABASE_BACKEND_SPEC.md` §25 — “Novo
   Supabase começa estruturalmente preparado; não inventa dados operacionais”; seeds fictícios
   proibidos) e BACKEND-DECISION-12 §34 (“Novo Supabase começa vazio; Importação PENDENTE”).
   Não é um default arbitrário desta passagem.
2. As duas famílias de migrations antigas são provenance; a app nova usa a família fresh-build (06_DATA §2).
3. Importação é **faseada e opcional por módulo**, sempre com aprovação explícita do owner:
   - Fase I1: utilizadores + templates (recriação deliberada na Administração, não cópia cega);
   - Fase I2: referências/lotes BQ e referências CM/MF (registos mestres);
   - Fase I3: histórico selecionado (traces/controlos) como factos imutáveis importados com marca `imported_from`.
4. Cada fase tem mapeamento source→target documentado, dry-run e reconciliação por contagens + amostras.
5. Nada é importado por inferência: registos ambíguos ficam em relatório para decisão humana.

## 3. Mapeamento source → target (GLM-MIG-03)

| Source (família A / legacy) | Target fresh-build | Regra |
|---|---|---|
| `internal_users` / `access_templates` | idem | Recriação com validação de catálogo novo; `profile_title` novo campo |
| `bq_lotes/bq_traces/bq_movements` | idem (schema novo) | Movimentos mantêm autoria/data; `allow_unmatched` descartado; retornos excecionais convertem-se em movimento + discrepancy importada |
| `peso_*` | idem | Controlos aprovados imutáveis; snapshots preservados |
| `admin_audit_log` | `audit_events` (eventos `moduleId=admin`) | Append-only importado na tabela única de auditoria global (TD-19); não recriar tabela separada |
| localStorage/JSON BQ | bq_* | Apenas se o owner fornecer o ficheiro; mesmas regras |
| ARTIGOS xlsx | `references`/`tool_references` | Candidatos sujeitos a confirmação; sem criação automática de regras |

## 4. Validação, dry-run e reconciliação (GLM-MIG-04)

1. Scripts de importação = código de migração dedicado (fora da app runtime), com dry-run obrigatório.
2. Reconciliação: contagens por tabela, somas de quantidades BQ por lote, médias de peso por controlo,
   amostras aleatórias comparadas antes/depois.
3. Qualquer divergência gera relatório e **pausa**; nunca correção automática silenciosa.
4. IDs novos (UUID) com tabela de correspondência `imported_id_map` preservada para auditoria.

## 5. Cutover e coexistência (GLM-MIG-05)

1. Coexistência permitida: sistema legacy continua operacional enquanto a app nova está em validação.
2. Sem dual-write. Cada módulo tem cutover próprio: Auth+Shell → **Job On** (contexto central de
   produção; landing) → Boquilhas → Peso → Pegamentos → Ferramentas/Repair/Armazém/Tampões
   (alinhado com os gates do roadmap; Peso e Pegamentos dependem do contexto Job On — DS-04/DS-05).
3. Por módulo: janela de leitura paralela (app nova lê os mesmos dados quando disponível) → escrita
   apenas na app nova → legacy fica read-only → legacy é desligado por decisão do owner.
4. Autoridade: no cutover do módulo, a app nova passa a fonte de verdade; o legacy não volta a escrever.

## 6. Backup e rollback (GLM-MIG-06)

1. Antes de qualquer fase de importação/cutover: backup completo da BD alvo (dump) + verificação de restauro.
2. Rollback por módulo: como os dados novos vivem em schema dedicado e o legacy permanece intacto
   até ao fecho do módulo, o rollback = reativar o legacy e congelar a escrita nova; registos criados
   na app nova são exportáveis para reconciliação manual.
3. Critérios de abortar: falha de reconciliação; perda de histórico detetada; autorização em falta;
   impossibilidade de restaurar backup validado.
4. Rollback de migrations: forward-only; o rollback faz-se por restauro de backup, nunca por scripts down.

## 7. Estado e perguntas diferidas (GLM-MIG-07)

- OPEN-03 (“como importar dados do sistema legacy?”) permanece formalmente aberto para o **conteúdo**
  concreto (que dados reais existem), com default seguro definido acima (começar vazio + fases opcionais).
  Não bloqueia a construção da aplicação.
- A localização real dos dados legacy (pasta partilhada/Supabase atual) é `SOURCE VERIFICATION REQUIRED`
  antes da fase I1.
