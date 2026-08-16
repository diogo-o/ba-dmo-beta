# 00 — START HERE

**Pacote:** `plans/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/`
**Autor:** Qwen (Principal Planning Architect / Specification Author)
**Data:** 2026-08-16
**Estado de aprovação:** `PENDING OWNER REVIEW` — este pacote só é autoridade depois de aprovado pelo owner.
**Baseline factual:** `D:\BA-QWEN-MAX-PRODUCTION` (pasta local). O Git está desatualizado e serve apenas como histórico/provenance.
**Handoff de origem:** `plans/QWEN_MASTER_DEFINITIVE_FRESH_BUILD_HANDOFF.md`
(SHA-256 `634FCAB9C6C562A7BEEE6E9655D95EBFD956539579612F8B1DD058CD7A25000F`, 42.285 bytes — verificado nesta sessão).

---

## 1. Finalidade do pacote

Este pacote é o ponto de entrada único para o GLM 5.3 construir a aplicação nova do BA DMO.
Ele substitui a leitura das fontes legacy contraditórias como base de implementação: as fontes
originais permanecem em disco apenas para provenance e auditoria.

O pacote contém specs canónicas, decisões, arquitetura, modelo de acesso, contratos de
dados/backend, design system, specs por módulo, testes, migração, roadmap, rastreabilidade e o
prompt final de implementação.

## 2. Natureza do programa — regra transversal (GLM-CORE-01)

> O BA DMO é um programa de **registo de factos operacionais, rastreabilidade e histórico**.
> Não é um motor de previsão, recomendação, decisão ou julgamento operacional.

Consequências obrigatórias em todos os módulos:

1. regista o que aconteceu; preserva autoria, timestamp, origem e histórico;
2. não prevê, não decide pelo utilizador, não recomenda ações operacionais;
3. não adivinha dados, não inventa estados, não corrige silenciosamente;
4. não bloqueia factos reais com base em heurísticas, saldos esperados ou sequências presumidas;
5. pode apresentar avisos e pedir observações; um aviso nunca impede a gravação do facto;
6. correções são eventos auditáveis com before/after, autor e motivo;
7. cálculos determinísticos confirmados (ex.: Peso) são permitidos e apresentados, mas nunca
   convertidos em recomendação, previsão ou decisão automática.

Classificação obrigatória de qualquer bloqueio (GLM-CORE-02):
`SECURITY` · `TECHNICAL INTEGRITY` · `CONFIRMED BUSINESS RULE` · `WARNING ONLY` · `UNSUPPORTED HEURISTIC`.
Apenas as três primeiras categorias podem permanecer como hard block. O inventário completo está em
`02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` §5.

Cadeia de rastreabilidade obrigatória em todo o pacote (GLM-CORE-06):

`utilizador → módulo → domínio → entidade → evento/registo → relações → histórico`

Cada entidade tem ID estável, ownership (módulo dono), autoria, timestamps, relações explícitas por
ID e histórico auditável. Texto apresentado nunca é chave de integração.

Contratos estruturais inegociáveis (GLM-CORE-07; detalhe nos ficheiros indicados):

- **Modularidade** — módulos atribuídos por templates editáveis na Administração; tabs derivadas dos
  módulos autorizados; enforcement server-side; título visual não concede poder; módulos novos entram
  por código/spec/schema/testes e ficam depois disponíveis para atribuição; a Administração não inventa
  identificadores arbitrários (03_ARCH; 04_ACC; modules/00).
- **Controlo** — área/domínio funcional; Peso e Pegamentos pertencem a Controlo, são atribuíveis
  separadamente, apenas filhos autorizados aparecem e nunca fundem lógica (modules/02).
- **Peso** — um módulo com duas experiências mutuamente exclusivas: Operador sem `peso.aprovar`,
  Responsável com `peso.aprovar`; sem seletor manual; sem acesso cruzado às páginas (04_ACC §5; modules/03).
- **Reparação Externa** — um único módulo atribuível `reparacao_externa` com Boquilhas, Contra Moldes,
  Moldes Finais, Envios, Histórico e Definições; BQ por referência/lote/quantidade; CM e MF por
  referência/lote/número individual com o mesmo fluxo de preparação/envio antes da produção, separados
  internamente por tipo de entidade/apresentação; Reparação Interna é outro módulo (modules/09; modules/08).
- **Boquilhas 20→25** — registo integral do retorno, reconciliação, discrepancia auditável, histórico
  completo e ausência de bloqueio/autorização especial (modules/01 §6).
- **Interação de listas/registos** — 1 clique seleciona; 2 cliques abrem/entram no registo. O BA DMO
  não tem shortcuts funcionais próprios (Enter/Espaço/Ctrl+Enter não são requisitos; NC-01 resolvido).
- **Conhecimento recuperado do legacy com provenance** — tabela de densidades (TD-25), identidade do
  lote CM (TD-26), Job On ativo/lifecycle (TD-27), volumes (TD-28), base da comparação (TD-29/30),
  filenames reais (TD-31), regras Pegamentos (TD-32), `ferramentas.configure` (TD-33) — tudo em
  `02_DECISIONS…` §3.25–3.34, com classificação A–E do legacy em §3.34.

