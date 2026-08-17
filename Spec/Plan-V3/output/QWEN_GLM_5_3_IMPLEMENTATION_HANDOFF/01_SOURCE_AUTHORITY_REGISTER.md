# 01 — SOURCE AUTHORITY REGISTER

Registo de todas as fontes investigadas nesta sessão de planeamento.
Cobertura de leitura: `FULL` (integral), `SAMPLED` (estrutura + secções relevantes), `PROVENANCE ONLY` (existência/hash, conteúdo coberto por outra fonte), `NOT ON DISK`.
Autoridade: classificação do papel da fonte para a aplicação nova.

**Método:** tamanho via filesystem; SHA-256 calculado nesta sessão (Get-FileHash). Nenhum ficheiro fonte foi modificado.

## 1. Fontes de planeamento (planos)

| Path | Bytes | SHA-256 (primeiros 16) | Cobertura | Autoridade |
|---|---|---|---|---|
| `plans/QWEN_MASTER_DEFINITIVE_FRESH_BUILD_HANDOFF.md` | 42.285 | `634FCAB9C6C562A7` | FULL (853 linhas; integridade confirmada) | **MANDATO** — instruções desta sessão |
| `plans/BA_DMO_VERIFIED_KNOWLEDGE_MERGED.md` | 65.574 | `E9128EDE34C4EE3B` | FULL | **VERIFIED KNOWLEDGE** — C1–C32, AB-01–03, MUST PRESERVE |
| `plans/DEEPSEEK_QWEN_HANDOFF_AUDIT.md` | 16.025 | `1BAA983D596EF6A4` | PROVENANCE ONLY | Auditoria (coberta pelo merged + handoff) |
| `plans/DEEPSEEK_QWEN_HANDOFF_AUDIT_V2.md` | 10.708 | `7FD9C33D047433BB` | PROVENANCE ONLY | idem |
| `plans/DEEPSEEK_QWEN_HANDOFF_AUDIT_V3.md` | 22.441 | `B37E5F3622F76940` | PROVENANCE ONLY | idem |
| `plans/DEEPSEEK_QWEN_HANDOFF_AUDIT_V4.md` | 18.484 | `2835A043B90706FF` | SAMPLED | idem (findings retomados pelo handoff §4) |
| `.kilo/plans/1786906796561-verified-context-extraction-v2.md` | — | — | NOT VERIFIED | Fonte original do merged; o merged é a versão consolidada e prevalece |

## 2. Design package ATUAL — `portal-dmo-design-final/` (raiz do workspace)

Autoridade: **DESIGN consolidado atual para funcionamento, interface e experiência** (precedência 2
na sincronização de 2026-08-16), nunca para regras de domínio que contrariem decisões explícitas.
Todos os documentos textuais foram lidos **integralmente** na passagem de sincronização; os HTML
foram inspecionados para anatomia/estados. Hashes SHA-256 calculados nesta passagem.

