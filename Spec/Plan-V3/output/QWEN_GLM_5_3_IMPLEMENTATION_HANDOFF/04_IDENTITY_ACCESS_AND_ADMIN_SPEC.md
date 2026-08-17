# 04 — IDENTITY, ACCESS AND ADMIN SPEC

Autoridade canónica para identidade, autorização e Administração. Referencia decisões UD-02,
UD-03, UD-04, UD-06, UD-10, TD-09, TD-10, TD-16 (`02_…`). Catálogo concreto em
`modules/00_MODULE_CATALOG.md`.

## 1. Identidade e sessão (GLM-ACC-01)

1. Autenticação: **Supabase Auth** (email/password). A aplicação nova nunca implementa autenticação própria.
2. Sessão web: cookie session ASP.NET Core criada após login com sucesso (cookie bridge).
3. Identidade interna: `auth_user_id` (uuid, referência lógica sem FK a `auth.users`) →
   `internal_users` (`actor_id`, `display_name`, `profile_title`, `template_id`, `active`) →
   `access_templates` (`template_id`, `name`, `active`, `modules jsonb`).
4. `profile_title` (título/função) é texto livre gerido pelo Administrador (ex.: Metrologia,
   Chefe). **Não concede permissões** (UD-02). Se vazio, o header mostra apenas o nome.
5. Grants resolvidos **por request** no servidor (com cache curta). Grants nunca são persistidos no cookie.
6. Utilizador sem registo interno ou `active=false` → erro `INTERNAL_USER_INACTIVE`; template
   inexistente/inativo → `ACCESS_TEMPLATE_INACTIVE`. Ambos resultam em estado seguro de sessão sem acesso.

## 2. Modelo de grants (GLM-ACC-02)

- Grant = entrada em `access_templates.modules`: `[{ moduleId, capabilities: [] }]`.
- A presença do módulo concede **entrada**; capabilities concedem operações específicas.
- Grant é **por template**; não existem overrides por utilizador na V1 (precedência: template único).
  Se futuramente forem necessários overrides, exigem spec própria com auditoria e precedência.
- `normalizeModules` server-side: descarta moduleIds/capabilities fora do catálogo; duplicados
  ignorados (primeiro prevalece); capabilities só valem com prefixo `{moduleId}.` (TD-10).

## 3. Catálogo controlado (GLM-ACC-03)

Catálogo canónico em `modules/00_MODULE_CATALOG.md`. Regras:

1. Fonte de verdade em código (`ModuleCatalog`); espelho DB apenas para ordem/visualização Admin.
2. A Administração atribui entradas existentes; **não** cria moduleIds/capabilities arbitrárias.
3. Validar no servidor em toda a escrita de template: moduleId existe; capability pertence ao módulo.
4. Ativo/inativo no catálogo não concede acesso por si; acesso vem da atribuição em templates ativos.

## 4. Enforcement em três níveis (GLM-ACC-04)

| Nível | Mecanismo | Regra |
|---|---|---|
| Frontend | Tabs derivadas de grants; guards de rota; comandos escondidos | Conveniência apenas — **não é segurança** |
| Backend | `[ModuleAuthorize("x")]` em todas as páginas/handlers + `HasCapabilityAsync` em cada caso de uso | Obrigatório em **toda** a operação |
| Dados | RLS ativado em todas as tabelas; browser sem acesso direto; role `ba_dmo_app` | Defesa em profundidade (06_DATA §6) |

## 5. Peso — separação Operador/Responsável (GLM-ACC-05)

Contrato autoritário (UD-06):

| Condição | Experiência |
|---|---|
| Módulo `peso` + capability `peso.aprovar` | **Responsável** — página de aprovações; nunca recebe a página do Operador |
| Módulo `peso` sem `peso.aprovar` | **Operador** — página de registo; nunca acede a rotas/comandos do Responsável |

1. Sem selector manual; a resolução é automática por identidade+capability (server-side).
2. Rotas separadas: `/peso` (Operador) e `/peso/responsavel`. Guards em ambos os sentidos:
   Operador em `/peso/responsavel` → redirect seguro para `/peso`; Responsável em `/peso` →
   redirect para `/peso/responsavel` (sem exposição cruzada).
3. Componentes/serviços internos podem ser reutilizados; a reutilização não concede acesso cruzado.
4. Comandos de decisão (`aprovar`, `nao_aprovar`, `reabrir`) validam `peso.aprovar` no caso de uso.
5. A Administração edita a atribuição que determina Operador vs Responsável (UD-06.8).
6. Testes negativos obrigatórios nos dois sentidos (09_TEST §4).

## 6. Matriz de acesso completa (GLM-ACC-06)

