# PROMPT — EXECUÇÃO PLAN-V3 (provenance)

> **Natureza:** ficheiro de **provenance da execução Plan-V3**, não um documento canónico de spec.
> O package canónico tem **26** documentos; este é o ficheiro adicional de provenance (1).
>
> **Prompt preservado literalmente** tal como foi recebido do owner nesta execução.

---

# PLAN-V3 — APPLY FINAL TECHNICAL READINESS PATCH

Quero que cries agora oficialmente o **Plan-V3** do pacote BA DMO.

Esta execução NÃO é uma nova fase de descoberta.

As decisões técnicas abaixo já foram revistas e aprovadas pelo owner.

O teu trabalho agora é:

1. aplicar o patch técnico mínimo;
2. propagar as decisões por todos os documentos afetados;
3. eliminar contradições antigas;
4. preservar integralmente as regras funcionais e de negócio;
5. validar o pacote;
6. guardar o prompt desta execução;
7. parar.

Não voltes a discutir opções já fechadas.

Não implementes a aplicação.

---

# 1. WORKSPACE

Workspace:

```text
D:\BA-QWEN-MAX-PRODUCTION
```

Pacote atual:

```text
D:\BA-QWEN-MAX-PRODUCTION\plans\QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF\
```

Design baseline:

```text
D:\BA-QWEN-MAX-PRODUCTION\portal-dmo-design-final\
```

O pacote atual corresponde ao **Plan-V2**.

O objetivo desta execução é transformar o pacote atual na versão técnica **Plan-V3**, mantendo a mesma estrutura de 26 ficheiros.

---

# 2. REGRA FUNDAMENTAL — V3 É PATCH TÉCNICO

Plan-V3 é apenas um:

```text
TECHNICAL BUILD READINESS PATCH
```

Não é:

* redesign;
* nova spec funcional;
* revisão de negócio;
* nova arquitetura de domínio;
* reinterpretação de módulos;
* implementação.

Regra obrigatória:

```text
FUNCTIONAL / BUSINESS CHANGES: 0
```

Se encontrares necessidade de alterar comportamento funcional para aplicar este patch:

```text
STOP AND REPORT BLOCKER
```

Não inventar.

---

# 3. DECISÕES APROVADAS — AUTORIDADE DESTA EXECUÇÃO

As seguintes decisões estão fechadas e substituem decisões técnicas antigas incompatíveis.

---

## 3.1 Target Framework

Target obrigatório:

```text
<TargetFramework>net10.0</TargetFramework>
```

Base:

```text
.NET 10 LTS
ASP.NET Core 10
C#
Razor Pages
```

Não manter:

```text
ASP.NET Core 8+
net8.0
net9.0
```

como target principal.

---

# 3.2 Arquitetura

Manter:

```text
Modular Monolith
```

Solution:

```text
BA-DMO.sln
```

Projetos:

```text
src/
  BA.Dmo.Domain/
  BA.Dmo.Application/
  BA.Dmo.Infrastructure/
  BA.Dmo.Web/

tests/
  BA.Dmo.UnitTests/
  BA.Dmo.IntegrationTests/
```

Responsabilidades existentes mantêm-se.

Não criar microservices.

Não criar SPA.

Não criar sétimo projeto CLI sem necessidade.

---

# 3.3 Deployment definitivo

A decisão atual do owner é:

```text
GitHub
→ Render
→ Docker build
→ Linux ASP.NET Core container
→ Supabase
→ browser
```

Isto substitui qualquer deployment legacy Windows.

---

## Workstation

```text
OS:
Windows

Application installation:
NONE

.NET installation:
NONE

Docker installation:
NONE

Node/npm installation:
NONE

Database installation:
NONE

Access:
Web browser only
```

O workstation é um thin client.

---

## Server

```text
Host:
Render

Execution:
Docker

OS:
Linux container

Backend:
ASP.NET Core

Database/Auth:
Supabase
```

---

# 3.4 SUPERSEDE legacy Windows deployment

Qualquer referência atual a:

