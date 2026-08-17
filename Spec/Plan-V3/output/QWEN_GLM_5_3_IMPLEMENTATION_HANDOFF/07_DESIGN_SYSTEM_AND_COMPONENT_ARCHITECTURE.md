# 07 — DESIGN SYSTEM AND COMPONENT ARCHITECTURE SPEC

Autoridade visual e de componentes da aplicação nova. Fontes: `portal-dmo-design-final/docs/
DMO_DESIGN_SYSTEM.md` (v2.7, normativa), `DESIGN_IMPLEMENTATION_CONTRACT.md` (auditoria),
`CODER_IMPLEMENTATION_HANDOFF.md` §3–7, briefs por módulo. Este documento converte os gaps P0/P1
do contrato em trabalho explícito de foundation **antes** dos módulos (contrato §20).

## 1. Decisões P0 fechadas (GLM-DSN-01)

| Gap P0 | Decisão | Justificação |
|---|---|---|
| 1. Tokens de tipografia/line-height/letter-spacing | Escala exata: `xs 12 / sm 13 / md 14 / lg 17 / xl 22 px` (corpo 14; botão 12–13; label 11; cabeçalho de tabela 10–11; títulos 15–24 conforme papel); line-heights: corpo 1.45–1.48, títulos 1.2, label/botão 1.2; letter-spacing `0.04em` para uppercase/table headers; pesos limitados a 400/600/700/800 | DMO_DESIGN_SYSTEM v2.7 §5 + 06_UI_UX §4 (valores confirmados) |
| 2. Escala z-index/layers | `--z-base:0 · sticky:100 · dropdown:200 · overlay:300 · modal:400 · toast:500` | TECHNICAL DECISION; substitui valores locais 80/100 dos mockups |
| 3. Page width e gutters | Layout fluido; `max-width: 1600px` centrado; gutters 28px desktop / 16px tablet / 12–14px mobile (alinhado com padding dos mockups canónicos) | TECHNICAL DECISION contra mockups centralizados estreitos |
| 4. Tamanho default do Button | API única: compact **34**, padrão **36** (ações), form/filter **40** (botão filho direto de `.filters/.search/.history-filters/.dmo-filter-row` herda 40 automaticamente); paginação 36×36; touch ≥44 | DMO_DESIGN_SYSTEM §8/§10; CODER_HANDOFF §3; resolve divergência 34/36/40 |
| 5. Interação de rows (sem shortcuts funcionais) | Contrato único: **1 clique seleciona; 2 cliques abrem/entram no registo**. O BA DMO não tem atalhos funcionais próprios — Enter/Espaço/Ctrl+Enter NÃO são requisitos do produto (decisão atual; NC-01 resolvido; variantes do design/legacy classificadas como demonstrativas — DO NOT CARRY FORWARD). Acessibilidade web standard (foco, navegação por tab, teclado em dropdowns/calendários) permanece válida como acessibilidade, não como shortcut funcional | Decisão atual do utilizador (passagem legacy recovery) |
| 6. Border/focus tokens + reduced motion | `--border-width:1px / strong:2px`; foco = contorno brand + halo `0 0 0 3px rgba(60,115,168,.24)`; `prefers-reduced-motion` desativa transições não essenciais | DMO §4/§23 + contrato §2.2 |
| 7. Fonte visual única | `dmo-design-system` como única fonte; proibido `<style>` de design, inline styles de design, `site.css` legacy, segundos componentes | Contrato §3.2 |

## 2. Tokens (GLM-DSN-02)

Tokens canónicos = DMO_DESIGN_SYSTEM v2.7 §4 (design atual; substituem a paleta V1 histórica de
06_UI_UX, que permanece como provenance). Marca `#3c73a8` preservada; semânticas dessaturadas:

- brand 950–050 (`#0f1d2a` … `#e8eff7`); superfícies page `#f6f9fc`, card `#ffffff`, subtle `#f1f6fa`;
- border `#d9e6f2`; texto `#172d42`/muted `#64778a`/on-color `#ffffff`;
- success `#527c72`+soft, warning `#a97943`+soft, danger `#9a625d`+soft, pending `#315d88`+soft, disabled `#cbd5df`;
- spacing 4–32; radius control 8/card 12/modal 16/pill 999; sizing control 40/compact 34/header 76/
  tabs 52/sidebar 276; sombras card/menu/modal; `--dmo-transition-fast: 150ms ease`.
