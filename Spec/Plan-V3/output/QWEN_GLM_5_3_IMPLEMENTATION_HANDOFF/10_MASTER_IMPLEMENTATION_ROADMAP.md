# 10 — MASTER IMPLEMENTATION ROADMAP

Execução faseada em unidades pequenas, recuperáveis e atribuíveis. O implementation agent implementa **apenas a
unidade autorizada** (GLM-CORE-04). Gates A–J conforme handoff §7. Formato por unidade: ID ·
objetivo/outcome · autoridade · dependências · scope (ficheiros) · contratos afetados · trabalho ·
proibições · impacto de dados · impacto de segurança · testes · acceptance · stop condition ·
rollback · gate seguinte.

## Visão global

| Fase | Unidades | Gate |
|---|---|---|
| Reconciliação | — (aprovação do pacote pelo owner) | A |
| Fundação | U-01..U-04 | B, C |
| Identidade/Admin/Shell | U-05..U-07 (+ auditoria global U-23 junto de U-06) | D |
| Design foundation | U-08..U-09 | E |
| Job On (contexto central de produção; landing) | U-13 | F-JOB |
| Controlo | U-10 (Peso) · U-11 (Pegamentos) — dependem do contexto Job On (DS-04/DS-05) | F |
| Restantes módulos | U-12, U-14..U-18 | G (por bounded context) |
| Boquilhas (migração comportamental central) | U-19 | G-BQ |
| Integração final | U-20 | H |
| Migração/cutover | U-21 | I |
| Produção | U-22 | J |

Notas:
- A ordem reflete a dependência funcional atual: Peso e Pegamentos iniciam no contexto do Job On,
  pelo que U-13 antecede U-10/U-11 (design consolidado — DS-01..DS-08).
- Boquilhas pode ser antecipada para a fase F+ se o owner preferir (é o módulo mais maduro);
  a ordem acima segue a prioridade identidade → design → Job On → Controlo do design atual.

---

## U-01 — Solution skeleton + shared kernel

- **Objetivo/outcome:** solução de 6 projetos compila em `net10.0` com as referências prescritas;
  `Result<T,E>`, `IClock`, `ICurrentUserAccessor`, `ModuleCatalog` vazios-funcionais.
- **Autoridade:** 03_ARCH (GLM-ARCH-12 build contract); `07_C_SHARP §3`.
- **Dependências:** nenhuma (Gate A aprovado).
- **Scope:** `BA-DMO.sln`, `src/*`, `tests/*`, `Directory.Build.props`, `database/migrations/` (vazio).
- **Build contract (net10.0):** criar a solution de raiz com `dotnet new sln -n BA-DMO` e
  `dotnet new classlib/web/xunit -f net10.0` (Domain, Application, Infrastructure, Web;
  UnitTests, IntegrationTests). Project references prescritas: Application→Domain;
  Infrastructure→Application+Domain; Web→Application+Infrastructure; UnitTests→Domain+Application;
  IntegrationTests→Web+Infrastructure. Comandos canónicos: `dotnet restore`, `dotnet build`,
  `dotnet test`; local run `dotnet run --project src/BA.Dmo.Web`. Produção via Docker/Render, não
  publish Windows manual.
- **Proibições:** módulos funcionais; dependências externas não aprovadas; tooling local proibido;
  `win-x64` como target de produção.
- **Dados/segurança:** nenhum.
- **Testes:** kernel unit tests (Result, catálogo vazio válido); CLI routing (`migrate`/`bootstrap-admin`
  vs web startup); no production debug bypass (09_TEST §10).
- **Acceptance:** build verde; grafo de dependências conforme 03_ARCH §4; TargetFramework `net10.0`.
- **Stop:** restore/build falha sem causa identificável.
- **Rollback:** apagar branch/diretório da unidade.
- **Gate seguinte:** U-02.

## U-02 — Schema fresh-build (migrations N01–N12, sem execução live)

- **Objetivo:** scripts SQL completos do schema alvo + roles + RLS + **`schema_migrations`** +
  **executor Npgsql full-script** + **CLI migrate**; validados por revisão.
- **Autoridade:** 06_DATA; BT-08; GLM-ARCH-12/15.
- **Dependências:** U-01.
- **Scope:** `database/migrations/N01_identity…N12_rls.sql`; `Infrastructure/Persistence`
  (custom migration runner Npgsql); tabela `schema_migrations` (version/id, filename, sha256,
  applied_at); comando CLI `migrate`.
