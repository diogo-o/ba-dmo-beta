# MODULES 02 — CONTROLO SPEC

Controlo é **área/domínio funcional** (UD-14; corrigida a descrição anterior de “simples grupo
visual”), não um módulo com grants próprios.

## 1. Natureza e boundary (GLM-CTR-01)

1. Controlo agrega os módulos **Peso** e **Pegamentos** na arquitetura de informação e de domínio.
2. Não possui tabelas, domínio nem casos de uso próprios; não concede entrada por si.
3. Peso e Pegamentos são **atribuíveis separavelmente** nos templates de acesso (UD-14).
4. As lógicas de Peso e Pegamentos **nunca são fundidas** (UD-05): domínios, dados, serviços,
   UI, rotas e testes separados. Partilham apenas a entrada de navegação e convenções visuais.

## 2. Navegação (GLM-CTR-02)

1. Entrada `Controlo` visível quando existe pelo menos um grant filho (`peso` e/ou `pegamentos`).
2. Dentro de Controlo: entradas apenas para filhos autorizados; sem áreas vazias.
3. Peso apresenta uma entrada; a experiência Operador/Responsável resolve-se por `peso.aprovar`
   (04_ACC §5; UD-15) — sem selector manual. Pegamentos apresenta a sua própria entrada.
4. Sem grants filhos → Controlo não aparece.

## 3. O que Peso e Pegamentos partilham (GLM-CTR-03)

Apenas: posição na área de navegação; convenções visuais do design system; convenções de
listas/calendário; identidade da Shell; contexto de produção proveniente do Job On (cada um pelo seu
contrato). **Não partilham** entidades, tabelas, serviços, cálculos nem workflow de aprovação.

## 4. Dados (GLM-CTR-04)

Peso → tabelas `peso_*` (06_DATA §3.3). Pegamentos → tabelas `pegamento_*` (06_DATA §3.4).
O modelo v2 de tabela única `controlo(control_type)` é evidência/proposal; na app nova os dois
domínios mantêm-se separados por coerência com UD-05 (decisão BT aplicada: separação física).

## 5. Testes e acceptance (GLM-CTR-05)

1. Utilizador só Peso vê Controlo com uma entrada; só Pegamentos idem; ambos → duas entradas; nenhum → sem grupo.
2. Nenhuma rota/handler de Peso é alcançável via Pegamentos e vice-versa (unit + E2E).
3. Tabs derivadas corretas nos três cenários.

## 6. DO NOT CARRY FORWARD (GLM-CTR-06)

Fusão Peso/Pegamentos num único ecrã ou serviço; tabela `controlo` única com `control_type` como
mecanismo de autorização; área Controlo vazia visível.