| Módulo/grupo | Capability | Tab/rota | Comandos permitidos | Guard FE | Guard BE | RLS | Audit |
|---|---|---|---|---|---|---|---|
| jobon | `jobon.view` (todos os utilizadores ativos) | Job On `/jobon` — **landing global** (UD-16) | Modo consulta: folha, planeamento, histórico | tabs+rotas | módulo/capability | ✅ | `audit_events` |
| jobon | `jobon.edit`, `jobon.configure` (papel/template técnico Responsável) | `/jobon` edição + Definições | criar, duplicar, Modo edição, substituir ferramentas, alterar campos/datas, guardar revisão, catálogos de opções | tabs+rotas+comandos | capabilities | ✅ | `audit_events` |
| jobon | `jobon.confirmar` (TD-20) | — | confirmar ocorrências de verificações (não concede edição estrutural) | comandos | capability | ✅ | `audit_events` |
| boquilhas | — | Boquilhas `/boquilhas` | lotes, movimentos, fecho, reopen, lifecycle, reparadores | tabs+rotas | `[ModuleAuthorize]` + casos de uso | ✅ | `audit_events` + domínio |
| peso (Operador) | — | Controlo→Peso `/peso` | criar/editar/submeter controlo e comparação no contexto do Job On; lotes/referências do Peso | tabs+rotas | módulo `peso` | ✅ | `audit_events` + domínio |
| peso (Responsável) | `peso.aprovar` | Controlo→Peso `/peso/responsavel` | aprovar/rejeitar/reabrir, decisões por CM, delete elegíveis | tabs+rotas+exclusividade | capability | ✅ | `audit_events` + domínio |
| pegamentos | — | Controlo→Pegamentos `/pegamentos` | folha com contexto obrigatório do Job On, medições, PDF local | tabs+rotas | módulo | ✅ | `audit_events` |
| ferramentas | — | Ferramentas `/ferramentas` | criar referência/lote, duplicar lote, ficha, regras de verificação | tabs+rotas | módulo | ✅ | `audit_events` |
| armazem | — | Armazém `/armazem` | entrada/saída, saídas programadas, correção localização | tabs+rotas | módulo | ✅ | `audit_events` |
| reparacao_interna | — | Reparação Interna `/reparacao-interna` | registar | tabs+rotas | módulo | ✅ | `audit_events` |
| reparacao_interna | `reparacao_interna.corrigir` | — | corrigir registo | comandos | capability | ✅ | `audit_events` |
| reparacao_externa | — | Reparação Externa `/reparacao-externa` — tabs Boquilhas, Contra Moldes, Moldes Finais, Envios, Histórico, Definições (UD-13) | listas programadas, acompanhamento, histórico | tabs+rotas | módulo | ✅ | `audit_events` |
| tampoes | — | Tampões `/tampoes` | quantidades, transformação, planeamento, opções | tabs+rotas | módulo | ✅ | `audit_events` |
| admin | `admin.gerir` | Administração `/admin` | users/templates CRUD, atribuição, reset password, aplicações; “Voltar ao Job On” | tabs+rotas | capability | ✅ | `audit_events` |
| admin | `audit.view` | Admin→tab Auditoria | consultar registo anual de eventos (todos os módulos) | tab | capability | ✅ | n/a (leitura) |
| admin | `audit.export` | Admin→tab Auditoria | exportar registo anual autorizado | comandos | capability | ✅ | evento próprio |
| historia | — | História `/historia` | consultas transversais **limitadas aos módulos autorizados do utilizador** (TD-24) | tabs+rotas | módulo + grants de origem | ✅ | n/a (leitura) |

**Controlo** é área/domínio funcional (UD-14): visível se o utilizador tiver pelo menos um grant
filho (`peso` e/ou `pegamentos`); dentro de Controlo só aparecem entradas autorizadas; sem áreas
vazias; Peso e Pegamentos são atribuíveis separavelmente e nunca fundem lógica
(UD-05/UD-14; detalhe em `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md` e `modules/02_CONTROLO_SPEC.md`).

## 7. Cenários obrigatórios (GLM-ACC-07)

