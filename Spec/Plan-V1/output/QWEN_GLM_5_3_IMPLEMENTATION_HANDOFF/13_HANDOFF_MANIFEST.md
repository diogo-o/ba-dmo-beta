# 13 — HANDOFF MANIFEST

**Pacote:** `plans/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/`
**Versão:** 1.1 (sincronização com o design consolidado `portal-dmo-design-final/` — 2026-08-16)
**Codificação:** UTF-8 em todos os ficheiros
**Estado:** entregue para revisão do owner (autoridade após aprovação).

## 1. Ficheiros entregues (26)

| # | Ficheiro | Finalidade | Linhas | Bytes | SHA-256 |
|---|---|---|---:|---:|---|
| 1 | `00_START_HERE.md` | Entrada, natureza do programa, trace, contratos estruturais, precedência | 134 | 8.665 | `B451A89D4E374302812CD37BD79E89B15386CF0A0856BE8C410EF67C1B532880` |
| 2 | `01_SOURCE_AUTHORITY_REGISTER.md` | Fontes, hashes, cobertura, autoridade (design novo incluído) | 154 | 14.187 | `29BA69E3C7D9C66F87BC20EDA3E9742DE44FEF24154C6EF02EA7ACEA6FDAD698` |
| 3 | `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` | Decisões UD/TD, C1–C32, AB-01–03, DESIGN-SYNC DS-01..14, hard blocks | 375 | 32.903 | `B9541B2D1F2FDBAE5C3E2E4DC9251FB55C20E31DF6851EEA6F583EFC97EA528E` |
| 4 | `03_TARGET_MODULAR_ARCHITECTURE.md` | Arquitetura modular alvo, ownership, contratos lookup/auditoria | 162 | 9.815 | `0BE8FAD2126FF749173A40D4909DAB3C507C90E688E5C49F5F9963266E6DB56D` |
| 5 | `04_IDENTITY_ACCESS_AND_ADMIN_SPEC.md` | Identidade, acesso, matriz 18 cenários, Admin + Auditoria | 172 | 13.645 | `DDB5D209BE80206C691D12A10BD8CA385B642EC689560A4DD29D643ECF5E08F4` |
| 6 | `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md` | Shell única, tabs, landing Job On, deep links | 105 | 6.222 | `4A23C324E8D3EA460F211959F50F56A503861091FDA8FED87AA17FCE89BCF373` |
| 7 | `06_DATA_BACKEND_AND_SECURITY_SPEC.md` | Schema alvo (job_on*, audit_events), RLS, concorrência, servidor/local | 206 | 15.230 | `884C1B23FC1EC8DCD9981B6B79277548B6C17063B569BDACA3375A1B4CA95F3E` |
| 8 | `07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md` | Tokens v2.7, componentes (incl. novos), Calendar único, a11y | 141 | 10.732 | `A4A423188C23C4BFF5EAD2B10E7DE1511AF30DA92B9A379B7ACABF02D2DC9F3C` |
| 9 | `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md` | Dados, cutover (Job On primeiro), rollback, provenance do default | 75 | 5.594 | `A0E8CC5D5D995AF5CBCE0CB37EE80B6CC82865B6081E54089F58B3CA54413A26` |
| 10 | `09_TEST_QUALITY_AND_ACCEPTANCE_SPEC.md` | Testes (auditoria, contexto Job On), 18 cenários, DoD | 101 | 6.226 | `7B7A575780CA05B4B51FB87667B3C31BF994BA1C43E8CE0618F48374F6089ED7` |
| 11 | `10_MASTER_IMPLEMENTATION_ROADMAP.md` | Unidades U-01..U-23 (incl. U-23 auditoria), gates A–J | 280 | 17.280 | `06EDF1F8B153D780812ED1D486027862B9DA78668A6F920ECEDB753BFB98E219` |
| 12 | `11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md` | Prompt final pronto a colar (sincronizado) | 123 | 7.127 | `923EA25B7FEBB8D2BEFC43FD65231402A12A8409A2329E8BB2376BEE1A37C3F3` |
| 13 | `12_REQUIREMENT_TRACEABILITY_MATRIX.md` | Todos os IDs de requisito + decisões UD/TD/DS | 134 | 9.201 | `67071D12CBAAD13D7B5FA13C6D8F59F63A3AD845EEF8C6B47397208839FE1130` |
| 14 | `modules/00_MODULE_CATALOG.md` | Catálogo canónico (jobon primeiro; capabilities novas) | 75 | 5.842 | `85F779E6F63FAD219418BA87B4C87F6BC32275CA6F174C4DEC9738D9CB9D55FF` |
| 15 | `modules/01_BOQUILHAS_SPEC.md` | Boquilhas (incl. caso 20→25; ligação à tab BQ da Rep. externa) | 128 | 7.784 | `1CDFE3B42A9596F30ED0CECCA635BBB38E776EFDB737B855AE4749E9016A49C4` |
| 16 | `modules/02_CONTROLO_SPEC.md` | Controlo (área/domínio funcional; filhos atribuíveis) | 43 | 2.386 | `6E4665202C7B0BE3AE0F547B998E0EA2133367574DD8A948E8A96CFB72B72D27` |
| 17 | `modules/03_PESO_SPEC.md` | Peso (lotes com processo; contexto Job On; fronteira servidor/local) | 155 | 10.928 | `C72B3BF6AAFA479012500EB4267C6341EACB9A72F61A2DE7010376DD07B01C43` |
| 18 | `modules/04_PEGAMENTOS_SPEC.md` | Pegamentos (Job On obrigatório; PDF local) | 117 | 6.746 | `E882DEE3005D94F39BBAD4864ADA8A52FA6131D2AF2F9B4973CDA1A077712F52` |
| 19 | `modules/05_JOB_ON_SPEC.md` | Job On (data model, modos, landing, verificações) | 165 | 11.864 | `05E3AA30A6C222CEC9EC7FD90152D0B5A7FF8435657292AD4CC5594FC75B71C0` |
| 20 | `modules/06_FERRAMENTAS_CM_MF_SPEC.md` | Ferramentas CM/MF (processo no lote; verificações) | 114 | 7.113 | `5DC3B652D4D200A037F89883F641FBF1E8E09C94B2758E1BA0CD8E460E61C24F` |
| 21 | `modules/07_ARMAZEM_SPEC.md` | Armazém (posição 1:1; relação Job On consulta/edição) | 93 | 5.845 | `07432A943A1D561447C4C81C26D3828F2D1F320BC016A266364EC554BCB6FCA3` |
| 22 | `modules/08_REPARACAO_INTERNA_SPEC.md` | Reparação interna (registo rápido; correção auditável) | 88 | 4.851 | `5592E34EA212DEF133503EC369DD76618BF37181C03E15DA1AF54683EF436631` |
| 23 | `modules/09_REPARACAO_EXTERNA_SPEC.md` | Reparação externa (módulo único; tab Boquilhas; CM/MF mesmo fluxo) | 105 | 6.637 | `090BF3A0E35EE6EDD6DEE4F7D3B08F2448786DC098F948571C801EC5E49D19D2` |
| 24 | `modules/10_TAMPOES_SPEC.md` | Tampões (quantidades; transformação atómica) | 90 | 5.331 | `F4D678C775B699AC557BD3A743B3DB509502C6D0E7CFC22D68D5A94753872204` |
| 25 | `modules/11_HISTORIA_E_AUDITORIA_SPEC.md` | História (grants de origem) + auditoria global `audit_events` | 108 | 7.257 | `44DD44AF04C2B33BCE0098F60608A127DEC3149002568B5AF92696254415512B` |
| 26 | `13_HANDOFF_MANIFEST.md` | Este manifesto | — | — | publicado no relatório final de entrega (auto-referência) |