```text
win-x64
self-contained Windows executable
PublishSingleFile as production requirement
Windows Service
IIS requirement
local backend installation
application installed on workstation
```

deve ser identificada e corrigida.

Classificação:

```text
SUPERSEDED BY CURRENT OWNER DEPLOYMENT DECISION
```

Não apagar provenance relevante.

Mas o contrato canónico V3 não pode continuar contraditório.

---

# 3.5 Docker

O repositório final da aplicação deve conter:

```text
Dockerfile
```

Contrato:

```text
multi-stage Docker build
```

Conceptualmente:

```text
.NET 10 SDK stage
→ restore
→ build
→ publish

.NET 10 ASP.NET runtime stage
→ published application
→ dotnet BA.Dmo.Web.dll
```

Imagens alvo conceptuais:

```text
mcr.microsoft.com/dotnet/sdk:10.0
mcr.microsoft.com/dotnet/aspnet:10.0
```

Não implementar Dockerfile agora.

Documentar contrato para o GLM.

---

# 3.6 RuntimeIdentifier

Produção:

```text
NO FIXED WINDOWS RUNTIMEIDENTIFIER
```

A aplicação deve ser cross-platform managed .NET.

Não fixar:

```text
win-x64
```

como target de produção.

---

# 3.7 Production publish

Contrato esperado dentro do Docker build stage:

```text
dotnet publish src/BA.Dmo.Web/BA.Dmo.Web.csproj -c Release -o /app/publish
```

Não tornar:

```text
--self-contained true
PublishSingleFile
win-x64
```

requisitos de produção.

---

# 3.8 Port model

Produção:

```text
Dynamic Render port configuration
```

A aplicação deve:

```text
bind to 0.0.0.0
respect hosting environment / PORT configuration
```

Não hardcode:

```text
5000
5001
5159
7294
```

em produção.

Ports locais:

```text
SAFE IMPLEMENTER CHOICE
```

---

# 3.9 Supabase

Manter:

```text
Supabase PostgreSQL
Supabase Auth
```

Database externa ao Render.

Não executar PostgreSQL no container.

Não usar DB local como source of truth.

---

# 3.10 Supabase Auth boundary

Contrato obrigatório:

```text
ISupabaseAuthAdapter
IAdminProvisioningAdapter
```

ou nomes equivalentes já coerentes com a arquitetura.

Application/Web não podem depender diretamente de:

```text
supabase-csharp
REST implementation details
service role
```

Implementação concreta:

```text
community supabase-csharp
OR
direct REST
```

é:

```text
SAFE IMPLEMENTER CHOICE BEHIND ADAPTER
```

Não descrever `supabase-csharp` como SDK oficial first-party.

---

# 3.11 Service Role

Aplicar least privilege.

```text
Service Role
```

é apenas para operações administrativas privilegiadas, como provisioning/bootstrap.

Não deve ser usada no request pipeline normal.

Não deve chegar ao browser.

Runtime normal usa credenciais apropriadas + user identity/JWT/RLS conforme contrato existente.

---

# 3.12 Migration implementation

Implementar futuramente via:

```text
Npgsql
```

Custom migration runner mínimo.

Cada migration `.sql` deve ser:

1. lida integralmente;
2. ter SHA-256 calculado;
3. comparada com `schema_migrations`;
4. enviada integralmente ao PostgreSQL;
5. executada sem parser SQL C# artesanal;
6. registada apenas após sucesso.

Proibido:

```text
split(';')
custom SQL parser
EF Core Migrations
```

---

# 3.13 Migration tracking

Contrato mínimo:

```text
schema_migrations
```

Com:

```text
version / id
filename
sha256
applied_at
```

Podes manter outros campos já aprovados, como execution time, se não criarem contradição.

---

# 3.14 Migration execution

Interface:

```text
CLI ONLY
```

Exemplo:

```text
dotnet BA.Dmo.Web.dll migrate
```

ou equivalente durante development:

```text
dotnet run --project src/BA.Dmo.Web -- migrate
```

Não criar:

```text
/admin/migrations
HTTP migration endpoint
```

Não correr migrations automaticamente no startup normal de Production.

---

# 3.15 Render migration lifecycle

Contrato de produção:

```text
GitHub push
→ Render build
→ pre-deploy migration command
→ success = continue deployment
→ failure = abort deployment
→ start Web service
```

Migration CLI:

```text
exit code 0 = success
exit code != 0 = failure
```

Documentar esta intenção.

Não inventar container registry como requisito.

O modelo normal é:

```text
GitHub
→ Render builds Dockerfile
```

---

# 3.16 Initial Admin bootstrap

Decisão fechada:

```text
ONE-SHOT CLI
```

Exemplo:

```text
dotnet BA.Dmo.Web.dll bootstrap-admin
```

Não usar:

```text
HostedService automatic bootstrap
public setup page
anonymous admin
startup privileged bootstrap
```

O comando deve ser:

* explícito;
* idempotente;
* auditável;
* privilegiado;
* executado apenas quando necessário.

Pode usar environment variables temporárias / Render operational environment.

Service Role deve existir apenas durante operação privilegiada apropriada.

---

# 3.17 Operational CLI

Não criar sétimo projeto.

O `BA.Dmo.Web` pode distinguir:

```text
migrate
bootstrap-admin
normal web startup
```

por argumentos de processo.

O desenho interno exato é implementer detail, desde que não contamine domínio/Application.

---

# 3.18 PDF architecture

A arquitetura está fechada:

```text
C# backend
→ generate PDF bytes in memory
→ HTTP binary/FileResult
→ Browser Blob
→ File System Access API
→ user's Windows filesystem
```

Backend Render:

```text
MUST NOT
```

escrever diretamente para filesystem do workstation.

Render filesystem:

```text
MUST NOT
```

ser usado como storage dos PDFs dos utilizadores.

DB:

```text
source of truth for structured data
```

PDF:

```text
export artifact
```

---

# 3.19 PDF renderer

Contrato:

```text
IPdfRenderer
```

ou equivalente coerente.

Library concreta:

```text
IMPLEMENTATION DECISION
```

Não fixar QuestPDF como obrigatório.

Razão:

* licença pode exigir decisão comercial;
* alternativas podem existir;
* arquitetura não depende da library.

O GLM deve implementar por abstraction.

Se uma unidade futura exigir escolha concreta, deve verificar licença/compatibilidade Linux antes.

Não bloquear U-01/U-02 por causa disto.

---

# 3.20 File System Access API

Utilização autorizada:

```text
EXPORT OF PDF FILES ONLY
```

Proibido usar para:

```text
domain persistence
offline database
business datastore
source of truth
```

---

# 3.21 IndexedDB exception — OWNER APPROVED

O owner autorizou uma exceção estritamente limitada.

IndexedDB pode ser usado:

```text
ONLY
```

para persistir:

```text
FileSystemDirectoryHandle
technical permission/state required for local directory selection
```

Não pode guardar:

* dados de domínio;
* Boquilhas;
* Peso;
* Job On;
* Pegamentos;
* ferramentas;
* movimentos;
* histórico;
* formulários;
* pending business actions;
* offline business cache;
* application state as source of truth.

Classificação:

```text
ALLOWED TECHNICAL HANDLE STORAGE ONLY
```

Isto substitui apenas a proibição literal de IndexedDB handles onde necessário.

A proibição de IndexedDB como datastore continua intacta.

---

# 3.22 Browser support

Contrato:

```text
File System Access API when supported
```

Secure context:

```text
HTTPS
```

Render fornece HTTPS no deployment normal.

Fallback:

```text
standard browser file download
```

quando:

* API não suportada;
* autorização recusada;
* handle inválido;
* permission lost.

---

# 3.23 Debug bootstrap

Fresh build:

```text
NO DEBUG AUTH BYPASS IN PRODUCTION CODE
```

Não transportar:

* debug user impersonation;
* anonymous admin;
* debug claims;
* fallback insecure identity;
* old temporary bootstrap hacks.