| # | Cenário | Resultado esperado |
|---|---|---|
| 1 | Utilizador apenas Boquilhas | Tab Boquilhas; landing **Job On** (consulta); tudo o resto 403/redirect |
| 2 | Peso Operador sem `peso.aprovar` | Experiência Operador; `/peso/responsavel` bloqueado |
| 3 | Peso Responsável com `peso.aprovar` | Experiência Responsável; `/peso` redireciona para responsavel |
| 4 | Pegamentos sem Peso | Controlo visível só com entrada Pegamentos |
| 5 | Peso sem Pegamentos | Controlo visível só com entrada Peso |
| 6 | Peso + Pegamentos | Controlo com duas entradas; lógicas não fundidas |
| 7 | Admin sem módulos operacionais | Tab Administração; landing **Job On** (consulta) e Administração pela navegação (UD-16) |
| 8 | Admin com vários módulos | Administração + módulos; Admin não concede acesso funcional implícito |
| 9 | Utilizador sem grants válidos | Estado seguro “sem acesso”, sem dados, sem loop de redirects |
| 10 | Deep link não autorizado | Bloqueado server-side; redirect seguro para área autorizada + feedback |
| 11 | Capability removida durante sessão | Re-resolução na próxima navegação; saída imediata da área perdida; 403 no comando |
| 12 | Template desativado | Sessão autenticada mas sem acesso; estado seguro |
| 13 | Self-lockout do último Admin | Operação recusada server-side na mesma transação |
| 14 | Capability atribuída ao módulo errado | Rejeitada na validação server-side do template |
| 15 | Título/função visual sem capability | Nenhuma permissão concedida pelo título |
| 16 | Utilizador sem papel técnico Responsável tenta criar/editar/duplicar Job On ou entrar em Definições do Job On | `Editar folha`/`Criar Job On`/`Definições` ocultos **e** comandos bloqueados server-side (`jobon.edit`/`jobon.configure`); Modo consulta permanece disponível (UD-16) |
| 17 | Utilizador sem `audit.view` tenta consultar/exportar a tab Auditoria | 403; tab não renderizada; exportação igualmente protegida |
| 18 | Utilizador consulta História transversal | Vê apenas eventos/registos dos módulos que o seu template autoriza (TD-24) |

Cada cenário tem teste positivo e negativo (09_TEST §4).

## 8. Alteração de grants durante sessão (GLM-ACC-08)

1. Grants re-resolvidos por request; alteração reflete-se na próxima navegação/refresh (V1).
2. Se a área atual deixar de estar permitida: o próximo pedido devolve 403 → Shell redireciona para
   área autorizada com mensagem adequada; comandos falham com `Forbidden`.
3. Se o próprio template do administrador for alterado, a Shell força re-resolução; comportamento
   seguro = manter apenas áreas ainda autorizadas.
4. `INSTANT ACCESS REFRESH` (push) é `SAFE TO DEFER`.

## 9. Administração — operações (GLM-ACC-09)

Operações V1 (todas server-side, auditadas quando escrita):

**Utilizadores internos:** listar/pesquisar/ver detalhe; criar associação a `auth_user_id`
existente; criar utilizador Auth via Admin API (TD-16); editar `display_name`/`profile_title`;
definir template; ativar/desativar; iniciar reset de palavra-passe (confirmação explícita; nunca
mostrar/recuperar password atual; auditoria com admin, afetado, data/hora e resultado).

**Templates:** listar/criar/editar nome; ativar/desativar; associar módulos/capabilities contra o
catálogo; ver utilizadores associados. Desativar em vez de apagar (UD-10).

**Aplicações (catálogo):** ver módulos disponíveis e ordem; a edição da ordem/ativação do espelho
é permitida; criar identificadores novos é impossível sem alteração de código aprovada.

**Auditoria (tab — UD-17/TD-19):** consulta do registo anual de ações de todos os módulos com
filtros por ano, utilizador, módulo, ação, resultado e intervalo de datas; paginação 20/40/60;
um clique seleciona, duplo clique abre o detalhe factual do evento; exportação anual autorizada por
`audit.export`; sem pontuações, rankings ou avaliação automática. A navegação do Admin inclui
“Voltar ao Job On” (a landing global nunca é a Administração — UD-16).

## 10. Self-lockout (GLM-ACC-10)

Invariante: após qualquer escrita administrativa permanece **pelo menos um** utilizador interno
ativo com template ativo que conceda `admin.gerir`. Validação atómica na mesma transação.
Autoexclusão permitida se sobrar outro admin funcional. Mensagens de erro definidas em
`04_ADMIN_COMPLETE_SPEC.md` §11.

## 11. Auditoria (GLM-ACC-11)

Auditoria global `audit_events` (TD-19; contrato completo em `modules/11_HISTORIA_E_AUDITORIA_SPEC.md`):
tabela única canónica, append-only, escrita na mesma transação que a operação (backend autoritativo);
campos mínimos `occurredAtUtc/year, actorUserId, actorNameSnapshot, moduleId, actionCode, entityType,
entityId, entityLabelSnapshot, result, reason, correlationId, jobOnId, revisionId, before/afterSummary`.
As operações administrativas são eventos com `moduleId=admin` (create/update/activate/deactivate/
change_template de internal_user; create/update/activate/deactivate/update_modules de access_template;
password_reset_request). O histórico de negócio próprio de cada domínio mantém-se nos respetivos módulos.

## 12. Concorrência administrativa (GLM-ACC-12)

Optimistic concurrency com `updated_at` (BT-06). Segunda submissão concorrente rejeitada com
mensagem “Este registo foi alterado por outro administrador. Recarregue e tente novamente.”

## 13. Bootstrap do primeiro Admin (GLM-ACC-13)

Fluxo operacional (não migration): conta Supabase Auth real → bootstrap cria template mínimo
`admin.gerir` + internal_user ativo. Sem seeds operacionais nem utilizadores fictícios
(BACKEND-DECISION-19). Admin inicial **não** recebe peso/boquilhas automaticamente.