| Path | Bytes | SHA-256 (16) | Estado vs registo anterior | Cobertura |
|---|---|---|---|---|
| `README.md` | 5.089 | `D8E6D67DAB2A8BE5` | CHANGED | FULL |
| `docs/HANDOFF_INDEX.md` | 6.620 | `EA2D643540F25ECE` | CHANGED (auditoria anual; Job On landing; JOB_ON_DATA_MODEL) | FULL |
| `docs/DESIGN_IMPLEMENTATION_CONTRACT.md` | 56.764 | `1B9572308487BEE0` | CHANGED (Job On/auditoria/PDF local/new components) | FULL |
| `docs/CODER_IMPLEMENTATION_HANDOFF.md` | 37.833 | `7634B3CF9E75BFEF` | CHANGED (capabilities jobon.*/audit.*; fluxo ponta a ponta) | FULL |
| `docs/DMO_DESIGN_SYSTEM.md` (v2.7) | 30.865 | `E0AD77F825076E80` | CHANGED (tokens novos §4; secções 29–36) | FULL |
| `docs/AUDITORIA_GLOBAL_HANDOFF.md` | 6.463 | `EE8CE5D311F7F0C8` | **NEW** | FULL |
| `docs/JOB_ON_DATA_MODEL.md` | 14.904 | `76B5F48A32880920` | **NEW** (contrato de persistência do snapshot) | FULL |
| `docs/JOB_ON_DESIGN_BRIEF.md` | 34.337 | `196937C5670F7A88` | CHANGED (Modo consulta/edição; contrato Peso/Pegamentos; ownership) | FULL |
| `docs/JOB_ON_VERIFICACOES_DESIGN_BRIEF.md` | 9.649 | `29D610BA6A4C6EDC` | CHANGED (regra/ocorrência; reset; permissões Chefe) | FULL |
| `docs/PESO_INTERFACE_HANDOFF.md` | 17.442 | `54C628CED72BC4ED` | CHANGED (Processo no lote; contexto Job On; fronteira servidor/local) | FULL |
| `docs/PEGAMENTOS_INTERFACE_HANDOFF.md` | 5.278 | `2333973962A900A9` | CHANGED (Job On obrigatório; PDF local) | FULL |
| `docs/PORTAL_LOGIN_ADMIN_HANDOFF.md` | 3.979 | `58F4484EB2A5D216` | CHANGED (landing Job On; tab Auditoria) | FULL |
| `docs/FERRAMENTAS_REGISTO_DESIGN_BRIEF.md` | 9.798 | `18D067F2E0B84B14` | CHANGED (Processo movido para o lote) | FULL |
| `docs/ARMAZEM_DESIGN_BRIEF.md` | 17.459 | `D7E6A27166FFEC38` | CHANGED (relação Job On §10; composição live na edição) | FULL |
| `docs/DESIGN_INPUT_EXTRACTION.md` | 5.968 | `E51400025FC03033` | CHANGED (auditoria global; padrões Job On) | FULL |
| `docs/MODULE_UI_HANDOFF_TEMPLATE.md` | 4.219 | `D364A3A010130B4C` | CHANGED (ações auditáveis; jobOnId) | FULL |
| `docs/BOQUILHAS_INTERFACE_BEHAVIOR.md` | 10.029 | `51706EBD503A86D2` | UNCHANGED | FULL |
| `docs/REPARACAO_EXTERNA_DESIGN_BRIEF.md` | 6.258 | `C2765CBBF99AD371` | UNCHANGED | FULL |
| `docs/REPARACAO_INTERNA_DESIGN_BRIEF.md` | 8.087 | `96BA2633315D49E7` | UNCHANGED | FULL |
| `docs/TAMPOES_DESIGN_BRIEF.md` | 13.117 | `958416E12566167D` | UNCHANGED | FULL |
| `dmo-design-system.css` | 8.189 | `3A601F167F5EC401` | UNCHANGED | FULL |
| `dmo-interactions.js` | 1.890 | `98DFECCB89CD6783` | UNCHANGED | FULL |
| `logo_recolored(1).png` | 38.205 | `9E3536D623F5D2D9` | UNCHANGED | inspecionado |
| `admin.html` | 15.494 | `60EF18A63EA42BB7` | CHANGED (tabs Utilizadores/Aplicações/Auditoria; "Voltar ao Job On") | FULL (anatomia) |
| `job-on.html` | 374 | `880151D791886AAE` | redirect para a folha | FULL |
| `job-on-v48-folha-producao.html` | 44.334 | `39E81288DEDD5DD5` | CHANGED (tabs Planeamento/Job On/Histórico/Definições; flags de permissão) | FULL (anatomia) |
| `peso-operador.html` | 32.968 | `6587A19E26BA3483` | CHANGED (contexto Job On; lote com Processo/Subpasta) | FULL (anatomia) |
| `peso-responsavel.html` | 17.314 | `A4DE13377C5A306A` | UNCHANGED | FULL (anatomia) |
| `pegamentos.html` | 112.759 | `5C7D456BD5E4B1E8` | CHANGED (contexto Job On; resíduos legacy identificados no contrato §23) | FULL (anatomia) |
| `boquilhas.html` | 48.311 | `5515E14973E32E56` | CHANGED (resíduos `confirm()` identificados) | FULL (anatomia) |
| `armazem.html` | 30.533 | `0017F8505FC8BEC5` | CHANGED | FULL (anatomia) |
| `login.html` | 4.359 | `AE61BB4BB83C7F9D` | CHANGED | FULL |
| `reparacao-interna.html` | 29.815 | `B3A889B556202DFD` | UNCHANGED | FULL (anatomia) |
| `reparacao-externa-v1.html` | 4.096 | `E6AC3C7FF9C6C56B` | UNCHANGED | FULL (anatomia) |
| `reparacao-v2.html` | 7.097 | `10CBFFE882B4240D` | UNCHANGED | FULL (anatomia) |
| `tampoes.html` / `tampoes-v38-standalone.html` | 34.826 | `2982934B9DF1475C` | UNCHANGED (idênticos entre si) | FULL (anatomia) |
| `moldes.html` / `moldes-v42/v43/v44` | 25.176 | `DE9E31C826DECEC4` | UNCHANGED (idênticos entre si; versões = evidência histórica) | FULL (anatomia) |