## 3. Ordem de leitura obrigatória do GLM

1. `00_START_HERE.md` (este ficheiro);
2. `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` — decisões e contrato de registo;
3. `03_TARGET_MODULAR_ARCHITECTURE.md`;
4. `04_IDENTITY_ACCESS_AND_ADMIN_SPEC.md`;
5. `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md`;
6. `06_DATA_BACKEND_AND_SECURITY_SPEC.md`;
7. `07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md`;
8. `modules/00_MODULE_CATALOG.md` e a spec do módulo autorizado no checkpoint atual;
9. `09_TEST_QUALITY_AND_ACCEPTANCE_SPEC.md`;
10. `10_MASTER_IMPLEMENTATION_ROADMAP.md` (unidade autorizada apenas);
11. `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md` (quando a unidade tocar dados/cutover);
12. `12_REQUIREMENT_TRACEABILITY_MATRIX.md` (consulta);
13. `01_SOURCE_AUTHORITY_REGISTER.md` (provenance, apenas quando o pacote indicar).

## 4. Precedência de fontes (GLM-CORE-03)

1. Decisões explícitas atuais do utilizador (registadas em `02_…` §2);
2. Regras operacionais confirmadas (verified knowledge reconciliado);
3. Este pacote aprovado (specs canónicas);
4. Specs aprovadas anteriores (`Spec/01_BQ…`, `02_PESO…`, `03_AUTH…`, `04_ADMIN…`, `07_C_SHARP…`, `08_SUPABASE…`) quando não contrariadas pelo pacote;
5. Implementação local (`Spec/src`, `ba-dmo-v2/src`) como evidência;
6. Código legacy, Git e mockups como referências secundárias.

O código antigo não seguiu integralmente o funcionamento estabelecido. A existência de um
validator, bloqueio ou workflow legacy **não prova** uma regra de negócio.

## 5. Execução por checkpoints (GLM-CORE-04)

- O GLM implementa **apenas uma unidade/checkpoint autorizado de cada vez**, definido em `10_MASTER_IMPLEMENTATION_ROADMAP.md`.
- Cada unidade termina num gate com evidência objetiva: testes `total/passed/failed/duration`, checks manuais pendentes, riscos e decisão avançar/parar.
- Nenhum SQL live, commit, push ou alteração de scope sem aprovação explícita do owner.
- Discrepâncias entre o pacote e a realidade são **reportadas**, nunca resolvidas alterando specs silenciosamente.

## 6. Proibição de reinterpretação silenciosa (GLM-CORE-05)

O GLM não pode reinterpretar fontes legacy para justificar desvios ao pacote. Se uma fonte legacy
contradiz o pacote, o pacote prevalece (após aprovação do owner) e o conflito é reportado. Se o
pacote estiver omisso, o GLM para e reporta — não inventa.

## 7. Lista de ficheiros do pacote

| Ficheiro | Conteúdo |
|---|---|
| `00_START_HERE.md` | Entrada, regras transversais, ordem de leitura |
| `01_SOURCE_AUTHORITY_REGISTER.md` | Fontes lidas, hash, cobertura, autoridade |
| `02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md` | Decisões, C1–C32, AB-01–03, hard blocks, MUST PRESERVE / DO NOT CARRY FORWARD |
| `03_TARGET_MODULAR_ARCHITECTURE.md` | Arquitetura modular alvo |
| `04_IDENTITY_ACCESS_AND_ADMIN_SPEC.md` | Identidade, templates, catálogo, matriz de acesso, Admin |
| `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md` | Shell única, tabs, landing, deep links |
| `06_DATA_BACKEND_AND_SECURITY_SPEC.md` | Schema alvo, RLS, concorrência, auditoria, histórico |
| `07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md` | Tokens, CSS, componentes, Calendar único, a11y |
| `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md` | Dados, cutover, rollback |
| `09_TEST_QUALITY_AND_ACCEPTANCE_SPEC.md` | Pirâmide de testes, gates de qualidade, DoD |
| `10_MASTER_IMPLEMENTATION_ROADMAP.md` | Unidades pequenas e gates A–J |
| `11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md` | Prompt final pronto a colar |
| `12_REQUIREMENT_TRACEABILITY_MATRIX.md` | Todos os IDs de requisito |
| `13_HANDOFF_MANIFEST.md` | Manifesto com line count, bytes e SHA-256 |
| `modules/00_MODULE_CATALOG.md` | Catálogo canónico de módulos e capabilities |
| `modules/01_BOQUILHAS_SPEC.md` … `modules/11_HISTORIA_E_AUDITORIA_SPEC.md` | Specs por módulo |

## 8. Restrições da sessão de planeamento (cumpridas)

Nenhum código de produção foi implementado; nenhuma spec, migration, teste, design package ou
verified knowledge foi alterado; nenhum SQL live foi executado; nenhum commit/push/merge/reset foi
feito; todo o output foi escrito apenas dentro de `plans/QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF/`.
