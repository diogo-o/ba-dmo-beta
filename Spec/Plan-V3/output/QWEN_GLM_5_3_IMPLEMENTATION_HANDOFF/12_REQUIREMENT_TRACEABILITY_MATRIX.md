# 12 — REQUIREMENT TRACEABILITY MATRIX

Todos os IDs estáveis do pacote, com origem, spec canónica, unidade de implementação e
teste/acceptance. Legenda de estado: `SPEC` (especificado, pronto para implementação),
`DEFERRED`, `SOURCE VERIFICATION REQUIRED`.

IDs de decisão (UD-xx decisões do utilizador, TD-xx decisões técnicas, BT-xx baseline/técnicas do
merged, NC-xx novas contradições) estão registados em `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md`
e mapeados na secção 3 desta matriz.

## 1. Requisitos transversais e de foundation

| ID | Origem/autoridade | Spec canónica | Módulo/área | Unidade | Teste/acceptance | Estado |
|---|---|---|---|---|---|---|
| GLM-CORE-01 | handoff §1.6 (USER) | 00_START §2; 02_DEC §1 | Todos | todas | GLM-TST-02/03 | SPEC |
| GLM-CORE-02 | handoff §1.6 (USER) | 00_START §2; 02_DEC §5 | Todos | todas | GLM-TST-02 | SPEC |
| GLM-CORE-03 | handoff §autoridade | 00_START §4 | Todos | — | auditoria | SPEC |
| GLM-CORE-04 | handoff §roadmap | 00_START §5; 10_ROADMAP | Todos | todas | gates A–J | SPEC |
| GLM-CORE-05 | handoff §restrições | 00_START §6 | Todos | todas | gates | SPEC |
| GLM-CORE-06 | sincronização §4 (trace) | 00_START §2 | Todos | todas | rastreabilidade por ID | SPEC |
| GLM-CORE-07 | sincronização §4 (contratos) | 00_START §2; 02_DEC §2 | Todos | todas | gates | SPEC |
| GLM-ARCH-01..11 | 07_C_SHARP + UD-01 | 03_ARCH §1–11 | Arquitetura | U-01/U-04 | build + verificação de dependências | SPEC |
| GLM-ARCH-12..18 | Plan-V3 TECHNICAL BUILD CONTRACT (owner) | 03_ARCH §12–18 | Arquitetura/build | U-01/U-02/U-22 | net10.0, Docker/Render, CLI, PDF boundary | SPEC |
| GLM-DSN-01..10 | design contract P0/P1 + DMO_DESIGN_SYSTEM v2.7 (design atual) | 07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE §1–10 | Design foundation | U-08/U-09 | laboratório + checklist + visual regression | SPEC |
| GLM-TST-01..08 | handoff §3.9 | 09_TEST §1–8 | Qualidade | U-20 | DoD global | SPEC |
| GLM-TST-10 | Plan-V3 technical contract | 09_TEST §10 | Qualidade/build | U-01/U-02/U-10/U-11 | migration checksum/failure, CLI routing, no debug bypass, PDF abstraction, directory-handle boundary | SPEC |
| GLM-MIG-01..07 | BACKEND-DECISION-12; OPEN-03 | 08_MIG §1–7 | Migração | U-21 | dry-run/reconciliação/rollback | SPEC |
| GLM-MIG-08 | Plan-V3 | 08_MIG §8; 06_DATA §13 | Migração técnica | U-02/U-22 | executor Npgsql + schema_migrations + CLI/pre-deploy | SPEC |

## 2. Requisitos por área

### Identidade/Acesso/Admin
| ID | Origem | Spec | Unidade | Teste | Estado |
|---|---|---|---|---|---|
| GLM-ACC-01 | 03_AUTH + TD-09 | 04_ACC §1 | U-05 | resolução ativa/inativa | SPEC |
| GLM-ACC-02 | 04_ADMIN + UD-02 | 04_ACC §2 | U-05/U-06 | normalização | SPEC |
| GLM-ACC-03 | UD-02 + ADMIN-DECISION-02 | 04_ACC §3; modules/00 | U-04 | catálogo controlado | SPEC |
| GLM-ACC-04 | UD-03 | 04_ACC §4 | U-05..U-07 | 18 cenários | SPEC |
| GLM-ACC-05 | UD-06/UD-15 (USER) | 04_ACC §5 | U-07/U-10 | cenários 2/3 | SPEC |
| GLM-ACC-06 | UD-02/05 + UD-13..17 | 04_ACC §6 | U-06/U-07 | matriz completa | SPEC |
| GLM-ACC-07 | handoff §3.3 + design sync | 04_ACC §7 | U-07/U-20 | 18 cenários pos/neg | SPEC |
| GLM-ACC-08 | handoff §1.5.9 | 04_ACC §8 | U-07 | cenário 11 | SPEC |
| GLM-ACC-09 | 04_ADMIN §4–6 + TD-16 + AUDITORIA_GLOBAL | 04_ACC §9 | U-06/U-23 | CRUD + audit + tab Auditoria | SPEC |
| GLM-ACC-10 | UD-10 | 04_ACC §10 | U-06 | cenário 13 | SPEC |
| GLM-ACC-11 | AUDITORIA_GLOBAL_HANDOFF + TD-19 | 04_ACC §11; modules/11 | U-23 | eventos na transação; filtros anuais | SPEC |
| GLM-ACC-12 | 04_ADMIN §13 + BT-06 | 04_ACC §12 | U-06 | conflito concorrente | SPEC |
| GLM-ACC-13 | BACKEND-DECISION-19 | 04_ACC §13 | U-05 | bootstrap idempotente | SPEC |