## 2. Dependências internas

- `00_START_HERE` → todos (ordem de leitura);
- `02_DECISIONS` é a fonte canónica de decisões: referenciado por 03–11 e modules/*;
  a secção §7.1 (DESIGN-SYNC) regista o delta da sincronização de design;
- `modules/00_MODULE_CATALOG` é a fonte canónica do catálogo: referenciado por 04_ACC, 05_SHL;
- `06_DATA` detalha tabelas referenciadas pelas specs de módulos (família `job_on*` — TD-18;
  `audit_events` — TD-19);
- `12_TRACEABILITY` indexa todos os IDs; `11_PROMPT` referencia 00/03/04/07/09/10 e módulos.

## 3. Validações executadas nesta passagem (2026-08-16, sincronização de design)

1. Os 26 ficheiros existem na estrutura mínima exigida pelo handoff §5;
2. Links/paths internos conferidos contra a estrutura do pacote; referência ao design aponta para
   `portal-dmo-design-final/` (pacote novo) e o anterior está marcado superseded em `01_SOURCE…`;
3. Requisitos sem traceability: nenhum encontrado (12_MATRIX §5);
4. IDs duplicados: nenhum (prefixos únicos por ficheiro/área);
5. Contradições internas: as quatro conhecidas da instrução de sincronização foram corrigidas —
   tab Boquilhas na Reparação externa (UD-13), fluxo CM/MF igual pré-produção (UD-13),
   `repair_exit_items` BQ/CM/MF consistente entre 02_DEC e 06_DATA (TD-22), Controlo como
   área/domínio funcional (UD-14); Operador/Responsável descritos como experiências exclusivas do
   módulo Peso (UD-15); História com autorização dos módulos de origem (TD-24); open questions
   resolvidas nas fontes (imagem Job On — TD-23; nomes de PDF — TD-21; landing — UD-16);
   default “começar vazio” com provenance BACKEND-DECISION-12/19 (08_MIG §2);
6. Cada módulo possui secções de testes e acceptance;
7. Caso Boquilhas 20→25 explícito em modules/01 §6 + 09_TEST §2.1;
8. Nenhuma instrução de previsão/decisão/bloqueio heurístico (02_DEC §5 inventário);
9. Operador/Responsável segregados (04_ACC §5; modules/03 §2);
10. Manifesto com line count/bytes/SHA-256 (esta tabela; hash próprio no relatório de entrega);
11. UTF-8 verificado em todos os ficheiros;
12. Nenhum ficheiro fora de `plans/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/` foi alterado nesta
    passagem (fontes apenas lidas; ficheiros temporários de apoio removidos).

## 4. Readiness

`READY FOR OWNER REVIEW` — após aprovação, o pacote torna-se autoridade de implementação e o
GLM 5.3 começa por `00_START_HERE.md` com a unidade autorizada (roadmap: U-01; sequência
atualizada: Job On — U-13 — antes de Peso/Pegamentos).