Test auth doubles são permitidos apenas dentro dos projetos/test environment apropriados.

---

# 4. BUILD BOOTSTRAP CONTRACT

Confirma e documenta comandos canónicos equivalentes a:

```bash
dotnet new sln -n BA-DMO

dotnet new classlib -n BA.Dmo.Domain -o src/BA.Dmo.Domain -f net10.0
dotnet new classlib -n BA.Dmo.Application -o src/BA.Dmo.Application -f net10.0
dotnet new classlib -n BA.Dmo.Infrastructure -o src/BA.Dmo.Infrastructure -f net10.0
dotnet new web -n BA.Dmo.Web -o src/BA.Dmo.Web -f net10.0

dotnet new xunit -n BA.Dmo.UnitTests -o tests/BA.Dmo.UnitTests -f net10.0
dotnet new xunit -n BA.Dmo.IntegrationTests -o tests/BA.Dmo.IntegrationTests -f net10.0
```

Adicionar projetos à solution.

Project references:

```text
Application → Domain

Infrastructure → Application + Domain

Web → Application + Infrastructure

UnitTests → Domain + Application

IntegrationTests → Web + Infrastructure
```

Depois:

```text
dotnet restore
dotnet build
dotnet test
```

Local run:

```text
dotnet run --project src/BA.Dmo.Web
```

Production deploy é via Docker/Render, não publish Windows manual.

---

# 5. DOCUMENTOS A REVER

Percorre todos os 26 ficheiros.

Não assumes que apenas quatro precisam de alteração.

Procura referências afetadas em:

```text
00_START_HERE.md
01_SOURCE_AUTHORITY_REGISTER.md
02_DECISIONS_CONTRADICTIONS_AND_OPEN_QUESTIONS.md
03_TARGET_MODULAR_ARCHITECTURE.md
04_IDENTITY_ACCESS_AND_ADMIN_SPEC.md
05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md
06_DATA_BACKEND_AND_SECURITY_SPEC.md
07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md
08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md
09_TEST_QUALITY_AND_ACCEPTANCE_SPEC.md
10_MASTER_IMPLEMENTATION_ROADMAP.md
11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md
12_REQUIREMENT_TRACEABILITY_MATRIX.md
13_HANDOFF_MANIFEST.md
modules/*
```

Alterar apenas os ficheiros materialmente afetados.

---

# 6. EXPECTED HIGH-PRIORITY PATCH LOCATIONS

No mínimo verificar:

---

## `03_TARGET_MODULAR_ARCHITECTURE.md`

Propagar:

* `net10.0`;
* Render/Linux Docker;
* cross-platform;
* no win-x64 target;
* operational CLI;
* Dockerfile contract;
* migration runner;
* Supabase adapter boundaries.

---

## `06_DATA_BACKEND_AND_SECURITY_SPEC.md`

Propagar:

* `schema_migrations`;
* migration role;
* least privilege;
* service role isolation;
* migration CLI;
* PDFs in-memory/backend;
* Render filesystem not user storage;
* browser-only local filesystem.

---

## `07_DESIGN_SYSTEM_AND_COMPONENT_ARCHITECTURE.md`

Propagar apenas se necessário:

* Local Directory Selector;
* File System Access is client/browser-only;
* HTTPS;
* fallback download;
* IndexedDB exception only for technical directory handle.

Não alterar visual/design.

---

## `08_MIGRATION_CUTOVER_AND_ROLLBACK_SPEC.md`

Confirmar coerência com:

```text
forward-only migration model
CLI/pre-deploy execution
backup rollback
```

---

## `09_TEST_QUALITY_AND_ACCEPTANCE_SPEC.md`

Adicionar apenas testes necessários ao novo technical contract, se faltarem:

* migration checksum/idempotency;
* migration failure;
* CLI routing;
* no production debug bypass;
* PDF abstraction;
* directory handle exception boundary.

Não aumentar artificialmente scope.

---

## `10_MASTER_IMPLEMENTATION_ROADMAP.md`

