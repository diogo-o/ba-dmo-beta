# 03 — TARGET MODULAR ARCHITECTURE

Autoridade: arquitetura técnica da aplicação nova. Base: `Spec/07_C_SHARP_SOLUTION_ARCHITECTURE.md`
(monólito modular, 6 projetos) + decisões UD-01..UD-05 e BT-01..BT-08
(`02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md`).

## 1. Forma geral (GLM-ARCH-01)

**Monólito modular** ASP.NET Core 10 (`net10.0`) com Razor Pages. Sem microserviços. 6 projetos:

| Projeto | Responsabilidade | Dependências |
|---|---|---|
| `BA.Dmo.Domain` | Entidades, value objects, serviços de domínio, invariantes, cálculos puros | Nenhuma |
| `BA.Dmo.Application` | Casos de uso (Commands/Queries), DTOs, portas, catálogo de módulos/capabilities | Domain |
| `BA.Dmo.Infrastructure` | Dapper/Npgsql, Supabase Auth adapter, audit, storage de documentos | Application, Domain |
| `BA.Dmo.Web` | Razor Pages, Shell, ViewModels, guards de rota, CSS/JS | Application, Infrastructure (DI) |
| `BA.Dmo.UnitTests` | Domain + Application | Domain, Application |
| `BA.Dmo.IntegrationTests` | Repositórios, RLS, fluxos Web | Infrastructure, Web |

Regra fundamental: **nenhuma regra de negócio, invariante ou autorização sensível vive na Web/UI.**

## 2. Organização por módulo (GLM-ARCH-02)

Separação lógica por pastas/namespaces (não há csproj por módulo):

```text
src/BA.Dmo.Domain/
├── Shared/Kernel/            (Entity, Result<T,E>, DomainError, IClock)
├── Shared/Access/            (ModuleCatalog, Capability, grants — leitura do catálogo)
└── Modules/
    ├── Boquilhas/  ├── Peso/  ├── Pegamentos/  ├── JobOn/
    ├── Ferramentas/ (CM/MF + registo de ferramentas)
    ├── Armazem/  ├── RepairInterna/  ├── RepairExterna/
    ├── Tampoes/  ├── Admin/  └── Historia/ (leitura)
src/BA.Dmo.Application/Modules/<mesmos nomes>/  (+ Ports/ partilhados)
src/BA.Dmo.Infrastructure/Repositories|Auth|Persistence|Storage
src/BA.Dmo.Web/Pages/<mesmos nomes>/ + Pages/Shared (Shell) + Pages/Auth
```

## 3. Shared kernel mínima (GLM-ARCH-03)

A foundation partilhada é a mínima necessária:

- `Result<T, Error>` próprio; categorias de erro uniformes (ValidationError, DomainConflict, NotFound, Unauthorized, Forbidden, ConcurrencyConflict, BackendUnavailable, Unexpected);
- `IClock`/`IDateTimeProvider`, `ICurrentUserAccessor` (identidade + grants resolvidos por request);
- `ModuleCatalog` (catálogo controlado — TD-10): módulos, capabilities, ordem canónica, tabs;
- Value objects transversais: `Line` (B1–C3), `Quantity`, `Periodo` (AAAANN), `RefCode`;
- Contrato de auditoria (`IAuditWriter`) e de histórico por domínio (BT-03);
- Portas de autorização (`IAuthorizationService.HasModuleAsync/HasCapabilityAsync`).

O que **não** pertence à foundation: lógica de qualquer módulo, saldos, cálculos de Peso,
regras de repair, UI de módulos.

## 4. Regras de dependência (GLM-ARCH-04)

| De \ Para | Domain | Application | Infrastructure | Web |
|---|---|---|---|---|
| Domain | — | ❌ | ❌ | ❌ |
| Application | ✅ | — | ❌ | ❌ |
| Infrastructure | ✅ | ✅ | — | ❌ |
| Web | ✅ (ViewModels) | ✅ | ✅ (apenas DI) | — |

Entre módulos:
1. **Proibido** um módulo importar Domain/Application de outro módulo diretamente;
2. comunicação apenas por (a) portas de leitura expostas pela Application do módulo dono e
   consumidas via contrato explícito (`IJobOnToolLookup`, `IPesoReferenceLookup`, …), ou
   (b) eventos de domínio publicados via contrato em `Application/Shared/Events`;