- **Trabalho:** todas as tabelas §3 do 06_DATA com constraints/índices; roles `ba_dmo_app/migrate`;
  RLS policies; idempotência; migration runner que lê cada `.sql` integralmente, calcula SHA-256,
  compara com `schema_migrations`, envia integralmente e **regista apenas após sucesso** (sem
  `split(';')`/parser próprio/EF Core).
- **Proibições:** executar SQL live; seeds operacionais; FK a `auth.users`; coluna `allow_unmatched`;
  HTTP migration endpoint; migrations no startup de produção.
- **Dados:** nenhum executado. **Segurança:** RLS + least privilege definidos em script; service role
  nunca no request pipeline.
- **Testes:** revisão por checklist + teste de aplicação em BD de teste apenas quando autorizada (U-20);
  migration checksum/idempotência e failure (09_TEST §10).
- **Acceptance:** checklist 06_DATA §3/§6 completo; scripts idempotentes; runner calcula checksums e
  falha devidamente sem registar.
- **Stop:** contradição com 06_DATA (reportar, não corrigir spec).
- **Rollback:** descartar scripts.
- **Gate seguinte:** **Gate C** (schema validado sem live) → U-03.

## U-03 — Persistence infrastructure

- **Objetivo:** `DbConnectionFactory`, `DapperUnitOfWork`, mappings base, política de timestamps/autoria.
- **Autoridade:** 06_DATA §1–2, §5, §8.
- **Dependências:** U-02.
- **Scope:** `Infrastructure/Persistence`, ports de repositório genéricos de suporte.
- **Proibições:** lógica de módulos; RPC Supabase.
- **Testes:** unit (mappings); integração smoke quando BD de teste disponível.
- **Acceptance:** transação ambiente funciona em teste; concurrency helper testado.
- **Rollback:** reverter pasta.

## U-04 — Catálogo de módulos + espelho DB

- **Objetivo:** `ModuleCatalog` canónico (modules/00) + migration de espelho + validação server-side.
- **Autoridade:** TD-10; `modules/00_MODULE_CATALOG.md`.
- **Dependências:** U-01.
- **Scope:** `Application/Shared/Access`, `module_catalog_mirror`.
- **Testes:** normalização (duplicados, prefixos, entradas inválidas descartadas).
- **Acceptance:** catálogo novo módulo dispensa alterações de navegação (verificado por teste).
- **Gate seguinte:** **Gate B** (arquitetura + catálogo aprovados).

## U-05 — Auth + identidade interna

- **Objetivo:** login Supabase + cookie bridge; resolução internal_users/access_templates por request; erros `INTERNAL_USER_INACTIVE`/`ACCESS_TEMPLATE_INACTIVE`; bootstrap do primeiro **Admin** (one-shot CLI `bootstrap-admin` — 06_DATA §15).
- **Autoridade:** 04_ACC §1, GLM-ACC-13; TD-09/TD-16.
- **Dependências:** U-03, U-04.
- **Scope:** `Infrastructure/Auth`, `Pages/Auth`, serviço de identidade, comando CLI `bootstrap-admin`.
- **Segurança:** grants nunca no cookie; `service_role` só server-side e apenas na operação privilegiada; sem anonymous admin / debug claims (03_ARCH §14/§18).
- **Testes:** resolução ativa/inativa; template inativo; bootstrap idempotente; CLI routing; no debug bypass (09_TEST §10).
- **Acceptance:** login/logout funcionais; identidade resolvida server-side.
- **Rollback:** reverter; sem dados alterados.

## U-06 — Administração completa

- **Objetivo:** CRUD users/templates com catálogo controlado, self-lockout atómico, auditoria, concorrência, reset password, cenários 7/8/13/14/15.
- **Autoridade:** 04_ACC §9–12; `04_ADMIN_COMPLETE_SPEC.md`.
- **Dependências:** U-05.
- **Scope:** `Modules/Admin` (Domain/Application/Infrastructure/Web).
- **Proibições:** conhecer domínio BQ/Peso; apagar registos; conceder por título.
- **Testes:** matriz Admin (positivos/negativos), self-lockout transacional, audit before/after.
- **Acceptance:** DoD 04_ADMIN §22 transposto e verde.
- **Stop:** auditoria não atómica.
- **Rollback:** reverter unidade.

## U-07 — Shell única + navegação derivada