Atualizar U-01/U-02 e qualquer outra unidade afetada.

Especialmente:

### U-01

Deve tornar possível criar a solution de raiz com:

```text
net10.0
6 projects
project references
build/test
```

### U-02

Deve incluir:

```text
SQL migrations
schema_migrations
Npgsql full-script runner
CLI migrate
```

### U-22

Substituir completamente o antigo:

```text
win-x64 self-contained single-file
```

por algo equivalente a:

```text
GitHub → Render Docker build → pre-deploy migrations → Web Service deployment
```

Acceptance deve cobrir:

* Dockerfile;
* Linux container;
* dynamic port;
* environment variables;
* Supabase external;
* no local installation;
* no Render filesystem PDF storage.

Não exigir container registry.

---

## `11_GLM_5_3_MASTER_IMPLEMENTATION_PROMPT.md`

Este é crítico.

O GLM não deve ter de deduzir estas decisões lendo vários ficheiros.

Adicionar uma secção explícita, por exemplo:

```text
TECHNICAL BUILD CONTRACT
```

contendo:

* net10.0;
* ASP.NET Core;
* Razor Pages;
* Modular Monolith;
* project structure;
* Dapper/Npgsql;
* Supabase;
* Render/Linux Docker;
* Dockerfile;
* migrations CLI;
* admin bootstrap CLI;
* PDF architecture;
* File System Access export-only;
* IndexedDB technical handle exception;
* no production debug bypass;
* no Windows production executable;
* browser-only workstations.

---

## `12_REQUIREMENT_TRACEABILITY_MATRIX.md`

Propagar as novas decisões técnicas para que exista cadeia:

```text
owner decision
→ architecture/data/design
→ roadmap
→ tests
→ GLM prompt
```

---

## `13_HANDOFF_MANIFEST.md`

Atualizar:

* Plan-V3 status;
* modified files;
* package consistency;
* technical readiness state.

---

# 7. PROMPT PRESERVATION — OBRIGATÓRIO

Nesta execução quero corrigir o problema histórico de:

```text
Prompt preserved: NO
```

Guarda o prompt exato desta execução num ficheiro:

```text
PROMPT.md
```

dentro do package:

```text
D:\BA-QWEN-MAX-PRODUCTION\plans\QWEN_GLM_5_3_IMPLEMENTATION_HANDOFF\PROMPT.md
```

IMPORTANTE:

O pacote tinha historicamente 26 ficheiros.

Como `PROMPT.md` é provenance da execução Plan-V3:

* não o tratar como um dos 26 ficheiros canónicos de spec;
* declarar claramente que é um ficheiro adicional de provenance;
* não alterar a contagem histórica dos 26 documentos canónicos;
* manifest deve separar:

```text
Canonical package documents: 26
Execution provenance files: 1
```

Se por qualquer razão não conseguires preservar literalmente o prompt recebido, NÃO o reconstruas.

Nesse caso reporta:

```text
PROMPT PRESERVATION FAILED
```

Mas tenta preservá-lo exatamente.

---

# 8. NÃO MEXER NO DESIGN BASELINE

Não alterar:

```text
D:\BA-QWEN-MAX-PRODUCTION\portal-dmo-design-final\
```

Design baseline é read-only.

---

# 9. NÃO IMPLEMENTAR

Não criar:

* `.cs`;
* `.csproj`;
* Dockerfile real;
* migrations SQL reais adicionais;
* Razor Pages;
* JS;
* CSS;
* Supabase project;
* Render service;
* deployment.

Esta execução é exclusivamente documentação/spec readiness.

---

# 10. CONSISTENCY PASS

Depois de aplicar as alterações:

faz uma única passagem de consistência completa.

Procurar:

```text
net8
net9
8+
win-x64
PublishSingleFile
Windows service
IIS
local installation
DbUp
/admin/migrations
HostedService bootstrap
QuestPDF mandatory
IndexedDB prohibited
Render filesystem PDF
hardcoded production ports
```

Cada ocorrência deve ser classificada:

