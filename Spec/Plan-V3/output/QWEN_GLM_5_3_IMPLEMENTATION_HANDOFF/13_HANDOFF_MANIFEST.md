# 13 — HANDOFF MANIFEST

**Pacote:** `Spec/Plan-V3/output/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/`
**Versão:** 2.1 (Plan-V3 — TECHNICAL BUILD READINESS PATCH + PORTABILITY PATCH — 2026-08-17; anterior: 1.2 legacy recovery)
**Design baseline:** `Design-Reference/portal-dmo-design-final/` (commit ref `3b23e30bfc4e33845d9cc708e3bcbd703dac0aa0`)
**Codificação:** UTF-8 em todos os ficheiros
**Estado:** Plan-V3 aprovado para implementação — autoridade de implementação para a fresh build.

## 0. Contagem do package (Plano-V3)

```text
Canonical package documents: 26
Execution provenance files: 1   (PROMPT.md — prompt literal da execução Plan-V3)
```

`PROMPT.md` é provenance da execução e **não** altera a contagem canónica de 26 documentos de spec.

## 1. Ficheiros entregues (26 canónicos)

| # | Ficheiro | Finalidade | Linhas | Bytes | SHA-256 |
|---|---|---:|---:|---:|---|
| 1 | `00_START_HERE.md` | Entrada, natureza do programa, contratos estruturais, precedência | 159 | 11.136 | `B26BEDC43002F334CC73FDCCB6D868A723169F22D254C61EB038F4A627380DAD` |
| 2 | `01_SOURCE_AUTHORITY_REGISTER.md` | Fontes, hashes, cobertura, autoridade (+ legacy recovery §8) | 181 | 17.415 | `8C6C45B0CD0E9D383910625C43A53F608C011E6D239AEB806A4DB248F13FA632` |
| 3 | `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` | Decisões UD/TD (incl. TD-25..33 legacy recovery e §3.35 PV-01..PV-13 Plan-V3), C1–C32, AB, DS, rejeições | 529 | 48.765 | `140BC64B1D90EFF62E2981AEFF2BB30F61FA89ECB5F037258AB4ECBE76C28E4F` |
| 4 | `03_TARGET_MODULAR_ARCHITECTURE.md` | Arquitetura modular alvo, ownership, contratos lookup/auditoria, build contract Plan-V3 (§12–18) | 268 | 16.165 | `CEE58E9981FCE896B47FEFDB6AA90E35061420BAA99949CD6621FE0CFE62A23A` |
| 5 | `04_IDENTITY_ACCESS_AND_ADMIN_SPEC.md` | Identidade, acesso, matriz 18 cenários, Admin + Auditoria | 172 | 13.645 | `DDB5D209BE80206C691D12A10BD8CA385B642EC689560A4DD29D643ECF5E08F4` |
| 6 | `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md` | Shell única, tabs, landing Job On, deep links | 105 | 6.222 | `4A23C324E8D3EA460F211959F50F56A503861091FDA8FED87AA17FCE89BCF373` |
| 7 | `06_DATA_BACKEND_AND_SECURITY_SPEC.md` | Schema alvo (job_on*, audit_events), RLS, concorrência, servidor/local, migrations/schema_migrations/CLI/PDF (§12–17) | 286 | 19.562 | `DBF79464D0D62947A525F5374BDE071767337E3E125472F5F1ED755B22714ED0` |
| 8 | `07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md` | Tokens v2.7, componentes (incl. novos), Calendar único, a11y; sem shortcuts funcionais | 151 | 11.907 | `95677FEF025F21270AE43B81957CE19FA439C16AF6F0454F58B1F1EEBFE05F7B` |
| 9 | `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md` | Dados, cutover (Job On primeiro), rollback, provenance do default, execução técnica §8 | 87 | 6.371 | `8BB1FF1B0BD6E942D539CECBF1C963A03639F082D91ECEA85CC328E3BEC59BD9` |
| 10 | `09_TEST_QUALITY_AND_ACCEPTANCE_SPEC.md` | Testes (auditoria, contexto Job On), 18 cenários, testes determinísticos §9, contrato técnico §10 | 173 | 10.863 | `77696BAA7719DE9580BC1370A2DA5B258B8500CC616D6205BDB4DB2F3E54E596` |
| 11 | `10_MASTER_IMPLEMENTATION_ROADMAP.md` | Unidades U-01..U-23 (incl. U-23 auditoria), gates A–J, build contract (U-01/U-02) e deploys (U-22) | 310 | 20.067 | `88E760833B062905E4EF5379CE5895D2DC34A6E8C8CFEE5D530A361AE5DADD02` |
| 12 | `11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md` | Prompt final (precedência corrigida; keyboard rule; legacy recovery; portability; model-independent; TECHNICAL BUILD CONTRACT) | 211 | 14.427 | `AC8183823DB2144B98A204AEBA39670A3B793D9F301333B746133E382134BC1E` |
| 13 | `12_REQUIREMENT_TRACEABILITY_MATRIX.md` | Todos os IDs de requisito + decisões UD/TD/DS/NC + PV-01..PV-07 | 159 | 12.369 | `550B38EAA23A15968A5420EB0643EFAECC2ACED9008A682F88A843D2FB912D9E` |
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

## 1.1 Ficheiro de provenance da execução Plan-V3