- **Objetivo:** `_Layout`/header/nav; tabs por grants; Controlo grupo; landing UD-04; deep links seguros; estados sem acesso; `/peso` vs `/peso/responsavel` exclusivos (guards de routing).
- **Autoridade:** 05_SHL; 04_ACC §5–6.
- **Dependências:** U-05, U-06, U-09 (componentes shell).
- **Testes:** cenários 1–12 da matriz de acesso ao nível de rotas.
- **Acceptance:** nenhum deep link expõe módulo não autorizado; zero/uma/múltiplas tabs corretas.
- **Gate seguinte:** **Gate D**.

## U-08 — Design tokens + componentes universais

- **Objetivo:** implementar 07_DESIGN §1–4: tokens completos, foundation/components CSS, componentes P1, página-laboratório.
- **Autoridade:** 07_DESIGN; DESIGN_IMPLEMENTATION_CONTRACT.
- **Dependências:** U-01 (estrutura Web/wwwroot).
- **Proibições:** copiar CSS dos mockups; `<style>` de design; segundos componentes.
- **Testes:** checklist §21 do contrato (foundation/components); a11y keyboard nos componentes.
- **Acceptance:** laboratório com todos os componentes/estados; contraste AA.
- **Stop:** valor de token sem provenance (reportar).
- **Rollback:** reverter CSS.

## U-09 — Calendar único + Shell visual

- **Objetivo:** Calendar canónico (GLM-DSN-05) + header/nav/account visual + responsive + visual regression baseline.
- **Autoridade:** 07_DESIGN §5–6, §8.
- **Dependências:** U-08.
- **Acceptance:** calendar consumido por página de teste; baseline desktop/tablet/mobile.
- **Gate seguinte:** **Gate E**.

## U-10 — Módulo Peso (Operador + Responsável)

- **Objetivo:** Peso completo: referências + **lotes do Peso** (processo NNPB/PS no lote, máquinas
  permitidas, `Subpasta dos relatórios` — TD-17), Novo controlo e Comparação iniciados no contexto do
  Job On (CM/lote herdados, sem segunda seleção — DS-04), cálculos C# únicos, workflow, day approvals,
  fronteira servidor/local dos PDFs (DS-08), email preparado, histórico; segregação de experiências.
- **Autoridade:** `modules/03_PESO_SPEC.md`; 04_ACC §5; PESO_INTERFACE_HANDOFF (design atual).
- **Dependências:** U-07, U-08/09, **U-13 (contexto Job On)**.
- **Scope:** `Modules/Peso/**`, `Pages/Peso/**`, migrations peso (já em U-02), `peso.js` behavior mínimo não-autoritativo.
- **Proibições:** duplicar fórmulas em JS; selector manual Operador/Responsável; página do Operador ao
  Responsável e vice-versa; segunda seleção de CM/lote; caminhos absolutos livres na subpasta; botão
  delete fora da policy (rascunho/nao_aprovado autor OU peso.aprovar, conforme 08_SUPABASE §9 CONFIRMED).
- **Testes:** WeightCalculator (tabela TD-25 completa + fronteiras; volumes TD-28), workflow estados,
  comparação no contexto do Job On (base imutável; decisão por CM; delta anterior cross-line TD-30),
  filename TD-31, cenários 2/3, delete policy, falha local do PDF não desfaz aprovação; testes
  documentais determinísticos em 09_TEST §9 (1–10, 16–17).
- **Acceptance:** spec do módulo verde; PDF gerado em `diretório principal / subpasta do lote`.
- **Stop:** divergência de cálculo vs casos confirmados.
- **Gate seguinte:** U-11.

## U-11 — Módulo Pegamentos (Controlo)

- **Objetivo:** Pegamentos: contexto **Job On obrigatório** (referência, produção, máquina e instâncias/
  lotes de CM, BQ e MF herdados — DS-05), sem fallback nem seleção alternativa; bloqueio acionável
  `Corrigir ferramentas no Job On`; folha de medições (Costura/Contra costura), histórico estruturado,
  PDF local no caminho resolvido; sem base local.
- **Autoridade:** `modules/04_PEGAMENTOS_SPEC.md`.
- **Dependências:** U-13 (contexto Job On obrigatório), U-10 (lotes CM via Job On), U-19 ou lookup BQ
  (contrato; pode ser stub até Boquilhas existir).
- **Proibições:** catálogos paralelos; fusão com Peso; fallback silencioso sem Job On; base local.
- **Testes:** contexto obrigatório; filtragem por ref+máquina; registos antigos preservam valores.
- **Gate seguinte:** **Gate F** (Controlo com segregação comprovada).

## U-12 — Ferramentas CM/MF + registo