- Fonte: `Inter` com fallback system (`ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`);
  regra de fallback local sem download externo obrigatório.

É proibido introduzir um novo hexadecimal num componente quando já existe um token com o mesmo papel.

## 3. Arquitetura CSS (GLM-DSN-03)

```text
styles/
  dmo-tokens.css → dmo-foundation.css → dmo-components.css → dmo-layout.css → dmo-utilities.css
  modules/<module>-layout.css   (apenas composição: grid, ordem, larguras)
```

Regras normativas (contrato §3.2): um componente universal implementado uma vez; CSS de módulo não
redefine cor/raio/sombra/tipografia/botão/field/table/modal/calendar/feedback; load order único
`tokens → components → layout → module composition`; exceções raras, nomeadas, documentadas, testadas.
Pode existir um único `dmo-design-system.css` na primeira implementação desde que mantenha tokens,
layout e componentes claramente separados (DMO §27).

## 4. Catálogo de componentes universais (GLM-DSN-04)

Inventário completo e estados no contrato §4–§5; a foundation implementa **todos** antes dos módulos,
com prioridade para os bloqueantes P1:

1. Button/Icon Button (state machine filled → inverted hover/focus; loading preserva largura; sem brightness);
2. Field completo (label+control+helper+error; required marker; estados readonly/disabled/error/success/loading);
3. Select curto + Dropdown pesquisável (teclado completo; sem auto-escolher primeiro resultado);
4. Card/Expandable Card (dirty state; um expandido de cada vez; ações que expandem cartões inline — DMO §9);
5. Tabs; Badge; Status (texto + cor, nunca só cor);
6. List/Table/Row + Pagination (contrato §6; sorting: headers sortable com estado inicial neutro, direção asc/desc, sem persistência na V1);
7. Filter Bar + Search (cartão de filtros `Filtros · N`; Aplicar/Limpar — DMO §14);
8. Loading/Empty/Error como componentes reais (skeleton sem layout shift; Empty com causa + próximo passo; Error com Retry);
9. Modal + Confirmation Dialog (focus trap; sem `confirm/prompt/alert` nativos; tamanhos sm/md/lg);
10. Context Menu `…`; Tooltip (delay; nunca conteúdo essencial);
11. **Calendar único** (secção 5);
12. Header/Shell/Sidebar responsiva/drawer; User/Profile menu + logout (`data-user-profile-name/title`);
13. Page Header, Section Header, Action Bar, Detail Panel, Form Group;
14. **History Entry** (READY no contrato; compacta: data/ator/ação/objeto/estado/motivo; expandida:
    Anterior/Novo, motivo, correção ligada ao evento; mesma representação em todos os módulos e na
    tab Auditoria do Admin);
15. Toast/Alert/Feedback conforme tabela contrato §12;
16. **Tool Availability Picker** (novo): lista filtrada de substituição na edição do Job On — posição
    do Armazém + estado técnico + % de uso com origem explícita; só em Modo edição; estados
    closed/loading/ready/selected/empty/partial-source/error; falha parcial por fonte distinguida
    (`Localização indisponível`);
17. **Local Directory Selector** (novo): autorização da pasta raiz local de relatórios; estados
    unconfigured/requesting/authorized/permission-lost/unavailable/error (Definições);
18. **Resolved Report Path** (novo): read-only `diretório principal / subpasta do lote`
    (ex.: `Capacidades / 5447T173`); estados resolved/missing-root/invalid-subfolder/permission-lost;
    não é editor de caminho absoluto (criação de lote do Peso + Pegamentos).

**Gate obrigatório:** página-laboratório com todos os componentes e estados usando apenas CSS global
(contrato §20, antes do passo 17 de módulos).

**Fronteira Local Directory Selector / Resolved Report Path (Plano-V3):** o File System Access API é
**cliente/browser-only** e destina-se **exclusivamente à exportação de PDFs** (nunca a domain
persistence, offline database, business datastore ou source of truth). Requer secure context
**HTTPS** (Render fornece em deployment normal). Fallback = **download padrão do browser** quando a
API não é suportada, a autorização é recusada, o handle é inválido ou a permission se perdeu.
**Exceção aprovada pelo owner:** IndexedDB pode persistir **apenas** o `FileSystemDirectoryHandle`
(permission/state técnico da seleção do diretório local); nunca dados de domínio
(03_ARCH §17; 06_DATA §16). Sem alterações ao aspeto visual/design.

