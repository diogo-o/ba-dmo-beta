# 13 — HANDOFF MANIFEST

**Pacote:** `plans/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/`
**Versão:** 1.2 (legacy recovery & package correction — 2026-08-17; anterior: 1.1 design sync)
**Design baseline:** `portal-dmo-design-final/` (commit ref `3b23e30bfc4e33845d9cc708e3bcbd703dac0aa0`)
**Codificação:** UTF-8 em todos os ficheiros
**Estado:** entregue para revisão do owner (autoridade após aprovação).

## 1. Ficheiros entregues (26)

| # | Ficheiro | Finalidade | Linhas | Bytes | SHA-256 |
|---|---|---|---:|---:|---|
| 1 | `00_START_HERE.md` | Entrada, natureza do programa, contratos estruturais, precedência | 140 | 9.268 | `34A9CA32E4C36FAC1CE93003259E61E7860B82A5C2B649B32FA77DF0D1183892` |
| 2 | `01_SOURCE_AUTHORITY_REGISTER.md` | Fontes, hashes, cobertura, autoridade (+ legacy recovery §8) | 181 | 17.386 | `11DC6F7FDFD53C18474355AF0E7FEFCA91474DA1B8CBEF9B193D9576638976A8` |
| 3 | `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` | Decisões UD/TD (incl. TD-25..33 legacy recovery), C1–C32, AB, DS, rejeições | 504 | 44.002 | `7EF688992838E7E446BF445B949EEF4C5BF83B8B587D101FC4EC38276AD0FEA5` |
| 4 | `03_TARGET_MODULAR_ARCHITECTURE.md` | Arquitetura modular alvo, ownership, contratos lookup/auditoria | 162 | 9.815 | `0BE8FAD2126FF749173A40D4909DAB3C507C90E688E5C49F5F9963266E6DB56D` |
| 5 | `04_IDENTITY_ACCESS_AND_ADMIN_SPEC.md` | Identidade, acesso, matriz 18 cenários, Admin + Auditoria | 172 | 13.645 | `DDB5D209BE80206C691D12A10BD8CA385B642EC689560A4DD29D643ECF5E08F4` |
| 6 | `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md` | Shell única, tabs, landing Job On, deep links | 105 | 6.222 | `4A23C324E8D3EA460F211959F50F56A503861091FDA8FED87AA17FCE89BCF373` |
| 7 | `06_DATA_BACKEND_AND_SECURITY_SPEC.md` | Schema alvo (job_on*, audit_events), RLS, concorrência, servidor/local | 206 | 15.230 | `884C1B23FC1EC8DCD9981B6B79277548B6C17063B569BDACA3375A1B4CA95F3E` |
| 8 | `07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md` | Tokens v2.7, componentes (incl. novos), Calendar único, a11y; sem shortcuts funcionais | 142 | 11.144 | `2E3D8573F57D8E57225F7BD065E777721FEEE09879EAB6819E366815F6D44DBF` |
| 9 | `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md` | Dados, cutover (Job On primeiro), rollback, provenance do default | 75 | 5.594 | `A0E8CC5D5D995AF5CBCE0CB37EE80B6CC82865B6081E54089F58B3CA54413A26` |
| 10 | `09_TEST_QUALITY_AND_ACCEPTANCE_SPEC.md` | Testes (auditoria, contexto Job On), 18 cenários, testes determinísticos §9 | 151 | 9.386 | `3A3EECB9DBCB2411889367F987F3B675D0E4F7B1CCDDCB6B5A29AC100AE2559F` |
| 11 | `10_MASTER_IMPLEMENTATION_ROADMAP.md` | Unidades U-01..U-23 (incl. U-23 auditoria), gates A–J | 281 | 17.454 | `C7FCD191E81B7CE61CA528756C20BBDB8D034E2683725211B511F35C62D3102A` |
| 12 | `11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md` | Prompt final (precedência corrigida; keyboard rule; legacy recovery) | 131 | 7.883 | `8AB5F951E4705CD22F064A98E2C918345738A6FB6C49CCBED348FFD826FB447F` |
| 13 | `12_REQUIREMENT_TRACEABILITY_MATRIX.md` | Todos os IDs de requisito + decisões UD/TD/DS/NC | 144 | 10.260 | `7751828BE080F2BD8AF2D57C57A30B3FDED462BBC58F9EA10E628E836D460BF3` |
| 14 | `modules/00_MODULE_CATALOG.md` | Catálogo canónico (jobon primeiro; ferramentas.configure) | 77 | 6.119 | `B958EDB7C3A837EE462A77FB6355F60FE7B4B57D1D20CD502375E685FAED3DAB` |
| 15 | `modules/01_BOQUILHAS_SPEC.md` | Boquilhas (20→25; rejeição legacy D registada) | 138 | 8.708 | `6E9F4EC2EACD264DCE3F1DCAAE1E23B633A9018A43EDBA58D1B87A6CFB28E6DE` |
| 16 | `modules/02_CONTROLO_SPEC.md` | Controlo (área/domínio funcional; filhos atribuíveis) | 43 | 2.386 | `6E4665202C7B0BE3AE0F547B998E0EA2133367574DD8A948E8A96CFB72B72D27` |
| 17 | `modules/03_PESO_SPEC.md` | Peso (densidades TD-25; volumes TD-28; comparações TD-29/30; lote CM TD-26) | 185 | 12.989 | `208D9263C1F8AE1D8242C5445F5C93CEA732E376335ABB513421F127A9414477` |
| 18 | `modules/04_PEGAMENTOS_SPEC.md` | Pegamentos (regras TD-32; Job On obrigatório; PDF local) | 134 | 8.163 | `A859695EDCDEA356118064E553FDA82AF4528E2D9D8B71F06D655D37AC3A84C8` |
| 19 | `modules/05_JOB_ON_SPEC.md` | Job On (data model; lifecycle/ativo TD-27; landing) | 182 | 13.590 | `37D759CF969DF8FEE78CA29EF5A6A20BFC0A003757D395499D255857407A3786` |
| 20 | `modules/06_FERRAMENTAS_CM_MF_SPEC.md` | Ferramentas CM/MF (ferramentas.configure; identidade TD-26) | 125 | 8.014 | `A0B5D4A0D863FE1C181B63B57D6DD9437773E120D134FD8ED99553E0E413C219` |
| 21 | `modules/07_ARMAZEM_SPEC.md` | Armazém (posição 1:1; relação Job On) | 97 | 6.218 | `534CE486CE7459A57CB0A3EBBDC881E481377C377F2D50A409F3235387B5EEAA` |
| 22 | `modules/08_REPARACAO_INTERNA_SPEC.md` | Reparação interna (registo rápido; correção auditável) | 91 | 5.117 | `3BB153443918D985F3CBCCB49B5CF1AD704C30E6A6348F7A43C7E7EF96BC5596` |
| 23 | `modules/09_REPARACAO_EXTERNA_SPEC.md` | Reparação externa (módulo único; tab Boquilhas; CM/MF mesmo fluxo) | 107 | 6.842 | `507AA2172501177949EE33FCA8CA481E641A8E2CE24CAFBEF833E7736C3A3C04` |
| 24 | `modules/10_TAMPOES_SPEC.md` | Tampões (quantidades; transformação atómica) | 93 | 5.609 | `BF52C9259D61A5D02E7388883AD539B2F6C05BBA6EF2FB60F4E6CA49AE775A44` |
| 25 | `modules/11_HISTORIA_E_AUDITORIA_SPEC.md` | História (grants de origem) + auditoria global `audit_events` | 110 | 7.432 | `6380CD815202B941D7B1FC5324166AF5E39CDDB034DA0E201ABC2E0E14F02ACE` |
| 26 | `13_HANDOFF_MANIFEST.md` | Este manifesto | — | — | publicado no relatório final de entrega (auto-referência) |