### Shell
| ID | Origem | Spec | Unidade | Teste | Estado |
|---|---|---|---|---|---|
| GLM-SHL-01 | UD-03/04 | 05_SHL §1 | U-07 | shell única | SPEC |
| GLM-SHL-02 | design §8 + 06_UI_UX §6 | 05_SHL §2 | U-09 | visual | SPEC |
| GLM-SHL-03 | UD-05/UD-14 + GLM-CAT-02 | 05_SHL §3 | U-07 | cenários 1–9/16 | SPEC |
| GLM-SHL-04 | UD-16 (design atual; substitui landing de UD-04) | 05_SHL §4 | U-07/U-13 | landing Job On universal | SPEC |
| GLM-SHL-05 | UD-03.6 | 05_SHL §5 | U-07 | cenário 10 | SPEC |
| GLM-SHL-06 | gaps auth legacy | 05_SHL §6 | U-07 | estados seguros | SPEC |
| GLM-SHL-07 | PORTAL_LOGIN brief | 05_SHL §7 | U-09 | logout/profile | SPEC |
| GLM-SHL-08 | design §8.3 | 05_SHL §8 | U-09 | responsive | SPEC |
| GLM-SHL-09 | 07_C_SHARP §8 | 05_SHL §9 | U-07 | arquitetura | SPEC |

### Dados/Backend/Segurança
| ID | Origem | Spec | Unidade | Teste | Estado |
|---|---|---|---|---|---|
| GLM-DATA-01..02 | BACKEND-DECISION-01..19 | 06_DATA §1–2 | U-02/U-03 | revisão/scripts | SPEC |
| GLM-DATA-03 | specs + BT-02 | 06_DATA §3 | U-02 | schema tests | SPEC |
| GLM-DATA-04 | GLM-CORE-01 | 06_DATA §4 | todas | invariantes | SPEC |
| GLM-DATA-05 | 08_SUPABASE §7 | 06_DATA §5 | U-03 | atomicidade | SPEC |
| GLM-DATA-06 | BACKEND-DECISION-03/05 | 06_DATA §6 | U-02/U-20 | RLS tests | SPEC |
| GLM-DATA-07 | BT-03 | 06_DATA §7 | todas | histórico imutável | SPEC |
| GLM-DATA-08 | BT-06 | 06_DATA §8 | U-03 | conflito | SPEC |
| GLM-DATA-09 | BACKEND-DECISION-13 + DS-08/DS-10 (design atual) | 06_DATA §9 | U-10/U-11 | servidor/local boundary; PDF no caminho resolvido | SPEC |
| GLM-DATA-10 | merged §5.16 | 06_DATA §10 | U-10 | email preparado | SPEC |
| GLM-DATA-11 | BACKEND-DECISION-19 | 06_DATA §11 | U-02 | sem seeds | SPEC |
| GLM-DATA-12 | Plan-V3 (owner) | 06_DATA §12 | U-02 | migration runner Npgsql full-script + checksum + regista após sucesso | SPEC |
| GLM-DATA-13 | Plan-V3 (owner) | 06_DATA §13 | U-02/U-22 | CLI only; pre-deploy Render; sem HTTP; sem startup | SPEC |
| GLM-DATA-14 | Plan-V3 (owner) | 06_DATA §14; 03_ARCH §14 | U-03/U-05 | Supabase adapter boundary; service role least privilege | SPEC |
| GLM-DATA-15 | Plan-V3 (owner) | 06_DATA §15 | U-05 | one-shot bootstrap-admin CLI | SPEC |
| GLM-DATA-16 | Plan-V3 (owner) | 06_DATA §16; 03_ARCH §16–17 | U-10/U-11 | PDF em memória/backend; Render filesystem não é storage; File System Access export-only; IndexedDB handle exception | SPEC |