| Ficheiro | Finalidade | Linhas | Bytes | SHA-256 |
|---|---:|---:|---:|---|
| `PROMPT.md` | Prompt literal da execução Plan-V3 (provenance; não canónico, não conta para os 26) | 1.416 | 20.958 | `2918270525AA18A231D02B013977766D7A34A07C0DD428D47DA8B92D1F55F90A` |

## 2. Dependências internas

- `00_START_HERE` → todos (ordem de leitura; contagem canónica 26 + provenance 1);
- `02_DECISIONS` é a fonte canónica de decisões: §2 UD, §3.1–3.33 TD, §3.35 PV-01..PV-13 (Plan-V3),
  §3.34 rejeições A–E, §7 NC, §7.1 DESIGN-SYNC;
- `modules/00_MODULE_CATALOG` é a fonte canónica do catálogo: referenciado por 04_ACC, 05_SHL;
- `06_DATA` detalha tabelas referenciadas pelas specs de módulos e o contrato técnico §12–17
  (migrations/schema_migrations/CLI, Supabase boundary, bootstrap, PDF);
- `12_TRACEABILITY` indexa todos os IDs (incl. PV-01..PV-07 e GLM-ARCH-12..18 / GLM-DATA-12..17);
- `11_PROMPT` referencia 00/02/03/04/07/09/10 e módulos, e contém a secção `TECHNICAL BUILD CONTRACT`.

## 3. Plan-V3 — ficheiros materialmente alterados (11)

`00_START_HERE.md`, `02_DECISIONS…md`, `03_TARGET_MODULAR_ARCHITECTURE.md`,
`06_DATA_BACKEND_AND_SECURITY_SPEC.md`, `07_DESIGN_SYSTEM…md`, `08_MIGRATION…md`,
`09_TEST_QUALITY…md`, `10_MASTER_IMPLEMENTATION_ROADMAP.md`, `11_GLM_5_3_MASTER…PROMPT.md`,
`12_REQUIREMENT_TRACEABILITY_MATRIX.md`, `13_HANDOFF_MANIFEST.md`.

Não alterados (15): `01_SOURCE_AUTHORITY_REGISTER.md`, `04_IDENTITY…md`, `05_SHELL…md` e
`modules/00`..`modules/11` (hashes inalterados). Design baseline `portal-dmo-design-final/` intacto.

## 4. Validações executadas nesta passagem (Plan-V3)

1. Os **26** documentos canónicos existem; `PROMPT.md` adicionado como provenance (1), separado da
   contagem canónica;
2. Propagação das decisões aprovadas com cadeia completa: owner decision (`02_DEC §3.35 PV-01..PV-13`)
   → arquitetura (`03_ARCH §12–18`) → dados/design (`06_DATA §12–17`; `07_DESIGN`) → roadmap
   (`10_ROADMAP U-01/U-02/U-22`) → testes (`09_TEST §10`) → traceability (`12`) → GLM prompt
   (`11` TECHNICAL BUILD CONTRACT);
3. `FUNCTIONAL / BUSINESS CHANGES: 0` — sem alteração de regras funcionais ou de negócio;
4. Consistency pass executado: refs legacy/Windows identificadas e classificadas; canónicas
   superseded corrigidas (ver relatório de execução);
5. `net8.0`/`net9.0`/“ASP.NET Core 8+”/`win-x64`/`PublishSingleFile`/Windows-deploy como **contrato
   atual** removidos; mantidos apenas como historical provenance onde aplicável;
6. UTF-8 verificado em todos os ficheiros; zero mojibake; zero temporários;
7. Nenhum ficheiro fora de `Spec/Plan-V3/output/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/` foi alterado; design, app e
   legacy tratados como read-only; nenhum código de aplicação implementado.
8. **PLAN-V3 PORTABILITY PATCH** (2026-08-17): `00_START_HERE.md` e `11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md`
   corrigidos para remover a dependência do workspace local antigo (`D:\BA-QWEN-MAX-PRODUCTION`) e do estado
   `PENDING OWNER REVIEW`; a autoridade de implementação passou a ser explicitamente o repositório de arquivo
   `diogo-o/ba-dmo-beta` (branch `main`), com o design baseline como autoridade de UI/UX e a fresh build em
   workspace separado. Sem alterações funcionais/de negócio (`FUNCTIONAL / BUSINESS CHANGES: 0`).
9. **MODEL-INDEPENDENT IMPLEMENTATION CONTRACT** (2026-08-17): o prompt mestre (`11`) e os documentos de
   orientação (`00`, `10`) passaram de referências operacionais a "GLM 5.3" para "implementation agent"
   (model-independent); agente de implementação atual: **Qwen 3.8 Max**. Os IDs de requisito `GLM-*`
   e os nomes canónicos históricos (ex.: `11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md` e a diretoria
   `QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF`) são identificadores estáveis/provenance e foram mantidos.
   Sem alterações funcionais/de negócio.

## 5. Readiness

`READY FOR IMPLEMENTATION (model-independent)` — o Plano-V3 está tecnicamente pronto para implementação **sem inferência
material**: target `net10.0`, estrutura de 6 projetos, Dapper/Npgsql, Supabase boundary, migrations
CLI (`schema_migrations`), bootstrap-admin CLI, Docker/Render/Linux, PDF em memória + File System
Access export-only, IndexedDB directory-handle exception e no-debug-bypass estão explícitos no prompt
mestre (`11`). Contrato model-independent; agente de implementação atual: Qwen 3.8 Max. Primeira
unidade autorizada após aprovação do owner: **U-01**.