```text
VALID HISTORICAL PROVENANCE
VALID CURRENT CONTRACT
SUPERSEDED REFERENCE
ERROR
```

Corrigir apenas ERROR ou referências canónicas superseded.

---

# 11. OPEN QUESTION POLICY

Não reabrir as 14 questões já classificadas:

```text
SAFE TO DEFER
```

salvo se este patch técnico entrar diretamente em conflito com uma delas.

Não fazer nova investigação funcional.

---

# 12. GLM BUILD READINESS CHECK

No final verifica:

### Can GLM create the solution?

YES/NO

### Can GLM know exact TargetFramework?

YES/NO

### Can GLM know project structure?

YES/NO

### Can GLM know DB technology?

YES/NO

### Can GLM know Auth boundary?

YES/NO

### Can GLM know migration implementation?

YES/NO

### Can GLM know migration execution?

YES/NO

### Can GLM know deployment target?

YES/NO

### Can GLM know Docker requirement?

YES/NO

### Can GLM know PDF ownership?

YES/NO

### Can GLM know browser filesystem boundary?

YES/NO

### Can GLM know first Admin bootstrap method?

YES/NO

### Can GLM execute U-01 without material inference?

YES/NO

### Can GLM execute U-02 without material inference?

YES/NO

---

# 13. DO NOT SPEND TIME ON COSMETIC REWRITES

We have a limited execution budget.

Do not:

* rewrite prose just for style;
* restructure modules unnecessarily;
* reformat every table;
* rename established IDs;
* regenerate specs from scratch;
* repeat legacy discovery already completed.

Apply only changes necessary for Plan-V3 technical readiness.

---

# 14. OUTPUT EXPECTED

At the end provide a concise report.

Format:

```text
PLAN-V3 TECHNICAL READINESS PATCH

Canonical documents:
26

Prompt provenance file:
YES / NO

PROMPT.md path:
...

Files modified:
<number>

Modified files:
- ...

Functional/business changes:
0 / NOT 0

Target Framework:
net10.0

Deployment:
GitHub → Render → Docker/Linux → Supabase

Windows production deployment removed:
YES / NO

Migration model:
Npgsql full-script + schema_migrations + CLI/pre-deploy

HTTP migration endpoint:
NO

Initial Admin:
one-shot bootstrap-admin CLI

PDF architecture:
C# → HTTP Blob → Browser File System Access API

PDF library fixed:
NO

IndexedDB:
technical directory-handle exception only

Cross-file contradictions remaining:
<number>

Broken references:
<number>

Open technical blockers:
<number>

Open business blockers:
<number>

U-01 executable without material inference:
YES / NO

U-02 executable without material inference:
YES / NO

GLM MASTER PROMPT SELF-SUFFICIENT:
YES / NO

PLAN-V3 FUNCTIONAL CHANGES:
0 / NOT 0

TECHNICAL READINESS PATCH APPLIED:
YES / NO

CROSS-FILE CONSISTENCY:
PASS / FAIL

GLM 5.3 BUILD READINESS:
YES / NO

NO APPLICATION CODE IMPLEMENTED:
YES / NO

DESIGN BASELINE MODIFIED:
NO / YES
```

---

# 15. FINAL GATE

Só declarar sucesso se puderes responder:

```text
PLAN-V3 FUNCTIONAL SPEC COMPLETE:
YES

PLAN-V3 TECHNICAL BUILD CONTRACT COMPLETE:
YES

PLAN-V3 READY FOR GLM 5.3 IMPLEMENTATION WITHOUT MATERIAL INFERENCE:
YES

PLAN-V4 READINESS PATCH REQUIRED:
NO
```

Se algum destes não puder ser `YES`, não maquilhar.

Reportar exatamente o blocker restante.

---

# FINAL INSTRUCTION

Executa agora o patch Plan-V3 completo numa única passagem.

Não pedir confirmação adicional para decisões já explicitamente fechadas neste prompt.

Não voltar a propor alternativas para essas decisões.

Aplicar → propagar → validar → guardar PROMPT.md → reportar → parar.