3. o módulo História/Auditoria consome apenas **projeções de leitura** dos módulos;
4. o ciclo de dependências entre módulos é proibido; o grafo permitido é:
   `Ferramentas → (Boquilhas? não)` — Ferramentas é dono dos registos CM/MF; Boquilhas é dono dos
   lotes BQ; Job On, Pegamentos, Repair*, Armazém e Tampões consomem lookups dos donos; Admin não
   conhece nenhum domínio funcional (ADMIN-DECISION-08).

Grafo de consumo (seta = “consulta contrato de”):

```text
JobOn → Ferramentas(CM/MF), Boquilhas, Armazem(lookup), Peso(lookup)
Pegamentos → Peso(CM/lotes), Boquilhas(lotes), Ferramentas(MF manual V1)
RepairInterna → JobOn(contexto ativo por linha), Ferramentas
RepairExterna → Ferramentas, Boquilhas, Armazem(execução física)
Armazem → Ferramentas/Boquilhas(identidade estável)
Historia → todos (read-only projections)
```

## 5. Ownership de dados (GLM-ARCH-05)

Cada tabela tem exatamente um módulo dono (06_DATA_BACKEND_AND_SECURITY_SPEC.md §3 detalha):

- Boquilhas: `bq_*` (lotes, traces, movimentos, discrepances, lifecycle);
- Peso: `peso_*` (referências, **lotes do Peso** com processo/máquinas permitidas/subpasta, controlos, comparações);
- Pegamentos: `pegamento_*` (registos ligados a `job_on_id`/revisão);
- Ferramentas: `tool_references`, `tool_lotes`, `physical_pieces`, `tool_check_rules/occurrences`;
- JobOn: família `job_on*` do JOB_ON_DATA_MODEL (job_on, job_on_revision, job_on_component,
  job_on_component_field, job_on_component_row, job_on_verification_occurrence, job_on_audit_event,
  job_on_field_option) — TD-18;
- Repair: `repairers`, `line_repairer_defaults`, `repair_exits(+items)` (BQ por quantidade; CM/MF por
  número — TD-22), `repair_events`, `internal_repair_records`;
- Armazém: `warehouse_locations/stock/movements`;
- Tampões: `tampao_*`; Admin/Shell: `internal_users`, `access_templates`, `module_catalog_mirror`;
- Auditoria global: `audit_events` — tabela única canónica transversal (TD-19), escrita por todos os
  módulos na transação do domínio; não pertence a um módulo funcional;
- Settings: `app_settings` (partilhado, escrita apenas pelo dono da definição).

Nenhum módulo escreve em tabelas de outro. Lookup read-only é permitido via contrato.

## 6. Contratos e eventos entre módulos (GLM-ARCH-06)

1. **Contratos de lookup** (síncronos): interfaces na Application do módulo dono; consumidores
   injetam a interface, nunca o repositório interno. Exemplos:
   - `IJobOnProductionContext` (JobOn → Peso/Pegamentos): Job On ativo por referência/produção/máquina
     com CM/lote exatos; bloqueio acionável quando falta ferramenta obrigatória (DS-04/DS-05);
   - `IActiveProductionLookup` (JobOn → Reparação interna/side panel BQ);
   - `IToolAvailabilityLookup` (composição read-only Armazém + domínio da ferramenta → edição do Job On);
   - `IPesoLoteLookup` (Peso → Job On): processo/máquinas permitidas do lote.
2. **Eventos de auditoria** (TD-19): cada comando relevante emite o evento `audit_events` na mesma
   transação do domínio (outbox/correlação se assíncrono). Consumidores apenas leem; nunca alteram o facto.
3. **Eventos de domínio** (append de facto): publicáveis dentro da mesma transação por handler
   simples em-process (sem broker). Ex.: `WarehouseExitConfirmed`, `RepairReturnRegistered`,
   `ControlApproved`. Consumidores apenas reagem com registos próprios; nunca alteram o facto de origem.
4. **Snapshots ≠ live**: qualquer valor copiado entre módulos é snapshot imutável; o estado atual é
   sempre consultado à fonte (MODELO_LOCKED princípio). Registos e documentos guardam o
   `job_on_revision_id` que consumiram (TD-18).

## 7. Composition root e registo de módulos (GLM-ARCH-07)

- `Program.cs` é o único composition root; regista serviços por módulo em blocos
  `AddBoquilhasModule()`, `AddPesoModule()`, … (extension methods por módulo).
- O catálogo de módulos em código (`ModuleCatalog`) lista moduleId, displayName, capabilities,
  ordem canónica e rota inicial; o espelho DB serve a UI Admin (TD-10).