### 2.1 Pacote de design anterior (superseded)

`portal-dmo-design-handoff-final/portal-dmo-design-final/` — versão anterior do design, substituída
pelo pacote atual. Mantida apenas como provenance; **não é autoridade**. As diferenças relevantes
estão registadas em `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` (DESIGN-SYNC).

## 3. Specs atuais — `Spec/`

| Path | Bytes | SHA-256 (16) | Cobertura | Autoridade |
|---|---|---|---|---|
| `Spec/99_CURRENT_STATE.md` | 6.173 | `1E5477F2B5CA3D4D` | FULL | CURRENT (estado do build production-cloud; 392 testes) |
| `Spec/00_MASTER_IMPLEMENTATION_INDEX.md` | 101.026 | `ADDA6A53CF1FFAE6` | SAMPLED (estrutura, fases, steps iniciais) | ROADMAP LEGACY (production-cloud; substituído pelo roadmap deste pacote) |
| `Spec/01_BQ_COMPLETE_SPEC.md` | 43.905 | `8C42637B8775AE17` | FULL | CURRENT SPEC BQ (regras BQ-RULE-001..011) |
| `Spec/02_PESO_COMPLETE_SPEC.md` | 35.569 | `31C0F155D9AE7131` | FULL | CURRENT SPEC Peso |
| `Spec/03_AUTH_ACCESS_SPEC.md` | 15.765 | `E58E4EA8EB934468` | FULL | CURRENT SPEC Auth (JS legacy) + gaps |
| `Spec/04_ADMIN_COMPLETE_SPEC.md` | 21.758 | `F0903D426046CC95` | FULL | CURRENT SPEC Admin |
| `Spec/05_SHELL_COMPLETE_SPEC.md` | 290.225 | `E4EEB852B71E57E9` | SAMPLED (título/cabeçalho) | ⚠️ **MISLABELED** — é consolidação legacy runtime.js BQ (C31); não é spec da Shell |
| `Spec/06_UI_UX_COMPLETE_SPEC.md` | 19.544 | `A8698032828C6344` | FULL | DESIGN (paleta V1 `#3C73A8`, componentes) |
| `Spec/07_C_SHARP_SOLUTION_ARCHITECTURE.md` | 37.516 | `E40AC2420EF05D95` | FULL | CURRENT ARCHITECTURE (6 projetos, Dapper/Npgsql, Razor) |
| `Spec/08_SUPABASE_BACKEND_SPEC.md` | 55.409 | `A4BA51B8414DBAF1` | FULL (incl. tail legacy/cutover §34) | CURRENT SPEC backend |
| `Spec/01-domain.md` | 27.601 | `E53F5FC640668C15` | PROVENANCE ONLY (resumo no merged §5.2) | LEGACY BQ domain |
| `Spec/02-data.md` | 27.312 | `CCEB90D37E2CE32D` | PROVENANCE ONLY | LEGACY BQ data |
| `Spec/03-workflows.md` | 51.616 | `35E97D9ED90B4857` | PROVENANCE ONLY | LEGACY 20 workflows BQ |
| `Spec/04-lifecycle.md` | 19.393 | `B20C3513FB8B7C75` | PROVENANCE ONLY | LEGACY lifecycle BQ |
| `Spec/05-repairs.md` | 24.534 | `5FE00D162AF1AE3A` | PROVENANCE ONLY (modelo no merged §7/§8) | LEGACY repair/saldo BQ |
| `Spec/06-persistence.md` | 26.559 | `218F913DC080C963` | PROVENANCE ONLY | LEGACY storage (removido, C28) |
| `Spec/07.md` / `08.md` / `09.md` | 39.671 / 38.166 / 33.669 | `96995E2B…` / `8FED580B…` / `32857393…` | PROVENANCE ONLY | LEGACY UI/eventos/regras escondidas BQ |
| `Spec/autonomous-implementation.md` | 6.240 | `A40D407D1BA7E58D` | FULL | Processo legacy do Codex (não vinculativo para a implementação) |
| `Spec/database/migrations/001_identity.sql` | 1.409 | `0046AA82BA40D247` | FULL | MIGRATION FAMILY A |
| `Spec/database/migrations/002_bq_core.sql` | 6.605 | `67897AA96D8DB3B9` | FULL | FAMILY A |
| `Spec/database/migrations/003_peso_core.sql` | 6.852 | `17BD872F93CEC804` | FULL | FAMILY A |
| `Spec/database/migrations/004_admin_audit.sql` | 792 | `7EE5D9A1C8BC63F3` | FULL | FAMILY A |
| `Spec/database/migrations/005_indexes.sql` | 2.491 | `80DC40BA2D99084B` | FULL | FAMILY A |
| `Spec/database/migrations/006_rls.sql` | 5.048 | `33CB32E5A020C6A0` | FULL | FAMILY A |
| `Spec/src/**` (Domain/Application/Infrastructure/Web/tests) | — | — | SAMPLED (inventário + ficheiros-chave: Program.cs, Services, Ports, Web pages) | CURRENT IMPLEMENTATION EVIDENCE |