- **Objetivo:** `tool_references`, `tool_lotes`, `physical_pieces`; criar referência+primeiro lote; duplicar lote; verificações (regras/ocorrências).
- **Autoridade:** `modules/06_FERRAMENTAS_CM_MF_SPEC.md`; FERRAMENTAS_REGISTO brief.
- **Dependências:** U-07..U-09.
- **Testes:** criação atómica ref+lote; identidade mestre read-only na duplicação; ocorrências por frequência.
- **Gate seguinte:** U-14 (Gate G inicia).

## U-13 — Job On (contexto central de produção)

- **Objetivo:** folha Job On como fonte operacional da produção: família `job_on*` do
  JOB_ON_DATA_MODEL (TD-18); Modo consulta (todos) vs Modo edição (Responsável técnico — TD-20);
  tabs Planeamento/Job On/Histórico/Definições; contexto fixo; famílias MP/CM, MF, BQ, PU, CAL, AN,
  ARR, PI, CS, TP, FO com campos tipados e linhas CAL; imagem por revisão (TD-23); verificações
  (ocorrências materializadas; `jobon.confirmar`); duplicações (anterior/histórico/branco) com
  snapshot completo; alteração de datas com auditoria e projeção no calendário; comparação
  snapshot≠live; histórico em dois níveis (Produções da Referência → Revisões da Produção);
  Definições com catálogos `job_on_field_option`; landing global (UD-16).
- **Autoridade:** `modules/05_JOB_ON_SPEC.md`; JOB_ON_DATA_MODEL.md; JOB_ON_DESIGN_BRIEF/VERIFICACOES (design atual).
- **Dependências:** U-07, U-08/09, U-23 (eventos de auditoria); lookups Ferramentas/Boquilhas/Armazém
  (contratos; stubs aceitáveis até os módulos existirem).
- **Proibições:** criar ferramentas/lotes; cópias silenciosas live sobre snapshots; UPDATE destrutivo
  de revisão guardada; inferir compatibilidades; disponibilidade live em Modo consulta.
- **Testes:** duplicações (origem imutável; tudo copiado; data substituída), datas auditadas + calendário,
  imutabilidade de revisões, modos consulta/edição e capabilities, verificações manuais persistidas,
  landing Job On para todos os perfis, `Resolve(line, at)` e transições de estado (09_TEST §9.11–15).

## U-14 — Armazém

- **Objetivo:** posições/ocupação/movimentos; execução de saídas programadas com fecho atómico; correção de localização; consulta.
- **Autoridade:** `modules/07_ARMAZEM_SPEC.md`.
- **Dependências:** U-12 (IDs estáveis).
- **Testes:** atomicidade do fecho (falha não liberta posições); posição só removida após persistência; sem estados inventados.

## U-15 — Reparação externa

- **Objetivo:** listas programadas BQ/CM/MF, reparadores por tipo/linha, ciclo completo com Armazém, histórico, definições.
- **Autoridade:** `modules/09_REPARACAO_EXTERNA_SPEC.md`; BT-07.
- **Dependências:** U-14, U-12.
- **Testes:** item noutra saída aberta bloqueado (técnico); retorno sem saída = aviso+registo; snapshot de reparador.

## U-16 — Reparação interna

- **Objetivo:** registo rápido Linha+tipo+número com contexto JobOn resolvido; histórico; correção auditável (`reparacao_interna.corrigir`).
- **Autoridade:** `modules/08_REPARACAO_INTERNA_SPEC.md`.
- **Dependências:** U-13 (contexto ativo), U-12.
- **Testes:** sem Job On ativo não guarda; ambiguidade exige escolha; correção preserva original.

## U-17 — Tampões

- **Objetivo:** campos/valores normalizados, configurações, saldos, movimentos (incl. transformação atómica), planeamento, opções geridas pelo Operador.
- **Autoridade:** `modules/10_TAMPOES_SPEC.md`.
- **Testes:** saldos nunca negativos; transformação atómica; destino existente reutilizado; planear não reserva.

## U-18 — História/Auditoria transversal

- **Objetivo:** vistas de leitura por ferramenta/produção/linha/ator sobre histórico dos domínios + audit admin; History Entry canónico.
- **Autoridade:** `modules/11_HISTORIA_E_AUDITORIA_SPEC.md`; BT-03.
- **Dependências:** módulos com histórico próprio já existentes.
- **Proibições:** escrever em dados de outros módulos; tabela universal.

## U-19 — Boquilhas (registo central + caso 20→25)