- A Shell deriva as tabs do catálogo ∩ grants do utilizador; nenhuma navegação hardcoded por módulo.

## 8. Contrato de extensão — acrescentar um módulo (GLM-ARCH-08)

Para adicionar o módulo `X`:
1. Domain: `Modules/X` (entidades/serviços);
2. Application: `Modules/X` (commands/queries/ports) + entrada no `ModuleCatalog` (+ capabilities novas, se existirem);
3. Infrastructure: repositórios Dapper + migration nova na família fresh-build;
4. Web: `Pages/X` com guard `[ModuleAuthorize("x")]`;
5. Admin: a entrada do catálogo passa automaticamente a estar disponível para atribuição em templates;
6. Testes: positivos e negativos de acesso + unidade de domínio.
**Nenhuma alteração à Shell, navegação ou módulos existentes é necessária.**

## 9. Regras anti-acoplamento (GLM-ARCH-09)

- Proibido ficheiros globais com lógica de vários módulos; proibido UI monólito;
- Proibido estado partilhado mutável entre módulos fora dos contratos;
- Proibido `window.*` globals para dados (legacy); interação JS apenas behavior;
- Cada módulo tem: páginas, CSS de composição próprio (`modules/<x>-layout.css`), testes e
  migrations identificáveis;
- Revisão de dependências em cada gate: nenhum using cruzado entre `Modules/*` fora dos contratos.

## 10. Critério modular monolith vs processos separados (GLM-ARCH-10)

Monólito modular porque: volume operacional pequeno (posto de trabalho), um único backend Supabase,
sem requisito de escala independente, restrição de ambiente sem tooling local (BACKEND-DECISION-14).
Separação de processos só seria justificada por requisito externo comprovado — não existe.

## 11. Ambiente e restrições (GLM-ARCH-11)

Sem dependência de Docker, Node/npm, PostgreSQL local, Redis, DbUp CLI ou Supabase CLI no posto de
trabalho. O workstation é um thin client: instalação de app, .NET, Docker, Node e BD local são
**NONE**; acesso apenas por browser. Docker existe apenas no lado do deployment (Render/Linux).
Migrations = scripts SQL autoritativos; frontend sem bundlers (CSS/JS simples). Testes de
integração contra instância Supabase de teste (não Testcontainers).

## 12. Plano-V3 — Target framework e bootstrap build (GLM-ARCH-12)

Decisões técnicas do Plano-V3 (aprovadas pelo owner; `TECHNICAL BUILD CONTRACT` — ver
`11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md` e `02_DECISIONS…` §3.35).

Target único de construção:

```xml
<TargetFramework>net10.0</TargetFramework>
```

Base: **.NET 10 LTS · ASP.NET Core 10 · C# · Razor Pages**. `net8.0`/`net9.0`/“ASP.NET Core 8+”
deixam de ser o target principal; referências legacy a versões anteriores são histórico/provenance,
não contrato. Solution `BA-DMO.sln`; 6 projetos conforme §1. Bootstrap canónico (dotnet new
classlib/web/xunit … `-f net10.0`), restrições de projeto e comandos `dotnet restore/build/test`
em `10_MASTER_IMPLEMENTATION_ROADMAP.md` U-01 (build contract). Local run: `dotnet run --project
src/BA.Dmo.Web`; produção via Docker/Render, não publish Windows manual.

## 13. Deployment e ambiente (GLM-ARCH-13)

Deployment definitivo do owner:

```text
GitHub → Render → Docker build → Linux ASP.NET Core container → Supabase → browser
```

- **Workstation (thin client):** OS Windows; instalação de app/.NET/Docker/Node/BD = NONE; acesso
  web browser apenas.
- **Server:** host Render; execution Docker; OS container Linux; backend ASP.NET Core; database/auth
  Supabase (externa ao Render; PostgreSQL/Auth não correm no container; nenhuma BD local como
  source of truth).
- **Cross-platform managed .NET:** produção **sem** `win-x64`/self-contained Windows/
  `PublishSingleFile` como requisito; sem Windows Service; sem IIS; sem instalação local da aplicação.
- **Port:** dinâmico — bind a `0.0.0.0` e respeito por hosting environment/`PORT` (Render); não
  hardcode portas de produção (5000/5001/5159/7294); portas locais = safe implementer choice.