Nota sobre `Spec/src`: o código é evidência do ramo production-cloud (392 testes unitários
reportados em `99_CURRENT_STATE.md`). Os serviços `BoquilhasService`, `PesoService`, `AdminService`,
`WeightCalculator`/`ControlValidator` e repositórios Dapper confirmam os contratos das specs;
o implementation agent não deve reutilizá-los automaticamente — ver `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md`.

## 4. Baseline v2 — `ba-dmo-v2/`

| Path | Bytes | SHA-256 (16) | Cobertura | Autoridade |
|---|---|---|---|---|
| `ba-dmo-v2/MODELO_LOCKED.md` | 13.924 | `7205BD397512130C` | FULL | SOURCE SPEC v2 (modelo locked v3) |
| `ba-dmo-v2/MAP.md` | 4.401 | `807E30DE37CD3AAE` | FULL | SOURCE SPEC v2 (vocabulário canónico) |
| `ba-dmo-v2/README.md` | 1.065 | `0153669551DAE354` | FULL | SOURCE SPEC v2 (contraditório — C18) |
| `ba-dmo-v2/migrations/000_tools.sql` | 6.121 | `1F6BA81D0C31F8EA` | SAMPLED | PROPOSAL v2 |
| `ba-dmo-v2/migrations/001_references.sql` | 4.445 | `BBE05603B3B92544` | SAMPLED | PROPOSAL v2 |
| `ba-dmo-v2/migrations/002_productions_jobon.sql` | 7.614 | `9CEB2609AAA96B84` | SAMPLED | PROPOSAL v2 |
| `ba-dmo-v2/migrations/003_controlo.sql` | 3.605 | `FECC1D354CEBABAE` | FULL | PROPOSAL v2 (controlo peso/pegamento) |
| `ba-dmo-v2/migrations/004_repair.sql` | 4.306 | `397A841F43D8F964` | SAMPLED | PROPOSAL v2 |
| `ba-dmo-v2/migrations/005_warehouse.sql` | 2.810 | `9C14ACFA8FB9935B` | SAMPLED | PROPOSAL v2 (VIEW bug C21) |
| `ba-dmo-v2/migrations/006_tampoes.sql` | 3.576 | `25D65734A15B1717` | SAMPLED | PROPOSAL v2 |
| `ba-dmo-v2/migrations/007_settings.sql` | 620 | `67235E7C1B795F1C` | SAMPLED | PROPOSAL v2 |
| `ba-dmo-v2/migrations/008_identity.sql` | 7.157 | `15FDC10B9B5C29DE` | FULL | PROPOSAL v2 (catálogo firebase — C20; rejeitado) |
| `ba-dmo-v2/src/**` | — | — | SAMPLED (12 ficheiros .cs: Tool, PhysicalPiece, JobOn, Production, Reference…) | PROTOTYPE EVIDENCE (domínio parcial) |