- **Objetivo:** BQ completo na app nova: lotes/traces/movimentos/cálculos, discrepances, lifecycle, reparadores, histórico, painel lateral, UI final (sem tab Fabrico), **caso 20→25 em todas as camadas**.
- **Autoridade:** `modules/01_BOQUILHAS_SPEC.md`.
- **Dependências:** U-07..U-09; pode executar em paralelo com U-10..U-18 após Gate E.
- **Proibições:** `allow_unmatched`; bloquear retorno excedente; tab Fabrico.
- **Testes:** BQ-RULE-001..011 transpostos (regras confirmadas), 20→25 (GLM-TST-02.1), saldo transacional vs inventário, reopen/lifecycle.
- **Gate seguinte:** **Gate G** completo quando U-12..U-19 verdes.

## U-20 — Integração, E2E, a11y, visual regression, segurança

- **Objetivo:** suite E2E dos fluxos críticos; testes RLS/autorização em BD de teste; a11y global; visual regression cross-module; revisão de segurança (CSRF/XSS/headers).
- **Autoridade:** 09_TEST; Gate H.
- **Dependências:** todos os módulos.
- **Acceptance:** DoD global (09_TEST §7) verde; relatório objetivos de teste.

## U-21 — Migração/cutover ensaiados

- **Objetivo:** executar fases I1–I3 contra ambiente de teste com dry-run/reconciliação; ensaio de rollback; plano de cutover por módulo aprovado.
- **Autoridade:** 08_MIG; Gate I.
- **Proibições:** SQL live em produção sem aprovação; importação sem reconciliação.

## U-22 — Readiness final (deploy Render/Docker)

- **Objetivo:** preparação de produção **GitHub → Render → Docker build → Linux ASP.NET Core container
  → Supabase**; Dockerfile multi-stage; documentação operacional; checklist final. Substitui totalmente
  o antigo objetivo de publicação `win-x64 self-contained single-file` (SUPERSEDED).
- **Autoridade:** Gate J; 03_ARCH §13; 06_DATA §13.
- **Acceptance:** Dockerfile multi-stage (stage SDK `mcr.microsoft.com/dotnet/sdk:10.0` + stage
  runtime `mcr.microsoft.com/dotnet/aspnet:10.0`; publish `dotnet publish
  src/BA.Dmo.Web/BA.Dmo.Web.csproj -c Release -o /app/publish`); Linux container; port dinâmico
  bind `0.0.0.0` / respeito por `PORT` e environment; variáveis de ambiente; Supabase externo (sem
  PostgreSQL no container); sem instalação local; sem storage de PDFs no filesystem do Render;
  migrations como pre-deploy command (exit 0/≠0); bootstrap-admin one-shot CLI. Container registry
  não é requisito. Todos os gates A–I fechados; veredicto de produção do owner.
- **Proibições:** exigir container registry; exigir `--self-contained true`/`PublishSingleFile`/
  `win-x64` como requisito de produção.

---

## U-23 — Auditoria global (transversal)

- **Objetivo:** `audit_events` canónica (TD-19): emissão por todos os comandos relevantes na transação
  do domínio (outbox/correlação se assíncrono), catálogo versionado de `actionCode` por módulo,
  tab Auditoria no Admin (filtros ano/utilizador/módulo/ação/resultado/período; 20/40/60; detalhe
  factual; exportação anual com `audit.export`); sem pontuações/rankings; History Entry canónico.
- **Autoridade:** AUDITORIA_GLOBAL_HANDOFF.md (design); 02_DEC UD-17/TD-19; modules/11.
- **Dependências:** U-05/U-06 (Admin), U-08/09 (componentes). Implementado junto da fase D e
  estendido a cada módulo à medida que entra (U-10..U-19).
- **Proibições:** tabelas separadas por módulo/ano; registo de segredos/binários; pontuação/avaliação;
  eventos criados apenas no browser.
- **Testes:** emissão por comando (1 evento principal + correlação); imutabilidade; filtros anuais;
  permissões `audit.view`/`audit.export`; falha de auditoria aborta a transação quando síncrona.
- **Acceptance:** cada módulo ativo demonstra eventos no registo anual.
- **Gate seguinte:** integrado no gate D e verificado em todos os gates seguintes.

---

## Atribuição

Unidades U-10..U-12 e U-14..U-19 são **atribuíveis separadamente** a agentes/implementadores
diferentes após Gate E + U-13 (contexto Job On) + U-23 (auditoria), pois partilham apenas
foundation e contratos. U-01..U-09, U-13, U-23 e U-20..U-22 exigem sequência.