- **Dockerfile** no repositório final (multi-stage): stage SDK `mcr.microsoft.com/dotnet/sdk:10.0`
  (restore → build → publish) e stage runtime `mcr.microsoft.com/dotnet/aspnet:10.0`
  (`dotnet src/BA.Dmo.Web/BA.Dmo.Web.dll`); publish `dotnet publish src/BA.Dmo.Web/BA.Dmo.Web.csproj
  -c Release -o /app/publish`. Contrato documentado, **não implementado** nesta passagem.
- **Migrations:** forward-only; tabela `schema_migrations`; executor Npgsql full-script; CLI only
  (GLM-ARCH-15); nunca HTTP; sem execução automática no startup normal de produção
  (Render pre-deploy command; exit code 0 = sucesso / ≠0 = falha).

## 14. Supabase auth boundary (GLM-ARCH-14)

Application/Web **não** dependem diretamente de `supabase-csharp`, detalhes de REST ou service role.
Adotar `ISupabaseAuthAdapter` + `IAdminProvisioningAdapter` (ou nomes equivalentes coerentes com a
arquitetura; 06_DATA §14). A implementação concreta (community `supabase-csharp` **ou** REST direto)
é `SAFE IMPLEMENTER CHOICE BEHIND ADAPTER`; **não** descrever `supabase-csharp` como SDK oficial
first-party. **Service Role** aplica least privilege: apenas operações administrativas privilegiadas
(provisioning/bootstrap), nunca no request pipeline normal, nunca no browser; runtime normal usa
credenciais apropriadas + identidade/JWT/RLS.

## 15. Operational CLI (GLM-ARCH-15)

Sem 7.º projeto CLI separado. `BA.Dmo.Web` distingue por argumentos de processo:

```text
migrate            → dotnet BA.Dmo.Web.dll migrate
bootstrap-admin    → dotnet BA.Dmo.Web.dll bootstrap-admin
(omissão)          → normal web startup
```

O desenho interno exato é implementer detail, desde que não contamine Domain/Application. Migrations
e bootstrap admin são `CLI ONLY`; proibidos `/admin/migrations`, HTTP migration endpoint, HostedService
automático, setup page pública, anonymous admin e startup privileged bootstrap.

## 16. PDF architecture (GLM-ARCH-16)

Fechada no Plano-V3 (`FUNCTIONAL / BUSINESS CHANGES: 0`):

```text
C# backend → gerar PDF bytes em memória → HTTP binary/FileResult → Browser Blob →
File System Access API → filesystem do utilizador (Windows workstation)
```

- Backend Render **MUST NOT** escrever diretamente para filesystem do workstation; Render filesystem
  **MUST NOT** ser storage dos PDFs dos utilizadores.
- DB = source of truth dos dados estruturados; PDF = artefacto de exportação derivado.
- Contrato `IPdfRenderer` (ou equivalente coerente); library concreta = `IMPLEMENTATION DECISION`
  (não fixar QuestPDF obrigatório: licença pode exigir decisão comercial; alternativas existem).
  Se uma unidade futura exigir escolha concreta, verificar licença/compatibilidade Linux antes de
  escolher. Não bloquear U-01/U-02 por isto.

## 17. Browser filesystem boundary (GLM-ARCH-17)

- File System Access API autorizada **apenas** para `EXPORT OF PDF FILES`; proibido usar para domain
  persistence, offline database, business datastore ou source of truth.
- Secure context **HTTPS** (Render fornece em deployment normal); fallback = download padrão do
  browser quando API não suportada / autorização recusada / handle inválido / permission lost.
- **Exceção aprovada pelo owner (estritamente limitada):** IndexedDB pode persistir **apenas** o
  `FileSystemDirectoryHandle` (permission/state técnico da seleção do diretório local). Proibido
  guardar em IndexedDB dados de domínio, Boquilhas, Peso, Job On, Pegamentos, ferramentas, movimentos,
  histórico, formulários, pending business actions, offline business cache ou application state como
  source of truth. Classificação: `ALLOWED TECHNICAL HANDLE STORAGE ONLY` (substitui a proibição
  literal de IndexedDB handles onde necessário; a proibição de IndexedDB como datastore mantém-se).

## 18. Debug bootstrap (GLM-ARCH-18)

Fresh build: **NO DEBUG AUTH BYPASS IN PRODUCTION CODE**. Não transportar debug user impersonation,
anonymous admin, debug claims, fallback insecure identity nem temporary bootstrap hacks. Test auth
doubles permitidos apenas dentro dos projetos/test environment apropriados (projetos `tests/*`).
Bootsrtrap do primeiro Admin = one-shot CLI privilegiado (GLM-ARCH-15; `06_DATA` §15).