## 5. Outras fontes

| Fonte | Estado | Uso |
|---|---|---|
| `ARTIGOS-MG_03-09-2025.xlsx` | Em disco; não aberto nesta sessão | Evidência operacional (Ref = MODELO+MARISA) já reconciliada no merged; não canónico |
| `Exemplo5.xlsx` | Em disco; não aberto | Evidência JobOn já reconciliada no merged |
| Qwen context files (`BA_PROJECT_COMPLETE_FLATTENED.md`, `BA_DMO_CONTEXT_FINAL.md`, `MASTER_PROMPT_QWEN.md`, `PORTAL_DMO_HANDOFF_FLAT.md`) | **NOT ON DISK** | **Não bloqueadores**: o conhecimento material está coberto pelo merged (regra §14 C do merged) |
| Git | Read-only; não usado como source of truth | Provenance apenas |
| `OP99PMD02.pdf` | Não presente no workspace | Evidência documental (anatomia de desenhos) já sumarizada no merged/FERRAMENTAS brief |

## 6. Contradições e lacunas extraídas por fonte

- `Spec/05_SHELL_COMPLETE_SPEC.md` — não é uma spec da Shell (C31); a Shell é reconstruída em `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md`.
- `Spec/03_AUTH_ACCESS_SPEC.md` — gaps: BQ module guard ausente, admin aberto, RLS indeterminado, redirect fixo para BQ; todos tratados nas specs 04/05/06 deste pacote.
- `ba-dmo-v2` — identidade autocontraditória (C18), firebase_uid (C20), catálogo sem admin (C25), tool_status VIEW bug (C21); tratado em `02_…` e DO NOT CARRY FORWARD.
- `00_MASTER_IMPLEMENTATION_INDEX.md` — roadmap de 65 steps para o ramo production-cloud de 3 módulos; não cobre a aplicação nova completa; substituído por `10_MASTER_IMPLEMENTATION_ROADMAP.md`.
- Design contract — P0/P1 tornados trabalho explícito de foundation em `07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md`;
  os gaps P2.9/P2.10 (nomes de PDF e permissões File System Access) são tratados em `modules/03_PESO_SPEC.md`.
- Design package mudou de localização/versão (`portal-dmo-design-final/` na raiz); delta completo em
  `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` §7.1 (DESIGN-SYNC).

## 7. Fontes ausentes e impacto

| Fonte ausente | Impacto |
|---|---|
| Qwen context files (4) | Nenhum — conhecimento coberto pelo merged (non-blocking rule) |
| `.kilo/plans/1786906796561-verified-context-extraction-v2.md` (não relido nesta sessão) | Nenhum — o merged é a consolidação oficial e foi lido integralmente |
| OP99PMD02.pdf | Nenhum para V1 — regras de desenho ficam como dados explícitos, sem parsing |
| Implementação funcional legacy dos Pegamentos | Não existe no workspace (verificado); regras promovem-se do design baseline congelado com provenance (TD-32) |