## 5. Calendar único (GLM-DSN-05)

Um único componente consumido por Boquilhas, Peso, Job On (e quaisquer outros):
semana começa segunda-feira; mês/ano centrado com setas nas extremidades; um clique seleciona/filtra;
dia com registo tem ponto discreto (`has-record`); selecionado brand fill + texto branco; hoje com
indicador próprio; dias fora do mês/disabled definidos; loading/error do calendário definidos;
`Mostrar todas as datas` limpa filtro de data; mudar de mês não auto-seleciona; teclado completo +
`aria-pressed`; desktop 300–340px junto de lista; mobile empilha por cima. Sincronização com listas
por data ISO (`data-date="AAAA-MM-DD"`). Registos planeados usam ID estável: mudar uma data atualiza
o mesmo evento e não duplica entradas (calendário Job On lê `planned_start_at/planned_end_at`).
Nenhuma variante por módulo; a diferença visual Peso/BQ é eliminada construindo o componente antes de ambos.

## 6. Shell visual (GLM-DSN-06)

Frame conforme `05_SHELL_NAVIGATION_AND_ROUTING_SPEC.md` §2 + contrato §8:
header global 76px com logo 44px, título/subtítulo da página e utilizador (nome + profile_title);
module switcher = tabs canónicas com active indication; account/logout no user indicator; conteúdo
fluido com gutters canónicos; sticky header com layer própria; sidebar contextual ≠ navegação;
apresentação para utilizador sem tabs próprias definida (Job On consulta está sempre disponível).
Indicador de modo do Job On distinguível sem cores agressivas (azul-cinza suave em consulta,
âmbar/castanho suave em edição).

## 7. Acessibilidade e responsive (GLM-DSN-07)

- WCAG AA: contraste verificado por combinação de tokens; foco visível em tudo; labels sempre;
  `aria-label` obrigatório em icon buttons; estados nunca apenas por cor.
- Teclado: navegação completa; contrato de rows §1-P0-5; modais com focus trap e retorno de foco;
  `aria-expanded`/`aria-controls` em expansores; `aria-live` para feedback importante.
- `prefers-reduced-motion` respeitado.
- Breakpoints 1200/980/720 + mobile estreito: grids colapsam, sidebars viram drawer, sem scroll
  horizontal de página; touch targets ≥44px em mobile.
- Registo rápido (DMO §33): foco automático no campo, fluxo keyboard-friendly, erro mantém valor e
  foco. O pacote não promove nenhum atalho de teclado como requisito funcional do produto (a
  submissão segue acessibilidade standard de formulários).

## 8. Visual regression (GLM-DSN-08)

- Capturas de referência desktop/tablet/mobile para: Button, Field, Card, Table/Row, Calendar,
  Modal, Shell, History Entry;
- comparação cross-module de alturas/radius/spacing/typography;
- temas/zoom/text scaling (150%) sem quebra;
- ferramenta sem dependência de tooling local proibido (03_ARCH §11) — screenshots manuais
  comparáveis são aceitáveis na V1.

## 9. Regra template/token/component-first (GLM-DSN-09)

Nenhuma página pode conter patches CSS locais, `<style>` de design ou inline styles de design.
Todo o layout de módulo consome tokens + componentes + templates partilhados; o CSS do módulo só
organiza composição. Violações são critério de rejeição no gate E. Resíduos demonstrativos conhecidos
nos mockups (`confirm()/prompt()/alert()`, bases locais de Pegamentos, CSS inline) estão listados no
contrato §23 e não podem ser copiados.

## 10. Fronteira design vs negócio (GLM-DSN-10)

Afirmações funcionais dos documentos de design marcadas `FUNCTIONAL INPUT REQUIRED` não criam
entidades nem regras próprias; a autoridade funcional é este pacote (specs de módulos), com a única
exceção explícita de `JOB_ON_DATA_MODEL.md`, contrato técnico de persistência validado pelo produto
(contrato §17). Exemplos preservados: utilização nunca bloqueia; comparação não altera o aprovado;
título não concede permissões; estados visuais de recolha não alteram localização oficial.
