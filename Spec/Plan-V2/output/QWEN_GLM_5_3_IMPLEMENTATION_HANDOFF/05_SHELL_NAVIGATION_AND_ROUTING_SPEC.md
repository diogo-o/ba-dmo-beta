# 05 — SHELL, NAVIGATION AND ROUTING SPEC

A Shell nova é **reconstruída** (C31): `Spec/05_SHELL_COMPLETE_SPEC.md` é evidência legacy BQ e não
é usado como autoridade. Fontes desta spec: decisões do utilizador (UD-03..UD-05), `06_UI_UX §6`,
`07_C_SHARP §8`, design contract §8 e `PORTAL_LOGIN_ADMIN_HANDOFF.md`.

## 1. Shell única (GLM-SHL-01)

1. Uma única Shell para toda a aplicação; um único login; **sem launcher central** e sem segunda
   aplicação por módulo (UD-03/UD-04).
2. Todos os módulos (Admin incluído) são módulos desta Shell.
3. Implementação: `_Layout.cshtml` + `_Header` + `_Navigation` + serviços `IShellService`/
   `INavigationService` (navegação não vive no markup).

## 2. Anatomia do frame (GLM-SHL-02)

```text
APP FRAME
├─ Global Header (76px desktop): logo BA (Spec/assets/ba-logo.png, sem deformação),
│   identidade do módulo/página atual, utilizador (nome + profile_title) + account trigger
├─ Module Navigation: tabs operacionais à esquerda; Administração/Definições à direita
└─ Work Area: sidebar contextual opcional (ex.: linhas BQ) + conteúdo da página
```

- O header identifica a área atual; o título da vista identifica a tarefa. Sem “role global” no header.
- O profile_title vem do perfil gerido na Administração; nunca concede permissões.
- Sidebars contextuais (painel de linhas do BQ) não substituem nem duplicam a navegação global.

## 3. Tabs derivadas de autorização (GLM-SHL-03)

1. Tabs = módulos autorizados ∩ catálogo, por ordem canónica do catálogo
   (`modules/00_MODULE_CATALOG.md`): **Job On** (presente para todos os utilizadores ativos —
   `jobon.view`), Boquilhas, Controlo(Peso/Pegamentos), Ferramentas, Armazém, Reparação Interna,
   Reparação Externa, Tampões, História; Administração/Definições à direita.
2. **Controlo** (área/domínio funcional — UD-14): entrada visível quando existe pelo menos um grant
   filho; dentro do grupo, apenas entradas para Peso e/ou Pegamentos autorizados; nunca área vazia
   nem entrada não autorizada (UD-05).
3. Peso apresenta **uma** entrada no grupo, cuja experiência (Operador/Responsável) é resolvida por
   `peso.aprovar` (GLM-ACC-05) — sem selector manual.
4. Zero tabs operacionais próprias: mesmo assim o utilizador vê Job On (consulta); se nem `jobon.view`
   existir (utilizador inativo/sem identidade), estado seguro “sem acesso” (sem dados, sem loop).
5. Múltiplas tabs: ordem canónica + overflow responsivo.
6. Estado ativo consistente; tabs não executam comandos; tabs não autorizadas não são renderizadas.

## 4. Landing determinístico (GLM-SHL-04)

Política atual (UD-16, design consolidado): após autenticação, **todos os utilizadores** (Operador,
Responsável, Administrador e restantes) são encaminhados para **Job On**. A landing não é
configurável por utilizador/template; Administração acede-se pela navegação. Job On abre em
`Modo consulta` para todos; `Editar folha`, `Criar Job On` e `Definições` apenas para o papel/template
técnico Responsável (`jobon.edit`/`jobon.configure` — TD-20). Sem launcher central.
`preferred_first_page`/`initial_module_id` permanecem campos read-only não usados na V1.
(Substitui a política anterior “Boquilhas como landing” — registado em 02_DEC §2/§7.1 DS-01.)

## 5. Rotas e deep links (GLM-SHL-05)

| Rota | Conteúdo | Guard |
|---|---|---|
| `/login`, `/logout` | Auth | público / sessão |
| `/` | redirect para landing | sessão |
| `/boquilhas` | Boquilhas (tabs internas: Registo, Boquilhas, Histórico; Definições à direita) | módulo |
| `/peso`, `/peso/responsavel` | Peso Operador / Responsável | módulo + exclusividade |
| `/pegamentos` | Pegamentos | módulo |
| `/jobon` (+ `/jobon/{id}`) | Job On (landing global) | `jobon.view`; edição/configuração exigem `jobon.edit`/`jobon.configure` |
| `/ferramentas` (+ ficha) | Ferramentas CM/MF | módulo |
| `/armazem` | Armazém | módulo |
| `/reparacao-interna`, `/reparacao-externa` | Reparação | módulo |
| `/tampoes` | Tampões | módulo |
| `/admin` (users/templates) | Administração | `admin.gerir` |
| `/historia` | História transversal | módulo |
| `/access-denied`, `/no-access` | Estados seguros | sessão |

Deep links:
1. URL não autorizado → 403 server-side → redirect seguro para área autorizada + feedback adequado;
2. registo específico (ex.: `/boquilhas?lote={id}`) abre o contexto se autorizado;
3. nenhuma rota reconstrói autorização a partir de parâmetros de URL (legacy `sessionRole` proibido);
4. URL strategy: rotas estáveis por página; estado de filtros em query string (não em paths dinâmicos gerados).

## 6. Estados da Shell (GLM-SHL-06)

| Estado | Comportamento |
|---|---|
| Sessão expirada | redirect para login, seguro |
| Grants alterados durante sessão | re-resolução; saída imediata de área perdida (GLM-ACC-08) |
| Utilizador sem módulos | `/no-access`: mensagem, logout disponível; sem dados |
| Erro de resolução de identidade | página de erro com retry; sem unhandled rejection (gap Peso legacy corrigido) |
| Refresh | estado de navegação preservado; filtros por query string |

## 7. Profile e logout (GLM-SHL-07)

- Account menu no indicador do utilizador: nome, profile_title, Logout.
- Logout termina sessão Supabase + cookie e redireciona para login.
- Apresentação consistente do perfil em todos os módulos (carregado uma vez pela Shell).

## 8. Responsive e layout (GLM-SHL-08)

- Breakpoints canónicos 1200 / 980 / 720 + mobile estreito; conteúdo fluido com gutters canónicos
  (07_DESIGN §3); sem scroll horizontal de página.
- Tabs em overflow/menu compacto abaixo de 980px; sidebars contextuais viram drawer em mobile.
- Header compacto em mobile; identidade do módulo e utilizador sempre visíveis.

## 9. O que a Shell não faz (GLM-SHL-09)

Não conhece domínio de módulos; não calcula saldos nem estados; não guarda grants no cliente; não
apresenta dados de módulos não autorizados; não decide landing por heurística (política fixa §4).
