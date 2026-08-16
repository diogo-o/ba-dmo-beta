# 03 — TARGET MODULAR ARCHITECTURE

Autoridade: arquitetura técnica da aplicação nova. Base: `Spec/07_C_SHARP_SOLUTION_ARCHITECTURE.md`
(monólito modular, 6 projetos) + decisões UD-01..UD-05 e BT-01..BT-08
(`02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md`).

## 1. Forma geral (GLM-ARCH-01)

**Monólito modular** ASP.NET Core 8+ com Razor Pages. Sem microserviços. 6 projetos:

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
trabalho. Migrations = scripts SQL autoritativos; frontend sem bundlers (CSS/JS simples). Testes de
integração contra instância Supabase de teste (não Testcontainers).