### Catálogo
| ID | Origem | Spec | Unidade | Teste | Estado |
|---|---|---|---|---|---|
| GLM-CAT-01..05 | UD-02/05/13..16 + C19/C25 | modules/00 §1–5 | U-04 | normalização + extensão | SPEC |

### Módulos
| ID range | Módulo | Spec | Unidade | Aceitação |
|---|---|---|---|---|
| GLM-BQ-01..15 | Boquilhas | modules/01 | U-19 | §13 (incl. 20→25) |
| GLM-CTR-01..06 | Controlo | modules/02 | U-07/U-10/U-11 | §5 |
| GLM-PESO-01..15 | Peso | modules/03 | U-10 | §12 |
| GLM-PEG-01..14 | Pegamentos | modules/04 | U-11 | §11 |
| GLM-JOB-01..15 | Job On | modules/05 | U-13 | §13 |
| GLM-FERR-01..14 | Ferramentas CM/MF | modules/06 | U-12 | §11 |
| GLM-ARM-01..12 | Armazém | modules/07 | U-14 | §10 |
| GLM-RI-01..12 | Reparação interna | modules/08 | U-16 | §10 |
| GLM-RE-01..13 | Reparação externa | modules/09 | U-15 | §10 |
| GLM-TP-01..13 | Tampões | modules/10 | U-17 | §11 |
| GLM-HIST-01..10 | História/Auditoria (incl. auditoria global) | modules/11 | U-18 + U-23 | §7/§8 |

## 3. IDs de decisão → requisito(s) dependentes