## 8. Fontes legacy inspecionadas na passagem de legacy recovery (2026-08-17)

| Source path | Tipo | Read scope | Conhecimento relevante |
|---|---|---|---|
| `Spec/src/BA.Dmo.Domain/Modules/Peso/Services/WeightCalculator.cs` | C# domain | FULL | Tabela de densidades 5–35 °C (31 valores), regras de lookup/arredondamento, constantes NNPB/PS, fórmulas (TD-25/TD-28) |
| `Spec/tests/BA.Dmo.UnitTests/Modules/Peso/Services/WeightCalculatorTests.cs` | testes xUnit | FULL | Confirmação ponto a ponto da tabela + fronteiras 4.49/4.50/35.49/35.50 |
| `Spec/src/BA.Dmo.Web/wwwroot/js/peso.js` | JS preview | FULL | Tabela NÃO duplicada — injetada do servidor; confirma mapping punção/marisa |
| `Spec/src/BA.Dmo.Application/Modules/Peso/PesoService.cs` | C# application | relevante (resolução previous/comparação) | `GetPreviousApprovedControlAsync` cross-line (TD-30); clarification §4 |
| `Spec/src/BA.Dmo.Application/Ports/IPesoOutputDirectories.cs` (PesoFilenameBuilder) | C# | FULL | Filename real `{mold}{neckring}__{periodo}__{line}__L{lote}.pdf` (TD-31) |
| `Spec/src/BA.Dmo.Domain/Modules/Peso/Entities/Template.cs` | C# entity | FULL | volumePu=punção (subtraído), volumeNeck=marisa (adicionado); allowed_lines/tipo na referência legacy |
| `Spec/database/migrations/003_peso_core.sql` | SQL | FULL | peso_templates UNIQUE(mold,neckring); sem tabela peso_lotes no legacy |
| `Spec/src/BA.Dmo.Domain/Modules/Boquilhas/Services/MovementValidator.cs` | C# | FULL | `BQ.RETURN_UNMATCHED_NOT_ALLOWED` (rejeitado — classe D) |
| `Spec/src/BA.Dmo.Domain/Modules/Boquilhas/Services/InventoryCalculator.cs` | C# | FULL | matched/unmatched/exceptionalReceived conforme 20→25 (classe A) |
| `Spec/src/BA.Dmo.Web/Pages/Boquilhas/Index.cshtml(.cs)` | Razor | relevante | checkbox “Entrada excecional”/AllowUnmatched (rejeitado — classe D) |
| `ba-dmo-v2/src/BA.Dmo.Domain/JobOn/JobOn.cs` | C# entity | FULL | `data_saida` derivada por sequência da linha (classe B; suporta Resolve) |
| `ba-dmo-v2/migrations/002_productions_jobon.sql` | SQL | FULL | estados readiness v2 (não transitam), fechado/fechado_em, verificações |
| `ba-dmo-v2/migrations/004_repair.sql` | SQL | FULL | repair_exits/items BQ/CM/MF, estados, fecho atómico (TD-22) |
| `ba-dmo-v2/migrations/008_identity.sql` | SQL | FULL | catálogo v2 (provenance; substituído) |
| `portal-dmo-design-final/pegamentos.html` | mockup v1.9 | FULL (lógica) | Regras Pegamentos (TD-32); `pegamentosDB` rejeitado |
| `portal-dmo-design-final/` restante árvore | design baseline | integral (passagem de sincronização anterior; commit ref 3b23e30) | UI/UX; interação 1 clique/2 cliques |

Nota: o commit `3b23e30bfc4e33845d9cc708e3bcbd703dac0aa0` não é alcançável no git local deste
workspace (HEAD local `2c57711…`, repositório diferente); a pasta `portal-dmo-design-final/` em disco
é tratada como o conteúdo baseline congelado. Read-only verificado — nenhum ficheiro do design foi
modificado nesta passagem.