## 2. Dependências internas

- `00_START_HERE` → todos (ordem de leitura);
- `02_DECISIONS` é a fonte canónica de decisões: §2 UD, §3.1–3.24 TD (design sync), §3.25–3.34
  TD (legacy recovery) + rejeições A–E, §7 NC, §7.1 DESIGN-SYNC;
- `modules/00_MODULE_CATALOG` é a fonte canónica do catálogo: referenciado por 04_ACC, 05_SHL;
- `06_DATA` detalha tabelas referenciadas pelas specs de módulos (família `job_on*` — TD-18;
  `audit_events` — TD-19);
- `12_TRACEABILITY` indexa todos os IDs; `11_PROMPT` referencia 00/02/03/04/07/09/10 e módulos.

## 3. Validações executadas nesta passagem (2026-08-17, legacy recovery)

1. Os 26 ficheiros existem na estrutura mínima exigida pelo handoff §5; nenhum ficheiro adicional
   criado (instrução §21 cumprida — sem `14_LEGACY_KNOWLEDGE_RECOVERY_REPORT.md`);
2. Legacy inspecionado com filesystem real (01_SOURCE §8): densidades (GAP-002), lote CM (GAP-001),
   Job On ativo/lifecycle (GAP-003), volumes (DG-02), base da comparação (DG-03), comparação
   anterior (DG-04), filename PDF, Pegamentos (sem legacy funcional), BQ 20→25 vs legacy;
3. Keyboard contract antigo removido de todos os ficheiros (NC-01/DS-11 resolvidos; 1 clique/2 cliques);
4. Precedência do GLM prompt corrigida (design não está acima de regras de negócio confirmadas);
5. Open questions pesquisadas globalmente: resolvidas com evidência ou marcadas
   `UNRESOLVED — NO AUTHORITATIVE SOURCE FOUND` com fontes pesquisadas (instrução §24/§25);
6. Cada descoberta promovida percorreu a cadeia source → decision → spec → data → test →
   traceability → prompt (instrução §22);
7. UTF-8 verificado em todos os ficheiros; zero mojibake; zero temporários;
8. Nenhum ficheiro fora de `plans/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/` foi alterado; design,
   app e legacy tratados como read-only.

## 4. Readiness

`READY FOR OWNER REVIEW` — após aprovação, o pacote torna-se autoridade de implementação e o
GLM 5.3 começa por `00_START_HERE.md` com a unidade autorizada (roadmap: U-01; sequência
atualizada: Job On — U-13 — antes de Peso/Pegamentos).