| Decisão | Onde está registada | Requisitos dependentes |
|---|---|---|
| UD-01..UD-12 | 02_DEC §2 | GLM-ARCH-01/02/08; GLM-ACC-02/05/06; GLM-SHL-01/03; GLM-BQ-06; GLM-PESO-05; GLM-TP-01 |
| UD-13 (reparacao_externa única) | 02_DEC §2 | GLM-CAT-02; GLM-RE-*; 04_ACC §6 |
| UD-14 (Controlo área/domínio) | 02_DEC §2 | GLM-CTR-*; GLM-SHL-03; GLM-CAT-01 |
| UD-15 (Peso: experiências exclusivas) | 02_DEC §2 | GLM-ACC-05; GLM-PESO-* |
| UD-16 (landing Job On) | 02_DEC §2 | GLM-SHL-04; GLM-JOB-*; 05_SHL §4 |
| UD-17 (auditoria global) | 02_DEC §2 | GLM-ACC-11; GLM-HIST-*; U-23 |
| BT-01..BT-08 | 02_DEC §3.1–3.8 | GLM-DATA-01/03/07/08; GLM-MIG-02 |
| TD-09..TD-16 | 02_DEC §3.9–3.16 | GLM-ACC-01/03; GLM-BQ-04/05; GLM-PESO-04/05; GLM-DATA-03 |
| TD-17 (processo no lote do Peso) | 02_DEC §3.17 | GLM-PESO-*; GLM-FERR-*; 06_DATA §3.3/3.5 |
| TD-18 (JOB_ON_DATA_MODEL) | 02_DEC §3.18 | GLM-JOB-*; 06_DATA §3.6 |
| TD-19 (audit_events única) | 02_DEC §3.19 | GLM-ACC-11; GLM-HIST-*; 06_DATA §3.1 |
| TD-20 (capabilities Job On) | 02_DEC §3.20 | GLM-CAT-03; 04_ACC §6; GLM-JOB-* |
| TD-21 (nomes de PDF) | 02_DEC §3.21 | GLM-DATA-09; GLM-PESO/GLM-PEG |
| TD-22 (repair_exit_items BQ/CM/MF) | 02_DEC §3.22 | GLM-RE-*; 06_DATA §3.7 |
| TD-23 (imagem por revisão Job On) | 02_DEC §3.23 | GLM-JOB-*; 06_DATA §9 |
| TD-24 (História com grants de origem) | 02_DEC §3.24 | GLM-HIST-*; cenário 18 |
| TD-25 (tabela densidades 5–35 °C — GAP-002) | 02_DEC §3.25 | GLM-PESO-05; 09_TEST §9.1–5; GLM prompt |
| TD-26 (identidade lote CM — GAP-001) | 02_DEC §3.26 | GLM-PESO-06; GLM-FERR-03; 06_DATA §3.3/3.5; 09_TEST §9.16–17 |
| TD-27 (Job On ativo + lifecycle — GAP-003) | 02_DEC §3.27 | GLM-JOB-05; 03_ARCH lookup; GLM-RI/GLM-BQ/GLM-PESO consumos; 09_TEST §9.11–15 |
| TD-28 (volumes punção/marisa — DG-02) | 02_DEC §3.28 | GLM-PESO-05; 09_TEST §9.6–8 |
| TD-29 (base da comparação — DG-03) | 02_DEC §3.29 | GLM-PESO-06.4 |
| TD-30 (comparação anterior cross-line — DG-04) | 02_DEC §3.30 | GLM-PESO-06.5; 09_TEST §9.9–10 |
| TD-31 (filenames PDF reais) | 02_DEC §3.31 | GLM-PESO-09; GLM-PEG-05/14 |
| TD-32 (regras Pegamentos — design baseline) | 02_DEC §3.32 | GLM-PEG-05; 09_TEST §9.18–20 |
| TD-33 (ferramentas.configure) | 02_DEC §3.33 | GLM-CAT-03; GLM-FERR-03; 09_TEST §9.21 |
| PV-01 (target net10.0 + deploy Render/Docker/Linux) | 02_DEC §3.35 (Plan-V3) | GLM-ARCH-12/13; GLM-DATA-13; GLM-MIG-08; U-01/U-22 |
| PV-02 (migrations Npgsql full-script + schema_migrations + CLI) | 02_DEC §3.35 (Plan-V3) | GLM-DATA-12/13; GLM-MIG-08; U-02 |
| PV-03 (Supabase adapter boundary; service role least privilege) | 02_DEC §3.35 (Plan-V3) | GLM-DATA-14; GLM-ARCH-14; U-03/U-05 |
| PV-04 (one-shot bootstrap-admin CLI) | 02_DEC §3.35 (Plan-V3) | GLM-DATA-15; GLM-ACC-13; U-05 |
| PV-05 (PDF em memória/backend; IPdfRenderer) | 02_DEC §3.35 (Plan-V3) | GLM-DATA-16; GLM-ARCH-16; U-10/U-11 |
| PV-06 (File System Access export-only + IndexedDB directory-handle exception) | 02_DEC §3.35 (Plan-V3) | GLM-DATA-16; GLM-ARCH-17; U-10/U-11 |
| PV-07 (no production debug bypass) | 02_DEC §3.35 (Plan-V3) | GLM-ARCH-18; GLM-TST-10; U-01 |
| NC-01 | 02_DEC §7 | RESOLVED — sem shortcuts funcionais; 1 clique/2 cliques (GLM-DSN-01 linha 5; 07_DESIGN) |
| NC-02..NC-05 | 02_DEC §7 | GLM-DSN-01; GLM-CAT-04; GLM-BQ-03; GLM-PESO-04 |
| DS-01..DS-14 (design sync) | 02_DEC §7.1 | todos os requisitos tocados pela sincronização |
| AB-01..AB-03 | 02_DEC §4 | GLM-FERR-02; GLM-JOB-04; GLM-RE-01; GLM-RI-01 |
| C1–C32 | 02_DEC §6 | matriz de resoluções (cada linha aponta a spec do pacote) |

## 4. Requisitos com estado especial

| ID | Estado | Motivo |
|---|---|---|
| GLM-MIG-07 | DEFERRED | conteúdo concreto da importação depende dos dados reais (OPEN-03); default seguro com provenance (BACKEND-DECISION-12/19) |
| GLM-DATA-17 itens (06_DATA §17) | SOURCE VERIFICATION REQUIRED | verificação live Supabase |
| GLM-JOB-15 / GLM-PEG-14 / GLM-FERR-14 / GLM-RI-12 / GLM-RE-13 / GLM-ARM-12 / GLM-TP-13 | DEFERRED dentro das unidades | itens remanescentes dos briefs (ex.: campos obrigatórios por família no Job On); não bloqueiam foundation; decidir durante a unidade com evidência/owner |

Resolvidos pela sincronização (antes abertos): ownership da imagem do artigo (TD-23), convenção de
nomes de PDF (TD-21), recovery de permissão do diretório local (06_DATA §9), landing page (UD-16),
capacidades de verificação do Job On (TD-20).

## 5. Verificação de cobertura

- Todos os requisitos normativos do pacote têm ID estável e aparecem nesta matriz.
- Todos os módulos têm secções de testes e acceptance (ver coluna “Aceitação”).
- O caso Boquilhas 20→25 está rastreado: UD-08 → GLM-BQ-06 → U-19 → GLM-TST-02.1.
