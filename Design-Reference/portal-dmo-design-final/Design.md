# BA DMO — CONSOLIDATED DESIGN SOURCE

## Status

CONSOLIDATED DESIGN/HANDOFF SOURCE

NOT A BUSINESS SPECIFICATION
NOT A DOMAIN MODEL
NOT A DATABASE SPECIFICATION

## Source Root

``D:\BA-QWEN-MAX-PRODUCTION\portal-dmo-design-final``

## Purpose

Este documento consolida os ficheiros existentes no Design/Handoff numa única fonte, preservando proveniência por ficheiro.

Será posteriormente usado juntamente com:

- BA_DMO_VERIFIED_KNOWLEDGE_MERGED.md
- current repository
- specs
- migrations
- tests

pelo agente de planeamento definitivo.

## File Inventory

| # | Relative Path | Type | Size | Included |
|---|---|---|---|---|
| 1 | `docs\ARMAZEM_DESIGN_BRIEF.md` | .md | 17459 | YES |
| 2 | `docs\AUDITORIA_GLOBAL_HANDOFF.md` | .md | 6463 | YES |
| 3 | `docs\BOQUILHAS_INTERFACE_BEHAVIOR.md` | .md | 10029 | YES |
| 4 | `docs\CODER_IMPLEMENTATION_HANDOFF.md` | .md | 37833 | YES |
| 5 | `docs\DESIGN_IMPLEMENTATION_CONTRACT.md` | .md | 56764 | YES |
| 6 | `docs\DESIGN_INPUT_EXTRACTION.md` | .md | 5968 | YES |
| 7 | `docs\DMO_DESIGN_SYSTEM.md` | .md | 30865 | YES |
| 8 | `docs\FERRAMENTAS_REGISTO_DESIGN_BRIEF.md` | .md | 9798 | YES |
| 9 | `docs\HANDOFF_INDEX.md` | .md | 6620 | YES |
| 10 | `docs\JOB_ON_DATA_MODEL.md` | .md | 14904 | YES |
| 11 | `docs\JOB_ON_DESIGN_BRIEF.md` | .md | 34337 | YES |
| 12 | `docs\JOB_ON_VERIFICACOES_DESIGN_BRIEF.md` | .md | 9649 | YES |
| 13 | `docs\MODULE_UI_HANDOFF_TEMPLATE.md` | .md | 4219 | YES |
| 14 | `docs\PEGAMENTOS_INTERFACE_HANDOFF.md` | .md | 5278 | YES |
| 15 | `docs\PESO_INTERFACE_HANDOFF.md` | .md | 17442 | YES |
| 16 | `docs\PORTAL_LOGIN_ADMIN_HANDOFF.md` | .md | 3979 | YES |
| 17 | `docs\REPARACAO_EXTERNA_DESIGN_BRIEF.md` | .md | 6258 | YES |
| 18 | `docs\REPARACAO_INTERNA_DESIGN_BRIEF.md` | .md | 8087 | YES |
| 19 | `docs\TAMPOES_DESIGN_BRIEF.md` | .md | 13117 | YES |
| 20 | `README.md` | .md | 5089 | YES |
| 21 | `admin.html` | .html | 15494 | YES |
| 22 | `armazem.html` | .html | 30533 | YES |
| 23 | `boquilhas.html` | .html | 48311 | YES |
| 24 | `job-on.html` | .html | 374 | YES |
| 25 | `job-on-v48-folha-producao.html` | .html | 44334 | YES |
| 26 | `login.html` | .html | 4359 | YES |
| 27 | `moldes.html` | .html | 25176 | YES |
| 28 | `moldes-v42-listas.html` | .html | 25176 | YES |
| 29 | `moldes-v43-alinhado.html` | .html | 25176 | YES |
| 30 | `moldes-v44-seletor-corrigido.html` | .html | 25176 | YES |
| 31 | `pegamentos.html` | .html | 112759 | YES |
| 32 | `peso-operador.html` | .html | 32968 | YES |
| 33 | `peso-responsavel.html` | .html | 17314 | YES |
| 34 | `reparacao-externa-v1.html` | .html | 4096 | YES |
| 35 | `reparacao-interna.html` | .html | 29815 | YES |
| 36 | `reparacao-v2.html` | .html | 7097 | YES |
| 37 | `tampoes.html` | .html | 34826 | YES |
| 38 | `tampoes-v38-standalone.html` | .html | 34826 | YES |
| 39 | `dmo-design-system.css` | .css | 8189 | YES |
| 40 | `dmo-interactions.js` | .js | 1890 | YES |
| 41 | `logo_recolored(1).png` | .png | 38205 | YES |

> Nota: os 40 ficheiros textuais têm uma secção própria (FILE 001..040); o PNG é inventariado como asset (FILE 041) sem conteúdo embebido.

---

# FILE 001

## Source Path
`docs\ARMAZEM_DESIGN_BRIEF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Armazém — brief funcional e de interface V1

Estado: base corrigida para mockup e handoff técnico  
Âmbito V1: registo e consulta de posições e movimentos de CM, MF e BQ

Este documento define a interface e a forma como o utilizador interage com ela. Regras de domínio, persistência, concorrência, permissões técnicas e efeitos finais dos comandos são contratos dos serviços responsáveis. O frontend apresenta os resultados devolvidos e não simula sucesso antes dessa resposta.

## 1. Limite do módulo

O Armazém é responsável apenas por:

- ferramenta que entrou ou saiu;
- posição física no Armazém;
- destino de uma Saída, quando aplicável;
- observações livres do movimento;
- operador e data/hora;
- diferenças encontradas fisicamente e correções posteriores.

Tudo o resto pertence ao domínio da ferramenta.

O Armazém não cria, altera nem recalcula:

- percentagem de vida/utilização;
- estados como `Sucatado`, `Arquivado`, `Reparado`, `Por reparar` ou `Novo`;
- Máquina/Linha associada;
- reparador;
- dados técnicos;
- Referência ou lote da ferramenta.

Esses valores podem aparecer como contexto read-only quando forem úteis para identificar uma ferramenta. A origem deve ser o domínio autoritativo de CM, MF ou BQ.

## 2. Estrutura da página

Tabs V1:

1. `Registo`
2. `Consulta`

Não criar Definições de reparadores, estados ou vida útil dentro do Armazém.

## 3. Tab Registo

### Estado inicial

Apresenta três ações:

- `Entrada`
- `Saída`
- `Saídas programadas`

Ao escolher uma ação, expande inline um cartão de registo. Não abre nova página nem modal no fluxo normal.

### Tipo de ferramenta

Dentro do cartão aparecem seletores filled:

- `CM`
- `MF`
- `BQ`

O tipo determina apenas qual domínio é pesquisado. CM, MF e BQ mantêm identidades, campos e históricos separados.

### Seleção da ferramenta

Campos de seleção:

1. Tipo;
2. Referência;
3. Lote, quando existirem vários lotes para a Referência.

Regras:

- escrever uma Referência pesquisa no domínio do tipo escolhido;
- resultados mostram Referência e lote suficientes para distinguir registos;
- resultados mostram também o Nome técnico vindo do domínio da ferramenta;
- nenhum resultado ambíguo é escolhido automaticamente;
- o Armazém guarda o ID estável da ferramenta/lote;
- Referência e lote são apresentados, não editados;
- vida útil, estado, Máquina/Linha ou outros dados técnicos, se mostrados, são sempre read-only.

## 4. Registar Entrada

Campos editáveis do movimento:

- `Posição`;
- `Observações`.

Antes de guardar, mostrar:

- Tipo;
- Referência;
- lote;
- localização atual conhecida, quando existir.

Ao confirmar com sucesso:

1. é criado o movimento de Entrada;
2. a posição passa a ser a localização atual no Armazém;
3. posição e ferramenta são associadas atomicamente;
4. operador e data/hora são registados automaticamente;
5. a interface só apresenta sucesso depois da persistência.

Cancelar ou falhar não altera a localização anterior.

## 5. Registar Saída imediata

Campos editáveis do movimento:

- `Destino`: Produção ou Reparação, quando necessário ao movimento;
- `Observações`.

A posição atual aparece read-only. Não é reescrita pelo utilizador durante a Saída.

Ao confirmar com sucesso:

1. é criado o movimento de Saída;
2. a ferramenta deixa de ocupar a posição na interface;
3. o destino fica registado no movimento;
4. operador e data/hora são registados automaticamente.

O formulário não contém:

- Máquina/Linha;
- percentagem de vida;
- estado da ferramenta;
- Reparador;
- campos técnicos de CM, MF ou BQ.

O campo `Observações` permite apenas informação livre relevante ao movimento. Não substitui dados estruturados do domínio da ferramenta.

“Retirar logo a posição” significa depois da confirmação e persistência com sucesso. Abrir o formulário ou escolher `Saída` não altera dados.

## 6. Saída programada para Reparação

`Saída programada` coordena a recolha física de vários lotes que vão ser enviados para Reparação.

É um fluxo partilhado entre Reparação e Armazém. A lista é criada no módulo de Reparação e executada no módulo do Armazém.

### 6.1 Origem: módulo de Reparação

1. O Manager inicia uma Reparação.
2. Seleciona os lotes/ferramentas que devem ser enviados.
3. Cria a lista de Saída programada.
4. A aplicação consulta a localização atual no Armazém para apresentar as posições de recolha.
5. Ao guardar, a lista passa a estar visível como pendente no módulo do Armazém.

Exemplo operacional confirmado: o Manager seleciona 15 lotes na Reparação; o Armazém recebe uma lista pendente para que o operador os retire.

A criação e seleção dos lotes não são responsabilidade do Armazém. O Armazém recebe a lista e executa apenas a recolha/saída.

### 6.2 Lista pendente no Armazém

O módulo do Armazém apresenta uma indicação visível de que existem Saídas programadas pendentes, por exemplo contador discreto junto de `Saídas programadas`.

A página apresenta uma lista canónica com:

| Estado | Origem | Criada por | Data | Itens | Progresso |
|---|---|---|---|---|---|

Interação:

- um clique seleciona a lista;
- duplo clique abre a lista para recolha;
- a lista aparece mesmo que nunca tenha sido impressa;
- o operador pode verificar e concluir todo o fluxo apenas no computador;
- a impressão é opcional e nunca condição para a lista ficar disponível.

Ao abrir, a tabela de recolha mostra, no mínimo:

| Retirado | Tipo | Referência | Lote | Posição |
|---|---|---|---|---|

Regras:

- a lista recebida identifica de forma estável cada ferramenta/lote;
- os checkboxes representam confirmação de recolha e não seleção de ferramentas para a lista;
- receber, abrir ou imprimir a lista não cria Saídas e não liberta posições;
- cada ferramenta continua a ocupar a sua posição até à confirmação final;
- a lista fica persistida para poder ser recuperada posteriormente noutro acesso ao módulo.

Quando a posição atual for diferente da posição registada no momento em que o Manager criou a lista, mostrar as duas separadamente como `Posição na criação` e `Posição atual`, com alerta. Não corrigir nem substituir silenciosamente o snapshot.

### 6.3 Impressão opcional

A impressão apresenta apenas a informação necessária à recolha:

- identificação da saída programada;
- data de criação;
- Tipo;
- Referência;
- lote;
- posição;
- espaço visual para confirmação física;
- observação geral, quando existir.

Imprimir não altera o estado da lista nem das posições. O operador pode executar exatamente o mesmo fluxo sem impressão.

### 6.4 Confirmar a recolha

O operador abre a lista pendente no módulo do Armazém e, à medida que retira fisicamente as ferramentas:

1. abre a lista pendente;
2. confirma cada linha através do respetivo checkbox `Retirado`;
3. os checks ficam guardados para que a lista possa ser retomada;
4. enquanto existir pelo menos uma linha sem check, nenhuma posição do conjunto é libertada;
5. ao confirmar o último item, a aplicação tenta concluir todo o conjunto;
6. apenas depois de a conclusão persistir com sucesso são criadas as Saídas e libertadas todas as posições.

O fecho do conjunto deve ser atómico:

- se falhar, nenhuma posição é libertada;
- a lista permanece pendente com as confirmações preservadas;
- a interface mostra o erro e permite tentar novamente;
- nunca apresentar conclusão parcial como sucesso total.

Quando o último check fecha o conjunto com sucesso:

- é criado um registo de Saída para cada ferramenta/lote;
- cada registo guarda a ligação à lista de Reparação;
- cada linha guarda o dia/hora da Saída e o operador que confirmou a Saída;
- todas as posições do conjunto ficam livres;
- a lista passa a estar registada e consultável no módulo de Reparação;
- o Manager consegue acompanhar posteriormente quais ferramentas saíram e quais já regressaram.

### 6.5 Regresso da Reparação e Entrada no Armazém

Quando as ferramentas regressam, o operador regista a Entrada no Armazém usando a ferramenta/lote já associado à lista.

Para cada linha, a aplicação conserva:

| Tipo | Referência | Lote | Saída | Operador da Saída | Entrada | Operador da Entrada | Estado do ciclo |
|---|---|---|---|---|---|---|---|

Ao guardar uma Entrada com sucesso:

1. é criada a posição atual no Armazém;
2. é criado o registo de Entrada;
3. são guardados dia/hora e operador da Entrada;
4. o ciclo dessa ferramenta/lote fica concluído;
5. a linha é atualizada simultaneamente no acompanhamento da Reparação.

Se apenas parte das ferramentas regressar, as linhas entradas ficam concluídas e as restantes continuam abertas. A lista completa só fica `Concluída` quando todas as linhas tiverem uma Entrada registada.

O Manager consulta este acompanhamento no módulo de Reparação; não precisa de abrir o histórico do Armazém para saber:

- quando cada ferramenta saiu;
- quem confirmou a Saída;
- quando regressou;
- quem registou a Entrada;
- quais ainda estão fora.

### 6.6 Estados e tratamento visual

Usar estados operacionais da lista, sem alterar os estados técnicos das ferramentas:

- `Pendente de saída`: criada pela Reparação e ainda não totalmente confirmada no Armazém;
- `Em reparação`: Saídas criadas e nenhuma Entrada registada;
- `Retorno parcial`: pelo menos uma Entrada registada, mas ainda existem ferramentas fora;
- `Concluída`: todas as ferramentas têm Entrada registada no Armazém.

Os checks indicam progresso de recolha, não a localização oficial da ferramenta. A localização só muda quando a fase de Saída é fechada com o último check e persistida com sucesso.

Tratamento visual recomendado:

- listas ativas mantêm fundo normal e estado textual;
- `Concluída` usa selo verde suave do design system, não verde vivo;
- a linha concluída pode usar fundo cinza muito claro para perder prioridade visual;
- cor nunca substitui o texto do estado;
- listas concluídas permanecem pesquisáveis e read-only.

## 7. Tab Consulta

### Pesquisa

Aceita:

- Referência;
- lote;
- posição;
- tipo de ferramenta.

Resultado mínimo:

| Tipo | Referência | Nome técnico | Lote | Localização/contexto | Posição | Último movimento |
|---|---|---|---|---|---|---|

Quando a ferramenta não está no Armazém, a posição aparece como `—`. A posição anterior permanece no histórico.

Vida útil e estado da ferramenta podem ser mostrados opcionalmente como colunas read-only provenientes do domínio da ferramenta, mas não são dados nem filtros próprios do Armazém na V1.

### Filtros

- Tipo: CM, MF, BQ;
- localização/contexto registado;
- posição;
- intervalo de datas do movimento;
- apenas com alertas.

Não duplicar filtros de vida, estado, Máquina/Linha ou Reparador pertencentes a outros domínios.

### Lista canónica

- um clique seleciona;
- duplo clique abre o histórico de localização/movimentos;
- ações dependentes da seleção ficam fora da lista;
- filtros nunca selecionam automaticamente um resultado;
- cada linha referencia o ID estável da ferramenta.

## 8. Histórico de localização

A ficha apresenta apenas:

- Entrada ou Saída;
- posição anterior e nova, quando aplicável;
- origem/destino registado no movimento;
- observações;
- data/hora;
- operador.

Uma Saída programada entra no histórico de movimentos quando a fase de recolha/Saída é concluída pelo último check. A criação e impressão da lista pertencem ao histórico operacional da própria lista, não ao histórico de localização da ferramenta.

Cada par Saída/Entrada conserva a ligação à mesma lista e à mesma ferramenta/lote, permitindo reconstruir o ciclo completo sem misturar dados técnicos da Reparação no Armazém.

Não repetir no histórico do Armazém:

- reparações;
- vida útil;
- alterações de estado técnico;
- arquivo ou sucata;
- histórico de produção da ferramenta.

Uma correção não reescreve silenciosamente movimentos anteriores; usa o mecanismo auditável confirmado pela implementação.

## 9. Localização registada e realidade física

No fluxo normal, uma Saída confirmada limpa a posição no sistema. Por isso, a Entrada não inclui um fluxo preventivo de `posição ocupada` nem uma ação `Substituir`.

A interface apresenta a disponibilidade devolvida pelo serviço. Se uma ferramenta estiver fisicamente numa posição mas não estiver registada, o frontend não tem forma de a detetar. Não mostrar alertas preditivos nem inventar o ocupante.

Quando o operador encontrar uma diferença física, deve poder selecionar o registo relacionado e abrir `Corrigir localização`. A correção fica separada de uma Entrada normal e apresenta claramente os valores registados e os valores encontrados.

### Ferramenta sem localização operacional

Se a informação consolidada indicar que uma ferramenta não está associada a Armazém, Produção ou Reparação, apresentar `Localização operacional não registada`.

O Armazém apenas sinaliza a inconsistência. Não inventa estado, localização, condição ou reparador e não cria um movimento automaticamente.

### Ferramenta em mais de um contexto

Se a mesma ferramenta surgir simultaneamente em contextos incompatíveis, mostrar conflito e encaminhar para correção humana. Não aplicar prioridade automática.

## 10. Relação com Job On

O Job On pode consultar o Armazém para apresentar:

- posição atual, quando existir;
- localização/contexto atual;
- posição atual exata, quando a ferramenta está no Armazém;
- último movimento relevante.

Regras:

- Job On não altera posições;
- selecionar uma ferramenta no Job On não cria uma Saída;
- movimentos reais são registados no Armazém;
- o Job On associa o ID estável da ferramenta/lote;
- snapshot histórico e localização atual aparecem separados;
- vida útil, estado, Máquina/Linha e reparação são obtidos no domínio da ferramenta, nunca no Armazém.

Na edição do Job On, a lista de substituição pode combinar a posição devolvida pelo Armazém com estado técnico e `% de uso` devolvidos pelo domínio da ferramenta. Esta composição é read-only. Em `Modo consulta` do Job On, a informação live de Armazém não ocupa a folha; a folha mostra apenas a associação guardada necessária à produção.

## 11. Estados vazios e erros

- Referência inexistente: `Ferramenta não encontrada`.
- Lote inexistente: `Não existem lotes registados`.
- Posição vazia: `Posição sem ocupação registada`.
- Erro de carregamento: mostrar erro e `Tentar novamente`; não apresentar como lista vazia.
- Falha ao guardar: manter os dados introduzidos e a localização anterior, sem falso sucesso.
- Sem listas programadas: `Não existem Saídas programadas pendentes`.
- Falha no fecho programado: manter a lista pendente e todas as posições ocupadas.
- Falha numa Entrada de retorno: manter essa linha aberta e preservar o último estado válido da lista.

## 12. Questões por confirmar antes do freeze técnico

- formato e limites válidos do código de posição;
- se o Destino é obrigatório em todas as Saídas;
- fluxo de correção/anulação de movimento;
- tipos adicionais depois de CM, MF e BQ.
- quem pode cancelar uma lista criada pelo Manager e em que estados;
- se a Reparação pode remover/adicionar linhas depois de a lista já estar visível no Armazém;
- se a confirmação final exige uma ação adicional ou é iniciada automaticamente pelo último check.
- se o Manager pode encerrar/cancelar uma linha que não regressará e qual o motivo obrigatório.

## 13. Critérios de aceitação do mockup V1

- Tabs Registo e Consulta usam o shell global;
- Entrada/Saída expandem inline;
- o movimento contém apenas dados próprios do Armazém;
- Máquina/Linha, vida útil, estado e Reparador não são editáveis no Armazém;
- CM, MF e BQ são pesquisados nas respetivas fontes;
- Entrada guarda posição e observações;
- Saída guarda destino, quando aplicável, e observações;
- a posição só é removida após persistência da Saída;
- o Manager cria a lista no módulo de Reparação;
- o Armazém recebe uma indicação e uma lista pendente mesmo sem impressão;
- o operador pode executar a lista integralmente no computador;
- imprimir é opcional e não altera o fluxo;
- imprimir ou marcar apenas parte da lista não liberta posições;
- o último check conclui o conjunto de forma atómica e só então liberta todas as posições;
- uma falha no fecho não produz libertação parcial;
- o último check cria um registo de Saída por ferramenta com dia/hora e operador;
- a lista permanece visível na Reparação durante todo o ciclo;
- cada Entrada guarda dia/hora e operador e fecha a respetiva linha;
- a lista só fica `Concluída` quando todas as ferramentas tiverem regressado;
- listas concluídas usam estado verde suave e apresentação visual secundária/cinza;
- Consulta encontra por Referência, lote e posição;
- listas seguem clique/duplo clique canónicos;
- diferenças físicas encontradas pelo operador podem abrir uma correção de localização;
- histórico contém apenas localização e movimentos;
- Job On consulta o Armazém sem o alterar;
- nenhuma falha produz falso sucesso.


## END FILE CONTENT

---

# FILE 002

## Source Path
`docs\AUDITORIA_GLOBAL_HANDOFF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Auditoria global de ações — contrato funcional e técnico

## 1. Objetivo

Todos os utilizadores autenticados geram um histórico de ações relevantes. Cada ação é registada como um evento próprio, associado ao utilizador, ao módulo e ao registo de negócio afetado. O Admin permite consultar esse histórico por ano.

Este histórico **não calcula pontuação, ranking, produtividade ou avaliação automática**. Limita-se a preservar factos operacionais para revisão autorizada com o respetivo contexto.

## 2. Unidade de registo

Uma ação de negócio concluída corresponde a um evento de auditoria. Não registar cada clique, hover ou navegação sem consequência. Exemplos de ações auditáveis:

- criar, duplicar, guardar, corrigir, confirmar, aprovar, rejeitar, enviar, fechar ou reabrir um registo;
- alterar uma data, ferramenta, lote, quantidade, localização ou configuração;
- confirmar ou repor uma verificação;
- iniciar ou concluir uma saída programada;
- criar/editar/desativar um utilizador ou opção administrável;
- tentar uma operação protegida que termine em falha ou acesso negado, quando relevante para segurança ou rastreabilidade.

## 3. Campos mínimos do evento

| Campo | Regra |
|---|---|
| `id` | identificador imutável do evento |
| `occurredAtUtc` | data/hora UTC gerada no servidor |
| `year` | derivado de `occurredAtUtc`; usado no filtro/índice anual |
| `actorUserId` | ID estável do utilizador autenticado |
| `actorNameSnapshot` | nome apresentado no momento da ação |
| `moduleId` | módulo que recebeu o comando, por exemplo `jobon`, `peso`, `boquilhas`, `armazem` |
| `actionCode` | código estável e pesquisável, por exemplo `jobon.revision.saved` |
| `entityType` | tipo de entidade afetada |
| `entityId` | ID da entidade afetada |
| `entityLabelSnapshot` | referência legível no momento do evento |
| `result` | `succeeded`, `failed`, `denied`, `corrected` ou outro estado controlado |
| `reason` | justificação quando obrigatória; nulo nas ações normais |
| `correlationId` | liga eventos do mesmo comando/transação |
| `jobOnId` | obrigatório quando a ação está associada a uma produção/Job On |
| `revisionId` | revisão/snapshot quando aplicável |
| `beforeSummary` / `afterSummary` | apenas campos necessários para compreender uma alteração auditável |

O registo é append-only. Uma correção gera um novo evento que referencia o anterior; nunca reescreve nem elimina o evento original.

## 4. Responsabilidade de implementação

- O backend é a fonte autoritativa do evento; um registo criado apenas no browser não é auditoria.
- Sempre que possível, a alteração de negócio e a criação do evento ocorrem na mesma transação. Se a arquitetura exigir eventos assíncronos, usar outbox/correlação para não perder ações.
- A data/hora e o utilizador são obtidos da sessão no servidor, não de campos editáveis enviados pelo cliente.
- A tabela é única e canónica. Pode ser particionada/indexada por ano, mas não se criam tabelas incompatíveis por módulo ou por ano.
- A retenção e o acesso seguem a política interna definida pela organização.

## 5. O que não deve ser guardado

Não incluir palavras-passe, tokens, cookies, credenciais, conteúdo integral de emails, PDFs, imagens ou cargas arbitrárias. O evento guarda IDs, metadados e resumos mínimos necessários. Dados sensíveis permanecem no respetivo domínio com as suas próprias regras de acesso.

## 6. Consulta no Admin

A tab `Auditoria` apresenta o registo anual com:

- filtros por ano, utilizador, módulo, ação, resultado e intervalo de datas;
- paginação canónica com 20, 40 ou 60 linhas;
- um clique para selecionar;
- duplo clique para abrir o detalhe;
- exportação anual autorizada;
- data/hora, utilizador, módulo, ação, registo associado e resultado sempre visíveis.

O detalhe mostra apenas informação factual do evento. Não existe coluna de pontos, nota, ranking ou classificação automática.

## 7. Catálogo inicial de ações por módulo

| Módulo | Exemplos de `actionCode` |
|---|---|
| Job On | `jobon.created`, `jobon.duplicated`, `jobon.revision.saved`, `jobon.tool.replaced`, `jobon.date.changed`, `jobon.verification.confirmed`, `jobon.verification.reset` |
| Peso | `weight.lot.created`, `weight.control.calculated`, `weight.control.submitted`, `weight.control.approved`, `weight.control.rejected`, `weight.comparison.decided`, `weight.pdf.generated`, `weight.email.prepared` |
| Pegamentos | `gluing.record.created`, `gluing.record.saved`, `gluing.pdf.generated` |
| Boquilhas | `bq.lot.created`, `bq.movement.created`, `bq.movement.corrected`, `bq.lot.closed` |
| Armazém | `warehouse.entry.created`, `warehouse.exit.created`, `warehouse.location.corrected`, `warehouse.scheduled_exit.completed` |
| Reparação | `repair.list.created`, `repair.exit.confirmed`, `repair.entry.confirmed`, `repair.internal.created`, `repair.internal.corrected` |
| Tampões | `stopper.quantity.added`, `stopper.quantity.removed`, `stopper.configuration.changed`, `stopper.plan.updated` |
| Administração | `admin.user.created`, `admin.user.updated`, `admin.user.deactivated`, `admin.password_reset.requested`, `admin.option.updated`, `admin.access_template.updated` |

O catálogo deve ser versionado. Novas ações usam códigos novos e estáveis; não reutilizar um código antigo com significado diferente.

## 8. Permissões

- `audit.view`: consultar eventos no Admin;
- `audit.export`: exportar o registo anual;
- apenas utilizadores administradores autorizados recebem estas capacidades na V1;
- o título livre apresentado no cabeçalho nunca concede acesso à auditoria.

## 9. Critérios de aceitação

1. Cada comando relevante concluído cria exatamente um evento principal, com correlação para eventos auxiliares quando necessário.
2. O evento identifica utilizador, módulo, ação, entidade, data/hora e resultado.
3. Alterar ou corrigir um registo não remove o evento anterior.
4. O Admin filtra por ano, utilizador, módulo, ação e período.
5. As listas seguem o comportamento global: clique seleciona; duplo clique abre detalhe.
6. A paginação oferece 20, 40 e 60 linhas.
7. A auditoria não apresenta nem calcula pontuações, rankings ou avaliações automáticas.
8. Apenas capacidades autorizadas permitem consulta e exportação.
9. Segredos e documentos integrais não são copiados para o evento.


## END FILE CONTENT

---

# FILE 003

## Source Path
`docs\BOQUILHAS_INTERFACE_BEHAVIOR.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Registo de Boquilhas — especificação de interface

## 1. Objetivo

Este documento é o handoff funcional e visual para implementação. O mockup define a apresentação; este ficheiro define os comportamentos observáveis, regras de negócio, estados e critérios de aceitação. Não inventar nomes de campos, RPCs ou persistência a partir deste documento.

## 2. Estrutura global

- Tabs operacionais à esquerda: `Registo`, `Boquilhas`, `Histórico`.
- `Definições` fica isolado à direita.
- O painel lateral de linhas está fixo e presente em todas as páginas.
- O tab `Fabrico` não existe.
- O cabeçalho não contém ações duplicadas pelo painel lateral.

## 3. Sistema de botões

Todos os botões têm dois estados visuais:

1. Repouso: fundo preenchido com a cor da ação e texto branco.
2. Hover ou foco visível: fundo branco, contorno e texto na cor original.

Regras:

- Não usar `brightness`, transparência ou tom médio no hover.
- Ações destrutivas usam vermelho nos dois estados.
- Botões desativados são cinzentos e não reagem ao hover.
- Um botão selecionável pode manter o estado invertido enquanto estiver ativo.
- Foco por teclado deve permanecer visível.

## 4. Painel lateral de linhas

O painel mantém a leitura rápida das BQ em produção, mas também funciona como ligação ao contexto de produção:

- cada cartão mostra BQ, lote e quantidade;
- mostra a referência de produção completa, separada do identificador da BQ;
- clicar especificamente na referência completa abre o Job On ativo associado àquela referência/linha;
- clicar no restante cartão abre o registo da BQ;
- a navegação usa os IDs relacionados devolvidos pelo sistema e não reconstrói a ligação a partir do texto visível.

### Conteúdo

- Mostrar linha, referência, lote, quantidade e hora de início quando aplicável.
- Não mostrar o total global de BQ na fábrica.
- Não mostrar o selo `Em produção`.
- Linha ocupada, menu `…`: `Substituir` e `Remover`.
- Linha livre, menu `…`: `Adicionar`.

### Navegação

- Clicar no corpo de um cartão ocupado abre o respetivo lote no tab `Registo`.
- Clicar no menu `…` não abre o lote.
- Uma linha livre não abre registo; informa que deve ser usada a ação `Adicionar`.
- Se existirem vários lotes da mesma referência, pedir escolha do lote antes de abrir.

### Alerta de conflito

- Vários lotes da mesma referência na mesma linha são permitidos e não geram alerta.
- Referências diferentes na mesma linha geram alerta.
- O cartão em conflito usa contorno/indicador de atenção e o texto `Referências diferentes na mesma linha`.
- O cartão apresenta referências e lotes em conflito.
- Clicar num cartão em conflito não abre nenhum lote nem apresenta uma escolha.
- Mostrar apenas `Referências diferentes nesta linha. Remova uma referência para continuar.`
- A correção é feita pelo menu `…`, removendo ou substituindo uma referência.
- O alerta desaparece quando resta apenas uma referência distinta.

## 5. Registo

### Pesquisa

- Pesquisar por referência ou lote.
- Um resultado selecionado abre resumo, estado e ações do lote.
- Sem resultado, mostrar `Nenhuma boquilha encontrada`.

### Criar novo lote

- Não abrir nova página nem modal.
- O formulário expande na própria página `Registo`.
- O botão muda de `Criar novo lote` para `Fechar criação` enquanto o painel estiver aberto.
- `Fechar criação` e `Cancelar` fecham o painel.
- Se houver conteúdo introduzido, confirmar antes de o descartar.
- Depois de criar, fechar o formulário, selecionar o lote e mostrar o seu registo.

Ordem dos campos:

1. Boquilha.
2. Lote (compacto).
3. Total do lote (compacto, até três dígitos no mockup).
4. Utilização inicial (compacta, percentagem de vida útil).
5. Data de abertura (último campo, preenchida por defeito).
6. Linhas permitidas em cartões selecionáveis.
7. Observações compactas.

Não incluir:

- Linha associada.
- Escolha `Fabricar/Reparar`.

### Ações do lote

- `Saída`, `Entrada`, `Não reparadas`, `Corrigir contagem`, `Editar ficheiro`, `Fechar`.
- Não apresentar o indicador `Contagem reconciliada`.

### Resumo do lote ativo e fecho

Ao selecionar um lote ativo, apresentar três blocos:

1. Resumo de abertura/configuração: referência, lote, estado, total do lote, data de entrada/abertura, número de registos e linhas permitidas.
2. Estado atual calculado: na produção, em reparação, não reparadas, saídas excecionais, entradas excecionais e linha atual.
3. Movimentos do lote: lista completa e paginada com tipo, quantidade, saldo, data e operador, com acesso a impressão/PDF. Esta lista evita obrigar o utilizador a abrir e filtrar o Histórico global para consultar um único lote.

Na lista de movimentos do lote, usar distinção cromática subtil:

- `Saída`: fundo laranja muito claro e rótulo laranja.
- `Entrada`: fundo verde muito claro e rótulo verde.
- Não depender apenas da cor; manter sempre o texto `Entrada` ou `Saída`.

O resumo de abertura e o estado atual não são totais independentes introduzidos manualmente: devem resultar do lote e dos seus movimentos.

Ao fechar:

- Pedir confirmação explícita.
- Calcular e guardar um snapshot final imutável do resumo, estado atual e metadados de fecho.
- Guardar data/hora e utilizador que fechou o lote.
- Manter os movimentos originais ligados ao lote.
- Retirar o lote das listas de ativos e disponibilizá-lo no Histórico/arquivados.
- Alterações futuras de reparadores, linhas ou configurações não podem modificar o snapshot fechado.
- Se o fecho falhar, o lote permanece ativo e nenhum snapshot parcial é apresentado como válido.

## 6. Formulário de Entrada/Saída

- Modal compacto.
- Cabeçalho em destaque: tipo de movimento e `Referência · Lote · Linha`.
- Remover `Material/trabalho`.
- Primeira linha: Data, Quantidade, Motivo.
- Segunda linha: Detalhe e Observações.
- Quantidade e Data são campos compactos.
- Observações começa com uma linha e pode crescer.
- O placeholder de Detalhe depende do Motivo:
  - Normal: `Opcional`.
  - Movimento anterior: `Ex.: saída de 12 BQ em 12/08`.
  - Correção: `Ex.: quantidade registada incorretamente`.
  - Outro: `Indique brevemente a razão`.
- O aviso de correspondência fica escondido em `Movimento normal` e aparece nos restantes motivos relevantes.

## 7. Boquilhas

- Mostrar filtros: referência/lote/linha, estado e linhas por página.
- Estados: atuais, ativas, em produção, em reparação, disponível, arquivados, sucata e todos.
- Os cartões mostram quantidade, linha/localização/reparador e percentagem de vida utilizada.
- A percentagem representa tempo de vida/desgaste, não quantidade.
- Não usar barra de progresso para a percentagem.
- Os totais `Na fábrica`, `Em reparação` e `Em produção` não pertencem a esta página; passam para o Histórico.

## 8. Histórico

### Responsabilidade da página

- O `Registo` apresenta apenas o lote atualmente selecionado, incluindo todos os seus movimentos.
- O `Histórico` é a visão geral e transversal do sistema.
- O Histórico serve para pesquisar, agregar e comparar movimentos entre referências, lotes, linhas, reparadores e períodos.
- Não obrigar o utilizador a usar o Histórico para consultar os movimentos de um único lote já aberto no Registo.
- A mesma fonte de dados alimenta ambas as páginas; muda apenas o âmbito da consulta.

### Organização

- Topo: calendário à esquerda e cartões de resumo à direita.
- Abaixo: filtros em largura total.
- Depois: tabela de movimentos em largura total.
- Os cartões respondem ao período e aos filtros aplicados.

### Filtros

- Referência, lote ou linha.
- Data/período.
- Tipo de movimento.
- Reparador.
- Estado do ficheiro.
- Linhas por página.
- `Mostrar todos os dias` remove a seleção de data.
- `Limpar filtros` restaura os valores por defeito.

### Tabela

Colunas:

- Referência.
- Lote.
- Movimento (`Entrada` ou `Saída`, sem texto redundante).
- Quantidade.
- Saldo.
- Reparador.
- Linha.
- Data e hora.
- Operador.

Não incluir `Detalhe` nem `Ficheiro` como colunas.

Interações:

- Um clique seleciona a linha e ativa `Corrigir movimento` e `Eliminar movimento`.
- A seleção fica visualmente marcada.
- Duplo clique abre o lote correspondente no tab `Registo`.
- Alterar filtros limpa a seleção e desativa as ações.
- Eliminar exige confirmação.

## 9. Definições — reparadores

- Usar uma tabela compacta com uma linha por B1, B2, B3, C1, C2 e C3.
- Cada linha configura um reparador predefinido e reparadores permitidos.
- O predefinido é sugerido ao criar uma saída, mas pode ser alterado no movimento.
- `Sem associação` é permitido e visível.
- Uma secção separada gere a lista de reparadores.
- Reparadores antigos são desativados, não eliminados, para preservar o Histórico.
- Se o predefinido for desativado, a linha exige nova associação.
- O movimento guarda o reparador efetivamente escolhido; mudanças posteriores de configuração não alteram o histórico.

## 10. Acessibilidade e feedback

- Todos os elementos clicáveis funcionam por teclado.
- Foco visível em botões, tabs, cartões e linhas selecionáveis.
- Menus e modais fecham com `Escape` na implementação final.
- Mensagens de sucesso são discretas e não bloqueantes.
- Confirmar apenas ações destrutivas ou perda de dados preenchidos.

## 11. Critérios mínimos de aceitação

- Não existe tab `Fabrico`.
- Criar lote é inline e pode ser fechado com proteção contra perda de dados.
- Cartões laterais abrem o lote; o menu não propaga o clique.
- Só referências distintas na mesma linha geram alerta.
- Um clique na tabela seleciona; duplo clique abre o lote.
- Histórico filtra por reparador e lote e recalcula os resumos.
- Botões usam apenas os dois estados definidos.
- Percentagem de utilização nunca é representada como quantidade.
- Configuração de reparadores preserva movimentos históricos.


## END FILE CONTENT

---

# FILE 004

## Source Path
`docs\CODER_IMPLEMENTATION_HANDOFF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Portal DMO — handoff técnico completo para implementação

Estado: especificação funcional e visual V1  
Destinatário: programador responsável pela implementação real  
Regra: os HTML deste pacote demonstram a interface e as interações; não substituem domínio, autorização, persistência, auditoria, cálculo, PDF ou email.

## 1. Começar aqui

O Portal DMO é uma aplicação única com módulos separados. A shell, a identidade, os componentes e as regras de interação são partilhados, mas os domínios não devem ser fundidos.

Ordem mínima de leitura:

1. este documento;
2. `DMO_DESIGN_SYSTEM.md`;
3. o brief específico do módulo;
4. o HTML correspondente;
5. `dmo-design-system.css` e `dmo-interactions.js`.

Quando o mockup, um texto auxiliar e o brief parecerem contraditórios, prevalece:

1. regra funcional explícita mais recente no brief;
2. regra transversal do Design System;
3. mockup visual;
4. dados fictícios presentes no HTML.

Não inferir regras de negócio a partir de valores de demonstração.

## 2. Modelo mental da aplicação

### Shell única

- Login único.
- Header global com logótipo, nome do módulo, descrição curta, nome do utilizador e título/função.
- Navegação e módulos disponíveis dependem do template de acesso do utilizador; a landing page após autenticação é sempre Job On.
- `Definições` fica no extremo direito das tabs quando existir.
- O título/função visível no header é texto livre gerido pelo Administrador. Não concede permissões.
- A autorização deve ser validada no servidor em cada comando; esconder botões não é segurança.

### Módulos e responsabilidade

| Módulo | Responsabilidade principal | Não deve fazer |
|---|---|---|
| Login | autenticar e encaminhar | decidir permissões no cliente |
| Administração | utilizadores, título, templates e aplicações | executar fluxos operacionais |
| Boquilhas | lotes BQ, movimentos, reparação e tracking | gerir CM/MF |
| Peso — Operador | criar controlos, calcular e enviar para aprovação | aprovar o próprio controlo |
| Peso — Responsável | analisar, aprovar/rejeitar e decidir comparações por CM | registar leituras operacionais |
| Job On | folha com a informação necessária para produzir | ser catálogo mestre de ferramentas |
| Armazém | localização, entradas, saídas e recolhas programadas | guardar estado técnico que pertence à ferramenta |
| Reparação interna | intervenções CM/MF durante produção | substituir a reparação externa programada |
| Reparação externa | ciclo de envio e retorno de BQ/CM/MF | fundir os três domínios |
| Moldes | registo de CM e MF em áreas separadas | combinar CM e MF num único tipo |
| Tampões | quantidades por configuração e planeamento | identificar peças individualmente |
| Pegamentos | controlo dimensional contextualizado | criar os catálogos de CM/BQ/MF |

## 3. Regras visuais globais obrigatórias

### Tokens

Usar apenas variáveis de `dmo-design-system.css`. Não introduzir cores, alturas, raios, sombras ou espaçamentos isolados na página sem primeiro os transformar num token reutilizável.

Base visual:

| Uso | Token/valor canónico |
|---|---|
| Marca principal | `--dmo-brand-600: #3c73a8` |
| Texto principal | `--dmo-text: #172d42` |
| Texto secundário | `--dmo-muted: #64778a` |
| Página | `--dmo-page: #f6f9fc` |
| Cartão | `--dmo-card: #fff` |
| Superfície suave | `--dmo-subtle: #f1f6fa` |
| Contorno | `--dmo-border: #d9e6f2` |
| Sucesso | `--dmo-success: #527c72` |
| Aviso | `--dmo-warning: #a97943` |
| Perigo | `--dmo-danger: #9a625d` |
| Altura de campo/filtro | `40px` |
| Botão normal | mínimo `36px` |
| Botão em linha de filtros | `40px` |
| Seta de paginação | `36 × 36px` |
| Ação mobile | mínimo `44px` |
| Raio de campo | `8px` |
| Raio de cartão | `12px` |

### Botões

- Repouso: fundo preenchido com a cor semântica, contorno igual e texto branco.
- Hover e foco: fundo branco, contorno e texto com a cor que preenchia o botão.
- Não aplicar apenas brilho/brightness.
- Disabled: visual neutro e sem ação.
- Perigo usa o vermelho moderado do sistema, nunca vermelho vivo.
- Botões contextuais ficam fora da tabela/lista.
- A largura acompanha o texto; evitar botões largos sem necessidade.

### Campos

- Altura canónica de 40px.
- Largura proporcional ao conteúdo esperado: máquina 2 caracteres, lote normalmente curto, percentagem 3 algarismos, data com largura própria.
- Textarea só quando o conteúdo é realmente longo.
- Placeholder deve exemplificar o formato ou a relação esperada.
- Foco: contorno de marca mais halo discreto.
- Datas aparecem no fim da linha quando essa ordem foi definida no fluxo.

### Cartões e densidade

- Um cartão representa um contexto ou tarefa, não cada valor isolado.
- Evitar espaços vazios e cartões-resumo sem ação ou significado.
- Informação crítica tem maior contraste/tamanho; metadados têm contraste secundário.
- Não esconder informação funcional necessária apenas para tornar o layout menor.

## 4. Contrato universal de listas

Todas as listas operacionais seguem o mesmo comportamento:

1. um clique seleciona uma linha;
2. a seleção fica visualmente mais escura;
3. ações externas passam a atuar sobre a linha selecionada;
4. duplo clique abre o detalhe/folha/registo associado;
5. teclado: linha focável via padrões web nativos; não existe atalho específico do BA DMO (clique seleciona, duplo clique abre);
6. seleção não executa uma mutação;
7. mudar filtros limpa uma seleção que deixou de estar visível;
8. após corrigir/eliminar, atualizar a lista e a paginação;
9. estado vazio explica por que não existem resultados;
10. estado de erro preserva filtros e permite repetir.

Paginação:

- limites disponíveis: 20, 40 e 60;
- mostrar total, página atual e total de páginas;
- setas junto às restantes ações da lista, alinhadas;
- anterior/seguinte ficam disabled nos limites;
- o servidor deve paginar listas grandes; o cliente não deve carregar tudo por defeito.

## 5. Contrato universal de filtros

- Pesquisa textual e filtros específicos vivem num cartão compacto.
- Botão `Limpar filtros` repõe o estado inicial e volta à primeira página.
- A lista, contadores e cartões-resumo respondem aos mesmos filtros quando pertencem ao mesmo contexto.
- Filtros devem poder ser representados na URL quando o utilizador é encaminhado de outra página.
- Datas usam intervalo `Desde`/`Até`; validar `Desde <= Até`.
- Dropdowns usam a aparência e hover do sistema; não usar menus brancos enormes para três opções.
- Quando uma Referência encaminha para Histórico, abrir com a referência já aplicada.

## 6. Contrato universal de calendários

- Reutilizar exatamente o componente canónico, não recriar por módulo.
- Cabeçalho: mês/ano e setas anterior/seguinte.
- Semana com sete colunas estáveis.
- Dia com registos: indicador discreto.
- Dia selecionado: fundo de marca e texto branco.
- Um clique filtra/seleciona o dia.
- Ação `Mostrar todas as datas/dias` remove o filtro diário.
- O calendário não pode dominar a página: largura aproximada de 300–340px em desktop quando está ao lado da lista.
- Em mobile passa para cima da lista.
- No Peso, substituir qualquer variante restante pelo mesmo calendário de Boquilhas.

## 7. Estados, feedback e auditoria

### Registo global de ações

- Aplicar `AUDITORIA_GLOBAL_HANDOFF.md` a todos os módulos e a todos os utilizadores autenticados.
- Cada comando relevante cria um evento append-only associado ao utilizador, módulo, ação, entidade, data/hora e resultado.
- A auditoria é criada pelo backend e acompanha a transação do domínio; não depende de JavaScript no browser.
- O Admin disponibiliza consulta anual, filtros, detalhe e exportação autorizada.
- O sistema regista ações factuais; não calcula pontuações, rankings ou avaliações automáticas.

### Operações assíncronas

Todo comando deve ter:

1. estado inicial;
2. loading e proteção contra duplo envio;
3. sucesso com mensagem curta;
4. erro com causa utilizável;
5. atualização dos dados afetados;
6. auditoria com utilizador e data/hora.

### Correções

- Corrigir não apaga silenciosamente a realidade anterior.
- Guardar valor anterior, valor novo, justificação, operador e timestamp.
- Eliminar movimentos exige confirmação e deve respeitar a política de auditoria do domínio.
- Um registo histórico é snapshot e não deve mudar quando o estado mestre atual muda.

### Estado visual

- `Pendente`, `Aprovado`, `Não aprovado`, `Por decidir`, `Manter` e `Colocar de parte` usam a mesma paleta moderada em todas as páginas.
- Cor nunca é o único indicador; manter texto explícito.

## 8. Login e Administração

### Login

- Email e palavra-passe.
- Erro genérico para credenciais inválidas.
- Após autenticação, encaminhar todos os utilizadores para Job On.
- Administrador também entra em Job On e abre Administração através da navegação quando necessário.
- Não persistir palavra-passe no browser.

### Administração

- Gerir utilizadores, título/função visual, templates de acesso e aplicações.
- A tab `Auditoria` consulta o histórico anual de ações de todos os módulos.
- Filtros mínimos: ano, utilizador, módulo, ação, resultado e período.
- Lista canónica: um clique seleciona e duplo clique abre o detalhe do evento.
- Paginação: 20, 40 ou 60 linhas.
- Apenas capacidades `audit.view` e `audit.export` autorizam consulta e exportação.

### Administração

Lista de utilizadores com pesquisa, template/perfil, estado e título/função.

Ações:

- criar/ativar/desativar utilizador;
- alterar template de acesso;
- editar título/função livre mostrado no header;
- reset de palavra-passe;
- gerir módulos/capacidades do template.

Separar:

- identidade de autenticação;
- perfil interno;
- título visual;
- template e capacidades.

O título `Responsável de qualidade`, `Chefe`, `Engenheiro`, etc. é uma variável do perfil e não uma role técnica.

## 9. Boquilhas

Documento de autoridade: `BOQUILHAS_INTERFACE_BEHAVIOR.md`.

### Estrutura

- Tabs: `Registo`, `Boquilhas`, `Histórico`, `Definições`.
- Remover tab `Fabrico`.
- Side panel fixo em todas as páginas do módulo.
- Botões antigos do header desaparecem; o side panel oferece o contexto de linhas.

### Side panel

Cada linha B1–C3 mostra:

- linha;
- referência completa ativa;
- lote(s) dessa referência;
- quantidade de BQ;
- estado temporal quando necessário.

Clicar na referência abre o respetivo lote/Job On conforme o contexto definido. Menu `…`:

- com referência: `Substituir` e `Remover`;
- sem referência: `Adicionar`;
- menu compacto, integrado no fundo escuro, sem branco excessivo.

Regra crítica: uma linha não pode ter duas referências diferentes. Pode ter dois lotes da mesma referência. Se dados existentes violarem a regra, mostrar alerta no cartão e pedir para remover/substituir uma referência; nunca abrir um prompt nativo do browser.

### Novo lote

- Abre inline na mesma página e pode ser fechado/cancelado.
- Não apresenta `Fabricar/Reparar`.
- Campos compactos: Boquilha, Lote, Total, Utilização inicial e Data no fim.
- Não existe dropdown `Linha associada`; máquinas/linhas são escolhidas nos cartões `Linhas permitidas`.
- Utilização é tempo de vida, não quantidade.
- Botões de linha são selecionáveis e mostram check legível.

### Lote ativo

- Resumo do lote e estado atual permanecem disponíveis.
- Mostrar movimentos apenas desse lote; o Histórico serve visão geral e comparação.
- Retirar `Contagem reconciliada`.
- Entrada/Saída usa cabeçalho grande com Referência, Lote e Linha.
- Formulário compacto: Data, Quantidade e Motivo na primeira área; Detalhe abaixo; Observações apenas quando necessário; retirar Material/Trabalho.
- Entrada e Saída na lista podem ter fundos subtilmente diferentes.

### Histórico

Campos mínimos: Referência, Lote, Movimento (`Entrada`/`Saída`), Quantidade, Saldo, Reparador, Linha, Data/hora e Operador.

Filtros: pesquisa, data/período, movimento, reparador, ficheiro/estado quando aplicável e limite 20/40/60.

## 10. Peso e Volume

Documento de autoridade: `PESO_INTERFACE_HANDOFF.md`.

### Origem dos dados

- Processo `NNPB/PS` é escolhido na criação do lote no módulo Peso. Não é pedido novamente no Novo controlo nem na Comparação.
- Máquinas permitidas e Processo pertencem ao contexto do lote do Peso usado nessa produção.
- O Job On fornece a ligação operacional à Referência, Produção e Máquina e identifica a instância concreta de CM usada nessa produção, incluindo o lote. Exemplo: `Produção 202601 · CM 5447 · Lote 4`.
- O lote do Peso referido pelo Job On fornece Processo e restantes dados técnicos; o controlo não volta a selecionar CM ou lote.
- Todo Novo controlo e toda Comparação guardam e mostram a referência ao Job On daquela produção.
- Valores calculados vêm do motor de cálculo do domínio.
- Todas as casas decimais apresentadas são limitadas a duas.

### Operador

- Existem dois tipos de registo: `Novo controlo` e `Comparação`.
- `Novo controlo` inicia no contexto do Job On da produção e controla os valores previstos/introduzidos para essa produção.
- `Comparação` é posterior: mede CM que já estão em produção e compara-os com o Novo controlo aprovado associado ao mesmo Job On.
- Cria e edita controlos.
- Leituras CM podem ser adicionadas/removidas realmente.
- Em cada leitura mostrar CM, peso em água e peso estimado em vidro em tempo real.
- `Calcular` recalcula os campos atuais; não é apenas uma decoração.
- Resultados em cartões compactos e tabela por CM.
- Botões `Adicionar leitura`, `Remover leitura`, `Calcular` e `Enviar para aprovação` são consistentes entre Novo controlo e Comparação.
- Retirar progress cards de processo e ações duplicadas de produção.

Campos compactos: máquina, lote e temperatura. Aumentar Notas; reduzir `Fim da produção anterior (SAP)` e `Peso médio anterior (SAP)`.

### Referências

Lista compacta sem scroll horizontal em desktop normal. Um clique seleciona; duplo clique encaminha para Histórico com a referência aplicada. Ações `Editar`, `Novo controlo` e `Comparar` ficam fora da lista.

Editar uma referência aprovada:

- exige justificação;
- retira o estado aprovado;
- cria nova revisão;
- volta a pedir aprovação.

Criar a Referência define a identidade mestre. Criar o lote no Peso inclui NNPB/PS e máquinas permitidas; estes valores são herdados pelos controlos através do contexto do Job On.

Na criação do lote existe também `Subpasta dos relatórios`. É um nome relativo, por exemplo `5447T173`, resolvido por baixo do diretório principal configurado, por exemplo `Capacidades / 5447T173`.

### Comparação

Compara CM que já estão em produção com a média do Novo controlo aprovado ligado ao mesmo Job On/produção.

- Não altera o controlo aprovado base.
- Mantém Job On, Produção e contexto da Referência ativa no topo.
- Mantém Data do registo da comparação, leituras, resultados e botão Calcular.
- Retirar `Procurar comparação` do rodapé depois da base já estar escolhida.
- Enviar para aprovação cria registo adicional de comparação.

Responsável decide cada CM individualmente. Tabela mínima:

- CM;
- Peso atual;
- Capacidade atual;
- Média aprovada;
- Capacidade aprovada;
- Diferença;
- Decisão `Manter` ou `Colocar de parte`.

### Responsável

- Não regista pesos.
- Vê calendário, lista pendente, detalhe completo e histórico.
- Aprova/rejeita controlos normais.
- Em comparações decide CM a CM.
- Rejeição exige nota.

Depois de aprovado, pode gerar folha ou enviar email para produção. Destinatários são escolhidos automaticamente pela linha/máquina e configurados em Definições; assunto/mensagem aceitam variáveis documentadas.

## 11. Job On

Documentos de autoridade: `JOB_ON_DESIGN_BRIEF.md` e `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`.

Definição: **Job On é a folha onde a equipa obtém toda a informação necessária para produzir**.

Job On é também a landing page global do Portal. Todos os utilizadores autenticados podem consultar; apenas o papel/template técnico `Responsável` pode editar. Validar no backend:

- `jobon.view`: todos os utilizadores ativos;
- `jobon.edit`: apenas Responsável;
- `jobon.configure`: apenas Responsável;
- criação, duplicação, substituição de ferramenta, alteração de datas/campos e gravação de revisão exigem `jobon.edit`;
- confirmação operacional de verificações usa capability própria e não concede edição estrutural da folha.

Ocultar `Editar folha`, `Criar Job On` e `Definições` aos restantes perfis, mas nunca usar a ocultação como controlo de autorização.

### Hierarquia

Informação instantânea:

1. Data início/fim;
2. Máquina/Linha;
3. Referência e Produção;
4. MP/CM, MF e BQ, incluindo referência e lote;
5. imagem do artigo.

Informação secundária permanece presente: PU, CAL, AN, ARR, PI, CS, TP, FO, parâmetros, notas e verificações.

### Comportamento

- Abre por defeito em `Modo consulta`, com aparência de folha técnica.
- `Editar folha` ativa campos apenas para utilizadores autorizados.
- Guardar fecha edição e volta a consulta.
- Não cria ferramentas mestre; escolhe lotes existentes dos módulos de domínio.
- Upload de imagem permite pré-visualização e armazenamento associado à folha/Referência.
- Planeamento é tab separada com calendário compacto.
- Um clique numa produção seleciona; duplo clique abre a folha.

### Criar

- `Novo em branco` cria template vazio para Referência sem Job On anterior.
- `Duplicar anterior` procura o Job On anterior da mesma Referência, copia conteúdo e limpa/atualiza a Data.
- `Duplicar histórico selecionado` usa explicitamente a linha escolhida.
- Criar Job On fica fora dos cartões/listas.
- A data planeada pode ser alterada; atualizar também a marcação no calendário e preservar auditoria.
- Data início e Data fim do Job On são a única fonte da marcação/range no calendário. Guardar uma alteração cria nova revisão, atualiza a projeção do calendário e preserva as datas antigas no histórico.
- O Histórico permite primeiro filtrar uma Referência e ver todas as suas Produções. Um clique seleciona e duplo clique abre; dentro da Produção, o utilizador pode consultar as revisões imutáveis.

### Ferramentas

Ao editar MP/CM, MF ou BQ:

- pesquisa por Referência/número;
- opções de lote são filtradas por Referência e Máquina;
- a lista mostra apenas lotes compatíveis existentes;
- selecionar lote atualiza os dados apresentados na folha;
- não existir resultado significa que não existe ferramenta compatível registada; não inventar.

Depois de o Job On ser guardado, as escolhas de CM/MP, MF e BQ são instâncias concretas, com IDs e lotes. Peso e Pegamentos consomem essas escolhas como dados não editáveis. Não voltam a filtrar para escolher uma alternativa. Se uma ferramenta obrigatória estiver ausente ou inválida, o utilizador corrige o Job On.

O Job On tem dois modos:

- `Consulta`: folha estritamente não editável; não adiciona, remove, substitui nem duplica.
- `Edição`: todos os campos da folha ficam editáveis, incluindo PU, CAL, AN, ARR, PI, CS, TP, FO, quantidades e notas; ativa também duplicação e `Alterar` em CM/MP, MF e BQ.

Ao alterar uma ferramenta, abrir lista canónica filtrada pela Referência da ferramenta e Máquina do Job On. Permitir refinar por lote, localização/contexto e estado. A linha agrega Referência, Lote, Nome técnico, compatibilidade, posição do Armazém, contexto atual, estado técnico e `% de uso`. Posição vem do Armazém; estado/% vêm do domínio da ferramenta. Selecionar não cria Saída nem altera o Armazém; apenas associa o ID/lote ao rascunho do Job On.

### Persistência do Job On

- Implementar o esquema e os limites de ownership descritos em `JOB_ON_DATA_MODEL.md`.
- Guardar uma fotografia completa por Job On/revisão, não apenas referências a entidades mestre.
- Revisões guardadas são imutáveis. Guardar uma edição cria uma revisão e respetivos filhos novos, atualizando apenas o apontador de revisão corrente; nunca altera os valores da revisão anterior.
- O snapshot inclui o contexto e todos os grupos visíveis. CAL guarda cada linha (`Elemento`, `Valor`, `Qtd. máquina`); PI guarda Pinças, Diâmetro e Notas; os restantes grupos guardam todos os seus campos, quantidades e notas.
- CM/MP, MF e BQ guardam `toolId`/`lotId` e também snapshot legível dos valores usados na produção.
- Estado técnico, `% de uso`, posição e movimentos permanecem nas bases das ferramentas/Armazém; são consultas live durante a seleção e não são copiados como estado atual do Job On.
- Editar o Job On altera apenas o snapshot/revisão do Job On. Nunca altera a ficha mestre nem a localização da ferramenta.
- Nenhum estado, localização, percentagem ou incompatibilidade bloqueia a escolha/gravação; apresentar aviso e deixar a decisão ao utilizador autorizado.
- Duplicar copia o snapshot completo. Apenas o novo ID, Produção e datas são preparados para o novo fabrico; todos os campos copiados podem ser alterados antes de guardar.
- Exemplo de aceitação: duplicar `202601 · 5447T173` para `202602` copia a configuração de PI/CAL/pinças usada em 202601 e permite substituí-la livremente em 202602, sem alterar 202601.
- Aprovações, históricos e documentos guardam o `job_on_revision_id` utilizado, para que uma correção posterior não altere retroativamente o que foi visto ou emitido.
- A interface não tem de imitar fotograficamente a folha histórica. Deve apresentar os campos pela forma mais clara para consulta e edição, mantendo todos os valores acessíveis através das tabelas normalizadas do snapshot.
- Dropdowns de negócio evolutivos são data-driven e administrados em Definições por Família/Campo. Permitir adicionar, editar, ordenar e desativar; nunca apagar retroativamente valores guardados nos snapshots. Ver `job_on_field_option` em `JOB_ON_DATA_MODEL.md`.

### Verificações

Regras configuradas na ficha da ferramenta/lote geram ocorrências no Job On:

- `Uma vez neste lote`: aparece até ao primeiro check desse lote;
- `Por fabrico`: cria ocorrência em cada Job On/fabrico.

Guardar quem confirmou e quando. O responsável pode desativar, reativar, apagar ou fazer reset conforme permissões e regras do brief.

A confirmação de uma ocorrência é manual e só existe depois de persistida. Não inferir a partir de outros módulos; guardar `completion_source=manual_job_on`, utilizador, data/hora e revisão.

## 12. Armazém

Documento de autoridade: `ARMAZEM_DESIGN_BRIEF.md`.

### Limite

Armazém regista onde a ferramenta está e os movimentos. `% vida`, sucata, arquivo e estado técnico pertencem ao domínio da ferramenta; no Armazém existe apenas Observações quando necessário.

### Entrada

- Escolher tipo CM/MF/BQ.
- Encontrar a ferramenta nos catálogos existentes.
- Registar posição e observações.
- Uma entrada numa posição ocupada é bloqueada pelo estado registado do sistema.
- A aplicação não consegue detetar fisicamente uma ferramenta não registada naquela posição; não apresentar essa inferência como facto.

### Saída

- Ao confirmar a saída, libertar imediatamente a posição registada.
- Destino: Produção ou Reparação.
- Para Reparação, reparador vem das associações do domínio da ferramenta.

### Saída programada

Manager/Reparação cria lista de lotes a recolher. Armazém vê e pode imprimir a lista. Operador confirma cada ferramenta com check. Quando todas estiverem confirmadas:

- finalizar saída;
- libertar posições;
- guardar operador e data/hora;
- criar registo histórico.

Quando regressam e recebem entrada no Armazém, fechar o ciclo e guardar dados de entrada. Linha concluída usa estado visual discreto verde/cinza.

### Consulta

Filtros, paginação 20/40/60 e últimos movimentos. Alertas apenas com base nos dados registados: duplicação lógica de posição ou ferramenta sem contexto operacional registado.

## 13. Reparação interna

Documento de autoridade: `REPARACAO_INTERNA_DESIGN_BRIEF.md`.

- Operador escolhe Linha B1–C3 em cartões no topo, ocupando toda a largura disponível sem overflow.
- O sistema carrega automaticamente Referência, Produção e Job On ativos da linha naquele dia.
- Depois escolhe `CM` ou `MF` e introduz o número individual.
- Guardar cria intervenção com linha, produção, referência, lote, tipo, número, operador e timestamp.
- Últimos registos ficam abaixo, nunca ao lado do seletor principal.
- Histórico permite filtros, seleção, duplo clique e correção auditada.
- Sem produção ativa, mostrar estado explícito e impedir associação automática falsa.

## 14. Reparação externa e Moldes

Documento de autoridade: `REPARACAO_EXTERNA_DESIGN_BRIEF.md`.

- Reparação externa acontece antes/fora da produção e é diferente da reparação interna.
- Boquilhas, CM e MF podem partilhar a shell e o ciclo de saída/retorno.
- CM e MF são domínios separados, mesmo quando partilham Referência.
- `Contra moldes` e `Moldes finais` são tabs do módulo, não dois botões de estado dentro da página.
- A tab ativa é preenchida; a inativa começa branca/outline.
- Não mostrar cartões `Produções ativas` se não forem necessários ao fluxo externo.
- Listas usam a regra universal; ações ficam fora.

## 15. Tampões

Documento de autoridade: `TAMPOES_DESIGN_BRIEF.md`.

- Unidade de consulta é a configuração técnica, por exemplo diâmetro + calote.
- Diâmetro e calote são escolhidos de opções configuradas, não texto livre repetido.
- Operador tem liberdade para adicionar/remover quantidades e alterar configuração.
- Selector de estado: `Enchidos` / `Por encher`.
- Alterar de calote 4mm para 7mm é transformação atómica: subtrair da origem e adicionar ao destino no mesmo comando/auditoria.
- Planeamento permite consultar quantidades disponíveis antes da produção.
- Histórico mostra configuração anterior/nova, movimento, quantidade, saldo antes/depois, operador e data.
- Interface mobile-first para consulta no telemóvel.

## 16. Pegamentos

Documento de autoridade: `PEGAMENTOS_INTERFACE_HANDOFF.md`.

- Ao abrir a tab, selecionar/receber o Job On da produção.
- A folha só abre depois de o Job On fornecer Referência, Produção, Máquina e as instâncias/lotes obrigatórios de CM, BQ e MF.
- CM vem do lote escolhido no Job On a partir do Peso; BQ do lote escolhido no Job On a partir de Boquilhas; MF do lote escolhido no Job On a partir do respetivo domínio.
- Pegamentos não apresenta uma segunda escolha de ferramentas. Se faltar uma ferramenta, bloquear e encaminhar para correção do Job On.
- Campo `Contra costura` substitui a designação antiga incorreta.
- Lista Referências: clique seleciona, duplo clique abre; sem botão `Abrir folha selecionada`.
- Remover base de dados/importar/apagar local e ações de impressão duplicadas.
- Manter apenas `Imprimir / Guardar PDF` quando aplicável.

## 17. Origem dos dados e ownership

| Dado | Fonte de verdade esperada | Consumidores |
|---|---|---|
| Utilizador/template/capacidades | Administração/identidade interna | shell e comandos |
| Título/função | perfil interno | header |
| Referência Peso | catálogo mestre do Peso | Peso, Job On, Pegamentos |
| Lote do Peso, processo NNPB/PS e máquinas permitidas | criação/gestão de lotes no Peso | Peso, Job On, Pegamentos |
| Lote CM | módulo Peso/CM definido no domínio | Job On, Pegamentos, Armazém |
| Lote BQ | Boquilhas | Job On, Pegamentos, Armazém |
| Lote MF | módulo MF; manual apenas onde explicitamente temporário | Job On, Pegamentos, Armazém |
| Produção ativa por linha | Job On/planeamento de produção | side panel, reparação interna |
| Localização atual | Armazém | pesquisa e recolhas |
| Reparadores permitidos | definição do domínio da ferramenta | reparação e armazém |
| Job On da produção | Job On/planeamento | Novo controlo, Comparação e restantes consumidores do contexto de produção |
| CM/BQ/MF concretos e respetivos lotes da produção | escolhas guardadas no Job On | Peso, Pegamentos e folha Job On |
| Novo controlo aprovado do Job On | Peso | Comparação e produção |
| Destinatários de email | Definições do Peso por grupo de linhas | envio de controlos aprovados |

Não copiar estes dados para campos paralelos sem necessidade. Guardar IDs e snapshots históricos apropriados.

### Fronteira servidor/local dos relatórios

- O servidor guarda os dados estruturados de Peso e Pegamentos, incluindo números, resultados, estado, revisão, auditoria e `jobOnId`.
- Os PDFs aprovados/enviados para Produção são guardados no computador/local configurado; não são a fonte primária do histórico.
- `Definições` fornece o diretório principal, por exemplo `Capacidades`.
- A criação do lote no Peso fornece a subpasta relativa, por exemplo `5447T173`.
- O caminho resolvido é `diretório principal / subpasta do lote` e é partilhado por Peso e Pegamentos associados ao mesmo Job On/lote.
- Os nomes dos ficheiros são derivados do snapshot do Job On/controlo, nunca introduzidos como identificação paralela pelo operador.
- A interface distingue sucesso de persistência no servidor de sucesso de escrita local. Uma falha local não desfaz uma aprovação nem apaga dados numéricos.
- O histórico do servidor continua disponível noutro computador; o PDF só abre onde a pasta local/partilhada estiver acessível e autorizada.

### Fluxo ponta a ponta: Job On → Peso/Pegamentos → Produção

1. No Peso, criar o lote e definir Processo NNPB/PS, máquinas permitidas e `Subpasta dos relatórios`.
2. No Job On, identificar Referência, Produção e Máquina; em `Modo edição`, escolher CM/MP, MF e BQ concretos com os respetivos lotes.
3. A lista de escolha do Job On consulta disponibilidade live: posição/contexto pelo Armazém e estado técnico/% de uso pelo domínio da ferramenta.
4. Guardar o Job On persiste IDs/lotes associados. Não cria um movimento de Armazém.
5. `Novo controlo` do Peso recebe desse Job On a Produção e o CM/lote exatos. Não apresenta outro seletor de CM.
6. `Pegamentos` recebe do mesmo Job On a Produção e os CM, BQ e MF/lotes exatos. Não apresenta seletores alternativos.
7. Peso e Pegamentos guardam os valores estruturados no servidor com `jobOnId` e snapshot das ferramentas usadas.
8. Depois do estado/aprovação aplicável, gerar o PDF de Produção a partir do snapshot, resolver `diretório principal / subpasta do lote` e escrever localmente.
9. O nome do ficheiro é derivado do Job On/snapshot. O servidor regista o evento de geração/envio e respetivo resultado, sem tratar o PDF como substituto dos dados numéricos.
10. Se uma ferramenta estiver em falta/inválida, corrigir o Job On. Se a pasta local falhar, preservar o registo aprovado e permitir repetir apenas a geração/gravação do PDF.

Não implementar:

- associação apenas por texto de Referência/Produção sem `jobOnId` e IDs das ferramentas;
- escolha automática de outro lote dentro de Peso ou Pegamentos;
- cópia da posição, estado técnico ou `% de uso` para propriedades live do Job On;
- gravação de PDF no servidor como única fonte do histórico;
- caminho absoluto introduzido livremente na criação do lote.

## 18. Contratos técnicos recomendados

### Comandos

Cada mutação recebe:

- ID do agregado/registo;
- versão/concurrency token quando aplicável;
- dados explícitos da ação;
- justificação em correções/reaberturas;
- identidade obtida da sessão, nunca enviada como verdade pelo cliente.

### Respostas

Devolver:

- estado atualizado;
- versão nova;
- mensagens de validação por campo;
- evento/audit ID quando útil;
- dados necessários para atualizar cartões, lista e paginação sem recarregar informação não relacionada.

### Histórico

Para movimentos e aprovações guardar pelo menos:

- tipo de evento;
- agregado e registo afetado;
- antes/depois ou delta suficiente;
- operador;
- timestamp UTC e apresentação no fuso local;
- justificação;
- origem da ação.

## 19. Responsividade e acessibilidade

- Desktop: conteúdo usa a largura disponível; evitar uma coluna estreita perdida no centro de ecrãs grandes.
- Side panel fixo só quando a largura o permite; mobile usa drawer/área recolhível.
- Tabelas podem ter scroll horizontal apenas em ecrãs pequenos e só depois de compactar colunas.
- Campos e botões têm labels acessíveis.
- Não usar `prompt`, `alert` ou `confirm` nativos para fluxos operacionais; usar modal do sistema.
- Foco visível, ordem de tab lógica e ações possíveis por teclado.
- Alvos mobile mínimos de 44px.
- Respeitar contraste AA.

## 20. Critérios de aceitação da implementação

### Shell

- [ ] Header igual em todos os módulos.
- [ ] Título do perfil vem do Administrador.
- [ ] Template controla módulos e comandos no servidor.
- [ ] Definições alinhadas à direita.

### Componentes

- [ ] Botões seguem filled → inverted hover.
- [ ] Campos/filtros têm 40px.
- [ ] Listas seguem clique seleciona / duplo clique abre; sem atalho de teclado específico do BA DMO.
- [ ] Paginação oferece 20/40/60.
- [ ] Calendário é o mesmo componente em todos os módulos.
- [ ] Modais substituem prompts nativos.

### Dados

- [ ] UI não inventa entidades ou estados.
- [ ] Snapshots históricos permanecem imutáveis.
- [ ] Correções têm justificação e auditoria.
- [ ] Identidade vem da sessão autenticada.
- [ ] Datas e números respeitam formatação PT-PT e duas casas quando aplicável.

### Fluxos críticos

- [ ] Admin entra em Administração.
- [ ] Operador Peso não aprova; Responsável não regista leituras.
- [ ] Comparação não altera controlo aprovado base.
- [ ] Uma linha BQ não aceita referências diferentes.
- [ ] Dois lotes da mesma Referência BQ são permitidos.
- [ ] Job On abre em consulta e contém toda a informação necessária para produzir.
- [ ] Saída do Armazém liberta posição ao confirmar.
- [ ] Reparação interna usa produção ativa da linha.
- [ ] CM e MF permanecem separados.

## 21. Não implementar a partir dos mockups

Os seguintes elementos são demonstração e precisam de serviço real:

- utilizadores, emails e palavras-passe fictícios;
- números, datas, saldos e contagens de exemplo;
- timeouts e mensagens simuladas;
- fórmulas JavaScript simplificadas;
- geração de PDF demonstrativa;
- envio por `mailto:`;
- persistência local;
- permissões apenas por ocultação de componentes.

## 22. Pontos ainda por confirmar

Não bloquear a estrutura V1, mas marcar como configuração/decisão pendente:

- destino definitivo de imagens do artigo e documentos;
- política exata de eliminação versus anulação por módulo;
- fonte final de produção ativa enquanto a integração não estiver concluída;
- catálogo definitivo de MF para Pegamentos;
- notificações reais e respetivos canais;
- regras finais de arquivo/retenção;
- passagem visual final do calendário do Peso para o componente canónico.

Se uma regra de domínio não estiver explicitamente documentada, não adivinhar: implementar o estado `não disponível`, registar a dependência e pedir confirmação.

## 23. Desvios conhecidos nos mockups — não copiar para produção

Esta passagem encontrou resíduos demonstrativos que não alteram a especificação:

| Ficheiro | Resíduo | Implementação correta |
|---|---|---|
| `boquilhas.html` | usa `confirm()` em fecho/cancelamento | modal canónico com ação primária e cancelar |
| `admin.html` | reset usa `confirm()` | modal canónico com utilizador, consequência e confirmação |
| `peso-operador.html` | reabertura usa `prompt()` | modal com campo obrigatório de justificação |
| `armazem.html` | fecho de registo usa `confirm()` | modal canónico apenas quando existem dados por perder |
| `reparacao-v2.html` | ação demonstrativa usa `alert()` | navegação/estado real; nunca alerta nativo |
| `pegamentos.html` | conserva código legado de importação/base local e confirmações | não expor Base de dados local, Importar/Apagar, `Enviar resumo` ou impressão duplicada |
| alguns HTML | tokens e CSS repetidos inline | na aplicação real importar a folha central e usar componentes partilhados |
| Peso | calendário visual ainda não é cópia exata do de Boquilhas | usar o componente canónico único |

Os HTML são úteis para composição, prioridade e fluxo. O coder deve usar os documentos para decidir comportamento final quando encontrar estes resíduos.

## 24. Definition of Done por página

Uma página não está terminada apenas porque se parece com o mockup. Só fica concluída quando:

1. usa a shell e tokens globais sem duplicar valores crus;
2. respeita capabilities e autorização no servidor;
3. carrega dados reais e apresenta loading/empty/error;
4. formulários têm validação cliente e servidor coerente;
5. comandos são idempotentes ou protegidos contra duplo envio;
6. listas, filtros, seleção, duplo clique, teclado e paginação funcionam;
7. ações críticas têm modal, justificação e auditoria quando aplicável;
8. estado atualizado aparece sem exigir refresh manual;
9. desktop e mobile foram testados;
10. testes cobrem caminho feliz, validação, permissão, concorrência e falha do serviço;
11. não existem `alert()`, `prompt()` ou `confirm()` nativos;
12. o conteúdo e a ordem visual permitem executar a tarefa sem consultar outra página desnecessariamente.


## END FILE CONTENT

---

# FILE 005

## Source Path
`docs\DESIGN_IMPLEMENTATION_CONTRACT.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# BA DMO — Design Implementation Contract

Versão da auditoria: 1.0  
Data: 2026-08-16  
Âmbito: exclusivamente design, UI e comportamento de interação  
Estado: auditoria final antes do planeamento da aplicação

## 0. Resultado executivo

O pacote comunica bem a direção visual, a hierarquia operacional e grande parte dos comportamentos. No entanto, **ainda não constitui por si só uma fundação executável totalmente fechada para uma fresh build**.

As duas causas principais são:

1. `DMO_DESIGN_SYSTEM.md` é mais completo do que `dmo-design-system.css`; vários componentes descritos ainda não existem como implementação canónica;
2. os mockups continuam a conter `<style>` local, valores hardcoded e variantes próprias de botões, calendários, tabelas e layouts.

Este documento transforma o pacote numa auditoria utilizável pelo agente de planeamento. Não altera regras de negócio nem confirma relações funcionais que pertencem a Verified Knowledge.

### Legenda

- `DEFINED`: suficiente e inequívoco para implementar.
- `PARTIAL`: direção existente, mas falta contrato ou implementação canónica.
- `MISSING`: decisão necessária antes de criar o componente/fundação.
- `READY`: módulo visualmente especificado sem bloqueio próprio conhecido.

## 1. Objetivo e fronteira

O futuro agente deve conseguir usar o pacote para decidir:

- composição da shell;
- navegação visual;
- anatomia das páginas;
- componentes universais;
- estados e interações globais;
- composição específica dos módulos;
- ordem de construção visual.

Este contrato **não** define:

- base de dados;
- entidades ou agregados;
- cálculos;
- permissões reais;
- invariantes de domínio;
- ownership funcional;
- arquitetura técnica C#;
- integração com serviços externos.

Sempre que a UI dependa destes pontos, o documento usa `FUNCTIONAL INPUT REQUIRED`.

## 2. Design System Foundation

### 2.1 Auditoria dos fundamentos

| Fundamento | Estado | Evidência atual | Falta para fechar |
|---|---|---|---|
| Design tokens | PARTIAL | conjunto em `DMO_DESIGN_SYSTEM.md` e `dmo-design-system.css` | tokens tipográficos, layers, breakpoints, border widths, ícones e motion |
| Cores da marca | DEFINED | escala 950–050 | documentar contraste calculado por combinação |
| Backgrounds/surfaces | DEFINED | page, card, subtle | estado elevado/overlay poderia ser alias explícito |
| Cores de texto | DEFINED | principal, muted, on-color | texto disabled e link não têm token próprio |
| Cores semânticas | DEFINED | success, warning, danger, pending e soft | info depende da marca; confirmar alias `info` |
| Borders | PARTIAL | cor global definida | falta escala/token de espessura e estilo para focus/divider/strong |
| Radius | DEFINED | control, card, modal, pill | nenhum gap bloqueante |
| Shadows | DEFINED | card, menu e modal | falta elevação de sticky header/sidebar se necessária |
| Spacing scale | PARTIAL | 4, 8, 12, 16, 20, 24 e 32px | falta convenção de uso por componente e aliases para page/gutter/section |
| Sizing scale | PARTIAL | control 40, compact 34, header 76, tabs 52, sidebar 276 | falta altura de row, icon sizes, max page widths, modal widths e touch target token |
| Typography family | DEFINED | Inter + system fallback | falta regra de carregamento/fallback quando Inter não está disponível |
| Font sizes | PARTIAL | tabela por papéis, alguns intervalos | transformar intervalos em tokens exatos; evitar 23–24 e 12–13 ambíguos |
| Font weights | PARTIAL | pesos por papel | falta escala/token e limitar pesos disponíveis |
| Line heights | MISSING | apenas corpo implícito 1.45 e alguns valores locais | definir body, heading, label, button e compact |
| Letter spacing | MISSING | não definido | definir especialmente uppercase/table headers |
| Z-index/layers | MISSING | valores locais 80/100 nos mockups/CSS | criar escala base, sticky, dropdown, overlay, modal, toast |
| Breakpoints | PARTIAL | 1200, 980 e 720px citados | criar tokens/mixins/contrato exato por breakpoint |
| Responsive behavior | PARTIAL | princípios gerais e algumas media queries | grid, page gutter, sidebar/drawer e action bars precisam de padrões executáveis |
| Animation/transitions | PARTIAL | `150ms ease` | definir propriedades permitidas, duração normal, modal/dropdown e reduced motion |
| Icon sizing | MISSING | apenas botão de ícone 34–40px | definir ícone 16/20/24, stroke e alinhamento |
| Density/compactness | PARTIAL | alturas de campos, rows e tabelas | falta matriz compact/regular por componente e comportamento mobile |
| Focus ring | PARTIAL | halo azul descrito e usado em fields | falta token e aplicação a todos os controlos |
| Page width/gutters | MISSING | layouts variam nos HTML | definir max-width ou fluid layout, gutters desktop/tablet/mobile |

### 2.2 Tokens que devem ser adicionados antes dos módulos

Requer `DESIGN DECISION REQUIRED`:

```css
:root {
  /* borders */
  --dmo-border-width: 1px;
  --dmo-border-width-strong: 2px;

  /* typography: valores finais a confirmar */
  --dmo-font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --dmo-font-size-xs: ...;
  --dmo-font-size-sm: ...;
  --dmo-font-size-md: ...;
  --dmo-font-size-lg: ...;
  --dmo-font-size-xl: ...;
  --dmo-line-height-tight: ...;
  --dmo-line-height-normal: ...;
  --dmo-line-height-relaxed: ...;

  /* layout */
  --dmo-page-gutter-desktop: ...;
  --dmo-page-gutter-tablet: ...;
  --dmo-page-gutter-mobile: ...;
  --dmo-page-max-width: ...;
  --dmo-row-height: ...;
  --dmo-touch-target: 44px;

  /* icons */
  --dmo-icon-sm: 16px;
  --dmo-icon-md: 20px;
  --dmo-icon-lg: 24px;

  /* layers */
  --dmo-z-base: ...;
  --dmo-z-sticky: ...;
  --dmo-z-dropdown: ...;
  --dmo-z-overlay: ...;
  --dmo-z-modal: ...;
  --dmo-z-toast: ...;

  /* motion */
  --dmo-duration-fast: 150ms;
  --dmo-duration-normal: ...;
  --dmo-ease-standard: ...;
}
```

Os `...` não devem ser inventados pelo coder. Precisam de uma pequena passagem de decisão visual antes da construção dos componentes.

## 3. CSS Architecture Contract

### 3.1 Estrutura obrigatória

```text
GLOBAL TOKENS
    ↓
GLOBAL COMPONENTS
    ↓
GLOBAL LAYOUT/SHELL
    ↓
MODULE COMPOSITION ONLY
```

### 3.2 Regras normativas

- Um componente universal é implementado uma vez.
- CSS de módulo só organiza composição, grid, ordem e larguras específicas.
- CSS de módulo não redefine cor, raio, sombra, tipografia, botão, field, table, modal, calendar ou feedback.
- Sem hexadecimal, `rgb()`, dimensão visual ou sombra hardcoded quando existe token.
- Sem `style="..."` para design.
- Sem `<style>` nas páginas finais.
- Sem `site.css` legacy a competir com o design system.
- Sem segunda implementação do mesmo componente por módulo.
- Markup usa classes/componentes canónicos.
- Exceções são raras, nomeadas, documentadas e testadas.
- Load order é único: tokens → components → layout → module composition.

### 3.3 Organização recomendada, independente da framework

```text
styles/
  dmo-tokens.css
  dmo-foundation.css
  dmo-components.css
  dmo-layout.css
  dmo-utilities.css
  modules/
    <module>-layout.css
```

### 3.4 Estado real do pacote

`PARTIAL`.

- 12 de 18 HTML ligam `dmo-design-system.css`; 6 não ligam a folha global.
- 17 de 18 HTML contêm pelo menos um `<style>` local.
- existem dezenas de `style="..."` inline;
- vários mockups redefinem `.btn`, fields, tables e calendários;
- `dmo-design-system.css` implementa apenas parte do inventário normativo.

Portanto, os HTML são referências de composição e prioridade, não código CSS a copiar.

## 4. Universal Component Inventory

As colunas `Size` e `States` referem o contrato esperado; `Estado` mede a definição atual no pacote.

| Componente | Purpose / variants | Size | States e interaction | Content / quando usar | Referência | Estado |
|---|---|---|---|---|---|---|
| Button | ação; primary, secondary semântico, danger, success, compact | 34/40px; 44 touch | default filled; hover/focus inverted; pressed; disabled; loading | verbo curto; não usar para navegação passiva | todos os mockups | DEFINED |
| Icon Button | fechar, menu, setas; neutral/danger | 34–40; ícone por definir | default, hover, focus, pressed, disabled | exige `aria-label`; não usar sem ícone reconhecível | modal, calendar, pagination | PARTIAL |
| Input | texto/pesquisa/número | 40; compact 34 | default, hover, focus, readonly, disabled, error, success | label sempre; placeholder só exemplo | formulários globais | PARTIAL |
| Textarea | notas/justificação | min-height por contexto | mesmos estados de field; resize controlado | apenas texto realmente longo | Peso, Job On, movimentos | PARTIAL |
| Select | lista curta e estável | 40/34 | default, hover, focus, open, selected, disabled, error | não usar para pesquisa longa | filtros e campos | PARTIAL |
| Custom Dropdown | pesquisa contextual/longa | anchor 40; menu limitado | open, hover option, active option, selected, empty, loading, error | teclado completo; não autoescolher primeiro | Pegamentos, Job On | PARTIAL |
| Checkbox | multi-select/confirmação | target 40/44 | unchecked, hover, focus, checked, indeterminate, disabled, error | batch/checklist; não substituir seleção de row | saídas programadas/verificações | PARTIAL |
| Radio | escolha exclusiva pequena | target 40/44 | unchecked, hover, focus, checked, disabled | 2–4 opções visíveis | Peso legado | PARTIAL |
| Segmented Selector | escolha exclusiva operacional | 40–48 | selected filled; unselected outline; hover/focus; disabled | tipos/linhas/opções de alta frequência | BQ, ferramentas, tampões | DEFINED |
| Date Input | introdução de data | 150–180 × 40 | default, focus, invalid, disabled, readonly | data isolada; formato localizado | vários formulários | PARTIAL |
| Date Picker | seleção assistida de data | associado ao date input | open, today, selected, disabled, keyboard | quando input de data necessita calendário | forms | MISSING |
| Calendar | filtrar/planear por dia | 300–340 desktop | month nav, today, selected, has-record, empty day, disabled, hover, focus | um componente global | Boquilhas, Peso, Job On | PARTIAL |
| Card | agrupar contexto/tarefa | padding 16–20; radius 12 | default, hover/selectable, selected, disabled/loading | não criar cartão por valor sem contexto | toda a app | DEFINED |
| Expandable Card | editor/filtros inline | largura do bloco origem | closed/open/loading/error/dirty | criação/edição extensa; não modal grande | BQ, forms | PARTIAL |
| List | coleção selecionável | fluida | loading, ready, empty, error | itens não tabulares | referências/controlo | PARTIAL |
| List Row | seleção/abertura | row 40–46 | hover, focus, selected, disabled | clique seleciona; duplo abre | listas globais | DEFINED |
| Table | dados tabulares | row 40–46 | loading/empty/error; sticky head | colunas comparáveis | históricos/movimentos | PARTIAL |
| Table Row | seleção/abertura/movimento | 40–46 | hover/focus/selected; entrada/saída subtis | sem botões repetidos na row | históricos | DEFINED |
| Filter Bar | filtros permanentes/expansíveis | fields 40 | collapsed/open, dirty/applied, loading | uma fonte para resumo+lista | históricos | PARTIAL |
| Search | filtro incremental | 40; flex | empty typing, loading, results, no result, error | dizer o que pesquisa | todos os módulos | PARTIAL |
| Tabs | vistas do módulo | 52px bar | default, hover, focus, active, disabled/hidden | não executar comandos | todos autenticados | DEFINED |
| Badge | metadata curta/tipo | compacto | default/neutral | não usar como estado normal sem necessidade | listas/cards | PARTIAL |
| Status | estado semântico com texto | pill compacto | pending/success/warning/error/inactive | nunca apenas cor | Peso, reparação | DEFINED |
| Alert | ação/risco persistente | inline no contexto | info/success/warning/error | problema que exige atenção | BQ/Armazém | PARTIAL |
| Toast | confirmação não bloqueante | conteúdo curto | enter/show/exit; success/error/info | sucesso breve; erro persistente também inline | mockups | PARTIAL |
| Modal | tarefa rápida/focada | sm/md; width final ausente | closed/open/loading/error/dirty | não usar para formulário extenso | forms/actions | PARTIAL |
| Confirmation Dialog | consequência difícil de reverter | small/medium | open, processing, error | verbo específico; nunca prompt nativo | delete/close/reset | PARTIAL |
| Context Menu `…` | ações contextuais | largura ao conteúdo | closed/open/hover/focus/disabled | só ações válidas; fecha Escape/outside | side panel BQ | PARTIAL |
| Tooltip | explicar ícone/estado truncado | auto | delayed open, hover/focus, close | ajuda curta; não esconder regra crítica | icon buttons | MISSING |
| Pagination | navegar dados paginados | arrows 36; select 40 | default/hover/focus/disabled/loading | total + página X/Y + 20/40/60 | listas globais | DEFINED |
| Empty State | ausência de dados/resultados | compacto no conteúdo | initial/no-results/no-data | causa + próximo passo | todos | PARTIAL |
| Loading State | aguardar dados/comando | preserva layout | initial, refresh, action | skeleton ou texto; evitar layout shift | transversal | PARTIAL |
| Error State | falha de load/save | inline/card | recoverable/fatal/field | explicar próxima ação e Retry quando possível | transversal | PARTIAL |
| Sidebar | contexto operacional persistente | 276 desktop | default/collapsed/mobile drawer/conflict | estado atual; não analytics | Boquilhas/shell | PARTIAL |
| Header | identidade global | 76 desktop | default/compact mobile | logo, page title, subtitle, user/profile | componentes globais | DEFINED |
| Page Header | título da vista e descrição | fluido | default/action/loading optional | não repetir tab sem contexto | módulos | PARTIAL |
| Section Header | título de cartão/secção | 15–16px | default/action attached | uma hierarquia clara | formulários/cards | PARTIAL |
| Form Group | campos relacionados | grid responsiva | default/error/disabled | legend/título quando necessário | forms | PARTIAL |
| Field | label+control+help+error | control 40 | required/optional/readonly/disabled/error/success | unidade e formato explícitos | global | PARTIAL |
| Action Bar | ações de página/editor/seleção | 36/40 | default, sticky optional, loading | ações dependentes fora da lista | tables/forms | PARTIAL |
| Detail Panel | detalhe da seleção | card/side/inline | empty/loading/ready/error/dirty | quando seleção precisa de contexto | Peso/Job On | PARTIAL |
| Tool Availability Picker | substituir associação no Job On | expandable table + filters | closed, loading, ready, selected, empty, partial-source, error | só em Modo edição; posição + estado técnico + uso com origem explícita | Job On | PARTIAL |
| History Entry | evento auditável | row/card | normal/correction/void/expanded | ator, módulo, ação, entidade, data, resultado, reason, before/after | históricos + Admin/Auditoria | READY |
| User/Profile Indicator | identidade da sessão | header right | default/compact/menu-open | nome+título; título não é permissão | header/admin | PARTIAL |
| Local Directory Selector | autorizar pasta raiz local | field + action 34/40 | unconfigured, requesting, authorized, permission-lost, unavailable, error | Definições; mostra nome da pasta e âmbito local | Peso/Pegamentos | PARTIAL |
| Resolved Report Path | mostrar raiz + subpasta do lote | read-only compact | resolved, missing-root, invalid-subfolder, permission-lost | não é editor de caminho absoluto; ex.: `Capacidades / 5447T173` | criação de lote/Pegamentos | PARTIAL |

### 4.1 Componentes em falta que bloqueiam reutilização

Antes de construir módulos, fechar e implementar:

- Tooltip;
- Date Picker;
- Dropdown pesquisável;
- Loading/Empty/Error como componentes reais;
- Confirmation Dialog sem APIs nativas;
- Field completo com helper/error;
- Sidebar responsiva/drawer;
- User/Profile menu e logout;
- History Entry expandível;
- Page Header e Action Bar canónicos.

### 4.2 Interaction e `when not to use` por componente

Esta tabela completa o inventário anterior. O comportamento de estado continua na secção 5.

| Componente | Interaction principal | Quando não usar |
|---|---|---|
| Button | click/keyboard executa uma ação explícita | navegação passiva, status ou mero destaque |
| Icon Button | click executa ação compacta; tooltip/aria identifica | ação ambígua ou sem ícone universal |
| Input | introdução direta, paste e keyboard | conjunto finito pequeno de opções |
| Textarea | texto multilinha | códigos, números ou notas de uma linha |
| Select | abrir e escolher uma opção curta | catálogo longo, pesquisável ou contextual |
| Custom Dropdown | escrever/navegar/escolher resultado | 2–5 opções estáveis visíveis |
| Checkbox | alternar item independente ou batch | escolha mutuamente exclusiva ou seleção normal de row |
| Radio | escolher exatamente uma opção | lista longa ou ação que muda imediatamente de página |
| Segmented Selector | clique numa opção visível e frequente | mais opções do que cabem claramente ou seleção múltipla |
| Date Input | escrever/escolher uma data | navegar/visualizar densidade de registos mensais |
| Date Picker | abrir popover e escolher data | calendário operacional sempre visível |
| Calendar | navegar mês e selecionar dia | introduzir apenas uma data num formulário simples |
| Card | agrupar conteúdo relacionado | valor isolado sem contexto ou cada célula de uma tabela |
| Expandable Card | trigger expande editor inline e gere dirty state | confirmação curta ou detalhe de leitura simples |
| List | selecionar/abrir itens com layout flexível | dados que exigem comparação por colunas |
| List Row | clique seleciona; duplo abre | comando imediato sem estado de seleção |
| Table | ordenar/filtrar/paginar dados tabulares | cartões sem relação colunar |
| Table Row | clique seleciona; duplo abre (sem atalho de teclado específico do BA DMO) | inserir vários botões repetidos de abertura |
| Filter Bar | alterar filtros e aplicar/limpar | único search field simples sem filtros adicionais |
| Search | input incremental/submissão conforme volume | seleção obrigatória de ID sem lista de resultados |
| Tabs | trocar vistas pares dentro do módulo | executar guardar, criar, aprovar ou eliminar |
| Badge | mostrar categoria/metadado curto | representar sozinho um estado crítico |
| Status | comunicar estado textual e semântico | tipo de registo ou ação |
| Alert | mensagem persistente junto do problema | sucesso breve sem consequência |
| Toast | confirmação breve após ação | erro que exige correção ou decisão |
| Modal | tarefa curta, focus trap e retorno ao trigger | formulário extenso ou fluxo que requer contexto da página |
| Confirmation Dialog | confirmar consequência difícil de reverter | ação segura/reversível normal |
| Context Menu | mostrar poucas ações válidas do item | ação primária, navegação principal ou lista longa |
| Tooltip | ajuda suplementar em hover/focus | instrução essencial, erro ou conteúdo necessário em mobile |
| Pagination | navegar páginas de dados | coleção pequena fixa ou virtual scrolling aprovado |
| Empty State | explicar ausência e próximo passo | mascarar erro/loading |
| Loading State | indicar espera preservando layout | operação síncrona instantânea |
| Error State | explicar falha e recuperação | validação simples de um único field |
| Sidebar | manter contexto operacional transversal à vista | analytics geral ou navegação duplicada |
| Header | identidade global e acesso à conta | repetir detalhe operacional da página |
| Page Header | identificar tarefa/vista e ação primária | repetir literalmente a tab sem informação adicional |
| Section Header | separar secções semânticas | criar hierarquia visual para cada field |
| Form Group | reunir fields da mesma tarefa | envolver um único field sem benefício |
| Field | label/control/helper/error | control sem label ou informação apenas de leitura |
| Action Bar | reunir ações do contexto/seleção | repetir a mesma ação dentro de cada row |
| Detail Panel | mostrar contexto da seleção | substituir navegação quando detalhe é uma página completa |
| History Entry | mostrar evento e expandir auditoria | apresentar estado atual sem evento |
| User/Profile Indicator | identidade atual e trigger de conta | conceder/representar permissões através do título visual |

## 5. Component State Contract

### 5.1 Matriz global

| Estado | Regra visual e de interação |
|---|---|
| Default | componente legível, disponível e sem sinal falso de seleção |
| Hover | alteração clara mas discreta; nunca única pista da ação |
| Focus | ring visível, consistente e independente do hover |
| Active/pressed | feedback imediato enquanto pressionado; não confundir com selected |
| Selected | estado persistente de escolha, com texto/ARIA quando aplicável |
| Disabled | contraste reduzido, sem hover/comando; contexto deve explicar a indisponibilidade |
| Loading | preserva dimensões, bloqueia reenvio e apresenta progresso |
| Success | apenas depois de persistência confirmada |
| Warning | atenção recuperável, sem simular erro fatal |
| Error | mensagem concreta, próxima ação e preservação de input quando possível |

`Focus`, `selected` e `active` são estados diferentes e não podem partilhar apenas o mesmo fundo sem outro indicador.

### 5.2 Button state machine

| Estado | Fundo | Border | Texto | Comportamento |
|---|---|---|---|---|
| Normal | cor da variante | mesma cor | branco | ação disponível |
| Hover | branco | cor da variante | cor da variante | cursor pointer |
| Focus visible | branco | cor da variante | cor da variante | ring global adicional |
| Pressed/active | branco | cor da variante, strong opcional | cor da variante | deslocamento máximo 1px opcional; sem brightness |
| Loading | cor da variante ou surface controlada | mesma cor | branco + indicador | label preserva largura; comando bloqueado |
| Disabled | token disabled | token disabled | branco/contraste validado | sem hover e `aria-disabled`/disabled |

A expressão original “filled rest e ao hover invertido” continua válida. O pacote não define ainda um token específico de `pressed`; isso é `DESIGN DECISION REQUIRED`, embora deva permanecer dentro do estado invertido.

### 5.3 Campos

- Hover: border ligeiramente mais forte, sem competir com focus.
- Focus: border brand + focus ring.
- Readonly: legível, fundo subtle, selecionável/copiável.
- Disabled: menor contraste e não focável quando nativo.
- Error: border danger + mensagem junto ao field; não apagar helper necessário.
- Success: usar apenas quando confirmar explicitamente validação remota relevante; evitar green em cada campo válido.
- Loading select/search: indicador dentro do control e menu não interativo.

### 5.4 Rows e cards selecionáveis

- Hover não equivale a seleção.
- Clique cria seleção única.
- Selected permanece até mudar de linha, filtro, contexto ou ação que remova o registo.
- Duplo clique abre sem exigir botão adicional.
- Focus por teclado é visível mesmo sem seleção.

## 6. List/Table Interaction Contract

### 6.1 Contrato principal

| Interação | Resultado |
|---|---|
| Um clique | seleciona uma linha e ativa ações externas |
| Duplo clique | abre detalhe/folha/registo associado |
| Hover | mostra que a linha é interativa |
| `Enter` | nenhum atalho de teclado específico do BA DMO; seleção/abertura definidas por clique/duplo clique |
| Escape | fecha menu/contexto, não perde filtros |
| Filtro/limite novo | regressa à página 1; limpa seleção invisível |

### 6.2 Exceções

- Checklist/batch usa checkbox para inclusão/progresso; clique na linha pode continuar a selecionar.
- Calendário não segue duplo clique: um clique seleciona/filtra.
- Segmented selector e cards de linha são escolhas diretas, não listas de detalhe.
- Rows com ação inline só são aceitáveis quando a própria célula representa uma decisão individual, como `Manter/Colocar de parte`; não usar para abrir.

### 6.3 Estados obrigatórios

- Loading inicial.
- Refresh mantendo a tabela.
- Empty sem dados.
- No results por filtros.
- Error de carregamento com Retry.
- Ready sem seleção.
- Ready com seleção.
- Processing numa ação sobre a seleção.

### 6.4 Sorting e scroll

`PARTIAL`:

- filtering e paginação estão definidos;
- sorting não tem contrato global: falta definir headers sortable, direção, estado inicial e persistência;
- scroll vertical/sticky header está mencionado;
- scroll horizontal é último recurso e deve ficar dentro do card.

`DESIGN DECISION REQUIRED`: fechar o sorting antes de implementar o componente. O comportamento de teclado de listas está resolvido: não existe atalho específico do BA DMO; clique seleciona e duplo clique abre.

## 7. Calendar Contract

### 7.1 Definição canónica existente

- semana começa na segunda-feira;
- mês/ano centrado;
- setas nas extremidades;
- sete colunas;
- um clique seleciona e filtra;
- dia com registo tem ponto discreto;
- selecionado usa brand fill e texto branco;
- `Mostrar todas as datas` remove o filtro;
- alteração de mês não auto-seleciona;
- teclado e `aria-pressed` obrigatórios;
- desktop aproximadamente 300–340px quando ao lado de lista;
- mobile passa para cima.

### 7.2 Estados auditados

O contrato transversal de persistência e consulta está em `AUDITORIA_GLOBAL_HANDOFF.md`. Todos os módulos emitem eventos factuais append-only para ações relevantes, associados ao utilizador, módulo e entidade. A vista anual no Admin não atribui pontuações nem rankings.

| Estado | Estado do contrato |
|---|---|
| Mês anterior/seguinte | DEFINED |
| Dia selecionado | DEFINED |
| Dia com registos | DEFINED |
| Dia sem registos | DEFINED |
| Dia disabled/fora do mês | PARTIAL |
| Hover | DEFINED |
| Focus/teclado | DEFINED no texto, parcial no CSS |
| Hoje | MISSING |
| Loading/error do calendário | MISSING |
| Vários tipos de registo no mesmo dia | MISSING |
| Relação com lista | DEFINED |
| Responsivo | DEFINED no texto, parcial na implementação |

### 7.3 Peso versus Boquilhas

`DESIGN GAP` confirmado.

O próprio `HANDOFF_INDEX.md` declara que o calendário do Peso ainda precisa da passagem visual final para reutilizar exatamente o de Boquilhas. O futuro build não deve portar nenhum dos dois diretamente: deve criar primeiro o calendar canónico e fazer ambos consumi-lo.

## 8. Shell / App Frame

### 8.1 Definido

- header global de 76px;
- logótipo 44px;
- título do módulo/página e descrição junto ao logo;
- nome e título do perfil à direita;
- tabs operacionais à esquerda;
- Definições/Administração à direita;
- page surface, cards e cores;
- side panel contextual fixo quando o módulo necessita tracking;
- drawer/bloco recolhível em mobile.

### 8.2 Contrato visual proposto sem inventar permissões

```text
APP FRAME
├─ Global Header
│  ├─ Logo
│  ├─ Module/Page identity
│  └─ User/Profile indicator + account trigger
├─ Module Navigation
│  ├─ Operational tabs
│  └─ Settings tab (right aligned)
└─ Work Area
   ├─ Optional contextual sidebar
   └─ Page content
```

- O título do header identifica a área atual; o título da vista identifica a tarefa dentro dela.
- Module switching deve ter uma indicação ativa consistente, mas o pacote não contém um componente canónico de launcher/menu global.
- Logout/account deve partir do indicador do utilizador ou menu de conta, mas a interação visual não está desenhada.
- Sidebars de contexto não substituem navegação global.
- O conteúdo deve ser fluido e usar o espaço disponível sem ficar numa coluna pequena no centro.

### 8.3 Estado

`SHELL VISUAL CONTRACT: PARTIAL`.

Falta `DESIGN DECISION REQUIRED` para:

- mecanismo visual de troca de módulos;
- menu do perfil/logout;
- largura máxima/gutters da área de conteúdo;
- comportamento exato do side panel em tablet/mobile;
- sticky behavior e layers;
- apresentação quando não existem tabs.

## 9. Canonical Page Anatomy

```text
APP SHELL
→ MODULE NAVIGATION
→ PAGE HEADER
→ PRIMARY ACTION / FILTER AREA
→ ACTIVE CONTEXT OR ESSENTIAL SUMMARY
→ MAIN CONTENT
→ DETAIL / SECONDARY CONTENT
→ SELECTION ACTION BAR
→ FEEDBACK
```

### Regras

- Page Header contém título, descrição e no máximo a ação primária contextual.
- Filtros ficam imediatamente antes da coleção que afetam.
- Contexto ativo permanece próximo das ações que dependem dele.
- Ações dependentes de seleção ficam após/ao lado do footer da lista, nunca repetidas em cada row.
- Feedback aparece junto do contexto ou no toast conforme persistência.
- Empty state ocupa o main content sem criar um grande vazio tracejado.

### Exceções justificadas

- Login não usa tabs nem page header autenticado.
- Job On em modo consulta comporta-se como folha técnica e dá prioridade ao contexto e às ferramentas.
- Side panel de Boquilhas persiste fora da anatomia interna da tab.
- Responsável Peso usa calendar + master list + detail, uma composição master-detail.
- Armazém e reparação interna usam registo rápido, reduzindo o número de áreas.

## 10. Form Contract

### Estrutura de Field

```text
Label [required marker]
Control [unit/suffix when applicable]
Helper text
Validation message
```

### Regras

- Label sempre visível.
- Obrigatório indicado visualmente e em acessibilidade; o símbolo final ainda é `DESIGN DECISION REQUIRED`.
- Opcional pode usar `(opcional)` quando evita dúvida; não marcar todos os opcionais.
- Helper explica formato/consequência; placeholder apenas exemplifica.
- Erro diretamente abaixo do field e resumo no topo apenas para formulários extensos.
- Agrupar por tarefa, não por estrutura de dados.
- Números alinhados; unidades visíveis; mobile keyboard apropriado.
- Máximo de duas casas decimais quando a apresentação assim estiver definida; regra de cálculo vem da spec funcional.
- Campos curtos usam largura proporcional.
- Data no final da linha de identificação salvo exceção documentada.
- Notas ocupam espaço restante e não competem com campos de 2–3 caracteres.
- Readonly é legível e copiável; disabled é indisponível.
- Save/Cancel no footer à direita, Cancel antes de Save.
- Ao fechar dirty state, usar Confirmation Dialog.
- Ação destrutiva fica separada das ações normais.

### Estados

`PARTIAL`: labels, focus, width e ordem estão definidos. Falta componente completo para required marker, validation summary, success validation, async validation e dirty-state indicator.

## 11. Modal / Dialog Contract

### Quando usar

- confirmação destrutiva;
- perder alterações;
- reset de palavra-passe;
- ação rápida e focada;
- edição curta que não beneficia do contexto completo da página.

Não usar para formulários extensos; esses expandem inline.

### Anatomia

- Backdrop.
- Header: título, contexto curto e Close icon.
- Body: mensagem ou fields.
- Error region persistente.
- Footer: secondary/cancel primeiro, primary/destructive no fim.

### Comportamento

- foco inicial no título ou primeiro campo seguro;
- focus trap;
- `Escape` e click no backdrop fecham apenas sem perda de dados/processamento;
- close restaura foco ao trigger;
- submit loading impede repetição;
- falha mantém modal aberto e input preservado;
- conteúdo longo faz scroll no body, não na página;
- backdrop não desaparece durante processamento.

### Variantes

`DESIGN DECISION REQUIRED`: larguras exatas small/medium/large e altura máxima. O contrato atual só define modal genérico `min(560px,100%)`.

## 12. Feedback Contract

| Situação | Componente | Persistência |
|---|---|---|
| Campo inválido | field error | até corrigir |
| Vários campos inválidos | field errors + summary | até corrigir |
| Informação contextual | inline info | enquanto relevante |
| Alerta recuperável | inline warning | até resolver/dispensar quando permitido |
| Conflito operacional | inline error/alert | até resolver |
| Sucesso simples | toast | temporário |
| Save falhado | inline error no editor + toast opcional | persistente |
| Load falhado | Error State com Retry | persistente |
| Loading inicial | skeleton/compact loader | até concluir |
| Comando em curso | estado loading no trigger | até concluir |
| Ação destrutiva pendente | Confirmation Dialog | bloqueante |
| Operação destrutiva falhada | dialog ou inline no mesmo contexto | até decisão |

Não usar modal para mensagens informativas normais. Não usar toast como única apresentação de um erro que exige correção. Não apresentar sucesso antes de confirmação da operação.

## 13. History / Audit Visualization

Este contrato trata apenas da representação visual. Os eventos e campos efetivamente disponíveis exigem validação funcional.

### 13.1 Vista compacta

Cada row/entry deve conseguir apresentar:

- data/hora;
- ator;
- ação;
- objeto/contexto curto;
- estado;
- motivo quando crítico;
- indicador de correção/anulação.

### 13.2 Detalhe expandido

Ao abrir:

| Informação | Representação |
|---|---|
| Ator | nome legível + identificador apenas se necessário |
| Ação | verbo explícito, não código técnico |
| Data/hora | data e hora local; timezone no detalhe se necessário |
| Valor anterior | bloco/coluna `Anterior` |
| Valor novo | bloco/coluna `Novo` |
| Motivo | texto próprio, não misturado com observações |
| Correção | status `Corrigido` + relação visual com o evento de correção |
| Void/cancel | estado textual moderado; registo original continua consultável |
| Estado | componente Status canónico |

### 13.3 Consistência

- O mesmo actor/date/status usa o mesmo formato em todos os módulos.
- Não misturar snapshot histórico com estado live.
- Comparação anterior/novo deve ter labels e alinhamento estáveis.
- Correção não deve parecer eliminação.
- Um clique seleciona e duplo clique abre quando o histórico é tabela canónica.
- Uma timeline só deve ser usada quando a sequência é mais importante do que comparar colunas.

Estado atual: `PARTIAL`. Existem muitas tabelas de histórico, mas não existe ainda um componente visual único `History Entry/Detail` implementado.

## 14. Cross-Module Visual Language

| Conceito | Representação canónica |
|---|---|
| Machine/Line | código curto em field/card compacto; label `Máquina` ou `Linha` conforme texto funcional confirmado; selector segmentado quando ação frequente |
| Reference | texto principal em bold; nunca truncar sem tooltip/detail |
| Lot | valor compacto junto da Referência, nunca campo excessivamente largo |
| Tool | tipo + Referência + Lote + número individual quando disponível; tipo não é status |
| Status | pill/status textual com cor semântica dessaturada |
| Date | PT-PT na apresentação; date control em edição; date/time juntos em históricos |
| User | nome legível; perfil/título apenas no header ou detalhe relevante |
| Repairer | valor textual/filter; visualmente pessoa/entidade, não status |
| Production | código compacto e consistente, próximo da Referência/Máquina |
| History | tabela/lista canónica + detalhe auditável |
| Approval | status pendente/aprovado/não aprovado + ações externas; nota obrigatória mostrada no contexto |
| Quantity | número alinhado + unidade explícita quando necessária |
| Current vs historical | blocos separados `Estado atual` e `Snapshot histórico` |

O significado e a origem destes conceitos são `FUNCTIONAL INPUT REQUIRED`; a tabela define apenas linguagem visual.

## 15. Module Design Coverage

Critério estrito: `READY` exige mockup/brief, estrutura, componentes, interações, estados e responsive sem gap específico do módulo. Dependências globais são registadas separadamente.

| Área | Mockup | Brief | Estrutura | Componentes | Interações | Empty/Loading/Error | Responsive | Design ready |
|---|---|---|---|---|---|---|---|---|
| Login | READY | READY | READY | READY | READY | PARTIAL | READY | READY |
| Admin | READY | READY | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |
| Shell | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL | PARTIAL | PARTIAL | PARTIAL |
| Peso Operador | READY | READY | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |
| Peso Responsável | READY | READY | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |
| Peso Comparação | READY | READY | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |
| Boquilhas | READY | READY | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |
| Job On | READY | READY | READY | PARTIAL | READY | PARTIAL | READY | PARTIAL |
| CM | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL | PARTIAL | PARTIAL |
| MF | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL | PARTIAL | PARTIAL |
| Warehouse/Armazém | READY | READY | READY | PARTIAL | READY | READY | PARTIAL | PARTIAL |
| Internal Repair | READY | READY | READY | PARTIAL | READY | READY | PARTIAL | PARTIAL |
| External Repair | READY | READY | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |
| Pegamentos | READY | READY | READY | PARTIAL | PARTIAL | PARTIAL | PARTIAL | PARTIAL |
| Tampões | READY | READY | READY | PARTIAL | READY | READY | READY | PARTIAL |
| Tool creation | PARTIAL | READY | READY | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |
| History transversal | PARTIAL | PARTIAL | PARTIAL | PARTIAL | READY | PARTIAL | PARTIAL | PARTIAL |

Resultado estrito: **1/17 READY**. Isto não significa que os outros módulos devam ser redesenhados; significa que devem ser montados sobre componentes globais ainda por fechar e completar os estados indicados.

## 16. Mockup → Component Map

| Mockup | Global components usados | Composição específica | Elementos únicos | Deve ser reutilizável / não duplicar |
|---|---|---|---|---|
| `login.html` | Input, Button, Alert, Card | split identity + form | painel de marca | Field/password reveal e form feedback globais |
| `admin.html` | Header, Tabs/nav, Table, Search, Status, Modal, Toast | utilizadores + aplicações | profileTitle editor/reset | User row, Confirmation Dialog e permission/template selector |
| `boquilhas.html` | Header, Tabs, Buttons, Fields, Table, Modal, Context Menu, Pagination | fixed line sidebar + lote/detail/history | cards B1–C3 e alerta de conflito | sidebar card e movement table sem nova implementação de base |
| `peso-operador.html` | Header, Tabs, Fields, Tables, Status, Action Bar | reference/control/comparison workspaces | readings/results matrix | Reading Group, Result Summary e canonical list/calendar |
| `peso-responsavel.html` | Header, Tabs, Calendar, List, Detail, Status | master calendar/list + approval detail | decisão individual CM | Calendar, Approval Panel, CM Decision Row |
| `job-on-v48-folha-producao.html` | Header, Tabs, Calendar, Fields, Cards, Status, Action Bar | technical production sheet | ferramenta groups, image, verifications | Tool Group shell, contextual selector, image field, checklist |
| `armazem.html` | Header, Tabs, Filters, Table, Status, Modal | quick movement + programmed list | location/context blocks | Quick Register, Batch Checklist, history components |
| `reparacao-interna.html` | Header, Tabs, segmented selector, Field, Table | top full-width line selector then register | active Job On context | Line Selector and Quick Register; table global |
| `reparacao-externa-v1.html` | Header, Tabs, Filters, Table, Status | external cycle/list | programmed repair batch | Batch Checklist shared with Armazém |
| `moldes.html` + `moldes-v4x` | Header, Tabs/segmented controls, Fields, Table | CM/MF separate areas | type-specific selectors | Tabs global; tool list/card; no separate button system |
| `pegamentos.html` | Header, Tabs, Dropdown, Fields, Tables, Print Action | context selection → control sheet | measurement grids | contextual selector and table; remove legacy local database UI |
| `tampoes.html` | Header, Tabs, Fields, Status, Table, Pagination | config selector + quantities + planning | atomic config transformation | Quantity editor, configuration selector, movement history |
| `job-on.html` | redirect only | canonical entry | none | not a UI component |
| versioned standalone files | mesma composição em iterações | historical visual variants | none authoritative | do not implement alongside canonical file |

### 16.1 Component-first build set

Construir antes das páginas:

1. tokens/foundation;
2. Button/Icon Button;
3. Field family;
4. Card/headers;
5. Tabs;
6. Status/Badge;
7. List/Table/Row/Pagination;
8. Filter Bar/Search/Dropdown;
9. feedback states;
10. Modal/Confirmation/Context Menu/Tooltip;
11. Calendar;
12. Header/Shell/Sidebar;
13. Page Header/Action Bar/Detail Panel;
14. History Entry;
15. module compositions.

## 17. Design vs Business Boundary

### DESIGN STATEMENTS REQUIRING FUNCTIONAL VALIDATION

As afirmações seguintes aparecem nos documentos para explicar a UI, mas **não são confirmadas por esta auditoria**. O agente de planeamento deve cruzá-las com Verified Knowledge:

- template de acesso determina módulos, capabilities e ordem, mas a landing page global é Job On;
- todos os utilizadores, incluindo Administrador, abrem Job On depois do login; Administração permanece acessível pela navegação;
- apenas o papel/template técnico Responsável pode criar, duplicar, editar, guardar revisões e gerir Definições do Job On;
- título do perfil é independente de permissões;
- Processo NNPB/PS é escolhido na criação do lote no Peso e herdado pelos registos; não é pedido no Novo controlo;
- máquinas permitidas associam Referência/lote às linhas;
- Peso Operador cria e envia; Peso Responsável aprova e não regista leituras;
- existem dois tipos de registo de Peso: Novo controlo e Comparação, ambos referenciando o Job On da produção;
- Novo controlo usa o contexto do Job On; Comparação usa os CM já em produção e a base aprovada do Novo controlo desse Job On;
- comparação não altera o Novo controlo aprovado base;
- decisão de comparação é individual por CM;
- destinatários de email são escolhidos pela máquina/linha;
- uma linha de Boquilhas não pode ter duas Referências diferentes;
- dois lotes da mesma Referência na mesma linha são permitidos;
- utilização de BQ representa tempo de vida e não quantidade;
- saldos/movimentos e fecho de lote geram snapshots específicos;
- Job On duplica a produção anterior da mesma Referência e reseta/atualiza datas;
- Job On obtém opções CM/MF/BQ por Referência e Máquina;
- Job On guarda as instâncias e lotes concretos de CM/MF/BQ usados por uma produção; Peso e Pegamentos consomem essas escolhas sem segunda seleção;
- Job On separa consulta não editável de edição; apenas na edição a lista de ferramentas agrega posição do Armazém, estado técnico e `% de uso` para suportar uma substituição;
- em edição, todos os campos do snapshot Job On são editáveis, incluindo CAL, PI, quantidades, notas e grupos secundários; informação live das fontes externas permanece apenas contextual;
- cada Job On/revisão guarda um snapshot completo de todos os grupos para duplicação; as bases mestre mantêm identidade, estado técnico, vida e localização; `JOB_ON_DATA_MODEL.md` define explicitamente as tabelas e o limite de ownership autorizado pelo produto;
- verificações `Uma vez neste lote` e `Por fabrico` geram ocorrências;
- saída de Armazém liberta imediatamente uma posição;
- saída programada conclui quando todos os checks estão confirmados;
- Reparação interna carrega automaticamente produção ativa por Linha;
- Reparação externa é separada da interna;
- CM e MF têm identidades/históricos separados;
- Tampões transforma quantidades atomicamente entre configurações;
- Pegamentos usa os CM, BQ e MF concretos associados ao Job On, cujas entidades vêm dos respetivos módulos de domínio;
- o servidor guarda os dados estruturados e históricos de Peso/Pegamentos, enquanto os PDFs de Produção são guardados na pasta local configurada;
- o diretório principal é configurado no computador e a subpasta relativa é definida na criação do lote no Peso;
- histórico preserva snapshots, correções, ator e motivo;
- eliminar, anular, arquivar e corrigir têm consequências específicas.

Nenhuma destas afirmações deve ser usada para desenhar entidades ou relações de DB apenas porque aparece no handoff visual. A exceção explícita é `JOB_ON_DATA_MODEL.md`, criado como contrato técnico de persistência após validação funcional do produto.

## 18. Contradictions Inside Design

| Conflict | Source A | Source B | Recommended canonical design |
|---|---|---|---|
| Altura de botão | Design System: compact 34, normal 40 | CSS `.dmo-button` min 36; Coder Handoff chama 36 standard | fixar API: compact 34, default 36 ou 40 a decidir, form/filter 40; um token por size |
| Hover/active | regra filled → hover inverted | alguns mockups têm `.btn` próprios e variantes de tom | usar o state machine da sec. 5.2; proibir brightness |
| Enter numa lista | RESOLVIDO: sem atalho de teclado específico do BA DMO; clique seleciona, duplo clique abre | antigas propostas (`Enter`/`Ctrl+Enter`/`Espaço`) removidas | contrato único confirmado: clique seleciona; duplo clique abre |
| Calendário | componente canónico documentado/Boquilhas | Peso apresenta variante visual | construir um único componente antes de Peso/Boquilhas |
| Inputs | contrato global 40px | HTML locais usam alturas/paddings diferentes | Field global 40; compact só por variante explícita |
| Card radius/shadow | 12px + shadow token | mockups redefinem radius/shadow localmente | usar Card global; module CSS só composição |
| CSS architecture | tokens/components globais | 17 HTML com `<style>` e inline styles; 6 sem global CSS | não copiar CSS dos mockups; reconstruir por componentes |
| Buttons vs tabs em Moldes | tabs representam áreas | mockup mostra `Contra moldes`/`Moldes finais` como botões | usar Tabs globais, ativa filled apenas se for segmented selector funcional |
| Page width | princípio de usar largura disponível | alguns mockups centralizados estreitos em ecrã largo | definir container fluido/max-width e gutters antes dos módulos |
| Sidebar | side panel fixo contextual | não existe sidebar global única noutros módulos | separar App Navigation de Context Sidebar; definir responsive |
| Header naming | header global usa título da página | alguns mockups misturam app title/module title | fixar dois níveis: module identity no header, view title no content |
| Job On tool label | exemplos usam `MP`; discussões referem `CM` como prioritário | mockup/documento usa `MP/CM` em alguns pontos | `FUNCTIONAL INPUT REQUIRED`; visual aceita label configurada, não fundir identidades |
| Modal confirmations | contrato proíbe APIs nativas | vários mockups usam `confirm/prompt/alert` | Confirmation Dialog/Modal global |
| Dropdown | contrato pede custom styled/searchable | alguns HTML usam select/datalist/browser menu | Select nativo estilizado para curto; custom Dropdown para contextual/pesquisa |
| Pegamentos | brief remove base local/actions duplicadas | HTML ainda contém código legacy | brief prevalece; não portar legacy UI |
| Versioned mockups | ficheiros canónicos coexistem com versões v38/v42/v43/v44 | agente pode implementar múltiplos | README/index define entrada canónica; versões são só evidência histórica |

## 19. Design Gaps

### P0 — bloqueia foundation/design system

1. Definir tokens exatos de typography/line-height/letter-spacing.
2. Definir escala de z-index/layers.
3. Definir page width e gutters responsivos.
4. Resolver tamanho default do Button: 36 ou 40px; manter variantes explícitas.
5. Teclado de row: RESOLVIDO — sem atalho específico do BA DMO; clique seleciona, duplo clique abre.
6. Fechar border/focus tokens e regras de reduced motion.
7. Declarar `dmo-design-system` como única fonte visual e impedir legacy/inline/local component CSS.

### P1 — bloqueia componente reutilizável

1. Calendar completo: today, disabled/outside month, loading/error e responsive implementado.
2. Field completo: required, helper, error, readonly, async/loading.
3. Custom Dropdown pesquisável e Select curto.
4. Modal/Confirmation com focus trap e size variants.
5. Loading, Empty e Error components.
6. Tooltip e icon system.
7. Sidebar/drawer responsiva.
8. User/Profile menu e logout visual.
9. History Entry/detail.
10. Sorting contract para Table.
11. Page Header, Action Bar e Detail Panel canónicos.

### P2 — bloqueia módulo

1. Peso deve adotar o Calendar canónico.
2. Shell precisa do module switcher e account interaction.
3. CM/MF precisam de handoff visual individual completo ou validação de que o brief conjunto basta.
4. Pegamentos precisa de mockup canónico limpo sem áreas legacy contraditórias.
5. Tool creation precisa de mockup próprio consolidado.
6. History transversal precisa de composição canónica.
7. Boquilhas/Admin/Armazém precisam de substituir confirmações nativas no mockup de referência final.
8. Job On precisa de validação funcional da nomenclatura MP/CM antes do label final.
9. Fechar convenção técnica dos nomes de PDF e estratégia quando a pasta local/partilhada não está acessível noutro computador.
10. Fechar estados e recuperação de permissão do File System Access API sem representar um ficheiro local como dado garantidamente disponível no servidor.

### P3 — cosmético / pode ser refinado depois

1. afinação da intensidade de sombras;
2. motion normal para entrada de menus/modais;
3. truncation/tooltip em referências muito longas;
4. microcopy uniforme de empty states;
5. refinamento de densidade em ecrãs ultra-wide;
6. animação de abertura de cartões expansíveis.

## 20. Implementation Order — Design Only

Esta ordem constrói a fundação visual; não é um plano da aplicação.

1. Resolver decisões P0.
2. Tokens completos.
3. Typography e foundation/reset.
4. Primitive controls e icon rules.
5. Buttons e state machine.
6. Fields e forms.
7. Cards, headers e layout primitives.
8. Lists, Tables, Rows, sorting e Pagination.
9. Filter Bar, Search, Select e Dropdown.
10. Status, Alert, Toast, Loading, Empty e Error.
11. Modal, Confirmation Dialog, Context Menu e Tooltip.
12. Calendar canónico.
13. Header, navigation, profile/account, Sidebar e Shell.
14. Page Anatomy components: Page Header, Action Bar e Detail Panel.
15. History Entry/detail.
16. Login e shell test page como prova da fundação.
17. Module layouts sem redefinir componentes.
18. Responsive pass em 1200, 980, 720 e mobile estreito.
19. Keyboard/accessibility pass.
20. Visual regression pass entre módulos.

Gate obrigatório antes do passo 17: uma página-laboratório deve apresentar todos os componentes e estados usando apenas CSS global.

## 21. Design Acceptance Checklist

### Foundation

- [ ] Todos os valores visuais vêm de tokens aprovados.
- [ ] Tipografia usa tokens exatos, sem intervalos ambíguos.
- [ ] Layers/z-index usam escala única.
- [ ] Breakpoints e gutters são canónicos.
- [ ] `prefers-reduced-motion` está implementado.
- [ ] Contraste WCAG AA foi verificado.

### CSS architecture

- [ ] Nenhuma página contém `<style>` de design.
- [ ] Nenhum markup contém `style="..."` de design.
- [ ] Não existe `site.css` legacy a sobrepor componentes.
- [ ] Module CSS contém apenas composição/layout.
- [ ] Universal components não são redefinidos por módulo.
- [ ] Load order é tokens → components → layout → module.
- [ ] Existe uma página de catálogo/teste dos componentes.

### Components

- [ ] Um único Button system.
- [ ] Filled → inverted hover/focus está consistente.
- [ ] Loading e disabled impedem ação repetida.
- [ ] Um único Field system de 40px.
- [ ] Um único Select/Dropdown contract.
- [ ] Um único Card system.
- [ ] Um único Table/List/Row system.
- [ ] Um único Calendar.
- [ ] Modal e Confirmation não usam APIs nativas.
- [ ] Tooltip e icon sizing são consistentes.
- [ ] Feedback usa inline/toast/modal conforme o contrato.

### Interaction

- [ ] Um clique seleciona rows aplicáveis.
- [ ] Duplo clique abre rows aplicáveis.
- [ ] Nenhum atalho de teclado específico do BA DMO permanece definido; clique seleciona, duplo clique abre.
- [ ] Focus é sempre visível.
- [ ] Filter change limpa seleção invisível e regressa à página 1.
- [ ] Paginação apresenta 20/40/60, total e página.
- [ ] Ações dependentes ficam fora da lista.
- [ ] Card expandido gere dirty state e foco.

### Shell

- [ ] Header contém logo, módulo/página e utilizador/título.
- [ ] Module navigation tem active indication.
- [ ] Definições está alinhado à direita.
- [ ] Account/logout interaction está definida.
- [ ] Sidebar contextual não se confunde com navegação.
- [ ] Conteúdo usa largura e gutters canónicos.
- [ ] Tablet/mobile não criam scroll horizontal da página.

### Page/module

- [ ] Cada página segue a anatomia canónica ou documenta exceção.
- [ ] Hierarquia visual preserva informação operacional crítica.
- [ ] Empty, loading e error foram implementados.
- [ ] Responsive foi verificado nos breakpoints.
- [ ] O mesmo conceito tem o mesmo visual entre módulos.
- [ ] Histórico separa snapshot de estado atual.
- [ ] Dados fictícios dos mockups não chegaram à aplicação.
- [ ] Nenhuma regra de negócio foi inferida do layout.

### Visual regression

- [ ] Capturas de referência existem para desktop/tablet/mobile.
- [ ] Button, Field, Card, Table, Calendar e Modal têm testes visuais.
- [ ] Comparação cross-module deteta divergências de altura, radius, spacing e typography.
- [ ] Temas/zoom/text scaling não quebram o layout.

## 22. Final Verdict

**DESIGN SYSTEM READY FOR FRESH BUILD: NO**

Motivo: a especificação normativa é forte, mas tokens essenciais e a arquitetura CSS executável ainda não estão completos; mockups contêm implementações concorrentes.

**COMPONENT CONTRACT READY: NO**

Motivo: componentes principais têm direção, mas Date Picker, Dropdown, Tooltip, feedback states, sorting, profile menu, History Entry e detalhes de Modal/Field ainda estão incompletos.

**SHELL VISUAL CONTRACT READY: NO**

Motivo: header e tabs estão bem definidos, mas module switching, account/logout, page width/gutters, layers e sidebar responsive não estão fechados.

**MODULE DESIGN COVERAGE: 1/17 READY**

Login é a única área sem bloqueio visual específico significativo. Os restantes módulos estão `PARTIAL`, sobretudo por dependência de componentes globais, estados incompletos ou contradições registadas.

### Blocking design gaps

- tokens P0;
- decisão de Button size;
- uma arquitetura CSS sem estilos locais/legacy;
- Calendar único;
- Shell navigation/account/responsive;
- componentes P1 indicados na secção 19.

### Non-blocking design gaps

- sombras/motion;
- microcopy de empty states;
- densidade ultra-wide;
- tooltips de truncation;
- animações de expansão.

### Information that must come from Functional Spec / Verified Knowledge

- atores, roles, capabilities e acesso real;
- significado e ownership de Referência, lote, ferramenta, máquina, produção e estados;
- regras de criação, duplicação, correção, fecho, eliminação e arquivo;
- cálculos e arredondamentos;
- origem autoritativa de cada dado;
- compatibilidades e filtros funcionais;
- lifecycle de aprovações, reparações, movimentos e Job On;
- regras de histórico/snapshot/auditoria;
- email, documentos, imagens e storage;
- nomenclatura final MP/CM;
- integrações e comportamento em concorrência/falha.

### Condição para mudar o veredicto para YES

1. resolver P0;
2. implementar e documentar o catálogo global de componentes P1;
3. fechar Shell visual;
4. criar um Calendar único;
5. remover competição de CSS na implementação nova;
6. rever cada módulo contra a matriz e elevar estados/response pendentes;
7. executar visual regression e checklist da secção 21.

O pacote deve continuar a acompanhar Verified Knowledge. Este contrato indica **como construir o design desde a fundação**, mas não substitui validação funcional.


## END FILE CONTENT

---

# FILE 006

## Source Path
`docs\DESIGN_INPUT_EXTRACTION.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Extração de ideias para o design global

Fonte analisada: `ba-project-main-flattened(1).md`  
Objetivo: separar padrões reutilizáveis de UI de ideias ainda específicas ou em discussão.

## 1. Incorporado na especificação global

| Ideia extraída | Aplicação no design |
|---|---|
| UI não contém regras de negócio | Botões solicitam comandos; a aplicação autoriza, valida e persiste |
| Não inferir factos em falta | Mostrar ausência/por confirmar; nunca transformar ausência em estado |
| Pesquisa ambígua não seleciona | Resultados apresentam contexto e exigem escolha explícita |
| Relações usam identidades estáveis | Labels visuais não servem como chaves entre módulos |
| Snapshot histórico ≠ estado live | Blocos e etiquetas diferentes, com data/atualização |
| Não confirmar antes de persistir | Estado de processamento, sucesso só após confirmação |
| Falha preserva dados | Editor continua aberto com valores recuperáveis |
| Correções são auditáveis | Autor, data/hora, motivo e anterior/novo quando aplicável |
| Todas as ações relevantes são auditadas | Um evento factual append-only por ação, ligado ao utilizador, módulo e entidade; consulta anual no Admin |
| UI visibility ≠ autorização | Ações validadas fora da camada visual |
| Registo frequente deve ser rápido | Padrão keyboard-first com foco, guardar, limpar e repetir |
| Componentes partilhados ≠ domínio partilhado | BQ/CM/MF reutilizam UI sem fundir regras ou identidades |

Estas regras foram adicionadas às secções 29–34 de `DMO_DESIGN_SYSTEM.md`.

## 2. Padrões específicos confirmados como evidência útil

### Reparação interna — registo rápido

Fluxo visual recomendado quando o módulo for desenhado:

```text
utilizador autenticado
→ pesquisa/scan com foco
→ selecionar ferramenta sem ambiguidade
→ confirmar intervenção
→ guardar
→ limpar e regressar ao campo inicial
```

- teclado primeiro;
- autor e data/hora automáticos;
- nota opcional salvo regra confirmada;
- histórico filtrável por operador, ferramenta/tipo e período;
- correções auditáveis.

A associação a Job On/Produção ficou confirmada: escolher a Linha resolve automaticamente o contexto ativo nessa Linha e data; ambiguidades exigem escolha explícita.

O detalhe completo do fluxo, Histórico e correções está em `REPARACAO_INTERNA_DESIGN_BRIEF.md`.

### Armazém — transferência e substituição

Ideias úteis para o futuro handoff:

- localização atual permanece válida até confirmação da operação;
- destino proposto aparece separado;
- cancelar não altera nada;
- falha não mostra sucesso;
- em `Substituir`, apresentar ferramenta atual e substituta lado a lado antes de confirmar;
- ações rápidas Entrada/Saída/Substituir devem funcionar bem em PWA/mobile.

### Job On — landing e detalhe

Ideias visuais fortes, ainda dependentes das decisões funcionais:

- landing com próximos dias/produções;
- cada entrada mostra Referência, Produção e Máquina;
- abrir a entrada leva diretamente ao Job On;
- o Job On guardado identifica as instâncias e lotes concretos de CM/MP, MF e BQ usados pela produção; Peso e Pegamentos não voltam a escolher alternativas;
- em modo consulta, a folha é não editável e não mostra disponibilidade live; em modo edição, substituir uma ferramenta abre a lista filtrada com posição do Armazém, contexto atual, estado técnico e `% de uso`;
- selecionar uma ferramenta na edição do Job On não cria movimento de Armazém;
- registos numéricos de Peso/Pegamentos ficam no servidor; PDFs enviados à Produção usam o diretório principal local e a subpasta definida na criação do lote do Peso;
- detalhe mantém contexto no topo;
- ferramentas selecionadas mostram lote e contexto atual sem duplicar a fonte dos dados;
- histórico anterior deve priorizar mesma Referência + mesma Máquina;
- comparação separa snapshot anterior e estado atual;
- avisos distinguem informação normal, atenção e bloqueio real, sempre explicando a ação necessária;
- tarefas atribuídas aparecem em destaque ao operador e com estado de conclusão para o responsável.

Estados exatos, intervalo de planeamento, permissões de edição e regras de bloqueio continuam em discussão.

Os cinco exemplos visuais fornecidos posteriormente foram analisados em detalhe em `JOB_ON_DESIGN_BRIEF.md`. Confirmam a densidade da folha, o contexto de produção e a existência visual dos grupos MP, MF, BQ, PU, CAL, AN, ARR, PI, CS, TP e FO, mas não confirmam automaticamente a semântica final das siglas nem os campos obrigatórios.

### Reparação — tipos diferentes na mesma área

- BQ, CM e MF podem viver na mesma área Reparação;
- cada tipo mantém campos, frequência, reparadores, estados e histórico próprios;
- filtros e componentes podem ser partilhados;
- a UI não transforma esta proximidade numa fusão de entidades.

## 3. Ideias não promovidas a regra

Não foram adicionadas como requisito global:

- estados finais exatos do Job On;
- limites automáticos de utilização;
- percentagem de utilização como diagnóstico ou bloqueio;
- regra concreta de rebaixo `0,3 mm`;
- associação obrigatória de Reparação interna a Produção;
- lista definitiva de campos de cada tipo de ferramenta;
- arquitetura antiga de aplicações separadas;
- schemas, nomes de tabelas ou contratos JavaScript antigos.

Estas matérias permanecem dependentes de decisão funcional e devem ser resolvidas no handoff do respetivo módulo.

## 4. Impacto nos próximos designs

Ao desenhar Reparação, Reparação interna, Armazém ou Job On:

1. começar pelo `MODULE_UI_HANDOFF_TEMPLATE.md`;
2. usar as regras globais das secções 29–34;
3. consultar os padrões específicos acima;
4. marcar itens ainda em discussão em vez de os apresentar como comportamento final;
5. não transportar a arquitetura ou nomes técnicos do sistema antigo.


## END FILE CONTENT

---

# FILE 007

## Source Path
`docs\DMO_DESIGN_SYSTEM.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Portal DMO — especificação global de UI

Versão: 2.7  
Estado: normativa  
Âmbito: todos os módulos presentes e futuros do Portal DMO

## 1. Autoridade deste documento

Para a passagem completa de implementação, ler primeiro `CODER_IMPLEMENTATION_HANDOFF.md`. Este documento é a autoridade visual e de componentes; os briefs de módulo são a autoridade funcional específica.

Este é o contrato global de interface. Define como uma página é composta, como os componentes se apresentam e como respondem ao utilizador.

Ordem de autoridade:

1. regras de negócio confirmadas do módulo;
2. esta especificação global;
3. handoff específico do módulo;
4. mockup visual.

Um handoff de módulo pode definir campos, permissões e fluxos próprios, mas não pode alterar silenciosamente botões, listas, calendários, cabeçalhos, tabs, estados ou tipografia. Uma exceção precisa de ser documentada e aprovada.

## 2. Princípios obrigatórios

1. A mesma ação tem sempre a mesma aparência e interação.
2. O utilizador permanece na página sempre que a tarefa puder ser concluída inline.
3. Informação compacta, sem campos desproporcionais ao conteúdo.
4. A cor complementa o texto; nunca é o único indicador.
5. Permissões pertencem à aplicação, não ao CSS.
6. Componentes usam tokens globais, sem cores ou medidas arbitrárias por módulo.
7. Dados de demonstração nunca passam para produção.
8. Não inventar campos, estados ou fontes de dados para preencher lacunas.

## 3. Anatomia obrigatória de um módulo

Cada módulo autenticado usa esta ordem:

1. Header global.
2. Barra de tabs.
3. Título e descrição da vista ativa.
4. Toolbar de ações e filtros, quando necessária.
5. Contexto ativo ou métricas essenciais.
6. Conteúdo operacional.
7. Ações dependentes de seleção fora das listas.
8. Feedback inline/toast.

Não repetir o título da tab em cartões sem acrescentar contexto.

## 4. Tokens canónicos

```css
:root {
  --dmo-brand-950: #0f1d2a;
  --dmo-brand-900: #193046;
  --dmo-brand-800: #234463;
  --dmo-brand-700: #315d88;
  --dmo-brand-600: #3c73a8;
  --dmo-brand-500: #568dc3;
  --dmo-brand-300: #98b9da;
  --dmo-brand-200: #bdd3e8;
  --dmo-brand-100: #d9e6f2;
  --dmo-brand-050: #e8eff7;

  --dmo-surface-page: #f6f9fc;
  --dmo-surface-card: #ffffff;
  --dmo-surface-subtle: #f1f6fa;
  --dmo-border: #d9e6f2;
  --dmo-text: #172d42;
  --dmo-text-muted: #64778a;
  --dmo-text-on-color: #ffffff;

  --dmo-success: #527c72;
  --dmo-success-soft: #e5f0eb;
  --dmo-warning: #a97943;
  --dmo-warning-soft: #f7f0e7;
  --dmo-danger: #9a625d;
  --dmo-danger-soft: #f3e9e7;
  --dmo-pending: #315d88;
  --dmo-pending-soft: #e7eef5;
  --dmo-disabled: #cbd5df;

  --dmo-space-1: 4px;
  --dmo-space-2: 8px;
  --dmo-space-3: 12px;
  --dmo-space-4: 16px;
  --dmo-space-5: 20px;
  --dmo-space-6: 24px;
  --dmo-space-8: 32px;

  --dmo-radius-control: 8px;
  --dmo-radius-card: 12px;
  --dmo-radius-modal: 16px;
  --dmo-radius-pill: 999px;
  --dmo-control-height: 40px;
  --dmo-control-height-compact: 34px;
  --dmo-header-height: 76px;
  --dmo-tabs-height: 52px;
  --dmo-sidebar-width: 276px;
  --dmo-shadow-card: 0 8px 24px rgba(25,48,70,.055);
  --dmo-shadow-menu: 0 10px 24px rgba(15,29,42,.22);
  --dmo-shadow-modal: 0 25px 70px rgba(15,29,42,.35);
  --dmo-transition-fast: 150ms ease;
}
```

É proibido introduzir um novo hexadecimal num componente quando já existe um token com o mesmo papel.

## 5. Tipografia

Família:

```css
font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont,
  "Segoe UI", sans-serif;
```

| Elemento | Tamanho | Peso | Cor |
|---|---:|---:|---|
| Título principal da vista | 23–24px | 700–800 | `--dmo-text` |
| Título do header | 18px | 800 | `--dmo-text` |
| Título de modal | 18px | 700 | `--dmo-text` |
| Título de cartão | 15–16px | 700 | `--dmo-text` |
| Corpo | 14px | 400 | `--dmo-text` |
| Botão | 12–13px | 700 | depende do estado |
| Label | 11px | 700–750 | `--dmo-text-muted` |
| Ajuda/metadados | 10–12px | 400–600 | `--dmo-text-muted` |
| Cabeçalho de tabela | 10–11px | 750–800 | `--dmo-text-muted` |

Não criar tamanhos intermédios apenas para fazer um elemento “caber”. Ajustar primeiro o layout.

## 6. Header global

Todas as páginas autenticadas usam:

```html
<header class="dmo-app-header">
  <img class="dmo-app-header__logo" src="logo_recolored(1).png" alt="BA Glass">
  <div class="dmo-app-header__page">
    <h1>Título da página</h1>
    <p>Contexto curto do módulo</p>
  </div>
  <div class="dmo-app-header__user">
    <strong data-user-profile-name>Nome</strong>
    <span data-user-profile-title>Título ou função</span>
  </div>
</header>
```

Regras:

- logo oficial com 44px no desktop;
- título da página ao lado do logo;
- subtítulo curto, sem repetir a tab;
- nome e título/função no canto direito;
- título/função vem do perfil editável na Administração;
- `profileTitle` é informativo e não concede acesso;
- altura aproximada de 76px;
- o módulo não escreve diretamente o nome ou título do utilizador na implementação final.

## 7. Tabs

- Tabs operacionais ficam à esquerda.
- `Definições` e `Administração` ficam à direita.
- Tab ativa: texto azul e linha inferior azul.
- Tab inativa: texto muted, sem fundo preenchido.
- Hover: fundo azul muito claro.
- Uma tab troca a vista dentro do módulo; não executa comandos.
- Tabs não autorizadas não são renderizadas.
- A tab não substitui o título da vista.

## 8. Botões

### Estados visuais

Todos os botões têm dois estados principais:

1. Repouso: fundo preenchido, contorno da mesma cor e texto branco.
2. Hover/foco: fundo branco, contorno e texto na cor original.

Não usar `brightness`, transparência ou alteração mínima de tom no hover.

### Tamanhos

| Tipo | Altura | Padding horizontal | Uso |
|---|---:|---:|---|
| Compacto | 34px | 10–12px | tabelas, toolbars, ações secundárias |
| Normal | 40px | 12–16px | formulários e ações principais |
| Apenas ícone | 34–40px | 0 | fechar, menu, navegação calendário |

- A largura segue o texto; não criar botões largos sem necessidade.
- Botões do mesmo grupo têm a mesma altura.
- Ação destrutiva usa `--dmo-danger`.
- Ação positiva usa `--dmo-success` apenas quando a semântica o exige.
- Desativado: cinzento, sem hover, com motivo compreensível pelo contexto.
- Ações primárias ficam à direita no rodapé de formulários; `Cancelar` vem antes de `Guardar/Criar`.

## 9. Ações que expandem cartões

Esta é a interação padrão para criar, editar ou filtrar sem sair da página.

Ao clicar num botão como `Criar novo`, `Editar` ou `Filtros`:

1. expande um cartão imediatamente abaixo da toolbar ou do bloco que originou a ação;
2. o botão atualiza `aria-expanded="true"` e aponta para o cartão com `aria-controls`;
3. o primeiro campo útil recebe foco;
4. o conteúdo existente permanece visível quando ajuda a tarefa;
5. apenas um editor principal fica aberto na mesma zona;
6. `Cancelar` fecha o cartão e limpa apenas o rascunho não guardado;
7. se houver alterações, fechar exige confirmação;
8. após guardar, o cartão fecha ou muda para modo de detalhe, e a lista é atualizada;
9. abrir o formulário não cria ainda um registo persistente;
10. não navegar para outra página nem usar modal para formulários extensos.

### Cartão de filtros

- O botão chama-se `Filtros` e pode mostrar a contagem ativa: `Filtros · 2`.
- Ao abrir, apresenta apenas filtros relevantes para essa lista.
- `Aplicar filtros` atualiza resumo, calendário e lista relacionados.
- `Limpar filtros` repõe todos os filtros dessa vista.
- Fechar o cartão não limpa filtros aplicados.
- Filtros simples e permanentes podem ficar sempre visíveis; não duplicar os mesmos filtros num cartão.

## 10. Campos e formulários

- Altura normal: 40px; compacta: 34px.
- Um botão colocado na mesma linha de inputs/selects usa `40px` para alinhar com os controlos. Esta regra contextual não altera os botões standard de `36px`, as ações de rodapé nem os botões de paginação `36 × 36px`.
- Esta regra é obrigatória no componente global: não deve ser recriada ou ajustada manualmente em cada módulo. As filas canónicas usam `.filters`, `.search`, `.history-filters` ou `.dmo-filter-row`; o botão filho direto herda automaticamente `40px`.
- Em seletores segmentados, a opção selecionada fica preenchida com a cor principal e texto branco; as restantes ficam brancas, delineadas e com texto na cor principal. O primeiro valor funcional do módulo deve iniciar selecionado.
- Labels ficam acima dos campos.
- Foco: borda azul e halo discreto.
- Erro: mensagem imediatamente abaixo do campo.
- Campos obrigatórios são validados antes de guardar.
- Placeholder dá exemplo, não substitui label.
- Valores numéricos apresentam no máximo duas casas decimais, salvo regra de domínio explícita.

### Largura proporcional

| Dado | Largura esperada |
|---|---|
| Pesquisa, nome, referência, observações | flexível |
| Máquina/linha, estado curto | 90–140px |
| Lote, quantidade, percentagem | 90–130px |
| Data | 150–180px |
| Produção | 110–140px |
| Notas | ocupa o espaço restante |

Não usar largura total para dois ou três dígitos.

### Ordem

- campos mais usados e de identificação primeiro;
- data no final da linha de identificação, salvo razão operacional;
- observações/notas na última linha;
- campos relacionados ficam juntos;
- uma linha não deve ficar artificialmente cheia quando campos pequenos podem ser compactados.

## 11. Dropdowns e pesquisa

- Usar o dropdown canónico, nunca `datalist` nativo quando o menu precisa de estilo consistente.
- Mesma altura e borda dos inputs.
- Hover de opção: `--dmo-brand-050`.
- Opção selecionada: `--dmo-brand-100`, texto `--dmo-brand-700`.
- Pesquisa incremental quando existirem muitas opções.
- `Escape` fecha; setas percorrem; `Enter` escolhe.
- Lista vazia mostra `Sem resultados` dentro do menu.
- Dados filtrados devem indicar a origem quando isso evita ambiguidade operacional.

### Opções de negócio configuráveis

- Valores que possam crescer ou mudar — materiais, tipos, versões, adaptadores, reparadores e equivalentes — não ficam hardcoded no HTML ou no frontend.
- Cada opção pertence a um catálogo do módulo e campo corretos, configurável em `Definições` por utilizador autorizado.
- A gestão permite adicionar, editar, ordenar e desativar; não usar eliminação destrutiva quando a opção já aparece no histórico.
- Desativar retira a opção de novas escolhas, mas preserva o rótulo guardado em registos e snapshots antigos.
- Máquinas, paginação, estados de sistema e outros valores técnicos usam os respetivos catálogos canónicos; não devem ser misturados num catálogo genérico só por também aparecerem num dropdown.

### Seletor contextual de registos relacionados

Usar quando um campo escolhe um registo existente de outro módulo, por exemplo um lote de CM, MF ou BQ:

- o menu rápido apresenta apenas resultados fornecidos pela fonte autoritativa e compatíveis com os filtros explícitos do contexto;
- quando a quantidade ou o detalhe dos resultados não couber no menu, incluir `Ver todos os resultados compatíveis` e abrir a lista canónica;
- a lista completa usa um clique para selecionar e duplo clique para abrir o registo; a confirmação da relação usa uma ação externa à lista;
- um resultado nunca é escolhido automaticamente apenas por ser o primeiro;
- cada opção mostra contexto suficiente para distinguir entidades semelhantes;
- se um valor copiado deixar de ser compatível, preservá-lo como valor anterior visível e apresentar aviso; só bloquear ou exigir nova escolha quando o contrato do domínio o determinar explicitamente;
- o seletor associa o ID estável do registo e não cria, edita nem elimina dados do módulo de origem;
- ausência real de resultados, erro de carregamento e falta de permissão são três estados diferentes e devem ter mensagens diferentes.
- uma pesquisa sem correspondência nunca cria automaticamente a entidade pesquisada; a criação continua no fluxo autorizado do módulo de origem.

## 12. Cartões

- Fundo branco, borda subtil, raio 12px e sombra muito leve.
- Padding: 16–20px.
- Espaço entre cartões: 12–16px.
- Evitar cartões dentro de cartões sem hierarquia.
- Cartão selecionável tem hover discreto e foco visível.
- Alerta usa indicador lateral e mensagem orientada à ação.
- Não usar selos para estados normais desnecessariamente.

## 13. Listas e tabelas canónicas

Todos os módulos usam a mesma regra, sem botões adicionais de abertura:

- um clique seleciona uma única linha;
- duplo clique abre o registo/detalhe associado;
- não existe atalho de teclado específico do BA DMO; a seleção/abertura é feita com clique/duplo clique;
- seleção usa classe `selected` e `aria-selected="true"`;
- contentor usa `data-dmo-list`;
- linha usa `data-dmo-row` e `data-id` estável;
- não existe botão `Abrir folha selecionada`;
- ações como corrigir, eliminar ou editar ficam fora da lista e dependem da seleção;
- linhas não contêm botões de ação repetidos;
- se um filtro remover a seleção, limpar o detalhe ou selecionar explicitamente o primeiro resultado conforme o fluxo documentado.

### Tabelas

- Cabeçalho neutro, não azul vivo.
- Cabeçalho fixo quando existir scroll vertical.
- Linhas com 40–46px.
- Números alinhados de forma consistente.
- Densidade suficiente para evitar scroll horizontal desnecessário.
- Scroll horizontal fica dentro do cartão e apenas quando inevitável.
- Não repetir colunas disponíveis no detalhe sem valor operacional.

### Limite e paginação

- Todas as listas com dados usam paginação; não crescer indefinidamente na página.
- O seletor de limite apresenta exclusivamente `20`, `40` e `60` linhas.
- O valor inicial é `20`, salvo requisito funcional explícito diferente.
- O rodapé mostra `N registos · Página X de Y`.
- Navegação usa setas `Anterior` e `Seguinte`, com rótulo acessível e estado desativado nos limites.
- Alterar filtros ou limite regressa à página 1 e limpa uma seleção que deixe de estar visível.
- Paginação é controlada pelos dados/serviço; o frontend não carrega todo o universo apenas para o cortar visualmente.

### Movimentos

- Entrada: texto explícito e fundo verde muito claro.
- Saída: texto explícito e fundo laranja muito claro.
- Seleção sobrepõe-se visualmente ao tom do movimento sem esconder o texto.

## 14. Filtros de listas

Ordem recomendada:

1. pesquisa livre;
2. tipo;
3. estado;
4. entidade específica do domínio;
5. intervalo de datas;
6. quantidade de linhas/página.

Regras:

- pesquisa descreve o que aceita: `Referência, lote ou linha`;
- intervalo usa `Desde` e `Até`;
- filtros afetam todos os resumos ligados à lista;
- mostrar um resumo curto dos filtros ativos;
- `Limpar filtros` é uma única ação;
- não ocultar a ausência de resultados: apresentar estado vazio claro.

## 15. Calendário canónico

- Todos os módulos reutilizam o mesmo componente e CSS.
- Cabeçalho: mês/ano ao centro, anterior e seguinte nas extremidades.
- Semana começa em segunda-feira.
- Dias usam grelha de sete colunas.
- Dia com registos apresenta ponto discreto.
- Um clique seleciona o dia e filtra a lista associada.
- Dia selecionado usa fundo azul e texto branco.
- `Mostrar todas as datas` remove apenas o filtro de data.
- Alterar mês não seleciona automaticamente um dia.
- Datas sem registos continuam clicáveis quando o fluxo permite consulta vazia.
- Quando um módulo permite criar a partir de uma data futura, a ação de criação fica junto da lista/estado vazio associado ao dia; o calendário em si não contém botões diferentes por módulo.
- Uma data passada pode filtrar movimentos e uma data futura pode iniciar criação, mas ambos reutilizam exatamente o mesmo estado visual de seleção.
- registos planeados usam ID estável; mudar uma data atualiza o mesmo evento e não duplica entradas;
- alterações de período só aparecem no calendário depois de persistidas;
- um campo de data operacional pode ser atualizado enquanto o registo estiver ativo; depois de fechado, o último valor persistido torna-se o valor final e as alterações anteriores permanecem auditáveis;
- Teclado e `aria-pressed` são obrigatórios.
- É proibido criar um calendário visual diferente dentro de um módulo.

## 16. Estados visuais

| Estado | Fundo | Texto |
|---|---|---|
| Pendente | `--dmo-pending-soft` | `--dmo-pending` |
| Aprovado/ativo | `--dmo-success-soft` | `--dmo-success` |
| Atenção | `--dmo-warning-soft` | `--dmo-warning` |
| Não aprovado/erro | `--dmo-danger-soft` | `--dmo-danger` |
| Inativo | superfície cinzenta | `--dmo-text-muted` |

- Tipo de registo não é estado.
- Estado aparece com texto; a cor não basta.
- Tons são dessaturados e próximos da base do sistema.

## 17. Alertas e feedback

### Inline

- junto do elemento que exige ação;
- mensagem curta e concreta;
- não usar `alert()` ou `prompt()` para explicar conflitos;
- exemplo: `Referências diferentes nesta linha. Remova uma referência para continuar.`

### Toast

- confirmação breve e não bloqueante;
- canto inferior;
- desaparece automaticamente;
- erro que exige correção permanece também junto do campo.

### Confirmação

Usar apenas para eliminar, fechar, perder alterações, reset de palavra-passe ou outra ação difícil de reverter.

## 18. Modais

- Apenas para ações rápidas, confirmação ou edição focada.
- Formulários longos abrem inline.
- Cabeçalho: título, contexto e fechar.
- Rodapé compacto com ações à direita.
- `Escape` fecha quando não há perda de dados.
- Fundo escurecido, mas reconhecível.
- Largura acompanha o conteúdo; não ocupar o ecrã sem necessidade.

## 19. Menu contextual `…`

- Extensão visual do cartão que o abriu.
- Largura ajustada ao maior texto.
- Padding compacto.
- Hover azul suave/escuro conforme a superfície de origem.
- Não usar branco dominante sobre painel escuro.
- Ação destrutiva mantém texto vermelho discreto.
- Com registo: mostrar apenas ações válidas, por exemplo substituir/remover.
- Sem registo: mostrar apenas adicionar.

## 20. Painel lateral

- Fixo no desktop quando o módulo acompanha máquinas/linhas.
- Fundo `--dmo-brand-900/950`.
- Mostra apenas estado operacional atual.
- Totais analíticos pertencem ao conteúdo da página.
- Clique no cartão abre o registo associado quando essa regra estiver definida.
- Conflitos aparecem no próprio cartão com mensagem curta; não abrir escolha por `prompt`.
- Em mobile transforma-se em gaveta ou bloco recolhível.

## 21. Paginação e estados vazios

- Paginação fica abaixo da lista.
- `Anterior` e `Seguinte` desativam nos limites.
- Mostrar `N registos · Página X de Y`.
- Estado vazio explica o motivo e o próximo passo.
- Não mostrar grandes áreas tracejadas sem conteúdo útil.
- Carregamento usa skeleton ou mensagem curta; não deslocar drasticamente o layout.

## 22. Responsividade

Breakpoints de referência: 1200px, 980px e 720px.

- Reorganizar grelhas antes de reduzir texto.
- Preservar campos essenciais.
- Tabelas podem ter scroll dentro do cartão.
- Botões mantêm área adequada ao toque.
- Header preserva logo, título e identidade.
- Side panel torna-se recolhível.
- Nunca criar scroll horizontal na página inteira.

## 23. Acessibilidade

- Objetivo mínimo WCAG AA.
- Foco visível em todos os controlos.
- Operação por teclado em tabs, listas, dropdowns, calendários e menus.
- `aria-label` em botões apenas com ícone.
- `aria-expanded` e `aria-controls` em expansores.
- `aria-live` para feedback importante.
- Não depender apenas de cor.
- Respeitar `prefers-reduced-motion`.

## 24. Login e Administração

### Login

- Identidade à esquerda e formulário à direita no desktop.
- Conteúdo centrado, sem vazio vertical excessivo.
- Email, palavra-passe, mostrar/ocultar e Entrar.
- Erros no formulário, sem pop-up.
- Não mostrar credenciais de teste.

### Administração

- Página própria para administradores.
- Gestão de utilizadores, templates de acesso e aplicações. A landing page global Job On não é configurável por utilizador.
- O campo livre `profileTitle` alimenta o título/função no header.
- `profileTitle` não altera permissões.
- Reset de palavra-passe exige confirmação e auditoria; nunca mostra a palavra-passe atual.
- A tab `Auditoria` usa a tabela/lista canónica: um clique seleciona, duplo clique abre detalhe, 20/40/60 linhas e filtros compactos.
- O histórico anual mostra factos por utilizador e módulo; não inclui pontos, ranking nem interpretação automática.
- Eventos são append-only e visualmente distinguem sucesso, falha, acesso negado e correção sem depender apenas da cor.

## 25. Contrato para criar um módulo novo

Antes de desenhar:

1. confirmar objetivo, atores e permissões;
2. confirmar entidades e fontes de dados existentes;
3. listar ações e estados reais;
4. identificar o que é seleção, abertura, criação, edição e filtro;
5. não inventar nomes de campos ou regras em falta.

Construção obrigatória:

1. aplicar o header global;
2. criar tabs operacionais e colocar Definições/Administração à direita;
3. criar título e descrição da vista;
4. definir toolbar compacta;
5. ações de criação/edição/filtro expandem cartões inline;
6. usar campos proporcionais;
7. aplicar lista canónica: clique seleciona, duplo clique abre;
8. reutilizar o calendário canónico quando houver datas;
9. colocar ações dependentes de seleção fora da lista;
10. usar estados e cores globais;
11. documentar estados vazios, erros e carregamento;
12. verificar desktop, 980px e 720px;
13. garantir teclado e atributos ARIA;
14. criar handoff do módulo apenas com regras específicas do domínio.

O handoff de cada módulo deve explicar:

- finalidade de cada tab;
- origem de cada grupo de dados;
- comportamento de cada botão;
- cartão que expande e campos apresentados;
- filtros e componentes afetados;
- ações de clique simples e duplo clique;
- estados, validações e mensagens;
- permissões necessárias;
- o que acontece depois de guardar, cancelar, eliminar ou fechar;
- integrações futuras sem inventar contratos ainda não confirmados.

## 26. Contratos JavaScript/HTML partilhados

| Componente | Contrato |
|---|---|
| Lista | `data-dmo-list`, `data-dmo-row`, `data-id` |
| Seleção | classe `selected`, `aria-selected` |
| Calendário | `data-dmo-calendar`, `data-date` |
| Expansor | `aria-expanded`, `aria-controls` |
| Perfil | `data-user-profile-name`, `data-user-profile-title` |
| Toast | região `aria-live` |

Eventos partilhados recomendados:

- `dmo:list-select`;
- `dmo:list-open`;
- `dmo:date-select`;
- `dmo:filters-change`;
- `dmo:editor-open`;
- `dmo:editor-close`.

A lógica de domínio não deve ser duplicada dentro dos componentes visuais.

## 27. Organização técnica

```text
shared/
  styles/
    dmo-tokens.css
    dmo-components.css
    dmo-layout.css
    dmo-utilities.css
  scripts/
    dmo-interactions.js
modules/
  <module>/
    <module>.css
    <module>.js
    HANDOFF.md
```

Pode existir um único `dmo-design-system.css` na primeira implementação, mas deve manter tokens, layout e componentes claramente separados. CSS do módulo contém apenas layout específico, nunca versões próprias dos componentes globais.

## 28. Critérios de aceitação global

- Uma alteração num token atualiza todos os módulos dependentes.
- Nenhum botão usa `brightness`.
- Todos os botões respeitam filled → hover invertido.
- Formulários extensos expandem inline.
- Filtros usam o mesmo padrão de cartão e limpeza.
- Todas as listas usam clique para selecionar e duplo clique para abrir.
- Não existem botões redundantes `Abrir folha selecionada`.
- Todos os calendários são o mesmo componente.
- Header contém logo, página, nome e título/função.
- Campos pequenos não ocupam largura excessiva.
- Estados usam texto e tokens semânticos.
- A interface funciona por teclado e a 720px.
- Cada módulo possui handoff específico sem contradizer esta especificação.

## 29. Verdade dos dados, pesquisa e ambiguidade

- A UI apresenta factos vindos da fonte autoritativa; não deduz relações a partir de nomes, códigos ou ausência de dados.
- Informação em falta aparece como `Não definido`, `Não disponível` ou `Por confirmar`, conforme o significado real.
- Não converter ausência de dados em estado operacional, avaria, reparação ou indisponibilidade.
- Relações entre áreas usam IDs estáveis; o texto apresentado ao utilizador não é uma chave de integração.
- Resultados de pesquisa mostram contexto suficiente para distinguir registos semelhantes.
- Uma pesquisa ambígua nunca seleciona automaticamente um resultado.
- Quando existirem vários resultados, o utilizador escolhe explicitamente.
- Filtros reduzem apenas factos registados; não “descodificam” uma referência para inventar associações.

Contexto mínimo recomendado num resultado ambíguo, quando disponível e relevante:

- referência;
- lote;
- tipo;
- máquina/linha;
- estado;
- data ou produção associada.

## 30. Estado atual versus snapshot histórico

Quando uma página apresenta passado e presente, os dois contextos devem estar visualmente separados.

### Snapshot histórico

- mostra o que estava selecionado, conhecido ou registado naquele momento;
- inclui uma etiqueta `Na produção/registo` ou `Snapshot histórico`;
- apresenta a data do snapshot;
- não muda quando o estado atual é alterado.

### Estado atual

- apresenta dados live provenientes da respetiva fonte autoritativa;
- inclui etiqueta `Estado atual` e, quando relevante, hora da última atualização;
- pode indicar alterações ocorridas desde o snapshot;
- não reescreve o histórico.

Comparações devem apresentar ambos lado a lado ou em blocos consecutivos claramente titulados. Não misturar valores históricos e atuais no mesmo cartão sem identificação por coluna.

## 31. Comandos, persistência e feedback

- Um botão solicita uma ação sem conter a regra de negócio.
- A UI só apresenta sucesso depois da autorização, validação e persistência confirmadas.
- Durante a operação, o botão mostra estado de processamento e bloqueia submissões repetidas.
- Se a persistência falhar, preservar os dados introduzidos sempre que possível.
- Uma falha não fecha o editor nem substitui o último estado válido.
- A mensagem de erro explica a ação mínima seguinte.
- Consultas, filtros, seleção de tabs e abertura de detalhes não alteram dados persistentes.
- Estado puramente visual — tab, pesquisa, modal, seleção e scroll — não se torna dado de domínio sem motivo confirmado.
- Ocultar um botão não substitui autorização no servidor/aplicação.

## 32. Correção e auditoria na interface

Quando uma correção altera um facto relevante, o fluxo deve conseguir apresentar:

- autor;
- data/hora;
- registo afetado;
- valor anterior e novo quando aplicável;
- motivo quando exigido pelo fluxo.

Regras visuais:

- `Corrigir` é diferente de `Eliminar`;
- histórico significativo não é silenciosamente reescrito;
- registos referenciados por outros fluxos preferem desativar/arquivar ao apagamento destrutivo, quando a regra de negócio o confirmar;
- revisão anterior permanece consultável;
- alteração de um registo aprovado deve indicar claramente quando perde aprovação e exige nova decisão.

## 33. Padrão de registo rápido

Usar apenas em fluxos frequentes e simples confirmados, como uma intervenção operacional curta.

Sequência:

1. caixa de pesquisa/scan recebe foco automaticamente;
2. utilizador introduz ou seleciona o registo;
3. contexto mínimo é confirmado;
4. ação principal guarda;
5. após sucesso, formulário limpa e regressa ao estado pronto;
6. autor e data/hora são capturados automaticamente.

Regras:

- deve ser possível concluir com teclado, sem rato;
- evitar grelhas extensas quando um identificador direto é suficiente;
- notas são opcionais salvo regra explícita;
- `Enter` avança/confirma apenas quando não houver ambiguidade;
- erro mantém valor e foco no campo relevante;
- o modo rápido não reduz validação, autorização ou auditoria.

## 34. Reutilização visual sem fusão de domínio

- BQ, CM, MF e outros tipos podem partilhar cartões, listas, filtros e padrões de reparação.
- Aparência semelhante não significa que campos, estados, frequência ou regras são iguais.
- Um componente partilhado recebe dados e ações específicas; não decide regras comuns por conveniência visual.
- Agrupar fluxos numa tab ou módulo não funde identidades nem históricos.
- A apresentação recebe apenas os dados necessários para a vista, através de um modelo próprio da página.
- A interface deve poder mudar sem alterar regras, permissões, histórico ou significado persistido.

## 35. Checklist de operação em lote

Usar apenas quando várias entidades participam na mesma operação confirmada, como uma Saída programada.

- a seleção múltipla usa checkboxes explícitos e não altera a regra das listas normais;
- clique na linha continua a selecionar e duplo clique continua a abrir o detalhe, quando aplicável;
- marcar um checkbox representa progresso da tarefa, não necessariamente alteração do estado de domínio;
- a UI explica claramente quando a alteração real acontece;
- progresso individual pode ser persistido sem produzir movimentos parciais quando o domínio exige conclusão conjunta;
- a operação final deve mostrar estado de processamento e impedir submissão duplicada;
- se a conclusão atómica falhar, não apresentar sucesso parcial;
- a versão impressa contém apenas informação operacional necessária e imprimir nunca executa o comando.

## 36. Verificações recorrentes por contexto

- separar a regra reutilizável da ocorrência concreta apresentada ao utilizador;
- a regra define o contexto e a repetição; a ocorrência guarda confirmação, operador e data/hora;
- uma confirmação nunca apaga a regra nem o histórico;
- itens concluídos podem ficar ocultos por defeito, com `Mostrar concluídos`;
- recorrência usa IDs estáveis do contexto, nunca comparação de texto;
- alterações à regra aplicam-se ao futuro e não reescrevem snapshots;
- arquivar impede novas ocorrências e preserva as anteriores;
- resetar cria nova pendência e preserva a confirmação anterior, o autor e a data do reset;
- falha ao confirmar mantém o item pendente;
- checkboxes representam comandos persistidos, não efeitos visuais locais.


## END FILE CONTENT

---

# FILE 008

## Source Path
`docs\FERRAMENTAS_REGISTO_DESIGN_BRIEF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Ferramentas — criação de Referência e lotes

Estado: base funcional para design e implementação V1  
Âmbito: primeira criação de uma ferramenta e criação posterior de novos lotes

## 1. Objetivo

O mesmo fluxo cobre:

- uma Referência de ferramenta nova que nunca trabalhou;
- uma ferramenta existente fisicamente, mas introduzida no sistema pela primeira vez;
- um novo lote de uma Referência já registada.

Não criar identidades paralelas no Armazém, Job On ou Reparação. Cada integração usa o ID estável criado no domínio da ferramenta.

## 2. Dois níveis de informação

### Referência da ferramenta

Identidade comum aos respetivos lotes:

- Tipo de ferramenta: CM, MF, BQ ou outro tipo confirmado;
- Referência;
- Nome técnico;
- Owner plant, com `MG — Marinha Grande` como valor inicial confirmado.

### Lote da ferramenta

Dados próprios da ocorrência física/operacional:

- Lote;
- Processo: `NNPB` ou `PS` para lotes criados no módulo Peso;
- quantidade;
- Máquinas/Linhas onde pode trabalhar;
- Nome/número do desenho;
- revisão do desenho, quando aplicável;
- restantes características específicas do tipo de ferramenta.

Uma Referência pode possuir vários lotes. Criar um lote não duplica a Referência mestre.

## 3. Nome técnico

O `Nome técnico` é um atributo principal de identificação e ajuda a distinguir ferramentas que não ficam claras apenas pelo código da Referência.

Não confundir:

- `Nome técnico`: designação humana/canónica da ferramenta;
- `Nome/número do desenho`: identificador documental do desenho associado ao lote;
- `Referência`: código operacional da ferramenta.

Regras de UI:

- aparece imediatamente junto da Referência;
- participa na pesquisa;
- aparece nas listas, seletores do Job On e detalhes da ferramenta;
- não fica escondido apenas na ficha expandida;
- não é tratado automaticamente como único sem regra confirmada;
- alterações posteriores devem ser auditáveis.

## 4. Página `Criar novo registo`

É uma página própria dentro do domínio da ferramenta, não um modal pequeno.

Ordem visual:

1. Identificação da Referência;
2. Compatibilidade;
3. Primeiro lote;
4. Informação do desenho;
5. características específicas do tipo;
6. ações.

### Identificação da Referência

| Campo | Componente | Regra visual |
|---|---|---|
| Tipo | seletor/dropdown | pode vir definido pelo módulo atual |
| Referência | texto | largura média |
| Nome técnico | texto | campo largo e visualmente principal |
| Owner plant | dropdown/seleção | `MG — Marinha Grande` predefinido na V1 |

Não inventar outras plantas ou processos até existirem no catálogo real.

### Compatibilidade

`Máquinas/Linhas onde trabalha` usa os cartões de seleção canónicos:

- B1;
- B2;
- B3;
- C1;
- C2;
- C3.

Pode selecionar várias. A relação é guardada explicitamente; não é deduzida a partir da Referência ou desenho.

### Primeiro lote

| Campo | Dimensão esperada |
|---|---|
| Lote | compacta |
| Processo do lote | dropdown `NNPB`/`PS` no módulo Peso |
| Quantidade | compacta, numérica |
| Nome/número do desenho | média |
| Revisão | compacta |

Campos específicos de CM, MF e BQ aparecem depois desta base comum e continuam definidos pelo respetivo domínio.

## 5. Guardar uma Referência nova

Ao guardar:

1. validar campos obrigatórios;
2. verificar correspondências existentes sem fundir resultados parecidos;
3. criar a Referência mestre;
4. criar o primeiro lote associado;
5. persistir ambos como uma operação consistente;
6. só depois apresentar sucesso e abrir a ficha criada.

Se a criação do lote falhar, não deixar uma Referência mestre parcialmente criada sem indicação/recuperação prevista pelo contrato técnico.

Uma pesquisa sem resultados nunca cria automaticamente uma ferramenta. O utilizador entra explicitamente em `Criar novo registo`.

## 6. Criar novo lote por duplicação

Na ficha/lista de lotes de uma Referência existente, selecionar um lote e usar o botão externo `Novo lote a partir deste`.

A lista respeita o padrão global:

- um clique seleciona;
- duplo clique abre o lote;
- o botão de duplicação fica fora da lista.

### Informação herdada e protegida

O novo lote mantém:

- Tipo;
- Referência;
- Nome técnico;
- Processo;
- Owner plant.

Estes campos aparecem read-only. Alterá-los exige editar a Referência mestre através de outro fluxo auditável.

### Informação copiada e editável

O novo rascunho parte do lote escolhido e permite ajustar:

- novo número de lote, obrigatório;
- quantidade;
- adicionar ou remover Máquinas/Linhas permitidas;
- Nome/número do desenho;
- revisão do desenho;
- características específicas do tipo que possam variar por lote.
- configuração da tab `Verificações`: manter, editar, adicionar, remover, desativar ou reativar linhas para o novo lote.

Guardar cria um novo lote e não altera o lote de origem. A ficha identifica `Criado a partir do lote …`.

As regras de verificação são copiadas como configuração do novo lote. Ocorrências, checks, operadores e histórico do lote anterior nunca são copiados.

## 7. Lista de Referências

Pesquisa por:

- Referência;
- Nome técnico;
- lote;
- desenho;
- Máquina/Linha;
- processo do lote;
- Owner plant.

Colunas mínimas:

| Tipo | Referência | Nome técnico | Owner plant | Lotes | Processo do lote | Máquinas/Linhas |
|---|---|---|---|---|---|---|

Interação:

- um clique seleciona;
- duplo clique abre a ficha da Referência;
- `Criar novo registo` e `Novo lote a partir deste` ficam fora da lista;
- resultados ambíguos exigem escolha explícita.

## 8. Ficha da Referência

O topo mostra Tipo, Referência, Nome técnico em destaque e Owner plant. Processo é apresentado no respetivo lote quando esse campo pertence ao fluxo do Peso.

A lista de lotes mostra:

- lote;
- processo do lote, quando aplicável;
- quantidade;
- Máquinas/Linhas permitidas;
- desenho e revisão;
- informação específica relevante;
- estado atual vindo do domínio da ferramenta, quando aplicável.

Cada lote inclui a tab `Verificações` definida em `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`.

Vida útil, `Sucatado`, `Arquivado` e outros estados continuam no domínio da ferramenta e não são copiados para Armazém.

## 9. Integrações

### Job On

- pesquisa por Referência ou Nome técnico;
- resultados mostram Nome técnico, lote e máquinas permitidas;
- filtra lotes pela máquina do Job On usando relações registadas;
- não interpreta o desenho para deduzir compatibilidade;
- associa o ID estável do lote.

### Armazém

- apresenta Referência, Nome técnico e lote como identificação read-only;
- regista apenas posição e movimentos;
- não edita compatibilidade, desenho, processo, vida útil ou estado.

### Reparação

- associa intervenções e listas programadas ao ID estável da ferramenta/lote;
- consulta os dados identificadores;
- mantém o próprio fluxo sem duplicar a Referência mestre.

## 10. Evidência do procedimento de desenhos

O procedimento `OP 99 PMD 02/d — Drawing numbers` confirma:

- estruturas distintas para desenhos de acabamento, artigo, moldes, suplementos e acessórios;
- desenhos de moldes com componentes de modelo, dimensão, tipo, processo, ventilação/material e revisão;
- `MP`, `MF` e `FF` como tipos documentais de molde;
- revisão por sufixo alfabético e códigos de ensaio `E1`, `E2`, etc.;
- desenhos aprovados controlados pelo Product Development;
- cópias impressas não controladas.

Implicação para a UI:

- guardar Nome/número do desenho explicitamente;
- guardar revisão separadamente quando disponível;
- permitir abrir a fonte oficial quando existir integração;
- não gerar, decompor ou validar automaticamente o código sem contrato confirmado.

O documento usa códigos de processo documentais (`PS`, `SS`, `NN`, `P4`), enquanto o requisito operacional atual pede `NNPB/PS`. A correspondência não deve ser inferida automaticamente.

## 11. Estados vazios e conflitos

- Referência não encontrada: oferecer `Criar novo registo`, sem criação automática;
- Referência existente com mesmo código: mostrar resultados e Nome técnico antes de permitir nova criação;
- lote já existente na mesma Referência: bloquear duplicação e indicar conflito;
- nenhuma Máquina/Linha selecionada: validar segundo a obrigatoriedade confirmada;
- desenho não definido: mostrar `Não definido`;
- falha ao guardar: preservar o rascunho e não mostrar sucesso.

## 12. Questões por confirmar

- tipos abrangidos inicialmente além de CM, MF e BQ;
- campos obrigatórios por tipo;
- se Nome técnico precisa de unicidade ou apenas pesquisa/identificação;
- se Máquinas/Linhas pertencem sempre ao lote ou podem ter base na Referência;
- formato e unicidade do lote por Referência;
- catálogo futuro de owner plants;
- relação oficial entre `NNPB/PS` e códigos documentais;
- fonte oficial para abrir o desenho aprovado;
- características editáveis ao duplicar cada tipo.

## 13. Critérios de aceitação do mockup V1

- `Criar novo registo` cria Referência e primeiro lote na mesma página;
- Nome técnico tem destaque e participa nas pesquisas;
- Processo usa `NNPB/PS` e Owner plant começa em `MG — Marinha Grande`;
- Máquinas/Linhas permitem seleção múltipla;
- a lista segue clique/duplo clique canónicos;
- `Novo lote a partir deste` fica fora da lista;
- novo lote mantém identidade mestre e permite alterar dados próprios;
- quantidade, linhas permitidas, desenho e revisão identificam o lote;
- lotes anteriores nunca são alterados pela duplicação;
- Job On, Armazém e Reparação usam IDs estáveis;
- códigos de desenho não são inferidos automaticamente.


## END FILE CONTENT

---

# FILE 009

## Source Path
`docs\HANDOFF_INDEX.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Handoff consolidado — índice de implementação

## Ordem recomendada

1. Ler `DESIGN_IMPLEMENTATION_CONTRACT.md`; é a auditoria final que separa o que está pronto, parcial, em falta ou dependente de validação funcional.
2. Ler `CODER_IMPLEMENTATION_HANDOFF.md`; é o ponto de entrada que consolida funcionamento, dados, permissões, interações, design e aceitação.
3. Resolver os gaps P0/P1 do contrato antes de integrar tokens e componentes de `DMO_DESIGN_SYSTEM.md`/`dmo-design-system.css`.
   Antes de criar um módulo, preencher `MODULE_UI_HANDOFF_TEMPLATE.md` com os fluxos e fontes de dados confirmados.
   Consultar `DESIGN_INPUT_EXTRACTION.md` para padrões derivados das discussões funcionais e respetivo estado de confirmação.
   Para Job On, usar `JOB_ON_DESIGN_BRIEF.md` como base de evidência antes do mockup e `JOB_ON_DATA_MODEL.md` como contrato de persistência do snapshot editável.
   Para observações e verificações do Job On, aplicar também `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`.
   Para Armazém, usar `ARMAZEM_DESIGN_BRIEF.md` antes de desenhar ou implementar movimentos/localizações.
   Para reparadores de turno, usar `REPARACAO_INTERNA_DESIGN_BRIEF.md`; este fluxo é separado da Reparação externa programada.
   Para Reparação externa, usar `REPARACAO_EXTERNA_DESIGN_BRIEF.md`; BQ, CM e MF partilham o ciclo externo mas permanecem domínios separados.
   Para Tampões, usar `TAMPOES_DESIGN_BRIEF.md`; a V1 controla quantidades agregadas, campos comparáveis e transformações atómicas entre configurações.
   Para criação de ferramentas e lotes, usar `FERRAMENTAS_REGISTO_DESIGN_BRIEF.md` antes dos handoffs específicos de CM, MF ou BQ.
   Para o registo transversal de ações, usar `AUDITORIA_GLOBAL_HANDOFF.md`; aplica-se a todos os utilizadores e módulos e é consultado no Admin por ano.
4. Aplicar a shell, Login e perfil do utilizador.
5. Integrar Administração e templates de acesso.
6. Ligar Peso Operador ao motor de cálculo e persistência existentes.
7. Ligar Peso Responsável ao fluxo de aprovação e decisões por CM.
8. Aplicar os componentes comuns a Boquilhas sem alterar as regras de domínio documentadas.
9. Integrar os restantes módulos apenas depois de confirmar os respetivos contratos de dados.

## Cobertura entregue

| Área | Design | Comportamento | Documento principal |
|---|---|---|---|
| Login | `login.html` | autenticação e encaminhamento | `PORTAL_LOGIN_ADMIN_HANDOFF.md` |
| Administração | `admin.html` | utilizadores, título/função, reset, aplicações e auditoria anual de ações | `PORTAL_LOGIN_ADMIN_HANDOFF.md` + `AUDITORIA_GLOBAL_HANDOFF.md` |
| Boquilhas | `boquilhas.html` | lotes, movimentos, histórico e painel lateral | `BOQUILHAS_INTERFACE_BEHAVIOR.md` |
| Peso Operador | `peso-operador.html` | controlo, referências, comparação e histórico | `PESO_INTERFACE_HANDOFF.md` |
| Peso Responsável | `peso-responsavel.html` | aprovação normal e decisão por CM | `PESO_INTERFACE_HANDOFF.md` |
| Pegamentos | `pegamentos.html` | contexto obrigatório do Job On, medições, histórico estruturado e PDF local | `PEGAMENTOS_INTERFACE_HANDOFF.md` |
| Tampões | `tampoes.html` | consulta, quantidades, transformação técnica, planeamento, opções e histórico | `TAMPOES_DESIGN_BRIEF.md` |
| Armazém | `armazem.html` | registo, consulta, saídas programadas e histórico | `ARMAZEM_DESIGN_BRIEF.md` |
| Job On | `job-on.html` → `job-on-v48-folha-producao.html` | folha necessária para produzir; consulta não editável; edição integral do snapshot; seleção live de ferramentas/localização; Data/Máquina e CM/MP/MF/BQ prioritários | `JOB_ON_DESIGN_BRIEF.md` + `JOB_ON_DATA_MODEL.md` + `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md` |
| Reparação interna | `reparacao-interna.html` | Linha, contexto de produção, CM/MF, Histórico e Correção | `REPARACAO_INTERNA_DESIGN_BRIEF.md` |
| Moldes | `moldes.html` | módulo principal semelhante a Boquilhas; CM e MF permanecem separados | `REPARACAO_EXTERNA_DESIGN_BRIEF.md` |
| Componentes | CSS + JavaScript partilhados | botões, listas, calendário e estados | `DMO_DESIGN_SYSTEM.md` |

## Regras transversais verificadas

- Botões: preenchidos em repouso; brancos com contorno e texto da cor no hover/foco.
- Listas: um clique seleciona; duplo clique abre.
- Ações ficam fora das listas.
- Tabelas usam densidade compacta e scroll apenas quando inevitável em ecrãs pequenos.
- Registo de peso e Comparação usam estados e filtros consistentes.
- Valores decimais apresentam no máximo duas casas.
- Título/função do cabeçalho vem do perfil gerido pelo Administrador e não concede permissões.
- Cada ação de negócio relevante gera um evento factual append-only associado ao utilizador e ao módulo; não existe pontuação automática.
- Job On é a landing page de todos os utilizadores autenticados; apenas o papel/template técnico Responsável possui edição e configuração do Job On.
- Processo NNPB/PS é definido ao criar o lote no módulo Peso e é herdado pelo Job On, Novo controlo e Comparação.
- Máquinas permitidas associam funcionalmente o lote às máquinas/linhas já suportadas pelo programa.
- O Job On guarda CM/MP, MF e BQ concretos com os respetivos lotes; Peso e Pegamentos consomem essas escolhas sem nova seleção.
- No Job On, localização e disponibilidade live aparecem apenas durante a edição/substituição; o modo consulta permanece uma folha não editável.
- Cada revisão do Job On guarda contexto, componentes, campos tipados e linhas CAL num snapshot completo; duplicar copia tudo e a nova folha pode ser alterada sem modificar a origem nem as bases mestre.
- Dados numéricos/históricos de Peso e Pegamentos ficam no servidor; PDFs de Produção ficam no diretório local configurado e na subpasta definida ao criar o lote.
- Comparações são registos adicionais; não alteram o controlo aprovado usado como base.
- Decisão de Comparação é individual por CM e inclui peso/capacidade atuais e aprovados.

## Integrações que o mockup não substitui

- Autenticação e reset real de palavra-passe.
- Autorização de comandos e templates de acesso.
- Persistência e sincronização com o servidor.
- Motor de cálculo de peso, capacidade e diferenças.
- Geração de documentos e envio de email.
- Auditoria de ações e snapshots históricos.

## Ponto visual adiado

O calendário do Peso está funcionalmente normalizado, mas a passagem visual final deve reutilizar exatamente o calendário de Boquilhas para eliminar a diferença ainda visível entre módulos.


## END FILE CONTENT

---

# FILE 010

## Source Path
`docs\JOB_ON_DATA_MODEL.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Job On — modelo de dados do snapshot editável

Este documento é o contrato técnico de persistência da folha Job On. A prioridade não é reproduzir fotograficamente uma folha antiga: é garantir que **todos os dados necessários à produção ficam acessíveis, editáveis, versionados e duplicáveis**.

## 1. Princípio

- A base das ferramentas descreve a ferramenta real e o seu estado atual.
- O Armazém descreve a localização e os movimentos atuais.
- O Job On descreve **o que foi decidido para uma produção concreta**.
- Cada gravação do Job On cria uma revisão com um snapshot completo.
- A folha em `Modo consulta` lê esse snapshot.
- Um utilizador autorizado entra em `Modo edição` e pode alterar qualquer valor do snapshot, incluindo CAL, PI, pinças, calibres, quantidades, notas, contexto e associações de ferramentas.
- Alterar o snapshot nunca altera automaticamente a ficha mestre, o estado técnico, a percentagem de uso ou a posição de uma ferramenta.
- Estado, localização, compatibilidade e percentagem de uso podem gerar avisos, mas não bloqueiam a gravação de uma revisão do Job On.
- Uma revisão guardada é imutável. `Guardar alterações` insere uma nova revisão e novos registos filhos; nunca executa um `UPDATE` destrutivo sobre os valores históricos da revisão anterior.

## 2. Tabelas recomendadas

Os nomes são orientadores e podem ser adaptados às convenções do backend. A separação e as responsabilidades são obrigatórias.

### `job_on`

Identidade estável do fabrico planeado.

| Campo | Tipo indicativo | Regra |
|---|---|---|
| `id` | UUID/ID | chave estável do Job On |
| `production_code` | texto | exemplo `202601`; indexado |
| `article_reference_id` | FK anulável | ligação à Referência mestre, quando existir |
| `article_reference_snapshot` | texto | referência legível usada na produção, exemplo `5447T173` |
| `machine_code` | texto/FK | B1, B2, B3, C1, C2 ou C3 |
| `planned_start_at` | data/hora | data planeada, editável |
| `planned_end_at` | data/hora anulável | data final planeada, editável |
| `status` | enum/texto | rascunho, planeado, em fabrico, fechado, cancelado |
| `current_revision_id` | FK | revisão atualmente apresentada |
| `copied_from_job_on_id` | FK anulável | origem da duplicação |
| `created_by`, `created_at` | ator/data | auditoria de criação |

Recomendação de unicidade: usar a identidade real do negócio. Se o programa permitir mais de um Job On para a mesma Produção/Referência/Máquina, não impor uma unicidade que elimine esse caso.

O calendário consulta diretamente `planned_start_at` e `planned_end_at`; não existe uma segunda cópia de datas exclusiva do calendário. Ao guardar uma revisão com novas datas, atualizar estes campos do Job On e a projeção no calendário na mesma transação/evento de domínio. As datas antigas continuam preservadas na revisão anterior e no evento de auditoria.

### `job_on_revision`

Cabeçalho imutável de cada gravação.

| Campo | Tipo indicativo | Regra |
|---|---|---|
| `id` | UUID/ID | chave da revisão |
| `job_on_id` | FK | Job On lógico |
| `revision_number` | inteiro | crescente por Job On |
| `production_code_snapshot` | texto | valor efetivamente guardado nesta revisão |
| `article_reference_snapshot` | texto | valor efetivamente guardado nesta revisão |
| `machine_code_snapshot` | texto | máquina efetivamente guardada |
| `start_at_snapshot`, `end_at_snapshot` | data/hora | datas desta revisão |
| `sections` | inteiro anulável | secções |
| `drop_count` | inteiro anulável | gota |
| `type_snapshot` | texto anulável | Tipo usado nesta produção |
| `stop_snapshot` | texto anulável | Paragem; preserva texto/código inserido pelo utilizador |
| `weight_snapshot` | decimal anulável | Peso decidido/apresentado no Job On |
| `process_snapshot` | texto anulável | NNPB/PS recebido do lote do Peso; pode ser corrigido apenas nesta revisão |
| `general_notes` | texto anulável | notas gerais |
| `image_asset_id` | FK/URI anulável | imagem de apoio, sem obrigar fotografia |
| `change_reason` | texto | obrigatório quando se altera uma revisão já fechada/aprovada, conforme permissões |
| `saved_by`, `saved_at` | ator/data | auditoria |

`job_on.current_revision_id` aponta para a revisão mais recente, mas históricos, aprovações, PDF de Peso, Pegamentos e outros consumidores devem guardar o `job_on_revision_id` exato que utilizaram. Atualizar o Job On não reescreve um PDF nem o contexto de um registo histórico já emitido.

### `job_on_component`

Um registo por cartão/grupo da folha em cada revisão: MP/CM, MF, BQ, PU, CAL, AN, ARR, PI, CS, TP, FO ou uma futura família.

| Campo | Tipo indicativo | Regra |
|---|---|---|
| `id` | UUID/ID | chave do componente no snapshot |
| `job_on_revision_id` | FK | revisão proprietária |
| `family_code` | texto | MP, MF, BQ, PU, CAL, AN, ARR, PI, CS, TP, FO… |
| `source_tool_id` | FK anulável | ferramenta mestre selecionada; não é o dono dos valores do snapshot |
| `source_lot_id` | FK anulável | lote mestre selecionado |
| `reference_snapshot` | texto anulável | referência apresentada nessa produção |
| `lot_snapshot` | texto anulável | lote apresentado nessa produção |
| `technical_name_snapshot` | texto anulável | nome legível no momento da associação |
| `planned_quantity` | decimal/inteiro anulável | quantidade decidida para a produção |
| `stock_snapshot` | decimal/inteiro anulável | valor informativo copiado quando necessário; não substitui stock live |
| `usage_snapshot` | decimal anulável | apenas se a equipa decidir preservar o valor visto; o valor atual continua no domínio da ferramenta |
| `notes` | texto anulável | notas deste componente |
| `display_order` | inteiro | ordem na folha |

Uma associação pode manter `source_tool_id` e, simultaneamente, valores de snapshot diferentes. Isto é intencional: o ID explica a origem; o snapshot explica o que foi usado/decidido nessa produção.

### `job_on_component_field`

Campos editáveis de cada componente. Evita criar uma coluna nova na tabela principal por cada futuro detalhe, mas mantém os valores pesquisáveis e tipados.

| Campo | Tipo indicativo | Regra |
|---|---|---|
| `id` | UUID/ID | chave |
| `job_on_component_id` | FK | componente proprietário |
| `field_code` | texto | código estável, por exemplo `pi_clamp_material`, `pi_diameter`, `cs_holes` |
| `label_snapshot` | texto | rótulo apresentado naquela revisão |
| `value_type` | enum | text, integer, decimal, boolean, date, select |
| `value_text` | texto anulável | textos, referências e opções |
| `value_number` | decimal anulável | números pesquisáveis/calculáveis |
| `value_boolean` | boolean anulável | checks |
| `value_date` | data/hora anulável | datas |
| `unit` | texto anulável | mm, %, unidades… |
| `display_order` | inteiro | ordem no cartão |

Regra: apenas a coluna compatível com `value_type` deve conter valor. Não guardar números exclusivamente dentro de JSON ou texto quando forem usados em cálculo/filtro.

Exemplo PI da Produção `202601`:

| `field_code` | `value_type` | valor | unidade |
|---|---|---|---|
| `pi_clamp_material` | text/select | `Latão` | — |
| `pi_diameter` | decimal | `44.00` | `mm` |
| `pi_notes` | text | `Boleadas.` | — |

### `job_on_component_row`

Linhas repetíveis, nomeadamente a tabela CAL. É preferível a oito colunas rígidas no cabeçalho do Job On.

| Campo | Tipo indicativo | Regra |
|---|---|---|
| `id` | UUID/ID | chave da linha |
| `job_on_component_id` | FK | componente CAL ou equivalente |
| `row_code` | texto anulável | código estável quando existe |
| `element_label` | texto | exemplo `Bucha marcada`, `Pinças`, `Nível` |
| `value_text` | texto anulável | valor livre quando não é um único número |
| `value_number` | decimal anulável | valor numérico quando aplicável |
| `unit` | texto anulável | mm, unidades… |
| `machine_quantity` | decimal/inteiro anulável | quantidade em máquina |
| `display_order` | inteiro | ordem das linhas |

As linhas podem ser adicionadas, removidas e reordenadas em edição. Nenhuma lista fixa de CAL impede guardar um elemento novo.

Todos os valores da tabela CAL são editáveis na nova revisão, incluindo `element_label`, valor, unidade e quantidade em máquina. A linha equivalente da revisão anterior permanece intacta.

### `job_on_verification_occurrence`

Materializa as verificações desta produção.

| Campo | Regra |
|---|---|
| `job_on_revision_id` / `job_on_id` | contexto do Job On |
| `source_rule_id` | regra de origem, quando existir |
| `component_id` | cartão/ferramenta associado |
| `description_snapshot` | instrução apresentada |
| `frequency_snapshot` | uma vez no lote / por fabrico |
| `status` | pendente, concluída, reposta, desativada |
| `completed_by`, `completed_at` | quem confirmou e quando |
| `completion_source` | `manual_job_on`; não inferir confirmação a partir de Armazém, Reparação, estado da ferramenta ou simples leitura |

A regra nasce na ficha da ferramenta/lote e é materializada como ocorrência no Job On. O estado só passa a confirmado quando um utilizador autorizado executa o check e a operação é persistida com sucesso. Marcar visualmente a checkbox antes da resposta do servidor não é confirmação. Em caso de erro, a ocorrência permanece `pendente`.

### `job_on_audit_event`

Regista criação, duplicação, abertura de edição, gravação, alteração de ferramenta, alteração de datas e checks. Guardar `before`/`after` apenas para auditoria; esses blocos não substituem as tabelas de snapshot.

### `job_on_field_option`

Catálogo configurável para dropdowns de negócio que podem crescer. Não deixar materiais, tipos, versões ou opções equivalentes presos ao HTML/código.

| Campo | Tipo indicativo | Regra |
|---|---|---|
| `id` | UUID/ID | chave estável da opção |
| `family_code` | texto | PI, MP, MF, BQ, PU, CS, TP, FO… |
| `field_code` | texto | exemplo `clamp_material` |
| `value_code` | texto | código estável, independente do rótulo |
| `display_label` | texto | exemplo `Latão`, `Grafite` |
| `sort_order` | inteiro | ordem no dropdown |
| `is_active` | boolean | ativa para novas escolhas |
| `created_by`, `created_at` | ator/data | auditoria |
| `updated_by`, `updated_at` | ator/data | última alteração |

Regra global: dropdowns de **dados de negócio evolutivos** usam o catálogo do módulo proprietário e são geridos em Definições. Máquinas, paginação e controlos puramente técnicos usam os respetivos catálogos/regras canónicas e não são misturados nesta tabela.

Desativar uma opção remove-a de novas escolhas, mas não elimina nem altera o `value_text`/rótulo guardado nas revisões antigas. Se uma revisão histórica for aberta, continua a mostrar exatamente a opção usada na altura.

## 3. Limite entre bases de dados

| Informação | Proprietário autoritativo | O que o Job On guarda |
|---|---|---|
| ID, nome técnico, desenho e lotes da ferramenta | BD CM/MF/BQ/… | FK de origem + texto legível do snapshot |
| Máquinas permitidas da ferramenta | BD da ferramenta | associação escolhida; pode mostrar aviso se divergir |
| Estado técnico, reparado/por reparar, % de uso | BD da ferramenta | consulta live na seleção; não é sobrescrito pelo Job On |
| Posição e movimentos | BD Armazém | consulta live; não cria reserva ou saída |
| Produção, máquina e datas decididas | BD Job On | valor integral por revisão |
| PI, pinças, CAL/calibres e restantes detalhes da folha | BD Job On | linhas/campos integrais e editáveis por revisão |
| Quantidades e notas decididas para este fabrico | BD Job On | snapshot integral |
| Controlo de Peso e Pegamentos | bases desses módulos | ligação pelo `job_on_id` e revisão usada |

## 4. Duplicação

Duplicar `202601 · 5447T173` para `202602` executa uma cópia transacional:

1. criar novo `job_on` com `copied_from_job_on_id`;
2. copiar a revisão atual, componentes, campos, linhas CAL e regras/ocorrências que devam nascer no novo fabrico;
3. atribuir nova Produção, datas e estado `rascunho`;
4. abrir em edição;
5. permitir mudar livremente PI, pinças, CAL, calibres, quantidades, notas e qualquer associação de ferramenta;
6. guardar como snapshot independente, sem alterar `202601` e sem escrever na BD mestre das ferramentas.

Não atualizar automaticamente os valores copiados a partir do estado atual das ferramentas. A interface pode propor ou avisar; o utilizador decide.

## 5. API/UI mínima

- `GET JobOn/{id}` devolve a revisão corrente completa com componentes, campos e linhas.
- `GET JobOn/{id}/revisions` devolve o histórico legível.
- `POST JobOn/{id}/duplicate` cria rascunho completo e devolve o novo ID.
- `PUT JobOn/{id}/draft` grava todos os grupos do rascunho numa operação transacional.
- `POST JobOn/{id}/revisions` fecha uma revisão e atualiza `current_revision_id`.
- A resposta de leitura pode agregar estado/localização live, claramente marcado como `liveContext`; esses dados não devem ser confundidos com o snapshot.

## 6. Critérios de aceitação

- Abrir `202601` sem consultar ferramentas externas continua a mostrar a configuração PI/CAL guardada.
- Duplicar para `202602` copia todos os campos e linhas.
- Alterar uma pinça ou calibre em `202602` não altera `202601` nem a ferramenta mestre.
- Corrigir CAL/PI no próprio Job On `202601` cria a revisão seguinte; consultar a revisão anterior continua a apresentar exatamente os valores antigos.
- É possível adicionar uma linha CAL não existente no modelo anterior.
- Uma ferramenta por reparar ou fora do Armazém gera aviso, mas não impede guardar o Job On por um utilizador autorizado.
- Peso e Pegamentos conseguem identificar o `job_on_id` e a revisão usada.
- O histórico identifica autor, data, motivo e valores alterados.
- Alterar Data início/fim atualiza o intervalo apresentado no calendário depois de guardar, sem apagar as datas da revisão anterior.

## 7. Histórico por Referência e Produção

A navegação histórica tem dois níveis diferentes:

1. **Produções da Referência**: ao selecionar uma Referência, listar todos os seus Job Ons por Produção, por exemplo `202601`, `202602`, com datas, máquina e estado. Um clique seleciona; duplo clique abre a produção.
2. **Revisões da Produção**: dentro de `202601`, listar as revisões imutáveis dessa produção. Abrir uma revisão antiga mostra exatamente o snapshot então guardado.

O filtro principal usa `article_reference_id` quando existir e mantém `article_reference_snapshot` para legibilidade histórica. Nunca agrupar apenas pelo texto se houver um ID mestre estável. A lista de Produções não substitui o histórico de revisões.


## END FILE CONTENT

---

# FILE 011

## Source Path
`docs\JOB_ON_DESIGN_BRIEF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Job On — brief de design extraído dos exemplos

Estado: base para discussão e mockup  
Fonte: cinco exemplos visuais de `JOB-ON MOLDES - MG`, procedimento de numeração de desenhos e discussões funcionais existentes.

## 0. Definição funcional

O **Job On é a folha onde a equipa consulta toda a informação necessária para executar uma produção**. Não é uma página de gestão de ferramentas nem um formulário que deva parecer permanentemente editável.

- agrega o contexto da produção e as ferramentas escolhidas nos respetivos módulos de domínio;
- não cria nem altera os registos mestres de CM/MP, MF, BQ ou restantes ferramentas;
- abre por defeito em **Modo consulta**, com leitura rápida e hierarquia semelhante a uma folha técnica;
- `Editar folha` ativa campos editáveis apenas para utilizadores autorizados;
- `Guardar alterações` fecha a edição e devolve a folha ao modo de consulta;
- criar ou duplicar prepara um novo rascunho de Job On; não duplica nem cria ferramentas;
- o calendário serve para localizar, planear e abrir produções, não substitui a folha.
- depois de guardado, o Job On é a fonte operacional da produção concreta para Peso e Pegamentos: identifica a Referência, Produção, Máquina e os CM/MP, MF e BQ exatos, incluindo os respetivos lotes.

### Informação que tem de ser interpretada imediatamente

1. Data de início e data de fim;
2. máquina/linha onde a produção trabalha;
3. referência e número de produção;
4. ferramentas principais `MP/CM`, `MF` e `BQ`, com referência e lote;
5. imagem do artigo, quando existir.

Os restantes parâmetros técnicos continuam disponíveis na mesma folha, com contraste secundário, para não competir com os dados operacionais críticos.

## 1. O que os exemplos confirmam visualmente

### Contexto principal da produção

Os cinco exemplos repetem no topo:

- Referência;
- Produção;
- Linha/Máquina;
- Secções;
- Gota;
- Tipo;
- Data de início;
- Data de fim;
- Processo (`NNPB`/`PS` nos exemplos);
- Peso;
- campo curto de paragem.

Referência, Produção e Linha formam o contexto mínimo que deve permanecer visível durante toda a edição.

Quando o Job On apresenta `Processo` (`NNPB`/`PS`), o valor vem do lote criado no módulo Peso. O Job On apenas o mostra no contexto da produção; o operador não redefine o processo nesta folha.

### Ações existentes na folha antiga

- Novo;
- Gravar;
- Replicar;
- Eliminar;
- Exportar;
- Comparar;
- pesquisar Referência;
- percorrer Produções anteriores da Referência.

Estas ações são evidência funcional, não uma aprovação da posição, ícones ou implementação antiga.

### Grupos de informação apresentados

| Código visual | Informação observada nos exemplos |
|---|---|
| MP | referência, lote, tipo, diâmetros, tolerâncias/folgas, stock/máquina, adaptador, inversão, parafuso, 3.ª almofada, reparador, utilização e notas |
| MF | referência, lote, tipo, diâmetros, tolerâncias/folgas, stock/máquina, fundo final, adaptador, inversão, parafuso, reparador, utilização e notas |
| BQ | referência, lote, stock/máquina, utilização e notas |
| PU | referência, versão, stock/máquina e notas |
| CAL | conjunto de medidas/valores operacionais, pinças e quantidade em máquina |
| AN | referência e notas |
| ARR | referência e notas |
| PI | pinças/material, diâmetro e notas |
| CS | referência, furos, tubo, stock/máquina e notas |
| TP | diâmetro PS, referência, bacia PS, stock/máquina e notas |
| FO | tipo, stock/máquina e notas |

Os códigos são mantidos como terminologia visível da fonte. A expansão oficial de cada sigla e a lista final de campos devem ser confirmadas antes do freeze.

### Informação complementar

- notas gerais extensas;
- imagem/desenho do artigo;
- notas específicas por grupo;
- valores de quantidade em stock e quantidade necessária/em máquina;
- referência a reparador e utilização em MP/MF/BQ;
- produções anteriores da mesma referência para consulta/comparação.

## 2. Problemas de UI observados

- todos os campos competem simultaneamente pela atenção;
- hierarquia depende sobretudo do tamanho enorme das siglas;
- labels e inputs são demasiado pequenos;
- elevada densidade dificulta leitura e utilização por toque;
- ações dependem de ícones pouco explícitos;
- pesquisa e histórico estão misturados no topo da edição;
- notas gerais ficam afastadas do contexto que as originou;
- estado atual e informação copiada de uma produção anterior não estão visualmente separados;
- não é claro quais campos são editáveis, calculados ou provenientes de outras áreas;
- scroll horizontal faz parte da página em vez de ficar contido.

## 3. Estrutura proposta

### Tab Planeamento

Landing operacional com o calendário canónico e uma lista associada ao dia selecionado.

O calendário é deliberadamente compacto, com largura aproximada de `300px` em desktop. Não deve dominar a página nem usar a altura disponível como área vazia. O cartão da lista ocupa o restante espaço.

`Criar Job On` pertence ao cabeçalho do cartão da lista/dia selecionado. Não fica isolado no cabeçalho geral da página. Ao clicar, expande o formulário de criação dentro desse mesmo cartão.

#### Comportamento do calendário

- um clique num dia passado mostra as Referências com movimentos/registos de entrada ou saída nesse dia;
- um clique num dia presente mostra os registos desse dia;
- um clique num dia futuro mostra a lista vazia quando ainda não existe planeamento e disponibiliza `Criar Job On para este dia`;
- mudar de mês não escolhe automaticamente um dia;
- o dia selecionado permanece visível no cabeçalho da lista e no formulário de criação;
- dias com atividade usam o indicador discreto do calendário canónico;
- o calendário consulta factos registados; não deduz entradas ou saídas a partir da ausência de um Job On.
- depois de um Job On ser criado e persistido, aparece automaticamente no calendário;
- o calendário referencia o mesmo ID estável do Job On; uma alteração de data atualiza o evento existente e não cria uma cópia.

Cada linha/cartão mostra:

- data;
- Referência;
- Produção;
- Máquina;
- resumo de atenção/preparação quando existir um facto registado.

Interação canónica:

- um clique seleciona;
- duplo clique abre o Job On;
- filtros por período, Referência e Máquina;
- nenhuma seleção automática quando existirem resultados ambíguos.

Ao criar a partir de um dia futuro, a data do novo Job On recebe o dia selecionado. O utilizador pode depois escolher entre:

- `Duplicar anterior` da Referência;
- procurar e duplicar um Job On histórico específico;
- `Novo em branco` quando não existir histórico ou quando o Manager pretender começar sem base.

### Tab Job On

Ordem da página:

1. Contexto fixo da produção.
2. Toolbar de ações.
3. Avisos e tarefas de preparação.
4. Grelha de famílias de ferramentas.
5. Notas gerais.
6. Desenho/visualização técnica.
7. Histórico e comparação.

A folha completa deve estar efetivamente presente no mockup e na implementação. Cartões-resumo não substituem a folha operacional. Para o exemplo atual, a folha apresenta as famílias confirmadas nos ficheiros de origem: `MP`, `MF`, `BQ`, `PU`, `CAL`, `AN`, `ARR`, `PI`, `CS`, `TP` e `FO`, incluindo os respetivos campos, quantidades e notas. Famílias futuras só são adicionadas com evidência do domínio.

No Planeamento, um clique seleciona a linha e um duplo clique abre a vista separada `Folha Job On`. A folha nunca aparece por baixo do calendário ou da lista. Um novo rascunho também abre imediatamente essa vista, preenchida a partir da origem escolhida ou vazia quando é usado `Novo em branco`.

### Imagem do artigo

A Folha Job On disponibiliza um bloco compacto `Imagem do artigo`, próximo do contexto prioritário. O utilizador pode carregar uma imagem e recebe pré-visualização imediata. Quando existir persistência, a interface deve permitir substituir ou remover a imagem com confirmação e auditoria adequada.

A propriedade definitiva da imagem ainda deve ser confirmada pelo domínio: imagem comum da Referência ou snapshot específico do Job On. A implementação não deve copiar nem substituir imagens entre produções sem uma regra explícita.

### Hierarquia visual prioritária

O operador deve interpretar a folha em poucos segundos. A ordem visual confirmada é:

1. Data de início e Data de fim.
2. Máquina/Linha onde o Job On trabalha.
3. CM, MF e BQ.
4. Referência, Lote, Quantidade e alertas dentro de CM/MF/BQ.
5. Restantes ferramentas e medidas técnicas.

MP, MF e BQ ficam destacados, respeitando a nomenclatura da folha de origem. `PU`, `CAL`, `AN`, `ARR`, `PI`, `CS`, `TP` e `FO` permanecem sempre visíveis, apenas com contraste secundário. Referência geral, Produção, Secções, Gota e Processo continuam presentes, mas não competem visualmente com Data, Máquina, MP, MF e BQ. A entrada direta do mockup abre a tab `Job On`; Planeamento e calendário ficam numa tab separada.

### Tab Histórico

- pesquisa por Referência, Produção e Máquina;
- intervalo de datas;
- lista canónica;
- duplo clique abre o Job On histórico;
- filtros mantêm resultados explícitos, sem inferir equivalência entre máquinas.

### Definições

Fica alinhada à direita e contém apenas configuração real autorizada, não ações operacionais.

## 4. Contexto fixo da produção

Cartão compacto no topo:

| Grupo largo | Campos compactos |
|---|---|
| Referência | Produção, Máquina, Secções, Gota, Processo, Peso |
| Datas | Início, Fim, Paragem quando aplicável |

Regras:

- Referência é larga;
- Produção, Máquina, Secções e Gota são compactas;
- datas ficam no final da linha;
- o contexto permanece visível ao percorrer os grupos;
- depois de iniciar/finalizar, campos protegidos usam modo de correção auditável em vez de edição silenciosa.

### Data de fim e atualização do calendário

Enquanto o Job On estiver em fabrico, o Manager autorizado pode usar `Alterar data de fim`.

Fluxo:

1. abre um cartão inline compacto;
2. apresenta a data de fim atual;
3. o Manager escolhe a nova data;
4. guarda a alteração;
5. apenas após persistência, o Job On e o calendário são atualizados;
6. a interface mantém o utilizador no mesmo contexto.

Exemplo confirmado: o fabrico estava previsto terminar no dia 6 e passa para o dia 8. O mesmo Job On prolonga/atualiza a sua presença no calendário até ao dia 8.

Auditoria mínima:

- data de fim anterior;
- nova data de fim;
- Manager que alterou;
- data/hora da alteração.

Regras:

- alterar a data de fim não muda a data inicial;
- não cria um novo Job On;
- não reescreve alterações anteriores;
- uma falha mantém a data anterior no Job On e no calendário;
- quando o fabrico terminar, o último valor guardado em `Data de fim` fica como data final do registo;
- depois de fechado, a Data de fim deixa de usar esta edição operacional; qualquer correção posterior segue o fluxo auditável aplicável.

## 5. Cartões das famílias de ferramentas

Cada família usa o mesmo componente visual, mas recebe campos e ações próprias.

### Estado fechado

Mostra apenas:

- código e nome confirmado da família;
- referência;
- lote quando aplicável;
- quantidade em stock / necessária;
- localização ou disponibilidade live quando proveniente da fonte autoritativa;
- utilização quando existir como indicador registado;
- estado curto: completo, atenção ou informação em falta.

### Estado expandido

Ao clicar no cartão ou em `Editar detalhes`:

- expande inline;
- mostra os campos específicos da família;
- apenas um cartão principal fica expandido de cada vez;
- primeiro campo editável recebe foco;
- Guardar valida e persiste antes de fechar;
- Cancelar não altera o estado guardado;
- apenas informação **live** de disponibilidade/localização é read-only e identifica a origem. Depois de um valor ser guardado como parte do snapshot do Job On, o seu campo de produção é editável em `Modo edição` sem alterar a fonte mestre.

Não criar um formulário genérico que obrigue BQ, MP, MF, PU ou acessórios a ter os mesmos campos.

## 6. Toolbar de ações

| Ação | Comportamento proposto |
|---|---|
| Novo Job On | expande cartão de criação do contexto; não cria até guardar |
| Guardar | ação primária; feedback apenas depois de persistir |
| Duplicar anterior | para a Referência do novo fabrico, procura o Job On imediatamente anterior e abre um novo rascunho com essa base |
| Duplicar histórico selecionado | copia o Job On escolhido na pesquisa/histórico da Referência |
| Novo em branco | cria um template vazio para o Manager decidir e preencher as ferramentas quando a Referência nunca teve Job On |
| Comparar | expande seletor de produção anterior, priorizando mesma Referência + mesma Máquina |
| Exportar | disponível após existir registo guardado |
| Eliminar/Arquivar | ação destrutiva autorizada, confirmada e auditável |

Evitar uma fila de ícones sem texto. Usar botões compactos com labels claros.

### Regra funcional de `Duplicar anterior`

Exemplo confirmado: ao preparar para amanhã um Job On da Referência `5774T173`, `Duplicar anterior` usa o Job On anterior dessa mesma Referência.

Regras de interface:

1. O utilizador inicia o novo Job On e identifica a Referência/Produção.
2. `Duplicar anterior` pesquisa o Job On imediatamente anterior da mesma Referência.
3. A aplicação mostra a origem da cópia: Referência, Produção, Máquina e data do Job On anterior.
4. O utilizador confirma e é criado apenas um rascunho; o registo histórico original nunca é alterado.
5. Todos os valores copiados da folha ficam editáveis no novo rascunho para o utilizador com permissão de editar Job On; a origem do valor não bloqueia a edição do snapshot.
6. A cópia só fica guardada depois da ação explícita `Guardar`.

Se não existir Job On anterior, `Duplicar anterior` fica indisponível e a interface explica `Não existe um Job On anterior para esta referência`. A ação `Novo em branco` permanece disponível. Não escolher outra Referência nem inventar uma base alternativa.

### Regra funcional de `Novo em branco`

Usar quando a Referência nunca trabalhou ou não possui Job On anterior.

1. O utilizador informa o contexto confirmado da nova produção.
2. `Novo em branco` cria um rascunho com a estrutura das famílias aplicáveis, mas sem Referências ou lotes de ferramentas escolhidos.
3. O Manager decide o que associar em cada família.
4. A aplicação não copia silenciosamente dados de outra Referência, Máquina ou produção semelhante.
5. O rascunho só se torna registo persistido após `Guardar` com as validações do domínio.

Os campos obrigatórios e as famílias que devem aparecer neste template continuam dependentes das regras confirmadas do programa.

O critério técnico exato de “anterior” deve usar a ordenação canónica do domínio (data/produção e desempate estável), a confirmar com o programador e a fonte de dados.

### Regra funcional de `Duplicar histórico selecionado`

Este fluxo permite usar um Job On mais antigo, mesmo quando não é o imediatamente anterior.

1. O utilizador pesquisa pela Referência.
2. A aplicação apresenta os Job On históricos dessa Referência numa lista canónica, com data, produção e máquina suficientes para distinguir os registos.
3. Um clique seleciona o Job On histórico.
4. Duplo clique abre o Job On histórico para consulta, sem o alterar.
5. O botão externo `Duplicar selecionado` cria um novo rascunho a partir da seleção.

Na duplicação:

- a data do Job On de origem é o único valor que não é reutilizado;
- quando o fluxo começou num dia futuro do calendário, a nova data recebe esse dia;
- sem dia previamente selecionado, a nova data fica por preencher;
- todo o restante conteúdo vem preenchido exatamente a partir do Job On escolhido;
- o registo de origem permanece imutável;
- o novo rascunho identifica qual Job On lhe deu origem;
- nenhuma ferramenta, lote, nota ou valor copiado é atualizado silenciosamente com dados live.

Depois de criar o rascunho, a interface pode apresentar separadamente o estado atual das ferramentas para o Manager decidir o que manter ou substituir. Essa consulta live não reescreve os valores copiados.

## 7. Seleção de lotes em CM, MF e BQ

Ao editar uma família CM, MF ou BQ, a Referência da ferramenta permanece visível e o campo `Lote` funciona como seletor contextual.

### Pesquisa da Referência da ferramenta

O campo `Referência da ferramenta` aceita pesquisa direta no registo autoritativo da respetiva família. Exemplo: ao escrever `6185`, a interface consulta as ferramentas CM, MF ou BQ já registadas, conforme o cartão em edição.

- resultados aparecem progressivamente e mostram contexto suficiente para distinguir registos;
- resultados apresentam o Nome técnico junto da Referência;
- escolher uma Referência não escolhe automaticamente o lote;
- depois da escolha, o campo `Lote` carrega os lotes compatíveis com a Referência e a Máquina/Linha do Job On;
- sem correspondência, mostrar `Referência de ferramenta não encontrada`;
- uma pesquisa sem resultado não cria um novo registo;
- a criação da ferramenta continua a pertencer ao respetivo módulo autoritativo;
- a aplicação deve distinguir `não existe no registo` de `existe, mas não possui lote compatível com esta máquina`, quando os dados permitirem essa distinção.

### Origem das opções

As opções do lote são obtidas dos registos já existentes da respetiva ferramenta e filtradas por:

- tipo de ferramenta: CM, MF ou BQ;
- Referência associada à família no Job On;
- Máquina/Linha do novo fabrico.

CM, MF e BQ mantêm fontes, identidade e histórico próprios. O Job On apenas consulta e associa o lote escolhido; não cria nem altera lotes desses módulos.

### Contrato com Peso e Pegamentos

- A escolha acontece nesta folha, durante a preparação/edição autorizada do Job On.
- Cada ferramenta guardada usa um ID estável e um snapshot legível de Referência e Lote.
- Exemplo: o Job On da Produção `202601` pode indicar `CM 5447 · Lote 4`; o Novo controlo de Peso dessa produção usa exatamente esse CM/lote.
- Pegamentos usa exatamente os CM/MP, MF e BQ guardados no mesmo Job On.
- Peso e Pegamentos mostram estas ferramentas como contexto herdado, sem oferecer uma segunda seleção.
- Se uma ferramenta obrigatória estiver em falta, eliminada ou incompatível, os módulos consumidores bloqueiam e disponibilizam `Corrigir ferramentas no Job On`.
- Alterar posteriormente uma ferramenta no Job On não reescreve snapshots de controlos históricos já aprovados. Um novo registo usa o contexto válido da revisão corrente do Job On.

### Consulta de disponibilidade durante a edição

O Job On possui dois modos com fronteira explícita:

| Modo | Objetivo | Ferramentas |
|---|---|---|
| `Modo consulta` | ler a folha necessária à produção | não permite adicionar, retirar, substituir, duplicar ou editar campos |
| `Modo edição` | preparar/corrigir o Job On | permite editar todos os campos da folha, duplicar e substituir associações de ferramentas |

O indicador de modo deve ser imediatamente distinguível sem usar cores agressivas: azul-cinza suave em `Modo consulta` e âmbar/castanho suave em `Modo edição`, sempre com contraste de texto suficiente.

Job On é a landing page de todos os utilizadores autenticados. Todos podem abrir a folha em `Modo consulta`; apenas o papel/template técnico Responsável recebe a capability de entrar em `Modo edição`, criar/duplicar Job Ons e gerir Definições. O título livre do perfil não concede esta capability. Confirmar verificações é uma ação operacional separada e não abre os restantes campos para edição.

- A informação live de disponibilidade não ocupa nem altera a folha em `Modo consulta`.
- Em `Modo edição`, todos os campos visíveis do Job On ficam editáveis, incluindo contexto da produção, quantidades, notas e todas as famílias secundárias: PU, CAL, AN, ARR, PI, CS, TP e FO.
- O contexto editável inclui Referência, Produção, Máquina/Linha, Secções, Gota, Tipo, Data início, Paragem, Data fim, Processo e Peso. Guardar cria uma nova revisão; não altera a revisão anterior.
- Em CAL, editar valores e quantidades por elemento; em PI, editar Pinças, Diâmetro e Notas; a mesma regra aplica-se aos campos equivalentes das restantes famílias.
- Opções evolutivas de dropdown, como o material das Pinças de PI, não ficam hardcoded. Definições permite adicionar, editar, ordenar e desativar opções para cada Família/Campo. A regra aplica-se aos campos equivalentes de todos os cartões.
- Desativar uma opção impede novas escolhas, mas preserva o valor em Job Ons e revisões antigas.
- Em `Modo edição`, usar `Alterar` no cartão CM/MP, MF ou BQ abre uma lista de seleção já filtrada pela Referência da ferramenta e pela Máquina do Job On.
- O utilizador pode refinar os filtros por lote, localização/contexto operacional, estado técnico e disponibilidade.
- Cada resultado mostra pelo menos: Referência, Lote, Nome técnico quando existir, Máquina compatível, Posição atual, Localização/contexto (`Armazém`, `Produção`, `Reparação` ou não registada), Estado técnico (`Novo`, `Reparado`, `Por reparar`), `% de uso` e disponibilidade.
- Exemplo legível: `CM 5447 · Lote 3 · Posição 2421 · Por reparar · 38% uso` ou `Fora — em reparação`.
- `Posição` e localização vêm do Armazém; estado técnico e `% de uso` vêm do domínio da ferramenta. O Job On agrega estes dados em leitura e não os copia como propriedades próprias.
- Um clique seleciona uma opção; duplo clique abre a ficha/histórico da ferramenta no módulo autoritativo; `Associar lote selecionado` confirma a substituição no rascunho do Job On.
- Pesquisar ou selecionar não cria movimento de Armazém e não reserva fisicamente a ferramenta. Qualquer saída continua a ser registada pelo fluxo próprio do Armazém.
- Ao guardar o Job On, persistir o ID/lote escolhido e o snapshot completo da revisão. A localização/estado live continuam consultáveis e podem mudar sem reescrever o Job On histórico.
- Localização, estado, compatibilidade ou `% de uso` geram informação/aviso, mas não bloqueiam a associação nem a gravação do rascunho. A decisão final pertence ao utilizador autorizado.

## 7.1 Ownership e persistência do snapshot

### Base de dados do Job On

Cada Job On/revisão guarda uma fotografia completa e autónoma da produção:

- `jobOnId`, Referência, Produção, Máquina, datas, Secções, Gota, Processo apresentado e restantes campos de contexto;
- origem da cópia (`copiedFromJobOnId`) quando foi duplicado;
- para CM/MP, MF e BQ: ID estável da ferramenta/lote associado **e** snapshot dos valores apresentados/usados naquela produção;
- conteúdo completo de PU, CAL, AN, ARR, PI, CS, TP e FO;
- linhas de CAL, incluindo Elemento, Valor e Quantidade em máquina;
- PI, incluindo tipo/material das Pinças, Diâmetro e Notas;
- quantidades, parâmetros, notas gerais, verificações/ocorrências e imagem/ligação conforme o contrato final;
- revisão, autor, datas de criação/alteração e auditoria.

Exemplo obrigatório: se `Job On 202601 · Referência 5447T173` usa uma configuração específica de PI, essa configuração fica gravada no snapshot do Job On 202601. Não depende de voltar a consultar o valor atual de PI para reconstruir a folha.

### Bases de dados das ferramentas e do Armazém

Permanecem fora do Job On:

- identidade mestre da ferramenta, Nome técnico, desenho, lotes e máquinas permitidas;
- estado técnico atual, `% de uso`/vida e histórico de reparações;
- posição/localização atual e movimentos do Armazém;
- restantes dados mestre que pertencem ao domínio CM, MF, BQ ou outro módulo autoritativo.

Editar um campo do snapshot do Job On não altera a ficha mestre, o estado técnico, a vida, a posição nem o histórico da ferramenta. Para mudar a associação concreta de CM/MF/BQ, o utilizador escolhe outra ferramenta/lote na lista live; para mudar valores de produção como PI, CAL, pinças ou calibres, edita diretamente o snapshot.

### Duplicação sem bloqueios

Ao duplicar `Job On 202601` para `202602`:

1. copiar a fotografia completa de todos os grupos e linhas, não apenas IDs de ferramentas;
2. atribuir novo `jobOnId`, Produção/datas novas e `copiedFromJobOnId`;
3. abrir imediatamente em `Modo edição`;
4. permitir alterar qualquer campo, incluindo Pinças, PI, CAL/calibres, quantidades, notas e associações CM/MF/BQ;
5. mostrar disponibilidade atual como ajuda, sem substituir valores nem bloquear a gravação;
6. guardar um novo snapshot independente. O Job On 202601 permanece imutável.

Não recalcular nem “atualizar” automaticamente os valores copiados a partir das bases mestre. O utilizador decide o que mantém e o que altera.

Mesmo quando se corrige o próprio Job On original, não substituir a revisão anterior: `Guardar alterações` cria uma nova revisão. A consulta do histórico deve conseguir abrir qualquer revisão e mostrar os valores exatos então guardados. Registos e documentos emitidos mantêm ligação ao `job_on_revision_id` usado.

O histórico principal organiza os Job Ons pela Referência e apresenta as respetivas Produções. Selecionar uma Referência deve permitir percorrer `202601`, `202602`, etc.; um clique seleciona a Produção e duplo clique abre a folha. O histórico de revisões dessa Produção fica dentro da folha/histórico detalhado e não se mistura com a lista de Produções.

O esquema técnico canónico está em `JOB_ON_DATA_MODEL.md`. Usa um cabeçalho de Job On, revisões imutáveis, componentes, campos tipados e linhas repetíveis para CAL. Não depende de reproduzir fotograficamente a folha antiga nem de guardar os detalhes num bloco opaco.

View model ilustrativo da lista de edição — não é uma imposição de esquema de base de dados:

```json
{
  "toolId": "cm-5447-l3",
  "family": "CM",
  "reference": "5447",
  "lot": "3",
  "technicalName": "Contra-molde 5447",
  "compatibleMachines": ["B1", "B3"],
  "location": {
    "context": "warehouse",
    "position": "2421",
    "source": "warehouse"
  },
  "technicalState": {
    "condition": "Por reparar",
    "usagePercent": 38,
    "source": "tool-domain"
  }
}
```

O cliente deve mostrar loading, vazio e erro por fonte. Se o domínio da ferramenta responder mas o Armazém falhar, pode mostrar os dados técnicos e `Localização indisponível`; nunca converter falha de consulta em `Não está no Armazém`.

### Dois níveis de interação

1. **Dropdown rápido:** clicar no campo `Lote` mostra diretamente os lotes compatíveis.
2. **Lista completa:** clicar no ícone/área de pesquisa do seletor, ou em `Ver todos os lotes compatíveis`, abre uma lista com mais contexto.

A lista completa segue o padrão global:

- um clique seleciona a linha;
- duplo clique abre o registo completo desse lote no respetivo módulo;
- o botão externo `Associar lote selecionado` confirma a seleção e regressa ao Job On;
- pesquisa e filtros não selecionam automaticamente um resultado;
- a linha apresenta pelo menos Referência, Lote e Máquina/Linha; outros campos só entram quando existirem na fonte autoritativa;
- a linha apresenta também o Nome técnico para distinguir ferramentas semelhantes;
- a seleção atual fica visualmente marcada.

### Estados do seletor

- **Um ou mais lotes compatíveis:** mostrar as opções, sem escolher automaticamente.
- **Nenhum lote compatível:** mostrar `Nenhum lote registado para esta referência e máquina` e ligação para abrir o módulo de origem, se o utilizador tiver permissão.
- **Valor copiado continua compatível:** manter selecionado e marcar como proveniente do Job On anterior.
- **Valor copiado deixou de ser compatível:** manter visível como valor anterior e mostrar atenção; permitir guardar se o utilizador autorizado decidir mantê-lo.
- **Erro de carregamento:** não apresentar lista vazia como se fosse um resultado válido; mostrar erro e permitir tentar novamente.

### Informação no cartão fechado

Depois da escolha, o resumo da família mostra:

- Referência;
- Lote selecionado;
- Máquina/Linha associada;
- origem `Copiado do anterior` ou `Alterado neste Job On`, quando aplicável.

Não usar um campo de texto livre para o lote quando o registo já existe no módulo autoritativo.

## 8. Comparação com produção anterior

A comparação deve separar:

### Snapshot da produção anterior

- ferramentas/lotes usados;
- máquina;
- valores e notas registados naquele momento;
- problemas e instruções aplicáveis naquele momento.

### Estado atual

- disponibilidade/localização atual;
- reparações posteriores;
- utilização atual quando disponível;
- informação atual de Controlo/Reparação relevante;
- valores atualmente propostos para o novo Job On.

Valores anteriores são sugestões/candidatos. O Responsável decide reutilizar, substituir ou verificar; copiar não transforma o histórico em verdade atual.

## 9. Notas, instruções e alertas

- Notas específicas permanecem no cartão da respetiva família.
- Observações verificáveis seguem o contrato `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`.
- As regras são configuradas na tab `Verificações` da ficha da ferramenta/lote.
- As frequências V1 são `Uma vez no lote` e `Por fabrico`.
- O Job On apresenta as ocorrências e permite ao operador fazer o check; não edita a configuração da ferramenta.
- Confirmar esconde das pendentes, preserva o histórico e guarda operador/data.
- Notas gerais ficam num cartão próprio, com área de texto ampla.
- Instruções recorrentes aparecem apenas quando foram explicitamente registadas para o contexto aplicável.
- Alertas distinguem informação, atenção e bloqueio real.
- Todo o alerta explica a ação necessária.
- Percentagem de utilização é contexto; não bloqueia nem diagnostica automaticamente a ferramenta.

## 10. Desenho/visual técnico

- Imagem do artigo fica num cartão lateral no desktop e abaixo do contexto em ecrãs menores.
- Clique abre visualização maior.
- O desenho deve indicar versão/origem quando disponível.
- A UI não tenta interpretar automaticamente códigos de desenho para criar relações operacionais.
- O PDF de numeração é referência documental; não deve ser usado como regra automática sem contrato confirmado.

## 11. Tarefas de acompanhamento

Padrão candidato vindo das discussões:

- Responsável cria ação/verificação;
- escolhe operador registado;
- descreve a tarefa;
- associa ao Job On quando aplicável;
- operador vê a tarefa em destaque;
- operador marca como concluída;
- Job On mostra autor e data/hora de atribuição e conclusão.

Estados exatos, comentário de conclusão e possibilidade de reatribuição continuam por confirmar.

## 12. Responsividade

Desktop:

- contexto numa linha;
- famílias numa grelha de 3 ou 4 colunas de resumos;
- cartão expandido ocupa a largura disponível;
- desenho/notas podem usar coluna lateral.

Tablet:

- grelha de 2 colunas;
- contexto divide em duas linhas;
- toolbar pode quebrar linha.

Mobile/PWA:

- uma coluna;
- contexto essencial sempre primeiro;
- cartões fechados compactos;
- edição de uma família de cada vez;
- sem scroll horizontal na página.

## 13. Questões que precisam de confirmação

- significado oficial das siglas visuais e respetivos nomes apresentados;
- campos obrigatórios por família;
- autoridade/origem de cada campo;
- quem cria, edita, replica, finaliza e elimina;
- estados reais do Job On;
- diferença exata entre stock e quantidade em máquina/necessária;
- relação de `Tipo` com processo;
- que ferramentas são obrigatórias por produção;
- regras oficiais de compatibilidade/apertos;
- se e como o desenho é obtido;
- quais ações do acompanhamento são obrigatórias.
- ordenação canónica usada para determinar o Job On imediatamente anterior;
- campos mínimos que identificam de forma inequívoca um Job On histórico na lista de duplicação;
- origem autoritativa dos movimentos de entrada/saída apresentados pelo calendário;
- estado dos lotes que os torna elegíveis no seletor (por exemplo, ativos, disponíveis ou também históricos);
- campos adicionais necessários na lista completa de lotes compatíveis.
- se as verificações precisam de prioridade, comentário de conclusão ou anulação.

Até confirmação, o mockup pode mostrar estes elementos como estrutura visual, mas não deve implementar regras automáticas.

## 14. Critérios de aceitação do futuro mockup

- preserva toda a informação relevante dos exemplos sem repetir a grelha antiga;
- Referência, Produção e Máquina permanecem visíveis;
- famílias são identificáveis no estado fechado;
- editar expande inline;
- lista/histórico segue clique e duplo clique canónicos;
- comparação separa snapshot e live;
- notas gerais e específicas não se confundem;
- nenhum código técnico é inferido automaticamente;
- estados e alertas explicam ações;
- funciona sem scroll horizontal da página.
- `Duplicar anterior` nunca altera o Job On de origem;
- duplicar um Job On histórico substitui apenas a data; todos os outros valores partem da cópia escolhida;
- selecionar um dia futuro aplica essa data ao novo rascunho;
- um dia passado apresenta os registos de entrada/saída associados sem alterar dados;
- `Novo em branco` nunca copia dados de outra Referência por aproximação;
- pesquisar `6185` ou outro código consulta o módulo autoritativo e nunca cria uma ferramenta implicitamente;
- CM, MF e BQ só apresentam lotes fornecidos pelo respetivo módulo e compatíveis com Referência + Máquina/Linha;
- um lote copiado incompatível nunca é substituído silenciosamente.
- verificações por lote reaparecem apenas para um ID de lote novo e nunca perdem o histórico confirmado.


## END FILE CONTENT

---

# FILE 012

## Source Path
`docs\JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Ferramentas e Job On — verificações configuráveis

Estado: contrato funcional V1  
Objetivo: configurar verificações na ferramenta e executá-las no Job On

## 1. Local onde são configuradas

O Chefe abre a ficha da ferramenta/lote e entra na tab `Verificações`.

As regras não são criadas dentro do Job On. O Job On apenas apresenta as verificações geradas para o lote associado à produção e recebe os checks dos operadores.

Exemplo de configuração:

| Verificação | Frequência | Estado |
|---|---|---|
| Verificar gargalo | Por fabrico | Ativa |
| Verificar rebaixos | Por fabrico | Ativa |
| Meter pernos nos fundos | Uma vez no lote | Ativa |

## 2. Regra e ocorrência

### Regra da ferramenta/lote

Configuração editável pelo Chefe:

- texto da verificação;
- frequência;
- lote a que pertence;
- estado ativa ou desativada;
- autor e data/hora de criação/alteração;
- origem quando foi copiada de outro lote.

### Ocorrência no Job On

Verificação concreta criada a partir da regra:

- regra de origem;
- ferramenta e lote;
- Job On/Produção;
- estado pendente ou confirmada;
- operador que confirmou;
- data/hora da confirmação;
- eventos de reset, quando existirem.

Confirmar uma ocorrência não elimina nem altera a regra.

## 3. Frequências V1

O campo `Frequência` é um dropdown canónico com:

| Opção | Comportamento |
|---|---|
| Uma vez no lote | aparece enquanto não existir um primeiro check válido para esse lote; depois deixa de aparecer, salvo reset pelo Chefe |
| Por fabrico | cria uma nova ocorrência em cada novo Job On/Produção onde esse lote esteja associado |

As opções são modulares e podem crescer no futuro, mas não criar na V1 um construtor livre de condições por percentagem de vida, data, estado técnico ou texto da Referência.

## 4. Tab `Verificações` da ficha da ferramenta

### Cabeçalho

- título `Verificações`;
- descrição curta;
- botão `Adicionar verificação`;
- botão `Histórico de verificações`.

### Lista de configuração

| Verificação | Frequência | Estado | Última confirmação |
|---|---|---|---|

Comportamento:

- um clique seleciona a linha;
- duplo clique abre o detalhe/histórico da regra;
- ações ficam fora da lista;
- o dropdown de frequência usa o mesmo componente do design system;
- a seleção apresenta contexto do lote atual.

Ações externas dependentes da seleção:

- `Editar`;
- `Desativar` ou `Ativar novamente`;
- `Resetar verificação`;
- `Apagar`.

## 5. Adicionar e editar

`Adicionar verificação` expande um cartão inline com:

- `Verificação` — texto largo;
- `Frequência` — Uma vez no lote ou Por fabrico;
- lote atual como contexto read-only;
- `Guardar` e `Cancelar`.

Guardar só fecha depois da persistência. Cancelar não altera dados.

Editar altera a configuração futura. Não reescreve ocorrências nem Job On históricos.

## 6. Desativar, reativar e apagar

### Desativar

- deixa de gerar novas ocorrências;
- pendências existentes devem manter estado histórico/operacional segundo o comando confirmado;
- histórico permanece consultável.

### Ativar novamente

- volta a gerar ocorrências segundo a frequência;
- não duplica uma ocorrência já existente para o mesmo Job On;
- regista autor e data/hora da reativação.

### Apagar

O Chefe pode usar `Apagar` para retirar a regra da configuração do lote.

Regras técnicas:

- deixa de aparecer na lista ativa e não é copiada para futuros lotes;
- ocorrências e confirmações históricas permanecem imutáveis;
- o evento de remoção fica auditado;
- apagar não significa destruir histórico já utilizado.

## 7. Resetar uma verificação

Uma regra `Uma vez no lote` deixa de aparecer após o primeiro check. Se o Chefe quiser verificá-la novamente, seleciona-a e usa `Resetar verificação`.

O reset:

1. preserva a confirmação anterior;
2. regista Chefe e data/hora do reset;
3. cria uma nova pendência para o mesmo lote;
4. apresenta-a imediatamente se existir Job On ativo com esse lote;
5. caso contrário, apresenta-a na próxima utilização relevante do lote;
6. mantém no histórico confirmação anterior, reset e confirmação seguinte.

Resetar não altera a frequência da regra.

## 8. Duplicar um lote novo

Ao usar `Novo lote a partir deste`, a configuração de verificações do lote de origem é copiada para o rascunho do novo lote.

No rascunho, o Chefe pode:

- manter as linhas;
- editar texto ou frequência;
- adicionar verificações;
- remover verificações;
- desativar ou reativar verificações antes de guardar.

Ao guardar:

- o novo lote recebe a própria configuração;
- nenhuma ocorrência/check do lote anterior é copiada;
- uma regra `Uma vez no lote` começa sem confirmação no novo lote;
- alterações no novo lote não alteram a configuração do lote de origem;
- a origem `Copiada do lote …` fica registada.

## 9. Apresentação no Job On

Quando o Job On associa um lote, carrega as regras ativas desse lote.

O cartão da família, por exemplo MF, apresenta:

- contador `Verificações pendentes`;
- lista curta das pendentes;
- ação `Ver verificações`;
- para o Chefe autorizado, ligação `Gerir na ficha da ferramenta`.

Não existe `Adicionar verificação` dentro do Job On.

### Pendentes

- checkbox;
- texto;
- frequência;
- lote/contexto.

### Confirmadas

- fechadas por defeito em `Mostrar confirmadas`;
- apresentam operador e data/hora;
- permanecem consultáveis;
- não ocupam o cartão fechado.

## 10. Confirmar no Job On

Ao marcar o checkbox:

1. mostrar processamento;
2. persistir a confirmação;
3. guardar operador e data/hora;
4. remover das pendentes apenas após sucesso;
5. atualizar o contador;
6. manter em `Confirmadas`.

Falha ao guardar mantém a ocorrência pendente e visível.

Abrir ou ler a lista não conta como confirmação. Apenas o check persistido responde a `Já foi verificado?`.

Na V1 a confirmação é exclusivamente manual no Job On. Não a inferir de movimentos do Armazém, Reparação, estado técnico, percentagem de uso ou passagem do tempo. A UI deve apresentar explicitamente `Confirmada manualmente por {utilizador} · {data/hora}`.

## 11. Geração por frequência

### Uma vez no lote

- permanece pendente até ao primeiro check desse lote;
- reutilizar o lote não cria nova ocorrência depois da confirmação;
- um reset explícito cria nova pendência;
- um lote novo recebe regra copiada, mas sem a confirmação anterior.

### Por fabrico

- usa o ID estável do Job On/Produção;
- cria uma ocorrência por cada novo Job On onde o lote é necessário;
- confirmar num fabrico não confirma os seguintes;
- duplicar um Job On gera as ocorrências do novo Job On, não copia checks antigos.

## 12. Histórico para o Manager

Na ficha da ferramenta, `Histórico de verificações` apresenta:

| Verificação | Frequência | Referência | Lote | Job On/Produção | Estado | Confirmada em | Confirmada por |
|---|---|---|---|---|---|---|---|

Regras:

- `Pendente` não apresenta operador/data de confirmação;
- `Confirmada` apresenta dia/hora e operador;
- um clique seleciona;
- duplo clique abre o Job On/ocorrência;
- filtros: Referência/lote, verificação, estado e datas;
- regras apagadas/desativadas continuam visíveis no histórico;
- resets aparecem como eventos auditáveis;
- consultar não reativa nem confirma.

Exemplo: `Verificar gargalo — lote 25 — Produção 202608 — Confirmada em 18/08/2026 09:42 por Ana Martins`.

## 13. Alterar o lote no Job On

Ao substituir o lote associado:

1. preservar o snapshot das ocorrências do lote anterior;
2. carregar a configuração ativa do novo lote;
3. gerar apenas as ocorrências aplicáveis;
4. mostrar o que entrou/saiu da lista;
5. nunca reutilizar checks de outro lote.

## 14. Permissões

- criar, editar, desativar, reativar, resetar e apagar regras: Chefe/Responsável autorizado;
- confirmar ocorrências: operador autorizado do Job On;
- consultar histórico: segundo o template de acesso.

Autorizações são validadas no comando, não apenas pela visibilidade dos botões.

## 15. Estados vazios e erros

- sem configuração: `Este lote não tem verificações configuradas`;
- sem pendentes: `Sem verificações pendentes para este lote`;
- regra desativada: não gera novas ocorrências;
- falha ao carregar: mostrar erro, não estado vazio;
- falha ao confirmar: manter pendente;
- falha ao resetar: manter último estado válido;
- falha ao apagar/desativar: manter regra visível e ativa conforme o último estado persistido.

## 16. Questões por confirmar

- se a lista do novo lote copia também regras desativadas ou apenas as ativas;
- se `Apagar` exige motivo;
- se a confirmação exige comentário;
- comportamento das pendências já criadas quando a regra é desativada;
- reset/correção num Job On já fechado;
- nomenclatura final das capacidades do Chefe.

## 17. Critérios de aceitação V1

- regras são configuradas na tab Verificações da ficha da ferramenta/lote;
- linhas mostram texto, dropdown de frequência e estado;
- frequências V1 são Uma vez no lote e Por fabrico;
- Job On apenas apresenta e confirma ocorrências;
- check só esconde após persistência;
- operador e data/hora ficam registados;
- novo lote copia configuração, mas nunca checks/histórico;
- o Chefe pode editar, desativar, reativar, resetar e apagar;
- apagar/desativar não destrói histórico;
- reset cria nova pendência e preserva confirmações anteriores;
- Manager consulta quem verificou e quando;
- duplicar Job On não copia checks antigos.


## END FILE CONTENT

---

# FILE 013

## Source Path
`docs\MODULE_UI_HANDOFF_TEMPLATE.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Template obrigatório — handoff de UI de um módulo

> Este ficheiro deve ser copiado e preenchido ao criar um módulo. Não repetir as regras globais: referenciar `DMO_DESIGN_SYSTEM.md` e documentar apenas decisões específicas.

## 1. Identificação

- Nome do módulo:
- Objetivo:
- Utilizadores:
- Página inicial:
- Ações auditáveis: listar os comandos deste módulo que geram eventos em `AUDITORIA_GLOBAL_HANDOFF.md`.
- Identificador associado: indicar `entityType`, `entityId`, label legível e, quando existir, `jobOnId`/produção.
- Permissões/capacidades confirmadas:
- Fontes de dados confirmadas:

## 2. Tabs

| Tab | Objetivo | Quem pode ver | Vista inicial |
|---|---|---|---|
| | | | |

## 3. Header

- Título da página:
- Subtítulo/contexto:
- Origem do nome do utilizador:
- Origem do título/função:

O componente usado é sempre `.dmo-app-header`.

## 4. Ações e cartões expansíveis

| Botão | Localização | Cartão que expande | Campos | Validação | Resultado ao guardar | Cancelar |
|---|---|---|---|---|---|---|
| | | | | | | |

Para cada botão indicar explicitamente:

- se abre/fecha inline;
- valor de `aria-controls`;
- primeiro campo a receber foco;
- se fecha outros editores;
- comportamento com alterações não guardadas;
- feedback apresentado depois da ação.

## 5. Filtros

| Filtro | Tipo | Valores/origem | Componentes afetados | Valor inicial |
|---|---|---|---|---|
| | | | | |

- Comportamento de `Aplicar filtros`:
- Comportamento de `Limpar filtros`:
- Estado vazio:
- Filtros preservados ao mudar de tab:

## 6. Listas e tabelas

| Lista | Colunas | Clique simples | Duplo clique | Ações externas | Paginação |
|---|---|---|---|---|---|
| | | Seleciona | Abre | | |

Todas as listas usam `data-dmo-list`, `data-dmo-row`, `data-id`, classe `selected` e `aria-selected`.

## 7. Calendário

- Existe calendário: Sim/Não
- Dados assinalados:
- Ação ao selecionar dia:
- Lista/resumos afetados:
- Comportamento de `Mostrar todas as datas`:

Usar sempre o calendário global, sem variante visual do módulo.

## 8. Formulários

| Campo | Tipo | Obrigatório | Origem | Largura | Validação | Exemplo/placeholder |
|---|---|---:|---|---|---|---|
| | | | | | | |

Confirmar:

- campos pequenos usam largura compacta;
- datas ficam na posição definida pelo fluxo;
- notas usam o espaço restante;
- erros aparecem junto do campo;
- valores decimais respeitam a precisão confirmada.

## 9. Estados e alertas

| Estado/conflito | Como é detetado | Texto apresentado | Cor/token | Ações permitidas |
|---|---|---|---|---|
| | | | | |

Não usar pop-up apenas para explicar um conflito.

## 10. Menus contextuais

| Contexto | Opções | Condições | Ação destrutiva |
|---|---|---|---|
| | | | |

## 11. Fluxos completos

### Criar

1.

### Editar

1.

### Selecionar e abrir

1. Um clique seleciona.
2. Duplo clique abre.

### Corrigir/eliminar/fechar

1.

## 12. Estados da página

- Carregamento:
- Sem dados:
- Erro de servidor:
- Sem permissão:
- Dados desatualizados:
- Operação concluída:

## 13. Responsividade e acessibilidade

- Comportamento abaixo de 1200px:
- Comportamento abaixo de 980px:
- Comportamento abaixo de 720px:
- Ordem de foco:
- Operação por teclado:
- Regiões `aria-live`:

## 14. Integrações

| Integração | Dados recebidos | Dados enviados | Falha/fallback | Confirmada? |
|---|---|---|---|---:|
| | | | | |

Não inventar endpoints, tabelas, campos ou contratos futuros.

## 15. Elementos removidos ou substituídos

| Elemento anterior | Decisão | Motivo | Destino alternativo |
|---|---|---|---|
| | | | |

## 16. Critérios de aceitação específicos

- [ ] Header global aplicado.
- [ ] Tabs seguem alinhamento global.
- [ ] Botões seguem filled → hover invertido.
- [ ] Expansores abrem cartões inline.
- [ ] Filtros afetam todos os componentes documentados.
- [ ] Clique seleciona e duplo clique abre.
- [ ] Calendário global reutilizado.
- [ ] Ações dependentes de seleção ficam fora da lista.
- [ ] Campos têm largura proporcional.
- [ ] Estados vazios, erros e carregamento estão definidos.
- [ ] Teclado e ARIA verificados.
- [ ] Não existem regras de negócio inventadas.


## END FILE CONTENT

---

# FILE 014

## Source Path
`docs\PEGAMENTOS_INTERFACE_HANDOFF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Pegamentos — handoff funcional e visual

## Objetivo

A folha mantém os cálculos, medições, tolerâncias, verificação dimensional e impressão do ficheiro original. A alteração é de fluxo e integração: antes de apresentar a folha, a aplicação exige um contexto de trabalho.

## Fluxo aprovado

1. Abrir o tab **Pegamentos**.
2. Selecionar/receber o **Job On** da produção.
3. A aplicação carrega do Job On a **Referência**, **Produção**, **Máquina** e as instâncias exatas de **CM**, **BQ** e **MF**, incluindo os respetivos lotes.
4. **Abrir folha** só prossegue quando o Job On contém todo o contexto obrigatório.
5. O contexto e as ferramentas herdadas permanecem visíveis no topo como dados não editáveis.
6. A folha mantém medições, limites, validações, mapa dimensional e **Imprimir / Guardar PDF**.

## Contrato de dados

Não criar catálogos paralelos. As opções vêm das tabelas já existentes no programa.

| Campo | Origem | Regra |
|---|---|---|
| Job On | Job On/planeamento | identificador estável e obrigatório |
| Referência | Job On | herdada; não voltar a escolher |
| Produção | Job On | herdada; seis dígitos |
| Máquina | Job On | herdada; B1–C3 |
| CM | instância/lote selecionado no Job On, proveniente do Peso | usar exatamente o CM e lote do Job On |
| BQ | instância/lote selecionado no Job On, proveniente de Boquilhas/Reparação | usar exatamente a BQ e lote do Job On |
| MF | instância/lote selecionado no Job On, proveniente do respetivo domínio | usar exatamente o MF e lote do Job On |

O protótipo contém `COMPONENT_CATALOG` apenas como dados demonstrativos. Na implementação, não é um seletor alternativo dentro de Pegamentos: a resolução vem do Job On e é validada contra os catálogos do backend.

## Integração obrigatória com Job On

Payload mínimo esperado:

```json
{
  "jobOnId":"JO-202601-B3",
  "reference":"9389T194",
  "production":"202601",
  "machine":"B3",
  "cm":{"id":"cm-5447-l4","reference":"5447","lot":"4"},
  "bq":{"id":"bq-t194-l12","reference":"T194","lot":"12"},
  "mf":{"id":"mf-9389-l26","reference":"9389","lot":"26"}
}
```

Ao receber o Job On:

- preencher Referência, Produção e Máquina como contexto não editável;
- carregar os IDs, referências e lotes concretos de CM, BQ e MF já escolhidos no Job On;
- validar que essas instâncias ainda existem e são compatíveis com a máquina;
- manter o contexto e as ferramentas visíveis para confirmação;
- gravar `jobOnId` em todo o registo e snapshot de Pegamentos.

Não existe fallback que permita escolher silenciosamente outra ferramenta. Se faltar CM, BQ ou MF obrigatório, ou se um lote estiver inválido, bloquear a folha com uma mensagem acionável: `Corrigir ferramentas no Job On`.

## Persistência e pasta do relatório

- O servidor guarda o registo estruturado de Pegamentos: Job On, Produção, Referência, Máquina, IDs/lotes CM-BQ-MF, medições, resultados, estado, revisão e auditoria.
- O PDF enviado/impresso para Produção é guardado no computador/local configurado e é gerado a partir do snapshot da folha.
- O diretório principal é definido em `Definições` do sistema, por exemplo `Capacidades`.
- A subpasta é a definida ao criar o lote no Peso, por exemplo `5447T173`; o caminho resolvido apresentado em Pegamentos é `Capacidades / 5447T173`.
- Pegamentos apenas apresenta o caminho resolvido. Não permite selecionar uma pasta diferente para o mesmo Job On/lote.
- O nome do ficheiro usa dados retirados do Job On, incluindo pelo menos Produção, Referência, tipo `Pegamentos` e revisão/data.
- Mostrar separadamente `Dados guardados no servidor` e `PDF guardado localmente`. Uma falha local não apaga o registo numérico.

## Histórico de Pegamentos

- Filtros: Job On, referência/produção, máquina, data inicial e data final.
- Um clique seleciona visualmente a linha.
- Duplo clique abre a folha associada.
- Não existe botão adicional para abrir a linha selecionada.
- Não colocar ações de abertura dentro ou abaixo da lista.

## Elementos removidos

- **+ Nova referência** acima dos tabs;
- cartão **Base de dados** em Configurações;
- **Guardar ficheiro para imprimir**;
- **Enviar resumo**.

As funções antigas podem permanecer temporariamente no JavaScript, mas não fazem parte da interface aprovada.

## Design e aceitação

- Usa `dmo-design-system.css` e os tokens canónicos.
- Usa o header canónico com logótipo, título da página, nome e título/função do perfil administrativo.
- Botões preenchidos em repouso e invertidos no hover.
- Campos compactos; cartões claros; estados dessaturados.
- Não abrir sem Job On, Referência, Produção, Máquina e as instâncias/lotes obrigatórios de CM, BQ e MF.
- Alterar o Job On substitui todo o contexto como uma unidade; não mistura ferramentas de produções diferentes.
- Registos antigos preservam valores.
- Adicionar/remover medições e cálculos originais continuam ativos.
- Números apresentam no máximo duas casas decimais.
- O relatório identifica referência, produção, máquina e data.
- O cabeçalho de medição usa **Costura** e **Contra costura**; o nome interno legado `noventa` pode ser migrado posteriormente sem alterar o cálculo.


## END FILE CONTENT

---

# FILE 015

## Source Path
`docs\PESO_INTERFACE_HANDOFF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Peso/Controlo — handoff da passagem de design

## Âmbito aplicado

- Operador mantém a estrutura funcional principal.
- Removido o botão manual `Atualizar` do cabeçalho.
- Sincronização é apresentada como estado automático, não como ação.
- Removidos os cartões `Preencher`, `Guardar`, `Aprovação`, `Produção`.
- `Gerar documento para produção` e `Enviar para produção` saem do Novo controlo.
- Estas ações passam para o Histórico e para a folha de uma revisão aprovada.
- Resultados deixam de ser texto corrido e passam a resumo + tabela por CM.
- Responsável usa a mesma fonte de resultados com menos colunas.

## Novo controlo

- Existem dois tipos de registo no módulo Peso: `Novo controlo` e `Comparação`. Ambos ficam ligados ao **Job On da produção** que lhes dá contexto.
- `Novo controlo` começa a partir do Job On selecionado/ativo para aquela produção e Referência.
- O Job On fornece o seu identificador, Referência, Produção, Máquina e o **CM/lote exatos** escolhidos para fabricar, por exemplo `Produção 202601 · CM 5447 · Lote 4`.
- Os dados técnicos do lote CM já referenciado pelo Job On fornecem Processo NNPB/PS e máquinas permitidas.
- O identificador/contexto do Job On permanece visível na folha e é guardado com o controlo para permitir abrir a produção correta mais tarde.

| Tipo de registo | Momento | Contexto apresentado | Base usada |
|---|---|---|---|
| Novo controlo | preparação/entrada da nova produção definida no Job On | Job On, Referência, Produção, Máquina, CM e lote escolhidos no Job On, Processo do lote | leituras introduzidas para essa produção |
| Comparação | depois de a produção estar a trabalhar | o mesmo Job On, Referência, Produção, Máquina, lote e Processo | Novo controlo aprovado desse Job On + leituras dos CM atualmente em produção |

O vínculo técnico deve usar o identificador estável do Job On/produção devolvido pela aplicação. O texto da Referência, Produção ou Máquina é apenas apresentação e filtro.
- `Calcular` e `Enviar para aprovação` permanecem ações explícitas.
- Guardar pode ser automático; mostrar `A guardar`, `Guardado` ou erro.
- Enviar para aprovação nunca é automático.
- A máquina atual deve pertencer às máquinas permitidas da referência.
- Referência, Produção, Máquina, CM e Lote são herdados do Job On; o Processo é herdado do lote CM referenciado. Não devem ser novamente pedidos ao Operador.
- Estado do molde, temperatura e leituras continuam editáveis quando aplicável.
- Campos devem respeitar o tamanho real dos valores: máquina e temperatura são compactos; lote comporta três caracteres; data e estado ocupam apenas o necessário.
- `Fim da produção anterior (SAP)` e `Peso médio anterior (SAP)` são compactos; `Notas` recebe o espaço restante.
- Todos os valores decimais introduzidos ou calculados são normalizados para, no máximo, duas casas decimais na interface e nos documentos.
- `Adicionar leitura` cria efetivamente um novo par CM/Peso e integra-o no cálculo.
- `Remover leitura` remove a última leitura; nunca permite ficar sem pelo menos uma leitura.
- Novo controlo e Comparação usam os mesmos componentes e os mesmos textos: `Remover leitura`, `Adicionar leitura`, `Calcular` e `Enviar para aprovação`.
- Por baixo de cada par CM/Peso existe um resultado compacto `Peso do vidro`, atualizado em tempo real através do motor de cálculo existente.

## Referências

- Ao criar o primeiro lote, ou ao duplicar/criar outro lote no Peso, pedir `Processo` obrigatório: NNPB ou PS.
- NNPB/PS pertence ao lote do Peso, não ao formulário do Novo controlo e não à Referência mestre isoladamente.
- Substituir a linha única por cartões de `Máquinas permitidas`: B1–C3.
- Pelo menos uma máquina é obrigatória.
- Os cartões B1–C3 representam a associação funcional entre o lote e as máquinas/linhas onde esse lote pode trabalhar; não são apenas uma preferência visual ou um filtro.
- Ao guardar, a seleção deve usar e atualizar a associação que já existe no programa, sem criar uma segunda estrutura paralela.
- Um lote pode ficar associado a várias máquinas/linhas permitidas, mas a máquina atual de cada controlo continua a ser uma única máquina.
- Novo controlo e Comparação só podem usar uma máquina incluída nessa associação.
- O Novo controlo e a Comparação mostram sempre o Job On/Produção a que pertencem.
- O CM e lote usados são os que já estão explícitos no Job On. O módulo Peso não apresenta uma segunda escolha de CM/lote para esse controlo.
- Se o Job On não tiver CM/lote válido, bloquear a abertura do controlo e encaminhar a correção para o Job On; não escolher automaticamente outro CM.
- A base normal da Comparação é o Novo controlo aprovado associado ao mesmo Job On da produção.

## Controlos da referência ativa

- Tabela sem botões dentro das linhas.
- Um clique seleciona e ativa `Editar controlo`.
- Duplo clique abre a folha.
- `Novo controlo para esta referência` fica fora da lista e permanece disponível.
- `Editar controlo` fica fora da lista e exige seleção.
- Editar revisão aprovada exige justificação, cria nova revisão e exige nova aprovação.
- A revisão aprovada anterior permanece imutável.

## Lista de referências

- Usa o mesmo contrato canónico das restantes listas.
- Um clique seleciona a referência e atualiza a área de detalhe e os controlos associados.
- Duplo clique encaminha para `Histórico` com o filtro da referência já aplicado.
- Se a abertura partir de um lote específico, aplicar simultaneamente referência e lote.
- Não criar uma segunda página dedicada aos registos do lote; o Histórico é a vista canónica desses registos.
- Ao entrar no Histórico por este atalho, manter disponíveis os restantes filtros e indicar claramente quais foram pré-aplicados.

## Histórico

- Contém apenas controlos enviados para aprovação.
- Um clique seleciona; duplo clique abre a folha.
- Não existe botão `Abrir folha`; a abertura é sempre feita por duplo clique na linha selecionada.
- `Gerar folha de produção` e `Enviar email para produção` ficam fora da tabela.
- Só ficam ativos para a revisão aprovada selecionada.
- Documento/email usam o snapshot aprovado, nunca valores entretanto alterados.
- Assim que o Responsável confirma `Aprovar`, a mesma folha apresenta imediatamente `Enviar para produção`; não obriga a navegar primeiro para o Histórico.
- A máquina/linha do snapshot aprovado escolhe automaticamente o grupo de destinatários: B1–B3 usam `Linha B` e C1–C3 usam `Linha C`.
- Antes do envio, mostrar máquina, destinatários resolvidos, assunto, mensagem e anexo. O envio exige confirmação explícita e nunca acontece automaticamente com a aprovação.
- Se não existir configuração de destinatários para a máquina, bloquear apenas o envio e indicar que a configuração está em falta; a aprovação mantém-se válida.
- Destinatários e template do email pertencem às Definições da aplicação. A interface apresenta a resolução devolvida pelo serviço e não altera o snapshot aprovado.
- Filtros mínimos: Job On, Referência, Produção, Tipo (`Novo controlo`/`Comparação`), Estado e intervalo de datas.

## Persistência dos valores e dos documentos

O módulo separa deliberadamente o **registo estruturado** do **documento enviado à Produção**:

| Conteúdo | Persistência | Regra |
|---|---|---|
| Job On, Produção, Referência, CM, lote CM, leituras, cálculos, estados, revisões, decisões e auditoria | servidor | constitui o histórico pesquisável e comparável do Peso |
| PDF aprovado/enviado para Produção | computador/local configurado | é um artefacto documental gerado a partir do snapshot aprovado |

- O servidor guarda os números e a ligação estável ao Job On. O PDF não substitui estes dados.
- Em `Definições`, o utilizador autorizado escolhe o `Diretório principal dos relatórios`, por exemplo `Capacidades`.
- Ao criar o primeiro lote ou um novo lote no Peso, existe o campo `Subpasta dos relatórios`, por exemplo `5447T173`.
- A aplicação resolve e apresenta o caminho final como `Capacidades / 5447T173`.
- A subpasta é um nome relativo dentro do diretório principal. O formulário não aceita nem guarda um caminho absoluto arbitrário.
- Se a subpasta ainda não existir, é criada dentro do diretório principal depois de confirmação/permissão do sistema local; se já existir, é reutilizada.
- Peso e Pegamentos associados ao mesmo Job On/lote resolvem a mesma subpasta. Pegamentos não cria uma pasta concorrente para essa Referência.
- O nome do PDF é gerado com dados do Job On/snapshot aprovado e deve incluir contexto suficiente para evitar colisões, pelo menos Produção, Referência, tipo de documento e revisão/data. A convenção final do nome é configuração técnica, não texto livre do operador.
- A interface distingue os estados `Dados guardados no servidor` e `PDF guardado localmente`. Uma falha ao escrever o PDF não apaga nem reverte o registo estruturado aprovado.
- Noutro computador, o utilizador continua a consultar o histórico numérico do servidor; abrir o PDF depende de esse computador ter acesso à pasta local/partilhada configurada.
- A permissão do diretório é local ao browser/computador e pode precisar de ser renovada. Nunca apresentar uma pasta como disponível antes de confirmar a permissão.

## Resultados

Resumo: densidade, capacidade média, peso médio estimado, peso nominal, diferença absoluta e percentual.

Tabela do Operador: CM, peso em água, capacidade, desvio cm³, desvio %, peso estimado e diferença anterior.

Tabela do Responsável: CM, capacidade, desvio %, peso estimado e diferença anterior.

As duas vistas usam os mesmos dados calculados; a vista do Responsável apenas esconde detalhe operacional.

## Origem do processo NNPB/PS

- O processo operacional é escolhido ao **criar o lote no módulo Peso**.
- NNPB ou PS é guardado no lote do Peso e pode variar apenas segundo as regras funcionais permitidas para os lotes; não é pedido no Novo controlo.
- Ao resolver o lote do Peso associado ao Job On, o Novo controlo e a Comparação herdam automaticamente esse processo.
- O Operador não escolhe novamente o processo no Novo controlo nem numa Comparação.
- O valor mostrado em `Referência ativa` é informativo e deve ser apresentado como não editável.
- O lote do Peso é a fonte de verdade para o processo apresentado no contexto do Job On, nos controlos e nas comparações.
- Registos anteriores mantêm no seu snapshot o processo e o lote usados naquele Job On.

## Comparações operacionais

- A `Comparação` é o segundo tipo de registo do Peso. É criada no contexto de um Job On/produção que já tem um Novo controlo aprovado.
- O objetivo visual é registar os CM que **já estão em produção** e compará-los com os valores aprovados no Novo controlo associado ao mesmo Job On.
- Novo controlo, Comparação e Job On partilham e apresentam o mesmo identificador de contexto da produção; não se relacionam apenas pelo texto da Referência.
- `Comparações` deixa de ser um separador principal.
- Selecionar o Novo controlo aprovado do Job On ativa o botão externo `Comparar`.
- A folha de Comparação mantém no topo o bloco `Referência ativa`: Referência, CM, Boquilha, Processo e Máquina atual.
- Este bloco inclui também Job On e Produção; a secção seguinte identifica separadamente o Novo controlo aprovado usado como base.
- A comparação usa esse controlo aprovado como base imutável.
- Job On, Referência, Produção e Máquina identificam a produção atual; Lote e Processo vêm do lote do Peso associado; médias de peso/capacidade vêm do Novo controlo aprovado desse Job On.
- Os CM atualmente medidos na Comparação pertencem à produção identificada pelo mesmo Job On; a base continua a ser o Novo controlo aprovado desse Job On.
- Os números de CM introduzidos na Comparação identificam elementos individuais do CM/lote já associado ao Job On; não permitem mudar para outro lote de ferramenta.
- O Operador introduz apenas CM atualmente em produção, respetivo peso e notas opcionais.
- A Comparação reutiliza o bloco completo do Novo controlo: leituras, resumo em tempo real, campos auxiliares, resultados e tabela detalhada.
- `Fim da produção anterior (SAP)` é substituído por `Data do registo da comparação`, preenchida com a data efetiva do novo registo.
- `Calcular` atualiza peso do vidro, capacidade e diferenças, comparando com o controlo/lote aprovado selecionado.
- A capacidade e diferenças são calculadas e limitadas visualmente a duas casas decimais.
- Guardar cria um registo complementar e envia-o ao Responsável; não altera a aprovação original.
- A ação final chama-se `Enviar para aprovação`, com o mesmo componente e estado visual do Novo controlo.
- O cálculo do mockup é apenas demonstrativo; a implementação deve chamar a função/motor de cálculo já existente no programa. Não duplicar nem reimplementar a fórmula no componente visual.

## Decisão da comparação pelo Responsável

- A comparação aparece na mesma lista diária dos controlos, identificada como `Comparação`.
- Cada CM recebe uma decisão independente: `Manter` ou `Colocar de parte`.
- Cada linha mostra peso atual, capacidade atual, peso aprovado, capacidade aprovada e as duas diferenças.
- A decisão é comparada com as médias de peso e capacidade do controlo aprovado usado como base.
- A confirmação fica bloqueada enquanto existir algum CM sem decisão.
- Se pelo menos um CM for colocado de parte, a justificação é obrigatória.
- O resumo mostra quantos CM foram mantidos, colocados de parte e permanecem sem decisão.
- A confirmação cria decisões por CM, com operador, responsável, data/hora e referência à revisão aprovada.
- O controlo aprovado original permanece imutável e mantém o respetivo estado.

## Estados e filtros de tipo

- Registo de peso e Comparação usam os mesmos estados canónicos: `Pendente`, `Aprovado` e `Não aprovado`.
- `Comparação` é um tipo de registo, nunca um estado como `Por decidir`.
- As listas do Responsável e do Histórico incluem o filtro `Tipo`: `Todos`, `Registo de peso` e `Comparação`.
- O estado usa tons suaves derivados da paleta base: azul/cinza para pendente, verde acinzentado para aprovado e terracota discreto para não aprovado.
- Os botões `Manter` e `Colocar de parte` usam os mesmos tons discretos; a decisão escolhida mantém destaque e a alternativa perde intensidade.

## Densidade das tabelas

- Tabelas compactas usam padding reduzido sem diminuir a área mínima de seleção.
- Colunas curtas, como máquina, lote, processo e revisão, não recebem largura elástica.
- A lista de referências deve caber no cartão em desktop sem scroll horizontal.
- Em ecrãs pequenos, o scroll horizontal continua permitido para não ocultar dados.

## Página do Responsável

- É uma única página de aprovação; não existe uma segunda vista de Comparações.
- O calendário escolhe o dia e a lista apresenta os controlos desse dia.
- A área direita mostra apenas o controlo selecionado e a comparação necessária à decisão.
- `Temperatura` deixa de aparecer na identificação principal e é substituída por `Operador`, que acrescenta rastreabilidade à decisão.
- A temperatura permanece no registo técnico e pode ser consultada na folha completa quando necessária.

## Identidade apresentada no cabeçalho

- O nome vem do utilizador autenticado.
- O texto por baixo do nome usa o campo de título/função do perfil, representado no mockup por `data-user-profile-title`.
- Esse título é texto livre gerido pelo Administrador na página de gestão de utilizadores, por exemplo `Metrologia`, `Chefe`, `Engenheiro` ou `Responsável de qualidade`.
- O título visual não concede permissões e não substitui o papel/template de acesso do utilizador.
- Não escrever o título diretamente em cada página; a shell partilhada carrega o valor canónico do perfil.

## Contrato comum de listas

O ficheiro `dmo-interactions.js` estabelece o comportamento canónico:

- O contentor usa `data-dmo-list`.
- Cada linha/cartão usa `data-dmo-row` e um `data-id` estável.
- Um clique seleciona uma única linha e emite `dmo:list-select`.
- Duplo clique abre o registo e emite `dmo:list-open`.
- Não existe atalho de teclado específico do BA DMO; a seleção/abertura é feita com clique/duplo clique.
- A seleção usa sempre a classe `selected` e `aria-selected`.
- Os botões de ação ficam fora da lista e respondem ao registo selecionado.
- Filtros nunca alteram ou eliminam seleção silenciosamente; se a linha deixar de estar visível, a seleção é limpa e as ações ficam desativadas.

## Contrato comum de calendários

- O contentor usa `data-dmo-calendar`.
- Cada dia usa `data-date="AAAA-MM-DD"`.
- Um clique seleciona uma única data e emite `dmo:date-select`.
- Dias com registos usam `has-record`; o ponto é apenas indicador, não outro botão.
- A data selecionada usa `selected` e `aria-pressed`.
- Setas alteram o mês sem selecionar automaticamente um dia.
- `Mostrar todas as datas` limpa o filtro de data e mantém os restantes filtros.
- Calendário e lista são sincronizados pela mesma data ISO, não pelo texto visível.


## END FILE CONTENT

---

# FILE 016

## Source Path
`docs\PORTAL_LOGIN_ADMIN_HANDOFF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Portal DMO — Login e Administração

## Login

- O Portal é a única entrada nas aplicações.
- O formulário recebe email e palavra-passe e apresenta estados de validação, autenticação em curso e erro.
- O botão Mostrar/Ocultar altera apenas a visibilidade local da palavra-passe.
- Após autenticação, encaminhar sempre para o módulo `Job On`, landing page comum de todos os utilizadores autenticados.
- Não apresentar escolha manual de papel no Login.
- Mensagens de erro não devem confirmar se um email específico existe.
- Sessão, expiração e recuperação de palavra-passe usam o serviço de autenticação existente.

## Identidade no cabeçalho

- Nome: utilizador autenticado.
- Título/função: campo livre do perfil, gerido pelo Administrador.
- Exemplos: Metrologia, Chefe, Engenheiro, Responsável de qualidade.
- O título não concede permissões e não substitui template, papel ou capacidades.
- Se estiver vazio, apresentar apenas o nome.
- A shell carrega este valor uma vez e apresenta-o de forma consistente em todos os módulos.

## Administração — utilizadores

Operações:

- Criar utilizador.
- Editar nome, email e título/função.
- Associar template de acesso.
- Ativar ou desativar conta.
- Iniciar reset de palavra-passe.

Reset de palavra-passe:

- Exige confirmação explícita.
- Nunca mostra nem recupera a palavra-passe atual.
- Usa o fluxo seguro do serviço de autenticação.
- Regista administrador, utilizador afetado, data/hora e resultado da operação.

## Administração — aplicações

- Listar módulos disponíveis.
- Alterar disponibilidade e ordem.
- Associar módulos/capacidades aos templates de acesso existentes.
- Desativar em vez de eliminar quando existirem registos históricos.
- Autorizações são validadas também no comando/serviço; ocultar botões não constitui autorização.

## Comportamento das listas

- Usa o componente canónico `data-dmo-list`/`data-dmo-row` quando houver seleção.
- Um clique seleciona; duplo clique abre a edição quando esse fluxo for adotado.
- Pesquisa e estado filtram sem eliminar dados.
- Ações destrutivas ou de identidade exigem confirmação e feedback final.

## Separação de responsabilidades

- Perfil: nome e título/função visual.
- Template de acesso: módulos, capacidades e ordem. A landing page global é Job On e não é configurável por utilizador.
- Estado da conta: possibilidade de autenticação.
- Administração: edição e auditoria destes dados.

## Auditoria global no Admin

- Todos os utilizadores autenticados geram eventos para cada ação de negócio relevante.
- Cada evento fica associado ao utilizador, módulo, ação, entidade, data/hora e resultado.
- A tab `Auditoria` permite consultar por ano e filtrar por utilizador, módulo, ação, resultado e período.
- Um clique seleciona o evento; duplo clique abre o detalhe.
- A vista usa paginação de 20, 40 ou 60 linhas e permite exportação anual autorizada.
- Não existe pontuação, ranking ou avaliação automática; a interface mostra apenas o registo factual.
- O contrato técnico completo está em `AUDITORIA_GLOBAL_HANDOFF.md`.

Não inferir permissões a partir do título/função apresentado no cabeçalho.

## Landing page e edição do Job On

- `Job On` é a landing page de Operador, Responsável, Administrador e restantes utilizadores autenticados.
- Todos recebem capacidade de consulta do Job On, Planeamento e Histórico dentro do âmbito autorizado.
- Apenas o papel/template técnico `Responsável` recebe `jobon.edit` e `jobon.configure`.
- Criar, duplicar, substituir ferramentas, alterar campos/datas, guardar revisão e gerir opções em Definições são operações de edição.
- Administração continua acessível ao Administrador através da navegação, mas deixa de ser a landing page.
- O título livre apresentado junto ao nome não concede `jobon.edit`; a autorização vem da capability validada pelo backend.


## END FILE CONTENT

---

# FILE 017

## Source Path
`docs\REPARACAO_EXTERNA_DESIGN_BRIEF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Reparação externa — brief funcional V1

Estado: contrato funcional para design e implementação  
Âmbito: preparação, envio, acompanhamento e retorno de BQ, CM e MF enviados para reparadores externos

## 1. Estrutura

Arquitetura de navegação revista:

1. `Boquilhas` permanece um módulo/tab principal existente (`boquilhas.html`);
2. `Moldes` é um novo módulo/tab principal (`moldes.html`);
3. dentro de Moldes, `Contra moldes` e `Moldes finais` usam o mesmo padrão visual de Boquilhas, mas mantêm dados e fluxos separados.

Não criar uma página intermédia obrigatória `Reparação` nem recriar Boquilhas. O menu principal encaminha diretamente para Boquilhas ou Moldes.

Moldes partilha componentes visuais e o ciclo externo, mas CM e MF não partilham entidades nem movimentos. CM e MF nunca são fundidos num único tipo.

Tabs adicionais:

- `Envios` — listas programadas e respetivo progresso físico no Armazém;
- `Histórico` — consulta transversal;
- `Definições` — reparadores permitidos por tipo e Linha.

## 2. Ciclo externo

Este fluxo não parte da ferramenta atualmente em produção. Essa associação pertence exclusivamente à Reparação interna de turno.

Na Reparação externa de Moldes, o responsável seleciona uma produção futura planeada e prepara a lista vários dias antes do início previsto do fabrico.

1. O responsável cria uma saída programada na Reparação.
2. Seleciona ferramentas/lotes e um reparador permitido.
3. A lista fica disponível no Armazém e pode ser impressa.
4. O operador do Armazém confirma cada retirada com um check.
5. Quando todos os itens estiverem confirmados, a saída é concluída e as posições ficam livres.
6. A Reparação acompanha o envio sem duplicar os movimentos do Armazém.
7. No retorno, o Armazém confirma cada entrada e posição.
8. Quando todos os itens regressarem, o ciclo fecha.

A página não apresenta cartões de Produções ativas ou futuras. A preparação começa diretamente pela seleção CM/MF e pesquisa das ferramentas. Quando a lista precisar de associação a uma produção prevista, essa escolha aparece como um campo compacto dentro do formulário da própria lista.

Cada item preserva datas e operadores de saída/entrada. Uma lista concluída não desaparece; passa para Histórico.

## 3. Boquilhas

Reutilizar o fluxo e o detalhe definidos em `BOQUILHAS_INTERFACE_BEHAVIOR.md`.

- unidade operacional: Referência + lote;
- movimentos por quantidade;
- saldo em fábrica, em reparação e não reparadas;
- reparador associado ao envio;
- registo local do lote continua acessível;
- clicar uma vez seleciona; duplo clique abre o lote.

O módulo BQ existente é aberto pela opção `Boquilhas` do menu da Reparação. Não existe migração, cópia ou redesenho da sua interface. A navegação não altera IDs, saldos ou histórico.

## 4. Contra moldes

- unidade operacional: ferramenta CM individual;
- seleção por Referência, lote, máquina permitida e número individual;
- estado e localização vêm dos domínios respetivos;
- saída programada referencia IDs estáveis de CM;
- retorno pode incluir observação, mas não altera automaticamente dados mestres.
- a lista é preparada para uma produção futura, antes do início do fabrico;

## 5. Moldes finais

Segue o mesmo ciclo externo do CM, usando exclusivamente ferramentas MF e respetivos IDs, campos, reparadores e histórico. Partilhar UI não autoriza combinar CM e MF no backend.

## 6. Lista programada

Cabeçalho:

- código da lista;
- tipo BQ/CM/MF;
- reparador;
- data prevista;
- criado por/data;
- estado.

Itens BQ mostram Referência, lote e quantidade. Itens CM/MF mostram Referência, lote, número individual, máquina/linha e posição atual quando conhecida.

Estados visuais V1:

- `Preparação`;
- `A retirar`;
- `Enviado`;
- `Retorno parcial`;
- `Concluído`;
- `Cancelado`.

Não inferir transições apenas pela abertura da página. Cada transição corresponde a confirmações persistidas.

## 7. Listas e ações

- um clique seleciona;
- duplo clique abre o detalhe;
- ações ficam fora da tabela;
- botões de ação usam 36px e ficam junto da paginação quando pertencem à seleção;
- paginação oferece 20, 40 e 60 linhas;
- filtros não selecionam automaticamente.

## 8. Alertas

- item sem localização conhecida: aviso, não localização inventada;
- item já incluído noutra saída aberta: bloquear duplicação;
- confirmação parcial: mostrar progresso explícito;
- retorno sem saída correspondente: bloquear e encaminhar para correção;
- falha de persistência: manter seleção e não mostrar sucesso.

## 9. Histórico

Campos mínimos:

| Lista | Tipo | Referência | Lote | Qtd./N.º | Reparador | Saída | Operador saída | Entrada | Operador entrada | Estado |
|---|---|---|---|---|---|---|---|---|---|---|

Filtros: período, tipo, Referência, lote, reparador, estado, Linha/máquina e operador.

## 10. Definições

Gerir reparadores e associações por:

- tipo BQ/CM/MF;
- Linha/máquina permitida;
- ativo/inativo.

Alterar uma associação não reescreve listas ou movimentos antigos. Cada envio guarda snapshot do reparador usado.

## 11. Ownership

- Reparação: plano, reparador, acompanhamento e ciclo externo;
- Armazém: posição e confirmação física de entrada/saída;
- domínio BQ/CM/MF: identidade, lote e características da ferramenta;
- Job On: contexto de produção, apenas quando existe relação explícita.

Nenhuma vista cria cópias divergentes das ferramentas.

## 12. Critérios de aceitação

- BQ, CM e MF estão separados dentro do mesmo módulo;
- saída programada criada na Reparação aparece no Armazém;
- checks parciais mostram progresso;
- concluir saída liberta posições confirmadas;
- retorno fecha o ciclo item a item;
- BQ usa quantidades; CM/MF usam números individuais;
- listas seguem clique/duplo clique e paginação canónica;
- histórico preserva saída, entrada e operadores;
- reparadores são filtrados pelo tipo e Linha/máquina;
- CM e MF nunca são combinados no domínio.
- Reparação externa não carrega a produção atualmente ativa;
- a lista parte de uma produção futura e mostra a data prevista de início;


## END FILE CONTENT

---

# FILE 018

## Source Path
`docs\REPARACAO_INTERNA_DESIGN_BRIEF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Reparação interna/turno — brief funcional V1

Estado: contrato funcional para design e implementação  
Âmbito: registo rápido de reparações de CM e MF durante a produção

## 1. Objetivo

Os reparadores de turno registam ferramentas CM e MF que vão saindo da produção para intervenção interna.

O fluxo exige apenas:

1. Linha;
2. Tipo `CM` ou `MF`;
3. número individual da ferramenta.

Referência, lote, produção e contexto vêm automaticamente do que está a trabalhar na Linha nesse dia.

## 2. Estrutura da página

Tabs V1:

1. `Registo`
2. `Histórico`

## 3. Tab Registo

### Seleção da produção por Linha

Usar cartões compactos B1, B2, B3, C1, C2 e C3. Cada cartão mostra:

- a Linha em primeiro nível;
- a Referência completa atualmente associada a essa Linha;
- `Sem Job On ativo` quando não existe contexto utilizável.

A Referência é uma variável read-only obtida do Job On ativo. Não é escrita, deduzida ou mantida localmente pela Reparação interna.

Ao escolher um cartão, consultar o Job On/produção ativa nessa Linha para a data/hora do registo. O cartão é mais pequeno que um botão de ação normal porque funciona como seletor de contexto.

#### Regra obrigatória de layout

A implementação antiga com seis botões numa única fila está rejeitada: provoca overflow e coloca C2/C3 por baixo do painel de contexto.

O seletor ocupa um cartão horizontal com toda a largura útil no topo. O painel de contexto e registo ocupa outra linha, imediatamente por baixo. Estes dois cartões nunca aparecem lado a lado.

Em desktop, apresentar as seis Linhas numa fila. Em larguras intermédias, usar três colunas; em mobile, duas. Como o seletor usa toda a largura, os seis cartões deixam de ficar comprimidos ou escondidos.

```css
.flow {
  grid-template-columns: minmax(0, 1fr);
}
.flow > * {
  min-width: 0;
}
.line-choice {
  display: grid;
  grid-template-columns: repeat(6, minmax(0, 1fr));
  gap: 8px;
}
.line-card {
  width: 100%;
  min-width: 0;
}
```

Não colocar o seletor num painel lateral estreito, não aumentar a largura da página para esconder o problema e não aplicar scroll horizontal. Nos breakpoints, reduzir a grelha para três e depois duas colunas; os cartões nunca podem invadir outro painel.

### Contexto automático

Mostrar cartão read-only:

| Produção | Referência | Job On | Data/contexto |
|---|---|---|---|

A Linha não é repetida como campo isolado neste resumo: já identifica o cartão selecionado e aparece junto da Referência ativa. Junto aos botões `CM`/`MF` e `Rever registo`, manter o identificador compacto `Referência ativa do Job On`. A Referência é o valor dominante; Produção e Linha aparecem como apoio. O bloco é read-only e atualiza sempre que muda o cartão. A repetição da Referência é intencional: mantém o contexto crítico visível no ponto exato da ação.

Regras:

- usar relações e IDs registados;
- não deduzir produção pelo código da Referência;
- não selecionar automaticamente quando existir mais de um contexto;
- não criar um Job On ausente;
- uma consulta não altera dados.

## 4. Registar a ferramenta

Depois de carregar o contexto:

1. escolher `CM` ou `MF`;
2. introduzir o número individual;
3. confirmar `Registar reparação`.

O número individual é validado no domínio do tipo e lote associados à produção.

Não voltar a pedir Referência, lote, produção, Linha, operador ou data/hora. Operador autenticado e data/hora são capturados automaticamente.

## 5. Guardar

Antes de guardar, mostrar Linha, Produção/Job On, Referência/lote, Tipo e número individual.

Ao confirmar:

1. validar contexto e ferramenta;
2. criar o registo de Reparação interna;
3. associar IDs estáveis da ferramenta/lote, Job On e produção;
4. guardar operador e data/hora;
5. mostrar sucesso apenas após persistência;
6. limpar o número individual e devolver-lhe o foco.

Linha e Tipo podem permanecer selecionados para registos consecutivos. Mudar de Linha recarrega obrigatoriamente o contexto.

O registo não altera automaticamente posição no Armazém, vida útil, estado técnico, Job On ou dados mestres.

## 6. Estados excecionais

### Sem produção ativa

O cartão da Linha mostra `Sem Job On ativo`. Ao selecioná-lo, mostrar `Não existe produção/Job On ativo para esta Linha e data` e impedir guardar.

### Mais de um contexto

Mostrar resultados com Produção, Referência e período. Exigir escolha explícita.

### Ferramenta não encontrada

Mostrar `Número individual não encontrado para o CM/MF associado a esta produção`.

Não criar automaticamente a ferramenta nem associar outro lote.

### Tipo errado

Se o número existir noutro tipo, informar sem mudar automaticamente de CM para MF ou vice-versa.

### Falha ao guardar

Manter Linha, Tipo e número introduzido; não limpar nem mostrar sucesso.

## 7. Tab Histórico

### Filtros

- intervalo de datas;
- Linha;
- Produção/Job On;
- Referência/lote;
- Tipo CM/MF;
- número individual;
- operador;
- apenas corrigidos.

### Lista

| Data/hora | Linha | Produção | Referência | Lote | Tipo | N.º individual | Operador | Estado |
|---|---|---|---|---|---|---|---|---|

- um clique seleciona;
- duplo clique abre o detalhe;
- `Corrigir registo` fica fora da tabela, na mesma barra da paginação e imediatamente antes das setas;
- `Corrigir registo` usa o botão standard do design system: altura mínima de `36px` e padding `7px 12px`; não criar uma variante maior para esta ação;
- filtros não selecionam automaticamente;
- correções usam indicador textual `Corrigido`.

## 8. Detalhe

Mostra todos os valores, contexto relacionado, operador/data original e histórico de correções. Abrir é apenas consulta.

## 9. Corrigir um engano

O utilizador autorizado seleciona uma linha e escolhe `Corrigir registo`.

O cartão inline apresenta os valores atuais e permite corrigir:

- Linha/contexto associado;
- Tipo CM/MF;
- número individual.

Data/hora e operador originais permanecem read-only.

Ao alterar a Linha, resolver novamente o contexto para a data/hora original. Se não existir resultado inequívoco, não guardar por aproximação.

## 10. Auditoria

Uma correção guarda:

- registo afetado;
- valores anteriores;
- valores novos;
- utilizador que corrigiu;
- data/hora da correção.

O original nunca desaparece. A lista apresenta a versão válida mais recente e o detalhe toda a sequência. Corrigir não altera Job On, Armazém ou ferramenta de origem. Eliminar não faz parte da V1.

## 11. Integração do histórico

O mesmo registo pode ser consultado no Histórico da Reparação interna, na ficha da ferramenta e no Job On histórico quando existir integração. As vistas referenciam o mesmo ID; não criam cópias divergentes.

## 12. Permissões

- registar: Reparador de turno autorizado;
- consultar Histórico: segundo template de acesso;
- corrigir: capacidade específica autorizada.

Autorizações são validadas no comando. Nome/título do header vem do perfil gerido na Administração.

## 13. Questões por confirmar

- formato/intervalo do número individual de CM e MF;
- se futuramente exige observação ou motivo;
- quem pode corrigir e se existe limite temporal;
- se pode existir mais de um lote do mesmo tipo ativo na Linha;
- momento exato em que um Job On é considerado ativo.

## 14. Critérios de aceitação V1

- cada cartão mostra Linha e Referência do Job On ativo;
- escolher o cartão carrega a produção ativa do dia;
- uma Linha sem Job On ativo não permite avançar;
- utilizador só escolhe CM/MF e número individual;
- Referência, lote, produção e operador não são reintroduzidos;
- nenhuma ambiguidade é resolvida automaticamente;
- guardar captura operador/data;
- sucesso limpa apenas o número;
- falha preserva dados;
- Histórico filtra por contexto, ferramenta e operador;
- clique seleciona e duplo clique abre;
- Correção preserva original, alterações, autor e data/hora;
- nenhuma correção altera silenciosamente outros domínios.


## END FILE CONTENT

---

# FILE 019

## Source Path
`docs\TAMPOES_DESIGN_BRIEF.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Tampões — brief funcional V1

Estado: contrato funcional para design e implementação  
Âmbito: consulta, movimentos de quantidade, transformação técnica e planeamento de tampões

## 1. Objetivo

O módulo permite ao operador consultar rapidamente, sobretudo no telemóvel, quantos tampões existem para uma configuração técnica e planear a necessidade antes da produção.

Exemplo de contexto: `Ø 28,95 mm · Calote 4 mm`, com `28 enchidos` e `5 por encher`.

Na V1, Tampões é um controlo agregado de quantidades. Não existem números individuais por tampão. Uma parte da quantidade pode ser alterada de uma configuração técnica para outra sem apagar a configuração ou o histórico de origem.

## 2. Modelo funcional confirmado

A entidade consultada é uma configuração técnica estável, identificada por ID próprio. As características comparáveis são configuradas nas opções do módulo e incluem inicialmente:

- diâmetro, em milímetros;
- profundidade/calote, em milímetros.

As características são campos numéricos comparáveis. A aplicação deve guardar a definição do campo separada dos seus valores: nome, unidade, precisão e estado ativo. Não codificar `Diâmetro` ou `Calote` numa string composta nem criar colunas novas por cada configuração.

Cada configuração tem dois saldos confirmados:

- `Enchidos`;
- `Por encher`.

O enchimento com Colmonoy e a maquinação fazem parte do processo real, mas `Maquinado` não é criado como terceiro estado na V1 sem confirmação funcional.

## 3. Estrutura do módulo

Tabs V1:

1. `Registo`
2. `Consulta`
3. `Planeamento`
4. `Histórico`

`Opções` fica alinhado à direita. O Operador tem acesso total e pode criar, editar, ordenar, ativar ou desativar os campos técnicos comparáveis.

O header segue a shell global. Nome e título/função vêm do perfil gerido na Administração.

## 4. Consulta mobile-first

No topo, mostrar filtros compactos para as características ativas, inicialmente:

- `Diâmetro (mm)`;
- `Profundidade/Calote (mm)`.

Diâmetro e Calote não são introduzidos como texto livre no Registo, Consulta, Planeamento ou transformação. Os valores possíveis são preenchidos e mantidos numa tabela em `Opções`; depois aparecem como dropdowns em todas as páginas operacionais. Esta normalização impede variantes equivalentes como `4`, `4.0` e `4,00`.

Os resultados aparecem numa lista canónica. Um clique seleciona; duplo clique abre o detalhe da configuração. Não existe botão `Abrir`.

O cartão selecionado apresenta de forma imediata:

| Configuração | Enchidos | Por encher |
|---|---:|---:|
| Ø 28,95 · Calote 4 mm | 28 | 5 |

No telemóvel, os dois saldos usam blocos lado a lado e algarismos dominantes. A página não pode criar scroll horizontal.

## 5. Registar quantidades

As ações `Adicionar quantidade` e `Remover quantidade` ficam fora da lista e abrem o mesmo cartão inline.

Campos:

1. configuração selecionada, read-only;
2. saldo: `Enchidos` ou `Por encher`;
3. quantidade, inteiro positivo e campo curto.

Regras:

- adicionar incrementa apenas o saldo escolhido;
- remover decrementa apenas o saldo escolhido;
- nunca permitir saldo negativo;
- consulta, seleção e abertura não alteram quantidades;
- o novo saldo só aparece depois da confirmação persistida pelo servidor;
- falha ao guardar preserva os valores introduzidos e não mostra sucesso.

Não adicionar campos de descrição técnica, vida, estado de sucata ou arquivo neste módulo. Esses dados pertencem ao domínio mestre da ferramenta quando existirem.

### Alterar estado

O botão `Alterar estado` abre um cartão inline com um seletor:

- `Enchidos`;
- `Por encher`.

O Operador escolhe o novo estado e a quantidade. O sistema determina o saldo de origem como o estado oposto e apresenta a transferência antes de confirmar.

Exemplo: selecionar `Enchidos` e quantidade `5` retira 5 de `Por encher` e adiciona 5 a `Enchidos`.

A transferência é atómica e cria um único movimento `Alterar estado`, com origem, destino, quantidade, saldos anteriores/novos, operador e data/hora. Não implementar como dois movimentos independentes. Impedir a operação quando a origem não tiver quantidade suficiente.

## 6. Alterar configuração

`Alterar configuração` transforma uma quantidade existente. Não edita a configuração original nem altera todos os tampões desse saldo.

Exemplo confirmado:

1. origem: `Ø 28,95 mm · Calote 4 mm`;
2. quantidade: `25`;
3. destino: `Ø 28,95 mm · Calote 7 mm`;
4. confirmar retira 25 unidades da origem e adiciona 25 unidades ao destino;
5. a aplicação passa a contar essas 25 unidades na configuração de 7 mm.

O cartão inline apresenta:

- configuração e saldo de origem, read-only;
- quantidade a transformar;
- características atuais e novos valores lado a lado;
- pré-visualização das diferenças;
- configuração de destino encontrada ou indicação de que será criada;
- saldos previstos antes da confirmação.

Regras:

- a quantidade é inteira, positiva e não pode exceder o saldo de origem;
- pelo menos uma característica tem de mudar;
- características não alteradas mantêm o valor de origem;
- origem e destino usam IDs diferentes, mesmo que os valores sejam visualmente semelhantes;
- se já existir uma configuração com os valores de destino, reutilizar o seu ID;
- se não existir, criar uma nova configuração apenas após validação e confirmação;
- a operação de retirar da origem e adicionar ao destino é atómica: ou ambas persistem ou nenhuma persiste;
- nunca implementar esta operação como edição direta de `4 mm` para `7 mm`;
- falha mantém o formulário e os saldos originais.

O movimento de transformação guarda origem, destino, quantidade, valores anteriores/novos, saldos antes/depois, operador e data/hora.

## 7. Opções e campos comparáveis

O Operador tem liberdade total para criar e gerir os campos usados para descrever e comparar configurações. Também pode criar uma configuração nova diretamente ou durante uma transformação.

Para cada campo guardar:

- nome visível;
- unidade;
- número máximo de casas decimais;
- ordem de apresentação;
- ativo/inativo.

Para cada valor disponível guardar:

- campo a que pertence (`Diâmetro` ou `Calote`);
- valor numérico normalizado;
- unidade herdada/apresentada;
- ordem;
- ativo/inativo.

`Opções` apresenta a tabela `Valores disponíveis`. `Adicionar valor` cria uma opção nova; desativar remove-a dos dropdowns para novos registos, mas não elimina configurações nem histórico existentes. Os dropdowns devem carregar apenas valores ativos, ordenados numericamente.

Na V1, os campos são numéricos. Desativar um campo não elimina valores nem histórico. Alterar nome, unidade ou precisão não pode reinterpretar silenciosamente valores já guardados; uma mudança incompatível exige migração explícita.

Os campos ativos aparecem de forma consistente na Consulta, na alteração de configuração e nos filtros. Esta gestão não fica reservada ao Administrador ou ao Responsável.

A liberdade operacional não elimina as regras de integridade: todas as alterações guardam operador e data/hora, nunca apagam movimentos anteriores e não podem produzir saldos negativos ou transferências parciais.

## 8. Planeamento

`Planear` cria uma necessidade prevista; não adiciona, remove nem reserva stock físico.

Cartão inline mínimo:

- configuração selecionada;
- quantidade necessária;
- data prevista;
- produção/Job On, apenas quando existir relação inequívoca no sistema.

Depois de guardar, mostrar:

- quantidade necessária;
- saldos atuais `Enchidos` e `Por encher`;
- diferença entre a necessidade e os `Enchidos` disponíveis.

A diferença é informativa. A V1 não deduz automaticamente quantidades nem converte tampões `Por encher` em `Enchidos`.

## 9. Histórico de movimentos

Cada alteração de quantidade cria um movimento imutável com:

| Data/hora | Origem/configuração | Destino | Movimento | Saldo | Quantidade | Antes | Depois | Operador |
|---|---|---|---|---|---:|---:|---:|---|

`Movimento` é `Adicionar`, `Remover`, `Alterar estado` ou `Alterar configuração`. `Destino` é preenchido nas transferências de estado e de configuração. `Saldo` é `Enchidos` ou `Por encher`.

Filtros:

- intervalo de datas;
- diâmetro;
- calote;
- movimento;
- saldo;
- valor anterior/novo das características configuradas;
- operador.

Comportamento canónico:

- um clique seleciona;
- duplo clique abre o detalhe;
- `Corrigir movimento` fica fora da lista;
- filtros não selecionam automaticamente uma linha.

Uma correção preserva movimento original, valores anteriores, valores novos, autor, data/hora e justificação. Não existe edição silenciosa do saldo.

## 10. Histórico de planeamento

O planeamento mantém registo separado dos movimentos físicos. Deve ser possível consultar necessidade, data prevista, produção/Job On associado, autor e estado do plano.

Cancelar ou alterar um plano não altera os saldos. O conjunto exato de estados do plano depende da integração futura com Job On e deve ser confirmado antes da implementação.

## 11. Estados e mensagens

- sem configuração: `Selecione o diâmetro e a calote.`
- sem resultado: `Não foi encontrada uma configuração com estes valores.`
- resultado ambíguo: apresentar a lista e exigir seleção explícita;
- quantidade insuficiente: impedir remoção e indicar saldo disponível;
- destino igual à origem: impedir confirmação e indicar que nenhuma característica mudou;
- transformação concorrente: recarregar saldos e exigir nova confirmação;
- sem movimentos: estado vazio, não erro;
- falha de carregamento: mensagem de erro com ação `Tentar novamente`.

## 12. Regras visuais e mobile

- aplicar tokens do `DMO_DESIGN_SYSTEM.md`;
- botões preenchidos em repouso e invertidos no hover/foco;
- alvos táteis com pelo menos 44 × 44 px;
- campos de diâmetro, calote e quantidade dimensionados para valores curtos;
- teclado numérico em dispositivos móveis;
- no máximo duas casas decimais na apresentação de diâmetro e calote;
- quantidades são inteiros;
- manter a configuração ativa visível enquanto o operador adiciona, remove ou planeia.
- na transformação, apresentar `Atual` e `Novo` em colunas comparáveis; no telemóvel, empilhar por característica sem perder os rótulos.

## 13. Integrações e ownership

- Tampões é autoridade dos seus saldos e movimentos agregados;
- definições de características, configurações e movimentos são entidades distintas;
- uma transformação transfere quantidade entre configurações e não reescreve dados históricos;
- o Operador pode gerir campos, configurações, movimentos e planeamento dentro do próprio módulo;
- Job On pode referenciar configuração, necessidade e saldo consultado, mas não altera stock por simples abertura ou planeamento;
- produção e máquina só são preenchidas a partir de relações existentes;
- não deduzir IDs a partir do texto `28,95 / 4`;
- operador autenticado e data/hora vêm do servidor.

## 14. Questões por confirmar

1. `Enchido` significa já pronto para produção ou ainda pode faltar maquinação?
2. A maquinação precisa de um saldo próprio (`Maquinados`) ou apenas de histórico de processo?
3. O planeamento deverá reservar stock numa versão posterior?
4. Quais são os limites e incrementos válidos de diâmetro e calote?
5. Onde é criada uma configuração técnica nova: neste módulo ou na ficha mestre da ferramenta?
6. Que estados finais deverá ter um plano (`Aberto`, `Cumprido`, `Cancelado`, outros)?
7. Além de diâmetro e profundidade/calote, que campos comparáveis entram inicialmente?

Até estas respostas existirem, não acrescentar estados nem automatismos por inferência.

## 15. Critérios de aceitação V1

- pesquisar por diâmetro e calote encontra a configuração correta;
- diâmetro e calote são selecionados em dropdowns alimentados pela tabela de valores disponíveis;
- a vista mobile mostra claramente `Enchidos` e `Por encher`;
- adicionar e remover alteram apenas o saldo escolhido;
- alterar estado transfere a quantidade entre `Por encher` e `Enchidos` num único movimento;
- transformar 25 unidades de calote 4 mm para 7 mm reduz 25 na origem e acrescenta 25 no destino;
- uma transformação nunca edita retroativamente a configuração de origem;
- origem e destino são atualizados na mesma transação;
- opções permitem definir campos numéricos comparáveis sem apagar histórico;
- o Operador consegue criar e gerir campos/configurações sem depender do Administrador;
- nenhum saldo pode ficar negativo;
- planear não altera nem reserva stock;
- cada movimento guarda saldos anterior/novo, operador e data/hora;
- clique seleciona e duplo clique abre;
- correção é auditável e não apaga o original;
- valores técnicos têm no máximo duas casas e quantidades são inteiros;
- `Maquinado` não existe como estado sem decisão funcional explícita.


## END FILE CONTENT

---

# FILE 020

## Source Path
`README.md`

## File Type
`.MD`

## BEGIN FILE CONTENT

# Portal DMO — pacote de design e handoff

## Páginas

- `login.html`: entrada única do Portal DMO.
- `admin.html`: gestão de utilizadores, aplicações e auditoria anual de ações por utilizador/módulo.
- `boquilhas.html`: mockup de Boquilhas.
- `peso-operador.html`: Novo controlo, Referências, Comparação, Histórico e configuração da pasta local de PDFs.
- `peso-responsavel.html`: aprovações de controlos e decisões individuais por CM.
- `pegamentos.html`: contexto obrigatório do Job On, ferramentas/lotes herdados, medições, histórico estruturado e PDF local.
- `armazem.html`: registo, consulta, saídas programadas e histórico do Armazém.
- `job-on.html`: entrada canónica que abre `job-on-v48-folha-producao.html`.
- `job-on-v48-folha-producao.html`: folha operacional com toda a informação necessária para produzir; abre em consulta não editável e, em edição, permite substituir ferramentas através de uma lista live que combina posição do Armazém com estado/% do domínio.
- `reparacao-interna.html`: registo rápido por Linha/CM/MF, últimos registos, Histórico e Correção.

## Base partilhada

- `dmo-design-system.css`: tokens e componentes visuais.
- `dmo-interactions.js`: comportamento canónico de listas e calendário.
- `logo_recolored(1).png`: identidade visual utilizada.

## Documentação

- `docs/DESIGN_IMPLEMENTATION_CONTRACT.md`: auditoria final do design para uma fresh build; avalia foundation, CSS architecture, componentes, shell, cobertura, contradições, gaps e critérios de aceitação sem criar regras de negócio.
- `docs/CODER_IMPLEMENTATION_HANDOFF.md`: ponto de entrada completo para o programador; consolida arquitetura funcional, módulos, fontes de dados, regras visuais, interações, auditoria e critérios de aceitação.
- `docs/DMO_DESIGN_SYSTEM.md`: especificação técnica de cores, componentes, estados, grelhas, interações e acessibilidade.
- `docs/MODULE_UI_HANDOFF_TEMPLATE.md`: template obrigatório para explicar o funcionamento da UI de cada módulo novo.
- `docs/DESIGN_INPUT_EXTRACTION.md`: padrões de UI extraídos das ideias funcionais, separados entre regras globais e candidatos por módulo.
- `docs/JOB_ON_DESIGN_BRIEF.md`: leitura dos exemplos Job On, reorganização proposta e questões ainda por confirmar.
- `docs/JOB_ON_DATA_MODEL.md`: contrato de persistência do snapshot integral e editável; separa BD Job On, ferramentas e Armazém e define a duplicação por revisão.
- `docs/JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`: configuração de verificações na ficha da ferramenta/lote, frequência Por fabrico, checks, reset e histórico no Job On.
- `docs/ARMAZEM_DESIGN_BRIEF.md`: contrato de UI de posições/movimentos, Consulta, alertas e Saídas programadas; regras de domínio e persistência ficam para os serviços responsáveis.
- `docs/REPARACAO_INTERNA_DESIGN_BRIEF.md`: registo rápido de CM/MF por Linha e produção ativa, Histórico e correções auditáveis.
- `docs/TAMPOES_DESIGN_BRIEF.md`: consulta mobile, campos técnicos comparáveis, transformação de quantidades entre configurações, planeamento e histórico.
- `docs/FERRAMENTAS_REGISTO_DESIGN_BRIEF.md`: criação da Referência mestre e primeiro lote, Nome técnico, desenho e duplicação controlada de novos lotes.
- `docs/PESO_INTERFACE_HANDOFF.md`: regras funcionais e técnicas de Peso/Controlo.
- `docs/BOQUILHAS_INTERFACE_BEHAVIOR.md`: regras funcionais e técnicas de Boquilhas.
- `docs/PORTAL_LOGIN_ADMIN_HANDOFF.md`: autenticação, perfil, título/função e gestão administrativa.
- `docs/AUDITORIA_GLOBAL_HANDOFF.md`: evento append-only por ação, associação a utilizador/módulo, consulta anual no Admin, filtros, permissões e critérios de aceitação.
- `docs/HANDOFF_INDEX.md`: índice de cobertura, ordem de integração e pontos ainda dependentes do programa real.
- `docs/PEGAMENTOS_INTERFACE_HANDOFF.md`: Job On obrigatório, ferramentas concretas herdadas, persistência no servidor e PDF local.

## Nota de implementação

Os HTML são mockups funcionais. Dados, fórmulas e atrasos simulados não são implementação de produção. Devem ser ligados aos serviços, permissões, perfis e motores de cálculo existentes.

Peso e Pegamentos guardam o histórico estruturado no servidor. Os PDFs enviados à Produção são gerados localmente no diretório principal configurado e na subpasta relativa definida ao criar o lote no Peso. O Job On é a fonte da Produção, Referência, Máquina e ferramentas/lotes concretos usados por ambos os módulos.

O Login encaminha todos os utilizadores autenticados para Job On. Todos podem consultar; apenas o papel/template técnico Responsável pode criar, duplicar, editar, guardar revisões e gerir Definições. Administração continua acessível ao Administrador através da navegação.

Todas as ações de negócio relevantes criam um evento de auditoria associado ao utilizador e ao módulo. O Admin pode consultar e exportar o registo anual autorizado; o sistema não atribui pontos nem produz rankings automáticos.


## END FILE CONTENT

---

# FILE 021

## Source Path
`admin.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Administração — Portal DMO
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 3
<footer>: 0
<form>: 1
UNIQUE IDS (16): users, newUser, userSearch, userState, userRows, apps, audit, auditRows, auditDetail, userModal, userModalTitle, userForm, editName, editEmail, editLabel, toast
DATA-* ATTRIBUTES (2): view, state
<button: 20
<input: 8
<select: 11
<textarea: 0
<table: 2
<a: 0
MODAL/DIALOG refs: modal-grid, dmo-modal-backdrop, userModal, dmo-modal, dmo-modal-head, userModalTitle, dmo-modal-body, dmo-modal-foot, modal, openModal, closeModal
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Administração — Portal DMO</title>
<link rel="stylesheet" href="dmo-design-system.css">
<style>
.admin-header{height:72px;background:#fff;border-bottom:1px solid var(--dmo-border);display:flex;align-items:center;padding:0 28px;gap:12px}.admin-logo{width:42px;height:42px;border-radius:50%}.admin-brand h1{margin:0;font-size:17px}.admin-brand p{margin:2px 0 0;color:var(--dmo-muted);font-size:11px}.admin-user{margin-left:auto;text-align:right}.admin-user strong{display:block;font-size:12px}.admin-user span{font-size:11px;color:var(--dmo-muted)}.admin-nav{height:50px;background:#fff;border-bottom:1px solid var(--dmo-border);display:flex;gap:25px;padding:0 28px}.admin-nav button{border:0;background:none;color:var(--dmo-muted);font-weight:700;border-bottom:3px solid transparent}.admin-nav button.active{color:var(--dmo-brand-600);border-color:var(--dmo-brand-600)}.admin-nav .back{margin-left:auto}.admin-main{max-width:1280px;margin:auto;padding:28px}.page-head{display:flex;justify-content:space-between;align-items:end;margin-bottom:18px}.page-head h2{margin:0;font-size:24px}.page-head p{margin:3px 0 0;color:var(--dmo-muted)}.admin-view{display:none}.admin-view.active{display:block}.toolbar{display:flex;align-items:end;gap:10px;padding:16px;margin-bottom:14px}.toolbar .search{flex:1}.table-card{padding:0;overflow:hidden}.table-title{padding:17px 18px;border-bottom:1px solid var(--dmo-border);display:flex;align-items:center;justify-content:space-between}.table-title h3{margin:0;font-size:15px}.row-actions{display:flex;gap:6px}.row-actions .dmo-button{min-height:30px;padding:5px 8px;font-size:11px}.modal-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}.span2{grid-column:1/-1}.helper{font-size:11px;color:var(--dmo-muted);margin:5px 0 0}.app-list{display:grid;gap:10px}.app-row{display:grid;grid-template-columns:1.4fr .7fr 80px 1fr auto;gap:12px;align-items:center;padding:13px 15px;background:#fff;border:1px solid var(--dmo-border);border-radius:10px}.app-name strong{display:block}.app-name span{font-size:11px;color:var(--dmo-muted)}.audit-filters{display:grid;grid-template-columns:110px minmax(150px,1fr) 135px 145px 125px 120px 120px 85px auto;gap:10px;align-items:end}.audit-detail{display:none;margin-top:14px;padding:16px}.audit-detail.open{display:block}.audit-detail-grid{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:10px}.audit-detail-grid div{padding:10px;border-radius:8px;background:var(--dmo-surface-soft)}.audit-detail-grid span{display:block;color:var(--dmo-muted);font-size:10px}.audit-detail-grid strong{font-size:12px}
@media(max-width:1100px){.audit-filters{grid-template-columns:repeat(4,minmax(0,1fr))}}
@media(max-width:900px){.app-row{grid-template-columns:1fr 1fr}.admin-main{padding:18px}.modal-grid{grid-template-columns:1fr}.span2{grid-column:auto}.audit-detail-grid{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media(max-width:640px){.audit-filters,.audit-detail-grid{grid-template-columns:1fr}.admin-nav{overflow-x:auto}.page-head{align-items:flex-start;gap:12px}}
</style>
</head>
<body>
<header class="admin-header">
<img class="admin-logo" src="logo_recolored(1).png" alt="BA">
<div class="admin-brand">
<h1>Portal DMO</h1>
<p>Administração</p>
</div>
<div class="admin-user">
<strong>João Silva</strong>
<span data-user-profile-title>Administrador</span>
</div>
</header>
<nav class="admin-nav">
<button class="active" data-view="users">Utilizadores</button>
<button data-view="apps">Aplicações</button>
<button data-view="audit">Auditoria</button>
<button class="back" onclick="location.href='job-on.html'">Voltar ao Job On</button>
</nav>
<main class="admin-main">
<section class="admin-view active" id="users">
<div class="page-head">
<div>
<h2>Utilizadores</h2>
<p>Gerir identidade, título/função do perfil, acessos e estado.</p>
</div>
<button class="dmo-button" id="newUser">Criar utilizador</button>
</div>
<div class="toolbar dmo-card">
<div class="dmo-field search">
<label for="userSearch">Pesquisar</label>
<input id="userSearch" placeholder="Nome, email ou texto do cabeçalho">
</div>
<div class="dmo-field">
<label>Estado</label>
<select id="userState">
<option value="all">Todos</option>
<option value="active">Ativos</option>
<option value="inactive">Inativos</option>
</select>
</div>
</div>
<div class="table-card dmo-card">
<div class="table-title">
<h3>Contas internas</h3>
<span class="dmo-pill">3 utilizadores</span>
</div>
<div class="dmo-table-wrap" style="border:0;border-radius:0">
<table class="dmo-table">
<thead>
<tr>
<th>Nome</th>
<th>Email</th>
<th>Título / função</th>
<th>Template</th>
<th>Estado</th>
<th>Último acesso</th>
<th>Ações</th>
</tr>
</thead>
<tbody id="userRows">
<tr data-row data-state="active">
<td>
<strong>João Silva</strong>
</td>
<td>joao.silva@empresa.pt</td>
<td>Chefe</td>
<td>Responsável</td>
<td>
<span class="dmo-pill active">Ativo</span>
</td>
<td>14/08/2026 · 10:42</td>
<td>
<div class="row-actions">
<button class="dmo-button edit-user">Editar</button>
<button class="dmo-button reset-password">Reset password</button>
</div>
</td>
</tr>
<tr data-row data-state="active">
<td>
<strong>Ana Martins</strong>
</td>
<td>ana.martins@empresa.pt</td>
<td>Engenheira</td>
<td>Administração</td>
<td>
<span class="dmo-pill active">Ativo</span>
</td>
<td>14/08/2026 · 09:16</td>
<td>
<div class="row-actions">
<button class="dmo-button edit-user">Editar</button>
<button class="dmo-button reset-password">Reset password</button>
</div>
</td>
</tr>
<tr data-row data-state="inactive">
<td>
<strong>Rui Costa</strong>
</td>
<td>rui.costa@empresa.pt</td>
<td>Operador</td>
<td>Operador</td>
<td>
<span class="dmo-pill inactive">Inativo</span>
</td>
<td>12/08/2026 · 16:03</td>
<td>
<div class="row-actions">
<button class="dmo-button edit-user">Editar</button>
<button class="dmo-button reset-password">Reset password</button>
</div>
</td>
</tr>
</tbody>
</table>
</div>
</div>
</section>
<section class="admin-view" id="apps">
<div class="page-head">
<div>
<h2>Aplicações</h2>
<p>Gerir disponibilidade, ordem e acesso aos módulos.</p>
</div>
</div>
<div class="app-list">
<div class="app-row">
<div class="app-name">
<strong>Boquilhas</strong>
<span>Gestão de lotes e reparações</span>
</div>
<span class="dmo-pill active">Ativa</span>
<div class="dmo-field">
<label>Ordem</label>
<input value="1" type="number">
</div>
<div class="dmo-field">
<label>Template</label>
<select>
<option>Operacional</option>
<option>Administração</option>
</select>
</div>
<button class="dmo-button">Editar</button>
</div>
<div class="app-row">
<div class="app-name">
<strong>Peso/Controlo</strong>
<span>Aprovação de peso e volume</span>
</div>
<span class="dmo-pill active">Ativa</span>
<div class="dmo-field">
<label>Ordem</label>
<input value="2" type="number">
</div>
<div class="dmo-field">
<label>Template</label>
<select>
<option>Operacional</option>
<option>Administração</option>
</select>
</div>
<button class="dmo-button">Editar</button>
</div>
</div>
</section>
<section class="admin-view" id="audit">
<div class="page-head"><div><h2>Auditoria de ações</h2><p>Histórico anual de ações executadas pelos utilizadores em todos os módulos.</p></div><button class="dmo-button">Exportar registo anual</button></div>
<div class="audit-filters dmo-card" style="padding:16px;margin-bottom:14px">
<div class="dmo-field"><label>Ano</label><select><option>2026</option><option>2025</option></select></div>
<div class="dmo-field"><label>Utilizador</label><select><option>Todos</option><option>João Silva</option><option>Ana Martins</option><option>Rui Costa</option></select></div>
<div class="dmo-field"><label>Módulo</label><select><option>Todos</option><option>Job On</option><option>Peso</option><option>Boquilhas</option><option>Armazém</option></select></div>
<div class="dmo-field"><label>Ação</label><select><option>Todas</option><option>Criação</option><option>Alteração</option><option>Confirmação</option><option>Correção</option><option>Envio</option></select></div>
<div class="dmo-field"><label>Resultado</label><select><option>Todos</option><option>Sucesso</option><option>Falha</option><option>Negado</option><option>Corrigida</option></select></div>
<div class="dmo-field"><label>Desde</label><input type="date"></div><div class="dmo-field"><label>Até</label><input type="date"></div>
<div class="dmo-field"><label>Linhas</label><select><option>20</option><option>40</option><option>60</option></select></div><button class="dmo-button">Limpar</button>
</div>
<div class="table-card dmo-card"><div class="table-title"><div><h3>Registo de 2026</h3><p class="helper">Um clique seleciona; duplo clique abre o detalhe da ação.</p></div><span class="dmo-pill">4 ações</span></div><div class="dmo-table-wrap" style="border:0;border-radius:0"><table class="dmo-table"><thead><tr><th>Data/hora</th><th>Utilizador</th><th>Módulo</th><th>Ação</th><th>Registo</th><th>Resultado</th></tr></thead><tbody id="auditRows"><tr data-audit-row class="selected"><td>18/08/2026 · 14:32</td><td>João Silva</td><td>Job On</td><td>Confirmar verificação</td><td>202603 · 5774T173</td><td><span class="dmo-pill active">Sucesso</span></td></tr><tr data-audit-row><td>18/08/2026 · 10:18</td><td>Ana Martins</td><td>Job On</td><td>Guardar revisão</td><td>202603 · Rev. 3</td><td><span class="dmo-pill active">Sucesso</span></td></tr><tr data-audit-row><td>17/08/2026 · 16:04</td><td>Rui Costa</td><td>Boquilhas</td><td>Registar entrada</td><td>T173 · Lote 24/33</td><td><span class="dmo-pill active">Sucesso</span></td></tr><tr data-audit-row><td>17/08/2026 · 15:51</td><td>Rui Costa</td><td>Armazém</td><td>Corrigir localização</td><td>CM 5447 · Lote 3</td><td><span class="dmo-pill">Corrigida</span></td></tr></tbody></table></div><div class="table-title"><span class="helper">4 ações · Página 1 de 1</span><div class="row-actions"><button class="dmo-button" disabled>‹</button><span class="helper">1 / 1</span><button class="dmo-button" disabled>›</button></div></div></div>
<div class="audit-detail dmo-card" id="auditDetail"><div class="table-title" style="padding:0 0 12px"><h3>Detalhe do evento selecionado</h3><span class="dmo-pill">Registo imutável</span></div><div class="audit-detail-grid"><div><span>Utilizador</span><strong>João Silva</strong></div><div><span>Módulo</span><strong>Job On</strong></div><div><span>Ação</span><strong>Confirmar verificação</strong></div><div><span>Resultado</span><strong>Sucesso</strong></div><div><span>Entidade</span><strong>Job On 202603</strong></div><div><span>Referência</span><strong>5774T173</strong></div><div><span>Data/hora</span><strong>18/08/2026 · 14:32</strong></div><div><span>Correlação</span><strong>evt-2026-0818-1432</strong></div></div></div>
</section>
</main>
<div class="dmo-modal-backdrop" id="userModal">
<div class="dmo-modal">
<div class="dmo-modal-head">
<h2 id="userModalTitle">Editar utilizador</h2>
<button class="dmo-button dmo-icon-button" data-close aria-label="Fechar">×</button>
</div>
<form id="userForm">
<div class="dmo-modal-body">
<div class="modal-grid">
<div class="dmo-field">
<label>Nome</label>
<input id="editName" required>
</div>
<div class="dmo-field">
<label>Email</label>
<input id="editEmail" type="email" required>
</div>
<div class="dmo-field span2">
<label>Título / função apresentado no cabeçalho</label>
<input id="editLabel" maxlength="40" placeholder="Ex.: Chefe, Engenheiro, Metrologia">
<p class="helper">Texto visual livre; não altera permissões.</p>
</div>
<div class="dmo-field">
<label>Template de acesso</label>
<select>
<option>Operador</option>
<option>Responsável</option>
<option>Administração</option>
</select>
</div>
<div class="dmo-field">
<label>Estado</label>
<select>
<option>Ativo</option>
<option>Inativo</option>
</select>
</div>
</div>
</div>
<div class="dmo-modal-foot">
<button class="dmo-button" type="button" data-close>Cancelar</button>
<button class="dmo-button">Guardar</button>
</div>
</form>
</div>
</div>
<div class="dmo-toast" id="toast">
</div>
<script>
const toast=document.querySelector('#toast'),showToast=t=>{toast.textContent=t;toast.classList.add('show');setTimeout(()=>toast.classList.remove('show'),2200)};document.querySelectorAll('.admin-nav [data-view]').forEach(b=>b.onclick=()=>{document.querySelectorAll('.admin-nav button,.admin-view').forEach(x=>x.classList.remove('active'));b.classList.add('active');document.querySelector('#'+b.dataset.view).classList.add('active')});const modal=document.querySelector('#userModal'),openModal=()=>modal.classList.add('open'),closeModal=()=>modal.classList.remove('open');document.querySelectorAll('[data-close]').forEach(b=>b.onclick=closeModal);document.querySelector('#newUser').onclick=()=>{document.querySelector('#userModalTitle').textContent='Criar utilizador';document.querySelector('#userForm').reset();openModal()};document.querySelectorAll('.edit-user').forEach(b=>b.onclick=()=>{const cells=b.closest('tr').cells;document.querySelector('#userModalTitle').textContent='Editar utilizador';document.querySelector('#editName').value=cells[0].innerText;document.querySelector('#editEmail').value=cells[1].innerText;document.querySelector('#editLabel').value=cells[2].innerText;openModal()});document.querySelectorAll('.reset-password').forEach(b=>b.onclick=()=>{const name=b.closest('tr').cells[0].innerText;if(confirm(`Iniciar reset de password para ${name}?`))showToast(`Reset iniciado para ${name}`)});document.querySelector('#userForm').onsubmit=e=>{e.preventDefault();closeModal();showToast('Utilizador guardado')};const search=document.querySelector('#userSearch'),state=document.querySelector('#userState'),filter=()=>document.querySelectorAll('#userRows tr').forEach(r=>{const q=search.value.toLowerCase();r.hidden=!r.innerText.toLowerCase().includes(q)||(state.value!=='all'&&r.dataset.state!==state.value)});search.oninput=filter;state.onchange=filter;
const auditRows=document.querySelector('#auditRows'),auditDetail=document.querySelector('#auditDetail');
const selectAuditRow=row=>{auditRows.querySelectorAll('[data-audit-row]').forEach(item=>item.classList.toggle('selected',item===row))};
const openAuditDetail=row=>{selectAuditRow(row);const cells=row.cells;auditDetail.querySelector('.audit-detail-grid').innerHTML=`<div><span>Utilizador</span><strong>${cells[1].textContent.trim()}</strong></div><div><span>Módulo</span><strong>${cells[2].textContent.trim()}</strong></div><div><span>Ação</span><strong>${cells[3].textContent.trim()}</strong></div><div><span>Resultado</span><strong>${cells[5].textContent.trim()}</strong></div><div><span>Registo associado</span><strong>${cells[4].textContent.trim()}</strong></div><div><span>Data/hora</span><strong>${cells[0].textContent.trim()}</strong></div><div><span>Ator</span><strong>Utilizador autenticado</strong></div><div><span>Evento</span><strong>Registo imutável</strong></div>`;auditDetail.classList.add('open');auditDetail.scrollIntoView({block:'nearest',behavior:'smooth'})};
if(auditRows){auditRows.addEventListener('click',event=>{const row=event.target.closest('[data-audit-row]');if(row)selectAuditRow(row)});auditRows.addEventListener('dblclick',event=>{const row=event.target.closest('[data-audit-row]');if(row)openAuditDetail(row)})}
</script>
</body>
</html>

```
## END FILE CONTENT

---

# FILE 022

## Source Path
`armazem.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Armazém — Portal DMO
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (1): dmo-interactions.js
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 4
<footer>: 0
<form>: 1
UNIQUE IDS (45): registo, movementPanel, movementHeading, closeMovement, movementForm, toolReference, toolLot, position, destination, movementNotes, toolContext, cancelMovement, registerEmpty, recentSearch, recentMovement, recentLimit, recentList, recent-1, recent-2, recent-3, recentCount, consulta, queryText, queryType, clearQuery, warehouseList, cm-9389-26, mf-5447-18, bq-t173-2433, cm-9121-11, correctLocation, programadas, printList, programList, sp-2026-0814-03, sp-2026-0814-04, sp-2026-0812-02, programState, confirmChecks, historico, calendarGrid, mov-1, mov-2, correctMovement, toast
DATA-* ATTRIBUTES (8): view, open-movement, mode, tool, id, kind, search, date
<button: 34
<input: 10
<select: 12
<textarea: 1
<table: 3
<a: 0
MODAL/DIALOG refs: --dmo-r-modal
CALENDAR refs: dmo-calendar__head, dmo-calendar__week, dmo-calendar__grid, dmo-calendar__day, calendar-layout, calendar, data-dmo-calendar, calendarGrid
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Armazém — Portal DMO</title>
<link rel="stylesheet" href="dmo-design-system.css">
<style>
:root{--dmo-brand-950:#0f1d2a;--dmo-brand-900:#193046;--dmo-brand-700:#315d88;--dmo-brand-600:#3c73a8;--dmo-brand-500:#568dc3;--dmo-brand-200:#bdd3e8;--dmo-brand-100:#d9e6f2;--dmo-brand-050:#e8eff7;--dmo-page:#f6f9fc;--dmo-card:#fff;--dmo-subtle:#f1f6fa;--dmo-border:#d9e6f2;--dmo-text:#172d42;--dmo-muted:#64778a;--dmo-success:#527c72;--dmo-success-soft:#e5f0eb;--dmo-warning:#a97943;--dmo-warning-soft:#f7f0e7;--dmo-danger:#9a625d;--dmo-danger-soft:#f3e9e7;--dmo-disabled:#cbd5df;--dmo-r-control:8px;--dmo-r-card:12px;--dmo-r-modal:16px;--dmo-shadow:0 8px 24px rgba(25,48,70,.06);--dmo-fast:150ms ease}
*{box-sizing:border-box}body{margin:0;background:var(--dmo-page);color:var(--dmo-text);font:14px/1.45 Inter,ui-sans-serif,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif}button,input,select,textarea{font:inherit}.dmo-card{background:#fff;border:1px solid var(--dmo-border);border-radius:var(--dmo-r-card);box-shadow:var(--dmo-shadow)}
.dmo-button{min-height:36px;padding:7px 12px;border:1px solid var(--dmo-brand-600);border-radius:var(--dmo-r-control);background:var(--dmo-brand-600);color:#fff;font-weight:700;cursor:pointer;transition:background var(--dmo-fast),color var(--dmo-fast)}.dmo-button:hover,.dmo-button:focus-visible{background:#fff;color:var(--dmo-brand-600);outline:none}.dmo-button:disabled{background:var(--dmo-disabled);border-color:var(--dmo-disabled);color:#fff;cursor:not-allowed}.dmo-icon-button{width:36px;padding:0;display:grid;place-items:center}
.dmo-field label{display:block;margin-bottom:6px;color:var(--dmo-muted);font-size:11px;font-weight:750}.dmo-field input,.dmo-field select,.dmo-field textarea{width:100%;min-height:40px;border:1px solid var(--dmo-border);border-radius:var(--dmo-r-control);background:#fff;color:var(--dmo-text);padding:9px 11px;outline:none}.dmo-field input:focus,.dmo-field select:focus,.dmo-field textarea:focus{border-color:var(--dmo-brand-600);box-shadow:0 0 0 3px rgba(60,115,168,.13)}
.dmo-pill{display:inline-flex;align-items:center;padding:4px 8px;border-radius:999px;background:var(--dmo-brand-050);color:var(--dmo-brand-700);font-size:10px;font-weight:800}.dmo-pill.pending{background:#e7eef5;color:#315d88}.dmo-pill.approved{background:var(--dmo-success-soft);color:var(--dmo-success)}
.dmo-table-wrap{overflow:auto;border:1px solid var(--dmo-border);border-radius:10px}.dmo-table{width:100%;border-collapse:collapse;white-space:nowrap}.dmo-table th{background:var(--dmo-subtle);color:var(--dmo-muted);font-size:10px;text-transform:uppercase;text-align:left;padding:11px 12px}.dmo-table td{padding:11px 12px;border-top:1px solid var(--dmo-border)}.dmo-table tr[data-dmo-row]{cursor:pointer}.dmo-table tr[data-dmo-row]:hover{background:var(--dmo-page)}[data-dmo-list] [data-dmo-row].selected{background:var(--dmo-brand-100)}
.dmo-app-header{min-height:76px;display:flex;align-items:center;gap:13px;padding:10px 28px;background:#fff;border-bottom:1px solid var(--dmo-border)}.dmo-app-header__logo{width:44px;height:44px;object-fit:contain;border-radius:50%}.dmo-app-header__page h1{margin:0;font-size:18px}.dmo-app-header__page p{margin:3px 0 0;color:var(--dmo-muted);font-size:11px}.dmo-app-header__user{margin-left:auto;padding-left:18px;text-align:right;border-left:1px solid var(--dmo-border)}.dmo-app-header__user strong,.dmo-app-header__user span{display:block}.dmo-app-header__user strong{font-size:12px}.dmo-app-header__user span{font-size:11px;color:var(--dmo-muted)}
.dmo-calendar__head{display:flex;align-items:center;justify-content:space-between}.dmo-calendar__week,.dmo-calendar__grid{display:grid;grid-template-columns:repeat(7,1fr);gap:5px}.dmo-calendar__week{margin-top:12px;color:var(--dmo-muted);font-size:10px;font-weight:800;text-align:center}.dmo-calendar__grid{margin-top:7px}.dmo-calendar__day{position:relative;min-height:38px;border:1px solid var(--dmo-border);border-radius:7px;background:#fff;color:var(--dmo-text);font-weight:700;cursor:pointer}.dmo-calendar__day:hover{border-color:var(--dmo-brand-600);background:var(--dmo-page)}.dmo-calendar__day.has-record:after{content:"";position:absolute;left:50%;bottom:4px;width:4px;height:4px;border-radius:50%;background:var(--dmo-warning)}.dmo-calendar__day.selected{background:var(--dmo-brand-600);border-color:var(--dmo-brand-600);color:#fff}.dmo-calendar__day:disabled{visibility:hidden}.dmo-toast{position:fixed;right:22px;bottom:22px;z-index:100;background:var(--dmo-brand-950);color:#fff;padding:11px 15px;border-radius:9px;opacity:0;transform:translateY(50px);transition:.2s}.dmo-toast.show{opacity:1;transform:none}
.shell{min-height:100vh}.tabs{height:52px;display:flex;gap:26px;padding:0 28px;background:#fff;border-bottom:1px solid var(--dmo-border);position:sticky;top:0;z-index:20}.tab{border:0;border-bottom:3px solid transparent;background:none;color:var(--dmo-muted);font-weight:750;cursor:pointer}.tab.active{color:var(--dmo-brand-600);border-color:var(--dmo-brand-600)}.main{max-width:1450px;margin:auto;padding:28px}.view{display:none}.view.active{display:block}.page-head{display:flex;align-items:end;justify-content:space-between;gap:16px;margin-bottom:18px}.page-head h2{margin:0;font-size:24px}.page-head p,.muted{margin:4px 0 0;color:var(--dmo-muted)}.toolbar{display:flex;gap:8px;flex-wrap:wrap}.panel{padding:18px;margin-bottom:16px}.panel-head{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:14px}.panel-head h3{margin:0;font-size:16px}.inline{display:none}.inline.open{display:block}.action-choice{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:16px}.action-choice button.active{background:#fff;color:var(--dmo-brand-600)}.type-choice{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}.type-choice button.active{background:#fff;color:var(--dmo-brand-600)}.form-grid{display:grid;grid-template-columns:150px minmax(220px,1.4fr) 130px minmax(180px,1fr);gap:12px;align-items:end}.form-grid .wide{grid-column:span 2}.form-grid .full{grid-column:1/-1}.context{display:grid;grid-template-columns:repeat(4,1fr);border:1px solid var(--dmo-brand-200);background:var(--dmo-brand-050);border-radius:10px;margin:14px 0;overflow:hidden}.context div{padding:11px;border-right:1px solid var(--dmo-brand-200)}.context div:last-child{border:0}.context span{display:block;color:var(--dmo-muted);font-size:9px;font-weight:800;text-transform:uppercase}.context strong{font-size:13px}.actions{display:flex;justify-content:flex-end;gap:8px;margin-top:14px;padding-top:14px;border-top:1px solid var(--dmo-border)}.filters{display:grid;grid-template-columns:2fr 120px 140px 140px 140px auto;gap:10px;align-items:end;background:var(--dmo-subtle);border:1px solid var(--dmo-border);border-radius:10px;padding:13px}.recent-filters{display:grid;grid-template-columns:minmax(240px,2fr) 150px 100px;gap:10px;align-items:end}.table-actions{display:flex;justify-content:flex-end;gap:8px;padding-top:12px}.list-footer{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:11px 2px 0;color:var(--dmo-muted);font-size:11px}.pager-controls{display:flex;align-items:center;gap:7px}.pager-controls .dmo-button{min-width:36px;padding:6px 9px}.dmo-table tr.selected td{background:var(--dmo-brand-100)}.status{font-size:10px;font-weight:800;border-radius:999px;padding:4px 8px;display:inline-flex}.warehouse{background:var(--dmo-success-soft);color:var(--dmo-success)}.production{background:var(--dmo-brand-050);color:var(--dmo-brand-700)}.repair{background:var(--dmo-warning-soft);color:var(--dmo-warning)}.conflict{background:var(--dmo-danger-soft);color:var(--dmo-danger)}.split{display:grid;grid-template-columns:360px minmax(0,1fr);gap:16px}.list-card{padding:0;overflow:hidden}.list-head{padding:16px 18px;border-bottom:1px solid var(--dmo-border)}.program-list{max-height:540px;overflow:auto}.program{padding:14px 18px;border-bottom:1px solid var(--dmo-border);cursor:pointer}.program:last-child{border:0}.program:hover{background:var(--dmo-page)}.program.selected{background:var(--dmo-brand-100)}.program-top{display:flex;justify-content:space-between;gap:10px}.program strong{font-size:13px}.program p{margin:4px 0 0;color:var(--dmo-muted);font-size:11px}.progress{height:5px;background:var(--dmo-brand-050);border-radius:99px;margin-top:10px;overflow:hidden}.progress i{display:block;height:100%;background:var(--dmo-brand-600)}.program-detail{padding:18px}.check-row{display:grid;grid-template-columns:32px 70px 1fr 100px 110px 1fr;gap:10px;align-items:center;padding:11px;border-bottom:1px solid var(--dmo-border)}.check-row.head{background:var(--dmo-subtle);color:var(--dmo-muted);font-size:9px;font-weight:800;text-transform:uppercase}.check-row input{width:18px;height:18px;accent-color:var(--dmo-brand-600)}.calendar-layout{display:grid;grid-template-columns:340px 1fr;gap:16px}.calendar{padding:18px}.history-table .in td{background:#f6faf8}.history-table .out td{background:#fbf8f4}.empty{padding:34px 20px;text-align:center;color:var(--dmo-muted);border:1px dashed var(--dmo-brand-200);border-radius:10px;margin-bottom:16px}.helper{font-size:11px;color:var(--dmo-muted);margin:8px 0 0}.selected-summary{display:none}.selected-summary.show{display:block}.footer-note{font-size:11px;color:var(--dmo-muted);margin-top:12px}.mobile-bar{display:none}
@media(max-width:1000px){.form-grid,.filters{grid-template-columns:repeat(2,1fr)}.form-grid .wide,.form-grid .full{grid-column:1/-1}.filters .dmo-button{width:max-content}.split,.calendar-layout{grid-template-columns:1fr}.context{grid-template-columns:repeat(2,1fr)}}
@media(max-width:650px){.dmo-app-header__page p{display:none}.tabs{padding:0 12px;gap:18px;overflow:auto}.tabs .tab{white-space:nowrap}.main{padding:18px 12px 84px}.form-grid,.filters,.recent-filters{grid-template-columns:1fr}.form-grid .wide,.form-grid .full{grid-column:auto}.context{grid-template-columns:1fr 1fr}.type-choice{grid-template-columns:repeat(3,1fr)}.dmo-table{min-width:850px}.check-row{grid-template-columns:30px 58px 1fr 85px}.check-row>*:nth-child(5),.check-row>*:nth-child(6){display:none}.mobile-bar{display:flex;position:fixed;z-index:30;bottom:0;left:0;right:0;padding:10px 12px;background:#fff;border-top:1px solid var(--dmo-border);gap:8px}.mobile-bar .dmo-button{flex:1}.page-head{align-items:start}.page-head .toolbar{display:none}.list-footer{align-items:flex-start;flex-direction:column}.pager-controls{align-self:flex-end}}
</style>
</head>
<body>
<div class="shell">
  <header class="dmo-app-header">
    <img class="dmo-app-header__logo" src="logo_recolored(1).png" alt="BA Glass">
    <div class="dmo-app-header__page"><h1>Armazém de Ferramentas</h1><p>Localizações, movimentos e saídas programadas</p></div>
    <div class="dmo-app-header__user"><strong>Ana Martins</strong><span>Operadora de Armazém</span></div>
  </header>
  <nav class="tabs" aria-label="Áreas do Armazém">
    <button class="tab active" data-view="registo">Registo</button>
    <button class="tab" data-view="consulta">Consulta</button>
    <button class="tab" data-view="programadas">Saídas programadas <span class="dmo-pill pending">2</span></button>
    <button class="tab" data-view="historico">Histórico</button>
  </nav>
  <main class="main">
    <section class="view active" id="registo">
      <div class="page-head"><div><h2>Registar movimento</h2><p>Selecione Entrada ou Saída para abrir os campos necessários.</p></div><div class="toolbar"><button class="dmo-button" data-open-movement="entrada">Entrada</button><button class="dmo-button" data-open-movement="saida">Saída</button></div></div>
      <div class="dmo-card panel inline" id="movementPanel">
        <div class="panel-head"><div><h3 id="movementHeading">Nova entrada</h3><p class="muted">Os dados técnicos apresentados vêm do módulo da ferramenta.</p></div><button class="dmo-button dmo-icon-button" id="closeMovement" aria-label="Fechar">×</button></div>
        <div class="action-choice"><button class="dmo-button active" data-mode="entrada">Entrada</button><button class="dmo-button" data-mode="saida">Saída</button></div>
        <div class="dmo-field"><label>Tipo de ferramenta</label><div class="type-choice"><button class="dmo-button active" data-tool="CM">CM</button><button class="dmo-button" data-tool="MF">MF</button><button class="dmo-button" data-tool="BQ">BQ</button></div></div>
        <form id="movementForm">
          <div class="form-grid" style="margin-top:14px">
            <div class="dmo-field"><label for="toolReference">Referência</label><input id="toolReference" required placeholder="Ex.: 9389T194"></div>
            <div class="dmo-field"><label for="toolLot">Lote ou número individual</label><input id="toolLot" required placeholder="Pesquisar no registo da ferramenta"></div>
            <div class="dmo-field entry-only"><label for="position">Posição</label><input id="position" inputmode="numeric" maxlength="4" placeholder="Ex.: 5126"></div>
            <div class="dmo-field exit-only" hidden><label for="destination">Destino</label><select id="destination"><option value="">Selecionar</option><option>Produção</option><option>Reparação</option></select></div>
            <div class="dmo-field full"><label for="movementNotes">Observações</label><textarea id="movementNotes" rows="2" placeholder="Opcional"></textarea></div>
          </div>
          <div class="context selected-summary" id="toolContext"><div><span>Ferramenta</span><strong>CM 9389 · Lote 26</strong></div><div><span>Referência</span><strong>9389T194</strong></div><div><span>Localização atual</span><strong>5126</strong></div><div><span>Contexto</span><strong>Armazém</strong></div></div>
          <p class="helper">O frontend envia o tipo, identificador selecionado, posição/destino e observações. A validação e persistência pertencem ao serviço do Armazém.</p>
          <div class="actions"><button type="button" class="dmo-button" id="cancelMovement">Cancelar</button><button class="dmo-button" type="submit">Confirmar movimento</button></div>
        </form>
      </div>
      <div class="empty" id="registerEmpty"><strong>Escolha Entrada ou Saída para começar.</strong><p>O formulário abre nesta página e mantém o contexto visível.</p></div>
      <div class="dmo-card panel">
        <div class="panel-head"><div><h3>Últimos registos</h3><p class="muted">Movimentos mais recentes do Armazém.</p></div></div>
        <div class="recent-filters"><div class="dmo-field"><label for="recentSearch">Referência, lote, número ou posição</label><input id="recentSearch" placeholder="Filtrar últimos registos"></div><div class="dmo-field"><label for="recentMovement">Movimento</label><select id="recentMovement"><option value="">Todos</option><option value="entrada">Entrada</option><option value="saida">Saída</option></select></div><div class="dmo-field"><label for="recentLimit">Linhas</label><select id="recentLimit"><option>20</option><option>40</option><option>60</option></select></div></div>
        <div class="dmo-table-wrap" style="margin-top:14px"><table class="dmo-table" data-dmo-list id="recentList"><thead><tr><th>Data/hora</th><th>Tipo</th><th>Referência</th><th>Lote / N.º</th><th>Movimento</th><th>Posição</th><th>Destino</th><th>Operador</th></tr></thead><tbody>
          <tr data-dmo-row data-id="recent-1" data-kind="entrada" data-search="cm 9389t194 26 5126 ana martins"><td>14/08 · 10:42</td><td>CM</td><td><strong>9389T194</strong></td><td>26</td><td><span class="status warehouse">Entrada</span></td><td>5126</td><td>—</td><td>Ana Martins</td></tr>
          <tr data-dmo-row data-id="recent-2" data-kind="saida" data-search="bq t173 24/33 3108 reparação joão silva"><td>14/08 · 09:16</td><td>BQ</td><td><strong>T173</strong></td><td>24/33</td><td><span class="status repair">Saída</span></td><td>3108</td><td>Reparação</td><td>João Silva</td></tr>
          <tr data-dmo-row data-id="recent-3" data-kind="entrada" data-search="mf 5447t173 18 2124 ana martins"><td>13/08 · 15:08</td><td>MF</td><td><strong>5447T173</strong></td><td>18</td><td><span class="status warehouse">Entrada</span></td><td>2124</td><td>—</td><td>Ana Martins</td></tr>
        </tbody></table></div>
        <div class="list-footer"><span id="recentCount">3 registos · Página 1 de 1</span><div class="pager-controls"><button class="dmo-button dmo-icon-button" disabled aria-label="Página anterior">‹</button><span>1 / 1</span><button class="dmo-button dmo-icon-button" disabled aria-label="Página seguinte">›</button></div></div>
      </div>
    </section>

    <section class="view" id="consulta">
      <div class="page-head"><div><h2>Consulta</h2><p>Localize ferramentas e identifique inconsistências.</p></div></div>
      <div class="dmo-card panel">
        <div class="filters"><div class="dmo-field"><label>Referência, lote, número ou posição</label><input id="queryText" placeholder="Pesquisar"></div><div class="dmo-field"><label>Tipo</label><select id="queryType"><option value="">Todos</option><option>CM</option><option>MF</option><option>BQ</option></select></div><div class="dmo-field"><label>Contexto</label><select><option>Todos</option><option>Armazém</option><option>Produção</option><option>Reparação</option><option>Não registado</option></select></div><div class="dmo-field"><label>Verificação</label><select><option>Todos</option><option>Por verificar</option><option>Sem indicação</option></select></div><div class="dmo-field"><label>Linhas</label><select><option>20</option><option>40</option><option>60</option></select></div><button class="dmo-button" id="clearQuery">Limpar filtros</button></div>
        <p class="helper">Um clique seleciona. Duplo clique abre a ficha associada no módulo responsável.</p>
        <div class="dmo-table-wrap" style="margin-top:14px"><table class="dmo-table" data-dmo-list id="warehouseList"><thead><tr><th>Tipo</th><th>Referência</th><th>Lote / N.º</th><th>Posição</th><th>Contexto</th><th>Último movimento</th><th>Alerta</th></tr></thead><tbody>
          <tr data-dmo-row data-id="cm-9389-26" data-search="cm 9389t194 26 5126"><td><strong>CM</strong></td><td>9389T194</td><td>26</td><td>5126</td><td><span class="status warehouse">Armazém</span></td><td>14/08 · Entrada</td><td>—</td></tr>
          <tr data-dmo-row data-id="mf-5447-18" data-search="mf 5447t173 18 2124"><td><strong>MF</strong></td><td>5447T173</td><td>18</td><td>2124</td><td><span class="status warehouse">Armazém</span></td><td>13/08 · Entrada</td><td>—</td></tr>
          <tr data-dmo-row data-id="bq-t173-2433" data-search="bq t173 24/33"><td><strong>BQ</strong></td><td>T173</td><td>24/33</td><td>—</td><td><span class="status repair">Reparação</span></td><td>12/08 · Saída</td><td>—</td></tr>
          <tr data-dmo-row data-id="cm-9121-11" data-search="cm 9121t173 11"><td><strong>CM</strong></td><td>9121T173</td><td>11</td><td>—</td><td><span class="status conflict">Não registado</span></td><td>—</td><td><span class="status conflict">Sem localização</span></td></tr>
        </tbody></table></div>
        <div class="list-footer"><span>4 registos · Página 1 de 1</span><div class="pager-controls"><button class="dmo-button dmo-icon-button" disabled aria-label="Página anterior">‹</button><span>1 / 1</span><button class="dmo-button dmo-icon-button" disabled aria-label="Página seguinte">›</button></div></div>
        <div class="table-actions"><button class="dmo-button" id="correctLocation" disabled>Corrigir localização</button></div>
      </div>
    </section>

    <section class="view" id="programadas">
      <div class="page-head"><div><h2>Saídas programadas</h2><p>Listas preparadas pela Reparação para execução no Armazém.</p></div><button class="dmo-button" id="printList">Imprimir lista</button></div>
      <div class="split">
        <div class="dmo-card list-card" data-dmo-list id="programList"><div class="list-head"><strong>Listas disponíveis</strong><p class="helper">Um clique seleciona; duplo clique abre o detalhe.</p></div><div class="program-list">
          <div class="program selected" data-dmo-row data-id="sp-2026-0814-03"><div class="program-top"><strong>SP-2026-0814-03</strong><span class="dmo-pill pending">Pendente</span></div><p>Reparador Externo A · 4 ferramentas</p><div class="progress"><i style="width:50%"></i></div></div>
          <div class="program" data-dmo-row data-id="sp-2026-0814-04"><div class="program-top"><strong>SP-2026-0814-04</strong><span class="dmo-pill pending">Pendente</span></div><p>Reparador Externo B · 3 ferramentas</p><div class="progress"><i style="width:0"></i></div></div>
          <div class="program" data-dmo-row data-id="sp-2026-0812-02"><div class="program-top"><strong>SP-2026-0812-02</strong><span class="dmo-pill approved">Em retorno</span></div><p>Reparador Externo A · 6 ferramentas</p><div class="progress"><i style="width:100%"></i></div></div>
        </div><div class="list-footer" style="padding:11px 16px"><span>3 listas · Página 1 de 1</span><div class="pager-controls"><select aria-label="Linhas por página"><option>20</option><option>40</option><option>60</option></select><button class="dmo-button dmo-icon-button" disabled aria-label="Página anterior">‹</button><button class="dmo-button dmo-icon-button" disabled aria-label="Página seguinte">›</button></div></div></div>
        <div class="dmo-card program-detail"><div class="panel-head"><div><h3>SP-2026-0814-03</h3><p class="muted">Externo A · criada por Rui Costa · 14/08/2026</p></div><span class="dmo-pill pending" id="programState">2 de 4 verificadas</span></div>
          <div class="check-row head"><span></span><span>Tipo</span><span>Ferramenta</span><span>Posição</span><span>Destino</span><span>Verificação</span></div>
          <label class="check-row"><input type="checkbox" checked><strong>CM</strong><span>9389T194 · Lote 26</span><span>5126</span><span>Reparação</span><span>Verificada</span></label>
          <label class="check-row"><input type="checkbox" checked><strong>MF</strong><span>5447T173 · Lote 18</span><span>2124</span><span>Reparação</span><span>Verificada</span></label>
          <label class="check-row"><input type="checkbox"><strong>BQ</strong><span>T173 · Lote 24/33</span><span>3108</span><span>Reparação</span><span>Pendente</span></label>
          <label class="check-row"><input type="checkbox"><strong>CM</strong><span>9121T173 · Lote 11</span><span>5130</span><span>Reparação</span><span>Pendente</span></label>
          <div class="actions"><button class="dmo-button" id="confirmChecks">Confirmar verificações</button></div>
          <p class="footer-note">O frontend envia os itens confirmados. O efeito nas posições e o fecho da lista são decididos e devolvidos pelo backend.</p>
        </div>
      </div>
    </section>

    <section class="view" id="historico">
      <div class="page-head"><div><h2>Histórico</h2><p>Consulta geral dos movimentos registados.</p></div><span class="dmo-pill">Agosto 2026</span></div>
      <div class="calendar-layout">
        <div class="dmo-card calendar" data-dmo-calendar><div class="dmo-calendar__head"><button class="dmo-button dmo-icon-button">‹</button><strong>Agosto 2026</strong><button class="dmo-button dmo-icon-button">›</button></div><div class="dmo-calendar__week"><span>SEG</span><span>TER</span><span>QUA</span><span>QUI</span><span>SEX</span><span>SÁB</span><span>DOM</span></div><div class="dmo-calendar__grid" id="calendarGrid"></div></div>
        <div class="dmo-card panel"><div class="filters" style="grid-template-columns:2fr 120px 140px 1fr 100px auto"><div class="dmo-field"><label>Referência, lote ou posição</label><input placeholder="Pesquisar movimentos"></div><div class="dmo-field"><label>Tipo</label><select><option>Todos</option><option>CM</option><option>MF</option><option>BQ</option></select></div><div class="dmo-field"><label>Movimento</label><select><option>Todos</option><option>Entrada</option><option>Saída</option><option>Correção</option></select></div><div class="dmo-field"><label>Operador</label><select><option>Todos</option><option>Ana Martins</option><option>João Silva</option></select></div><div class="dmo-field"><label>Linhas</label><select><option>20</option><option>40</option><option>60</option></select></div><button class="dmo-button">Limpar filtros</button></div>
          <div class="dmo-table-wrap history-table" style="margin-top:14px"><table class="dmo-table" data-dmo-list><thead><tr><th>Data/hora</th><th>Tipo</th><th>Referência</th><th>Lote / N.º</th><th>Movimento</th><th>Posição</th><th>Destino</th><th>Operador</th></tr></thead><tbody>
            <tr class="in" data-dmo-row data-id="mov-1"><td>14/08 · 10:42</td><td>CM</td><td>9389T194</td><td>26</td><td><strong>Entrada</strong></td><td>5126</td><td>—</td><td>Ana Martins</td></tr>
            <tr class="out" data-dmo-row data-id="mov-2"><td>14/08 · 09:16</td><td>BQ</td><td>T173</td><td>24/33</td><td><strong>Saída</strong></td><td>3108</td><td>Reparação</td><td>João Silva</td></tr>
          </tbody></table></div><div class="list-footer"><span>2 movimentos · Página 1 de 1</span><div class="pager-controls"><button class="dmo-button dmo-icon-button" disabled aria-label="Página anterior">‹</button><span>1 / 1</span><button class="dmo-button dmo-icon-button" disabled aria-label="Página seguinte">›</button></div></div><div class="table-actions"><button class="dmo-button" id="correctMovement" disabled>Corrigir movimento</button></div>
        </div>
      </div>
    </section>
  </main>
</div>
<div class="mobile-bar"><button class="dmo-button" data-open-movement="entrada">Entrada</button><button class="dmo-button" data-open-movement="saida">Saída</button></div>
<div class="dmo-toast" id="toast"></div>
<script src="dmo-interactions.js"></script>
<script>
const $=s=>document.querySelector(s),$$=s=>[...document.querySelectorAll(s)];
function toast(text){const t=$('#toast');t.textContent=text;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),2200)}
$$('.tab').forEach(tab=>tab.addEventListener('click',()=>{$$('.tab,.view').forEach(x=>x.classList.remove('active'));tab.classList.add('active');$('#'+tab.dataset.view).classList.add('active')}));
let movementMode='entrada';
function setMode(mode){movementMode=mode;$('#movementHeading').textContent=mode==='entrada'?'Nova entrada':'Nova saída';$$('[data-mode]').forEach(b=>b.classList.toggle('active',b.dataset.mode===mode));$$('.entry-only').forEach(x=>x.hidden=mode!=='entrada');$$('.exit-only').forEach(x=>x.hidden=mode!=='saida')}
function openMovement(mode){$('[data-view="registo"]').click();setMode(mode);$('#movementPanel').classList.add('open');$('#registerEmpty').hidden=true;$('#toolReference').focus()}
$$('[data-open-movement]').forEach(b=>b.onclick=()=>openMovement(b.dataset.openMovement));$$('[data-mode]').forEach(b=>b.onclick=()=>setMode(b.dataset.mode));
function closeMovement(){if($('#toolReference').value||$('#toolLot').value){if(!confirm('Fechar o registo e limpar os dados introduzidos?'))return}$('#movementForm').reset();$('#movementPanel').classList.remove('open');$('#registerEmpty').hidden=false;$('#toolContext').classList.remove('show')}
$('#closeMovement').onclick=$('#cancelMovement').onclick=closeMovement;
$$('[data-tool]').forEach(b=>b.onclick=()=>{$$('[data-tool]').forEach(x=>x.classList.remove('active'));b.classList.add('active')});
$('#toolLot').addEventListener('input',()=>$('#toolContext').classList.toggle('show',$('#toolLot').value.trim().length>1));
$('#movementForm').onsubmit=e=>{e.preventDefault();toast('Pedido de movimento preparado para o serviço do Armazém')};
document.addEventListener('dmo:list-select',e=>{const list=e.target;if(list.id==='warehouseList')$('#correctLocation').disabled=false;if(list.closest('#historico'))$('#correctMovement').disabled=false});
document.addEventListener('dmo:list-open',e=>{if(e.target.id==='warehouseList')toast('Abrir ficha no módulo responsável: '+e.detail.id);else if(e.target.id==='programList')toast('Lista programada aberta: '+e.detail.id);else toast('Abrir detalhe do movimento: '+e.detail.id)});
$('#correctLocation').onclick=()=>toast('Abrir cartão de correção da localização selecionada');$('#correctMovement').onclick=()=>toast('Abrir cartão de correção auditável');
function filterWarehouse(){const q=$('#queryText').value.toLowerCase(),type=$('#queryType').value;$$('#warehouseList tbody tr').forEach(r=>r.hidden=!r.dataset.search.includes(q)||(type&&r.children[0].innerText.trim()!==type))}$('#queryText').oninput=filterWarehouse;$('#queryType').onchange=filterWarehouse;$('#clearQuery').onclick=()=>{$('#queryText').value='';$('#queryType').value='';filterWarehouse()};
function filterRecent(){const q=$('#recentSearch').value.toLowerCase().trim(),kind=$('#recentMovement').value;let visible=0;$$('#recentList tbody tr').forEach(row=>{const show=row.dataset.search.includes(q)&&(!kind||row.dataset.kind===kind);row.hidden=!show;if(show)visible++});$('#recentCount').textContent=`${visible} registo(s) · Página 1 de 1`}$('#recentSearch').oninput=filterRecent;$('#recentMovement').onchange=filterRecent;$('#recentLimit').onchange=()=>{filterRecent();toast(`Limite alterado para ${$('#recentLimit').value} linhas por página`)};
const checks=$$('.check-row input');function updateChecks(){const done=checks.filter(x=>x.checked).length;$('#programState').textContent=`${done} de ${checks.length} verificadas`}checks.forEach(c=>c.onchange=updateChecks);$('#confirmChecks').onclick=()=>toast('Verificações enviadas; aguardar resultado do serviço');$('#printList').onclick=()=>toast('Pré-visualização de impressão da lista selecionada');
const grid=$('#calendarGrid');for(let i=0;i<5;i++)grid.insertAdjacentHTML('beforeend','<button class="dmo-calendar__day" disabled></button>');for(let d=1;d<=31;d++)grid.insertAdjacentHTML('beforeend',`<button class="dmo-calendar__day ${[3,7,12,14,18,25].includes(d)?'has-record':''} ${d===14?'selected':''}" data-date="2026-08-${String(d).padStart(2,'0')}">${d}</button>`);document.addEventListener('dmo:date-select',e=>toast('Histórico filtrado por '+e.detail.date));
</script>
</body>
</html>

```
## END FILE CONTENT

---

# FILE 023

## Source Path
`boquilhas.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Registo de Boquilhas — Responsável
EXTERNAL CSS (0): 
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 1
<main>: 1
<section>: 4
<footer>: 0
<form>: 2
UNIQUE IDS (38): registo, recordSearch, newLot, recordResults, selectT194, notFound, recordEmpty, inlineCreate, inlineLotForm, cancelInlineLot, recordDetail, closeLot, boquilhas, newLotBatches, lotSearch, batchStatus, lots, historico, calendar, clearHistoryDate, historyText, historyDateFilter, historyType, historyRepairer, historyState, clearFilters, movements, editMovement, deleteMovement, historyCount, definicoes, movementModal, movementTitle, movementForm, movementReason, movementDetail, movementWarning, toast
DATA-* ATTRIBUTES (10): view, bq, job, act, movement, state, search, text, type, repairer
<button: 61
<input: 18
<select: 13
<textarea: 2
<table: 0
<a: 0
MODAL/DIALOG refs: modal-backdrop, modal, modal-head, modal-foot, modal-body, movementModal, close-modal, openModal, closeModals
CALENDAR refs: calendar-head, calendar
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!DOCTYPE html>
<html lang="pt-PT">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Registo de Boquilhas — Responsável</title>
<style>
:root{--brand:#3c73a8;--brand-dark:#193046;--brand-deep:#0f1d2a;--brand-mid:#315d88;--brand-soft:#e8eff7;--bg:#f6f9fc;--line:#d9e6f2;--text:#172d42;--muted:#64778a;--white:#fff;--green:#527c72;--green-bg:#e5f0eb;--orange:#a97943;--orange-bg:#f7f0e7;--red:#9a625d;--red-bg:#f3e9e7;--shadow:0 8px 24px rgba(25,48,70,.07);--r:14px}
*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--text);font:14px/1.45 Inter,ui-sans-serif,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif}.app{min-height:100vh}.header{height:76px;position:fixed;z-index:30;inset:0 0 auto 0;background:rgba(255,255,255,.96);border-bottom:1px solid var(--line);display:flex;align-items:center;padding:0 30px;backdrop-filter:blur(12px)}.brand{display:flex;gap:12px;align-items:center}.logo{width:42px;height:42px;border-radius:50%;object-fit:cover}.brand h1{font-size:18px;margin:0;color:var(--brand-dark)}.brand p{margin:2px 0 0;color:var(--muted);font-size:12px}.user{margin-left:auto;text-align:right}.user strong{display:block;font-size:13px}.user span{font-size:12px;color:var(--muted)}
.tabs{position:fixed;z-index:29;top:76px;left:276px;right:0;height:52px;background:var(--white);border-bottom:1px solid var(--line);display:flex;gap:28px;padding:0 30px}.tab{appearance:none;border:0;background:none;color:var(--muted);font-weight:650;padding:0 2px;border-bottom:3px solid transparent;cursor:pointer}.tab.active{color:var(--brand);border-color:var(--brand)}
.sidebar{position:fixed;z-index:31;top:76px;bottom:0;left:0;width:276px;background:linear-gradient(180deg,var(--brand-dark),var(--brand-deep));padding:20px 16px;overflow:auto;color:#fff}.side-head{padding:0 4px 16px}.side-head h2{margin:0;font-size:12px;letter-spacing:.08em}.side-head p{margin:3px 0 0;color:#a6c3df;font-size:11.5px}.machine{position:relative;background:#ffffff0c;border:1px solid #ffffff13;border-radius:12px;padding:13px;margin-bottom:10px}.machine.production{background:#234463;border-color:#3c74a9}.machine.alert{border-color:#f0a13e;background:#4a3528;box-shadow:inset 3px 0 #f0a13e}.machine-top{display:flex;align-items:center;gap:8px}.machine-name{font-weight:800;font-size:15px}.line-alert{color:#ffd18a;font-size:10px;font-weight:800;margin-top:7px}.more{margin-left:auto;border:0;background:transparent;color:#d9e6f2;font-size:20px;line-height:1;cursor:pointer;padding:0 3px}.job-reference{display:block;margin-top:8px;padding:0;border:0;border-bottom:1px solid #8eb5d9;background:transparent;color:#fff;font:inherit;font-size:13px;font-weight:850;cursor:pointer}.job-reference:hover,.job-reference:focus-visible{color:#bcd8f1;border-color:#bcd8f1;outline:none}.machine-ref{font-weight:650;font-size:11.5px;margin-top:5px;color:#c5d9ec}.machine-ref.secondary{color:#ffd9a3;border-top:1px solid #ffffff20;padding-top:7px}.machine-empty{color:#98b9da;font-size:12px;margin-top:7px}.machine-meta{display:flex;justify-content:space-between;color:#aac6e1;font-size:10.5px;margin-top:5px}.menu{display:none;position:absolute;z-index:5;right:10px;top:38px;width:120px;background:#fff;border-radius:9px;padding:5px;box-shadow:0 12px 30px #0005;color:var(--text)}.menu.open{display:block}.menu button{width:100%;border:0;background:none;text-align:left;padding:8px;border-radius:6px;font-size:12px;cursor:pointer}.menu button:hover{background:var(--brand-soft)}.menu .danger{color:var(--red)}
.main{margin-left:276px;padding:152px 30px 40px;min-height:100vh}.view{display:none}.view.active{display:block}.page-head{display:flex;align-items:flex-end;justify-content:space-between;margin-bottom:20px}.page-head h2{font-size:24px;margin:0;color:var(--brand-dark)}.page-head p{margin:4px 0 0;color:var(--muted)}.date-chip{background:var(--brand-soft);color:var(--brand-mid);border-radius:9px;padding:8px 12px;font-weight:650;font-size:12px}.metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;margin-bottom:18px}.metric,.card{background:#fff;border:1px solid var(--line);border-radius:var(--r);box-shadow:var(--shadow)}.metric{padding:17px}.metric span{color:var(--muted);font-size:12px}.metric strong{display:block;font-size:25px;margin-top:3px;color:var(--brand-dark)}.metric small{color:var(--green)}.card{padding:20px}.card-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:15px}.card-head h3{margin:0;font-size:16px}.search{width:260px;border:1px solid var(--line);background:var(--bg);border-radius:9px;padding:9px 11px;color:var(--text)}.lots{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px}.lot{border:1px solid var(--line);border-radius:11px;padding:15px}.lot-top{display:flex;justify-content:space-between;gap:10px}.lot h4{margin:0;font-size:16px}.pill{font-size:10px;font-weight:750;padding:4px 8px;border-radius:99px}.pill.repair{background:var(--orange-bg);color:var(--orange)}.pill.ready{background:var(--green-bg);color:var(--green)}.lot-data{display:grid;grid-template-columns:repeat(3,1fr);margin-top:14px}.lot-data span{display:block;color:var(--muted);font-size:10px}.lot-data strong{font-size:12px}.progress{height:5px;background:var(--brand-soft);border-radius:99px;margin-top:13px;overflow:hidden}.progress i{display:block;height:100%;background:var(--brand);border-radius:99px}
.history-layout{display:grid;grid-template-columns:360px minmax(0,1fr);gap:18px}.calendar-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:15px}.icon-btn,.btn{border:1px solid var(--line);background:#fff;border-radius:9px;padding:8px 11px;cursor:pointer;color:var(--text);font-weight:650}.week,.days{display:grid;grid-template-columns:repeat(7,1fr);gap:5px}.week span{text-align:center;color:var(--muted);font-size:10px;font-weight:750;margin-bottom:4px}.day{aspect-ratio:1;border:0;background:transparent;border-radius:8px;cursor:pointer;color:var(--text);position:relative}.day:hover{background:var(--brand-soft)}.day.selected{background:var(--brand);color:#fff;font-weight:800}.day.has::after{content:"";position:absolute;width:4px;height:4px;background:var(--orange);border-radius:50%;bottom:5px;left:calc(50% - 2px)}.day.selected::after{background:#fff}.day.blank{pointer-events:none}.legend{display:flex;gap:14px;color:var(--muted);font-size:10.5px;margin-top:15px}.legend i{width:7px;height:7px;display:inline-block;border-radius:50%;background:var(--orange);margin-right:5px}.movement-card{padding:0;overflow:hidden}.movement-title{padding:19px 20px 14px;border-bottom:1px solid var(--line)}.movement-title h3{margin:0}.movement-title p{margin:3px 0 0;color:var(--muted);font-size:11.5px}.movement-list{max-height:440px;overflow:auto}.movement{width:100%;display:grid;grid-template-columns:1.25fr 1fr .7fr .7fr .8fr 1fr 1.2fr;gap:10px;text-align:left;border:0;border-bottom:1px solid var(--line);background:#fff;padding:13px 20px;cursor:pointer;color:var(--text);font-size:12px}.movement:hover{background:var(--bg)}.movement.selected{background:#d0dfee}.movement.head{position:sticky;top:0;z-index:2;background:var(--bg);color:var(--muted);font-size:9.5px;font-weight:750;text-transform:uppercase;cursor:default}.movement strong{color:var(--brand-dark)}.type{font-weight:700}.type.out{color:var(--orange)}.type.in{color:var(--green)}.list-actions{display:flex;justify-content:flex-end;gap:8px;padding:14px 20px;background:var(--bg)}.btn:disabled{opacity:.42;cursor:not-allowed}.btn.primary{background:var(--brand);border-color:var(--brand);color:#fff}.btn.danger{color:var(--red);background:var(--red-bg);border-color:#f0c5c1}.empty-state{text-align:center;padding:60px 20px;color:var(--muted)}
.toast{position:fixed;z-index:100;right:24px;bottom:24px;background:var(--brand-deep);color:#fff;padding:12px 16px;border-radius:10px;box-shadow:0 10px 30px #0003;transform:translateY(80px);opacity:0;transition:.2s}.toast.show{transform:translateY(0);opacity:1}
.register-search{display:flex;gap:10px;align-items:end}.register-search .field{flex:1}.field label{display:block;font-size:11px;font-weight:750;color:var(--muted);margin-bottom:6px}.field input,.field select,.field textarea{width:100%;border:1px solid var(--line);border-radius:9px;padding:10px 11px;background:#fff;color:var(--text);font:inherit;outline:none}.field input:focus,.field select:focus,.field textarea:focus{border-color:var(--brand);box-shadow:0 0 0 3px #3c73a820}.search-results{margin-top:10px;border:1px solid var(--line);border-radius:10px;overflow:hidden}.result{width:100%;border:0;background:#fff;padding:12px 14px;display:flex;align-items:center;gap:12px;text-align:left;cursor:pointer;color:var(--text)}.result:hover{background:var(--brand-soft)}.result .pill{margin-left:auto}.register-empty{margin-top:18px;border:1px dashed #b9d0e6;border-radius:var(--r);padding:42px;text-align:center;color:var(--muted)}.record{display:none;margin-top:18px}.record.open{display:block}.record-bar{display:flex;gap:8px;align-items:center;flex-wrap:wrap;margin-bottom:14px}.record-bar .push{margin-left:auto}.summary-line{font-size:13px;margin-bottom:14px}.stat-grid{display:grid;grid-template-columns:repeat(5,1fr);gap:10px}.stat{background:var(--bg);border-left:3px solid var(--brand);border-radius:9px;padding:11px}.stat span{display:block;color:var(--muted);font-size:10px}.stat strong{font-size:15px}.modal-backdrop{display:none;position:fixed;z-index:80;inset:0;background:#0f1d2aaa;padding:24px;align-items:center;justify-content:center}.modal-backdrop.open{display:flex}.modal{width:min(760px,100%);max-height:calc(100vh - 48px);overflow:auto;background:#fff;border-radius:16px;box-shadow:0 25px 70px #0005}.modal-head,.modal-foot{display:flex;align-items:center;padding:18px 20px;border-bottom:1px solid var(--line)}.modal-head h3{margin:0}.modal-head button{margin-left:auto}.modal-body{padding:20px}.modal-foot{border:0;border-top:1px solid var(--line);justify-content:flex-end;gap:8px}.destination{background:var(--bg);border:1px solid var(--line);border-radius:11px;padding:14px;margin-bottom:16px}.segmented{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:10px}.segmented button{border:1px solid var(--line);border-radius:9px;background:#fff;padding:11px;font-weight:750;color:var(--text);cursor:pointer}.segmented button.on{background:var(--brand);border-color:var(--brand);color:#fff}.form-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}.span2{grid-column:span 2}.span4{grid-column:1/-1}.checks{display:grid;grid-template-columns:repeat(6,1fr);gap:7px}.check{border:1px solid var(--line);border-radius:9px;padding:10px;background:var(--bg)}.context{display:grid;grid-template-columns:repeat(3,1fr);background:var(--brand-soft);border:1px solid #bdd3e8;border-radius:11px;margin-bottom:16px}.context div{padding:11px;border-right:1px solid #bdd3e8}.context div:last-child{border:0}.context span{display:block;font-size:9px;font-weight:800;color:var(--muted)}.context strong{font-size:14px}.warning{background:var(--orange-bg);border-left:4px solid var(--orange);color:#9c5917;border-radius:7px;padding:11px;margin-top:12px}.hidden{display:none!important}
.filter-bar{background:var(--bg);border:1px solid var(--line);border-radius:11px;padding:13px;margin-bottom:14px}.filter-grid{display:grid;grid-template-columns:2fr 1.15fr 1.25fr 1.25fr 1.15fr .7fr;gap:10px;align-items:end}.filter-grid.batches{grid-template-columns:2fr 1.2fr .7fr}.filter-actions{display:flex;justify-content:space-between;align-items:center;margin-top:10px}.filter-help{font-size:10.5px;color:var(--muted)}.summary-period{display:grid;grid-template-columns:repeat(4,1fr);gap:8px;margin-top:15px;padding-top:15px;border-top:1px solid var(--line)}.summary-period div{background:var(--bg);padding:9px;border-radius:8px}.summary-period span{display:block;color:var(--muted);font-size:9px}.summary-period strong{font-size:14px}.pager{display:flex;justify-content:space-between;align-items:center;padding:12px 20px;color:var(--muted);font-size:11px;border-top:1px solid var(--line)}
@media(max-width:1200px){.filter-grid{grid-template-columns:repeat(3,1fr)}}@media(max-width:980px){.sidebar{width:235px}.tabs{left:235px}.main{margin-left:235px}.metrics{grid-template-columns:1fr}.history-layout{grid-template-columns:1fr}.lots{grid-template-columns:1fr}.stat-grid{grid-template-columns:repeat(2,1fr)}}@media(max-width:720px){.header{padding:0 16px}.user{display:none}.sidebar{position:relative;top:128px;width:100%;height:auto;bottom:auto;display:grid;grid-template-columns:repeat(2,1fr);gap:8px}.side-head{grid-column:1/-1}.machine{margin:0}.tabs{left:0}.main{margin:0;padding:152px 14px 30px}.sidebar+.main{padding-top:155px}.movement{min-width:760px}.movement-list{overflow:auto}.page-head{align-items:flex-start;gap:10px}.date-chip{display:none}.filter-grid,.filter-grid.batches,.form-grid{grid-template-columns:1fr}.span2,.span4{grid-column:auto}.checks{grid-template-columns:repeat(2,1fr)}}
.history-layout{margin-bottom:18px}.history-summary{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;align-content:start}.history-summary .metric{padding:16px}.history-summary .metric strong{font-size:22px}.movement.wide{grid-template-columns:.9fr .7fr 1.25fr .55fr .6fr .9fr .6fr 1fr 1fr;min-width:930px}.movement-subtitle{font-size:15px!important;font-weight:750;color:var(--brand-mid)!important;margin-top:4px!important}.inline-create{display:none;margin-top:18px}.inline-create.open{display:block}.create-grid{display:grid;grid-template-columns:minmax(220px,2fr) 100px 115px 155px 100px;gap:12px;align-items:end}.create-grid .lines-field,.create-grid .notes-field{grid-column:1/-1}.create-grid .notes-field textarea{min-height:64px}.checks{gap:8px}.check.line-choice{position:relative;min-height:48px;display:flex;align-items:center;justify-content:center;padding:0;font-size:14px;font-weight:800;color:var(--brand-mid);cursor:pointer}.check.line-choice input{position:absolute;opacity:0}.check.line-choice:has(input:checked){background:var(--brand);border-color:var(--brand);color:#fff}.check.line-choice:has(input:checked)::after{content:'✓';position:absolute;right:7px;top:6px;width:17px;height:17px;border-radius:50%;background:#fff;color:var(--brand);display:grid;place-items:center;font-size:10px}.percent-wrap{position:relative}.percent-wrap input{padding-right:30px}.percent-wrap span{position:absolute;right:10px;top:50%;transform:translateY(-50%);font-weight:800;color:var(--muted)}
/* Mockup do sistema visual canónico */
.tabs .settings-tab{margin-left:auto}.btn,.icon-btn{background:var(--brand);border-color:var(--brand);color:#fff;padding:7px 11px;font-size:12px;transition:background .15s,border-color .15s,color .15s}.btn:hover,.icon-btn:hover{background:#568dc3;border-color:#568dc3}.btn:active,.icon-btn:active,.btn.is-selected{background:#fff;border-color:var(--brand);color:var(--brand)}.btn.danger{background:var(--red);border-color:var(--red);color:#fff}.btn.danger:hover{background:#d06a63;border-color:#d06a63}.btn.danger:active{background:#fff;border-color:var(--red);color:var(--red)}.btn:disabled{background:#cbd5df;border-color:#cbd5df;color:#fff}.day{border:1px solid transparent}.day:hover{background:#77a3ce;color:#fff}.day:active,.day.selected{background:#fff;color:var(--brand);border-color:var(--brand)}.list-actions{padding:10px 16px}.inline-create .list-actions{min-height:52px}.filter-bar{box-shadow:inset 0 1px #fff}.field input,.field select,.field textarea{min-height:38px}.card{box-shadow:0 8px 24px rgba(25,48,70,.055)}
.movement-form-grid{display:grid;grid-template-columns:155px 120px minmax(220px,1fr);gap:12px;align-items:end}.movement-form-grid .detail-field{grid-column:1/3}.movement-form-grid .notes-field{grid-column:3}.movement-form-grid textarea{min-height:42px;height:42px;resize:vertical}.movement-form-grid .field input,.movement-form-grid .field select{height:40px}.warning[hidden]{display:none}.modal.compact{width:min(720px,100%)}
@media(max-width:1050px){.create-grid{grid-template-columns:2fr 1fr 1fr}.create-grid .date-field{grid-column:span 2}}@media(max-width:720px){.create-grid,.movement-form-grid{grid-template-columns:1fr}.create-grid .date-field,.movement-form-grid .detail-field,.movement-form-grid .notes-field{grid-column:auto}}
.create-grid .date-field{order:5}.btn,.icon-btn{background:var(--brand);border:1px solid var(--brand);color:#fff}.btn:hover,.btn:focus-visible,.icon-btn:hover,.icon-btn:focus-visible{background:#fff;border-color:var(--brand);color:var(--brand);filter:none}.btn.danger{background:var(--red);border-color:var(--red);color:#fff}.btn.danger:hover,.btn.danger:focus-visible{background:#fff;border-color:var(--red);color:var(--red)}.btn:disabled,.btn:disabled:hover{background:#cbd5df;border-color:#cbd5df;color:#fff}.movement.wide{grid-template-columns:82px 72px 1.2fr 60px 65px .9fr 60px 1fr 1fr;min-width:850px}.inline-close{margin-left:auto}.settings-grid{display:grid;gap:10px}.setting-row{display:grid;grid-template-columns:70px minmax(170px,1fr) 2fr auto;gap:12px;align-items:center;padding:11px 12px;background:var(--bg);border:1px solid var(--line);border-radius:10px}.setting-line{font-weight:850;color:var(--brand-dark)}.repairer-tags{display:flex;flex-wrap:wrap;gap:6px}.repairer-tag{background:var(--brand-soft);color:var(--brand-mid);border-radius:999px;padding:5px 9px;font-size:11px;font-weight:700}.settings-section+.settings-section{margin-top:16px}.machine{cursor:pointer}.machine:hover{border-color:#72a0cd}.machine .menu{cursor:default}
.snapshot-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:10px}.snapshot-note{color:var(--muted);font-size:11px;margin:4px 0 0}.lot-movements{margin-top:14px;border:1px solid var(--line);border-radius:10px;overflow:hidden}.lot-movement{display:grid;grid-template-columns:110px 100px 90px 90px 1fr;gap:12px;padding:10px 12px;border-bottom:1px solid var(--line);font-size:12px}.lot-movement:last-child{border:0}.lot-movement.head{background:var(--bg);color:var(--muted);font-size:10px;font-weight:800;text-transform:uppercase}@media(max-width:1000px){.snapshot-grid{grid-template-columns:repeat(3,1fr)}}
.record>.card:first-of-type .snapshot-grid{grid-template-columns:repeat(5,1fr)}.record>.card:first-of-type .snapshot-grid .stat:last-child{display:none}
</style>
</head>
<body>
<div class="app">
  <header class="header"><div class="brand"><img class="logo" src="logo_recolored(1).png" alt="BA"><div><h1>Registo de Boquilhas</h1><p>Gestão de lotes, reparações e movimentos</p></div></div><div class="user"><strong>João Silva</strong><span>Responsável · Metrologia</span></div></header>
  <nav class="tabs" aria-label="Áreas da aplicação"><button class="tab active" data-view="registo">Registo</button><button class="tab" data-view="boquilhas">Boquilhas</button><button class="tab" data-view="historico">Histórico</button><button class="tab settings-tab" data-view="definicoes">⚙ Definições</button></nav>
  <aside class="sidebar" aria-label="Estado atual das linhas"><div class="side-head"><h2>LINHAS DE PRODUÇÃO</h2><p>Estado atual em máquina</p></div>
    <div class="machine production alert" data-bq="T173" title="Confirme qual referência está realmente montada nesta linha"><div class="machine-top"><span class="machine-name">B1 ⚠</span><button class="more" aria-label="Menu da linha B1">⋯</button></div><div class="line-alert">2 referências registadas</div><button class="job-reference" data-job="5447T173">5447T173</button><div class="machine-ref">BQ T173 · Lote 24/33 · 192 BQ</div><button class="job-reference" data-job="9121T173">9121T173</button><div class="machine-ref secondary">BQ T173 · Lote 25/08 · 96 BQ</div><div class="menu"><button data-act="replace">Substituir</button><button class="danger" data-act="remove">Remover</button></div></div>
    <div class="machine production" data-bq="T194"><div class="machine-top"><span class="machine-name">B2</span><button class="more" aria-label="Menu da linha B2">⋯</button></div><button class="job-reference" data-job="9389T194">9389T194</button><div class="machine-ref">BQ T194 · Lote 26</div><div class="machine-meta"><span>144 BQ</span><span>desde 09:15</span></div><div class="menu"><button data-act="replace">Substituir</button><button class="danger" data-act="remove">Remover</button></div></div>
    <div class="machine"><div class="machine-top"><span class="machine-name">B3</span><button class="more" aria-label="Menu da linha B3">⋯</button></div><div class="machine-empty">Sem referência atribuída</div><div class="menu"><button data-act="add">Adicionar</button></div></div>
    <div class="machine production" data-bq="T173"><div class="machine-top"><span class="machine-name">C1</span><button class="more" aria-label="Menu da linha C1">⋯</button></div><button class="job-reference" data-job="9121T173">9121T173</button><div class="machine-ref">BQ T173 · Lote 11</div><div class="machine-meta"><span>192 BQ</span><span>desde ontem</span></div><div class="menu"><button data-act="replace">Substituir</button><button class="danger" data-act="remove">Remover</button></div></div>
    <div class="machine"><div class="machine-top"><span class="machine-name">C2</span><button class="more" aria-label="Menu da linha C2">⋯</button></div><div class="machine-empty">Sem referência atribuída</div><div class="menu"><button data-act="add">Adicionar</button></div></div>
    <div class="machine production" data-bq="T173"><div class="machine-top"><span class="machine-name">C3</span><button class="more" aria-label="Menu da linha C3">⋯</button></div><button class="job-reference" data-job="5447T173">5447T173</button><div class="machine-ref">BQ T173 · Lote 24/33</div><div class="machine-meta"><span>240 BQ</span><span>desde 11:30</span></div><div class="menu"><button data-act="replace">Substituir</button><button class="danger" data-act="remove">Remover</button></div></div>
  </aside>
  <main class="main">
    <section id="registo" class="view active"><div class="page-head"><div><h2>Registo</h2><p>Selecione um lote existente ou crie um novo para começar.</p></div><span class="date-chip">Operação diária</span></div>
      <div class="card"><div class="register-search"><div class="field"><label for="recordSearch">Procurar boquilha ou lote</label><input id="recordSearch" placeholder="Ex.: T194, Y232 ou lote"></div><button class="btn primary" id="newLot">Criar novo lote</button></div><div class="search-results hidden" id="recordResults"><button class="result" id="selectT194"><strong>T194</strong><span>Lote 12 · Linha B1</span><span class="pill ready">EM PRODUÇÃO</span></button><div class="result hidden" id="notFound">Nenhuma boquilha encontrada.</div></div></div>
      <div class="register-empty" id="recordEmpty">Crie ou selecione um lote para ativar as ações do registo.</div>
      <div class="card inline-create" id="inlineCreate"><div class="card-head"><div><h3>Criar novo lote</h3><p style="margin:3px 0 0;color:var(--muted);font-size:12px">O lote fica imediatamente selecionado depois de criado.</p></div></div><form id="inlineLotForm"><div class="create-grid"><div class="field"><label>Boquilha</label><input required placeholder="Ex.: A123"></div><div class="field"><label>Lote</label><input required maxlength="3"></div><div class="field"><label>Total do lote</label><input required type="number" min="1" max="999"></div><div class="field date-field"><label>Data de abertura</label><input required type="date" value="2026-08-14"></div><div class="field"><label>Utilização</label><div class="percent-wrap"><input type="number" min="0" max="100" placeholder="0"><span>%</span></div></div><div class="field lines-field"><label>Linhas permitidas</label><div class="checks"><label class="check line-choice"><input type="checkbox" checked><span>B1</span></label><label class="check line-choice"><input type="checkbox"><span>B2</span></label><label class="check line-choice"><input type="checkbox"><span>B3</span></label><label class="check line-choice"><input type="checkbox"><span>C1</span></label><label class="check line-choice"><input type="checkbox"><span>C2</span></label><label class="check line-choice"><input type="checkbox"><span>C3</span></label></div></div><div class="field notes-field"><label>Observações</label><textarea rows="2"></textarea></div></div><div class="list-actions" style="margin:12px -20px -20px"><button type="button" class="btn" id="cancelInlineLot">Cancelar</button><button class="btn primary">Criar lote</button></div></form></div>
      <div class="record open hidden" id="recordDetail"><div class="record-bar"><button class="btn primary" data-movement="Saída">Saída</button><button class="btn" data-movement="Entrada">Entrada</button><button class="btn" data-movement="Não reparadas">Não reparadas</button><button class="btn" data-movement="Corrigir contagem">Corrigir contagem</button><span class="push"></span><button class="btn">Editar ficheiro</button><button class="btn danger" id="closeLot">Fechar lote</button></div><div class="card"><div class="card-head"><div><h3>Resumo do lote ativo</h3><p class="snapshot-note">Este resumo será guardado como fotografia final quando o lote for fechado.</p></div></div><div class="summary-line"><strong>T194</strong> · Lote 12 · Aberto em 13/08/2026 · Linha atual B1</div><div class="snapshot-grid"><div class="stat"><span>Estado</span><strong>Em produção</strong></div><div class="stat"><span>Total do lote</span><strong>192</strong></div><div class="stat"><span>Data de abertura</span><strong>13/08/2026</strong></div><div class="stat"><span>Movimentos</span><strong>4</strong></div><div class="stat"><span>Linhas permitidas</span><strong>B1 · B3 · C1</strong></div><div class="stat"><span>Vida utilizada</span><strong>68%</strong></div></div></div><div class="card" style="margin-top:14px"><div class="card-head"><h3>Estado atual</h3></div><div class="snapshot-grid"><div class="stat"><span>Na produção</span><strong>144</strong></div><div class="stat"><span>Em reparação</span><strong>48</strong></div><div class="stat"><span>Não reparadas</span><strong>0</strong></div><div class="stat"><span>Saídas excecionais</span><strong>0</strong></div><div class="stat"><span>Entradas excecionais</span><strong>0</strong></div><div class="stat"><span>Linha atual</span><strong>B1</strong></div></div></div><div class="card" style="margin-top:14px"><div class="card-head"><div><h3>Últimos movimentos</h3><p class="snapshot-note">Confirmação rápida do lote selecionado. O tracking completo está no Histórico.</p></div><button class="btn">Imprimir / Guardar PDF</button></div><div class="lot-movements"><div class="lot-movement head"><span>Movimento</span><span>Quantidade</span><span>Saldo</span><span>Data</span><span>Operador</span></div><div class="lot-movement"><strong>Saída</strong><span>48</span><span>144</span><span>14/08</span><span>João Silva</span></div><div class="lot-movement"><strong>Entrada</strong><span>24</span><span>168</span><span>13/08</span><span>Ana Martins</span></div></div></div></div>
    </section>
    <section id="boquilhas" class="view"><div class="page-head"><div><h2>Boquilhas</h2><p>Estado dos lotes e disponibilidade para produção.</p></div><span class="date-chip">14 agosto 2026</span></div>
      <div class="card"><div class="card-head"><h3>Ficheiros de Boquilhas</h3><button class="btn primary" id="newLotBatches">Criar novo lote</button></div><div class="filter-bar"><div class="filter-grid batches"><div class="field"><label for="lotSearch">Boquilha, lote ou linha</label><input id="lotSearch" placeholder="Pesquisar ficheiros"></div><div class="field"><label for="batchStatus">Estado</label><select id="batchStatus"><option value="current">Atuais (inclui disponíveis)</option><option value="open">Ativas</option><option value="production">Em produção</option><option value="repair">Em reparação</option><option value="available">Disponível</option><option value="archived">Arquivados</option><option value="scrapped">Sucata</option><option value="all">Todos</option></select></div><div class="field"><label>Linhas</label><select><option>20</option><option>40</option><option>60</option><option>100</option></select></div></div></div><div class="lots" id="lots">
        <article class="lot" data-state="production" data-search="t173 24/33 b1"><div class="lot-top"><h4>T173 · 24/33</h4><span class="pill ready">EM PRODUÇÃO</span></div><div class="lot-data"><div><span>Quantidade</span><strong>288 BQ</strong></div><div><span>Linha</span><strong>B1</strong></div><div><span>Vida utilizada</span><strong>68%</strong></div></div></article>
        <article class="lot" data-state="repair" data-search="p446 24/29 reparacao"><div class="lot-top"><h4>P446 · 24/29</h4><span class="pill repair">EM REPARAÇÃO</span></div><div class="lot-data"><div><span>Quantidade</span><strong>96 BQ</strong></div><div><span>Reparador</span><strong>Externo A</strong></div><div><span>Vida utilizada</span><strong>81%</strong></div></div></article>
        <article class="lot" data-state="production" data-search="v902 25/08 b2"><div class="lot-top"><h4>V902 · 25/08</h4><span class="pill ready">EM PRODUÇÃO</span></div><div class="lot-data"><div><span>Quantidade</span><strong>144 BQ</strong></div><div><span>Linha</span><strong>B2</strong></div><div><span>Vida utilizada</span><strong>42%</strong></div></div></article>
        <article class="lot" data-state="available" data-search="m310 25/11 disponivel"><div class="lot-top"><h4>M310 · 25/11</h4><span class="pill ready">DISPONÍVEL</span></div><div class="lot-data"><div><span>Quantidade</span><strong>192 BQ</strong></div><div><span>Localização</span><strong>Fábrica</strong></div><div><span>Vida utilizada</span><strong>19%</strong></div></div></article>
      </div></div>
    </section>
    <section id="historico" class="view"><div class="page-head"><div><h2>Histórico de movimentos</h2><p>Entradas e saídas de boquilhas para reparação.</p></div><span class="date-chip">Agosto 2026</span></div><div class="history-layout">
      <div class="card"><div class="calendar-head"><button class="icon-btn" aria-label="Mês anterior">‹</button><strong>Agosto 2026</strong><button class="icon-btn" aria-label="Mês seguinte">›</button></div><div class="week"><span>SEG</span><span>TER</span><span>QUA</span><span>QUI</span><span>SEX</span><span>SÁB</span><span>DOM</span></div><div class="days" id="calendar"></div><div class="legend"><span><i></i>Com movimentos</span><span>Selecione um dia</span></div></div>
      <div class="history-summary"><div class="metric"><span>Na fábrica</span><strong>1 248</strong><small>Saldo do período filtrado</small></div><div class="metric"><span>Em reparação</span><strong>336</strong><small style="color:var(--orange)">3 reparadores</small></div><div class="metric"><span>Em produção</span><strong>864</strong><small>4 linhas ativas</small></div><div class="metric"><span>Saídas</span><strong>168</strong><small style="color:var(--orange)">No período</small></div><div class="metric"><span>Entradas</span><strong>144</strong><small>No período</small></div><div class="metric"><span>Saldo de movimentos</span><strong>+24</strong><small>Saídas menos entradas</small></div></div>
    </div><div class="card movement-card"><div class="movement-title"><div class="card-head" style="margin:0"><div><h3>Movimentos de 14 de agosto</h3><p>Selecione um registo para corrigir ou eliminar.</p></div><button class="btn" id="clearHistoryDate">Mostrar todos os dias</button></div></div><div class="filter-bar" style="margin:14px 20px"><div class="filter-grid"><div class="field"><label for="historyText">Referência, lote ou linha</label><input id="historyText" placeholder="Pesquisar no tracking"></div><div class="field"><label for="historyDateFilter">Data</label><input id="historyDateFilter" type="date" value="2026-08-14"></div><div class="field"><label for="historyType">Movimento</label><select id="historyType"><option value="">Todos</option><option value="start">Inícios</option><option value="out">Saídas</option><option value="in">Entradas</option><option value="failed">Não reparadas</option><option value="line">Mudanças de linha</option><option value="count">Correções</option><option value="close">Fechos</option></select></div><div class="field"><label for="historyRepairer">Reparador</label><select id="historyRepairer"><option value="">Todos</option><option>Externo A</option><option>Externo B</option></select></div><div class="field"><label for="historyState">Ficheiros</label><select id="historyState"><option value="current">Atuais</option><option value="archived">Arquivados</option><option value="scrapped">Sucata</option><option value="all">Todos</option></select></div><div class="field"><label>Linhas</label><select><option>20</option><option>40</option><option>60</option><option>100</option></select></div></div><div class="filter-actions"><span class="filter-help">Os cartões de resumo e a lista respondem ao período, lote, movimento e reparador selecionados.</span><button class="btn" id="clearFilters">Limpar filtros</button></div></div><div class="movement-list" id="movements"><div class="movement wide head"><span>Referência</span><span>Lote</span><span>Movimento</span><span>Qtd.</span><span>Saldo</span><span>Reparador</span><span>Linha</span><span>Data</span><span>Operador</span></div>
      <button class="movement wide" data-text="t173 b1 lote 24/33" data-type="out" data-repairer="Externo A" data-state="current"><strong>T173</strong><span>24/33</span><span class="type out">Saída</span><span>96</span><span>192</span><span>Externo A</span><span>B1</span><span>14/08 · 10:42</span><span>João Silva</span></button>
      <button class="movement wide" data-text="p446 lote 24/29" data-type="in" data-repairer="Externo A" data-state="current"><strong>P446</strong><span>24/29</span><span class="type in">Entrada</span><span>48</span><span>144</span><span>Externo A</span><span>—</span><span>14/08 · 09:16</span><span>Ana Martins</span></button>
      <button class="movement wide" data-text="v902 b2 lote 25/08" data-type="out" data-repairer="Externo B" data-state="current"><strong>V902</strong><span>25/08</span><span class="type out">Saída</span><span>72</span><span>144</span><span>Externo B</span><span>B2</span><span>14/08 · 07:55</span><span>Rui Costa</span></button>
      <button class="movement wide" data-text="t521 c1 lote 24/41" data-type="in" data-repairer="Externo B" data-state="current"><strong>T521</strong><span>24/41</span><span class="type in">Entrada</span><span>96</span><span>288</span><span>Externo B</span><span>C1</span><span>14/08 · 06:21</span><span>João Silva</span></button>
    </div><div class="list-actions"><button class="btn" id="editMovement" disabled>Corrigir movimento</button><button class="btn danger" id="deleteMovement" disabled>Eliminar movimento</button></div><div class="pager"><span id="historyCount">4 movimento(s) · Página 1 de 1</span><span><button class="btn" disabled>Anterior</button> <button class="btn" disabled>Seguinte</button></span></div></div></section>
    <section id="definicoes" class="view"><div class="page-head"><div><h2>Definições</h2><p>Configuração operacional do módulo de Boquilhas.</p></div></div><div class="card settings-section"><div class="card-head"><div><h3>Reparadores por linha</h3><p style="margin:3px 0 0;color:var(--muted);font-size:12px">O reparador predefinido é sugerido automaticamente nas saídas.</p></div></div><div class="settings-grid"><div class="setting-row"><span class="setting-line">B1</span><select><option>Externo A</option><option>Externo B</option><option>Sem associação</option></select><div class="repairer-tags"><span class="repairer-tag">Externo A</span><span class="repairer-tag">Externo B</span></div><button class="btn">Editar</button></div><div class="setting-row"><span class="setting-line">B2</span><select><option>Externo B</option><option>Externo A</option><option>Sem associação</option></select><div class="repairer-tags"><span class="repairer-tag">Externo B</span></div><button class="btn">Editar</button></div><div class="setting-row"><span class="setting-line">B3</span><select><option>Sem associação</option><option>Externo A</option><option>Externo B</option></select><div class="repairer-tags"><span style="color:var(--muted);font-size:12px">Nenhum reparador permitido</span></div><button class="btn">Editar</button></div><div class="setting-row"><span class="setting-line">C1</span><select><option>Externo A</option><option>Externo B</option></select><div class="repairer-tags"><span class="repairer-tag">Externo A</span></div><button class="btn">Editar</button></div><div class="setting-row"><span class="setting-line">C2</span><select><option>Externo B</option><option>Externo A</option></select><div class="repairer-tags"><span class="repairer-tag">Externo B</span><span class="repairer-tag">Externo A</span></div><button class="btn">Editar</button></div><div class="setting-row"><span class="setting-line">C3</span><select><option>Externo A</option><option>Externo B</option></select><div class="repairer-tags"><span class="repairer-tag">Externo A</span></div><button class="btn">Editar</button></div></div><div class="list-actions" style="margin:16px -20px -20px"><button class="btn primary">Guardar alterações</button></div></div><div class="card settings-section"><div class="card-head"><div><h3>Gerir reparadores</h3><p style="margin:3px 0 0;color:var(--muted);font-size:12px">Reparadores desativados permanecem disponíveis no Histórico.</p></div><button class="btn primary">Adicionar reparador</button></div><div class="repairer-tags"><span class="repairer-tag">Externo A · Ativo</span><span class="repairer-tag">Externo B · Ativo</span></div></div></section>
  </main>
</div>
<div class="modal-backdrop" id="movementModal"><div class="modal compact"><div class="modal-head"><div><h3 id="movementTitle">Saída</h3><div class="movement-subtitle">T194 · Lote 12 · Linha B1</div></div><button class="icon-btn close-modal" aria-label="Fechar">×</button></div><form id="movementForm"><div class="modal-body"><div class="context"><div><span>BOQUILHA</span><strong>T194</strong></div><div><span>LOTE</span><strong>12</strong></div><div><span>LINHA / MÁQUINA</span><strong>B1</strong></div><div><span>ESTADO</span><strong>Em fabrico</strong></div><div><span>QUANTIDADE ATUAL</span><strong>144</strong></div><div><span>LINHAS PERMITIDAS</span><strong>B1 · B3 · C1</strong></div></div><div class="movement-form-grid"><div class="field"><label>Data</label><input type="date" value="2026-08-14"></div><div class="field"><label>Quantidade</label><input required type="number" min="1" max="999"></div><div class="field"><label>Motivo</label><select id="movementReason"><option value="normal">Movimento normal</option><option value="missing">Movimento anterior não registado</option><option value="correction">Correção operacional</option><option value="other">Outro</option></select></div><div class="field detail-field"><label>Detalhe</label><input id="movementDetail" placeholder="Opcional"></div><div class="field notes-field"><label>Observações</label><textarea rows="1" placeholder="Informação adicional, se necessária"></textarea></div></div><div class="warning" id="movementWarning" hidden>Qualquer parte sem correspondência fica assinalada no histórico e não altera a quantidade física do lote.</div></div><div class="modal-foot"><button type="button" class="btn close-modal">Cancelar</button><button class="btn primary">Guardar</button></div></form></div></div>
<div class="toast" id="toast"></div>
<script>
const toast=(t)=>{const e=document.querySelector('#toast');e.textContent=t;e.classList.add('show');setTimeout(()=>e.classList.remove('show'),2200)};
document.querySelectorAll('.tab').forEach(t=>t.onclick=()=>{document.querySelectorAll('.tab,.view').forEach(e=>e.classList.remove('active'));t.classList.add('active');document.querySelector('#'+t.dataset.view).classList.add('active')});
const openModal=id=>document.querySelector(id).classList.add('open'),closeModals=()=>document.querySelectorAll('.modal-backdrop').forEach(m=>m.classList.remove('open'));
const closeInlineLot=()=>{const form=document.querySelector('#inlineLotForm'),hasData=[...form.querySelectorAll('input,textarea')].some(i=>i.type!=='date'&&i.type!=='checkbox'&&i.value.trim());if(hasData&&!confirm('Fechar a criação e perder os dados preenchidos?'))return;document.querySelector('#inlineCreate').classList.remove('open');document.querySelector('#recordEmpty').classList.remove('hidden');document.querySelector('#newLot').textContent='Criar novo lote'};const showInlineLot=()=>{const panel=document.querySelector('#inlineCreate');if(panel.classList.contains('open'))return closeInlineLot();document.querySelector('#recordEmpty').classList.add('hidden');document.querySelector('#recordDetail').classList.add('hidden');panel.classList.add('open');document.querySelector('#newLot').textContent='Fechar criação'};document.querySelector('#newLot').onclick=showInlineLot;document.querySelector('#cancelInlineLot').onclick=closeInlineLot;document.querySelector('#inlineLotForm').onsubmit=e=>{e.preventDefault();document.querySelector('#inlineCreate').classList.remove('open');document.querySelector('#recordDetail').classList.remove('hidden');document.querySelector('#newLot').textContent='Criar novo lote';toast('Novo lote criado e selecionado')};document.querySelectorAll('.close-modal').forEach(b=>b.onclick=closeModals);document.querySelectorAll('.modal-backdrop').forEach(m=>m.onclick=e=>{if(e.target===m)closeModals()});
const rs=document.querySelector('#recordSearch'),results=document.querySelector('#recordResults'),found=document.querySelector('#selectT194'),nf=document.querySelector('#notFound');rs.oninput=()=>{const q=rs.value.trim().toUpperCase();results.classList.toggle('hidden',!q);const ok='T194'.includes(q)||'12'.includes(q);found.classList.toggle('hidden',!ok);nf.classList.toggle('hidden',ok)};found.onclick=()=>{results.classList.add('hidden');document.querySelector('#recordEmpty').classList.add('hidden');document.querySelector('#recordDetail').classList.remove('hidden');rs.value='T194'};
document.querySelectorAll('[data-movement]').forEach(b=>b.onclick=()=>{document.querySelector('#movementTitle').textContent=b.dataset.movement;openModal('#movementModal')});const movementReason=document.querySelector('#movementReason'),movementDetail=document.querySelector('#movementDetail'),movementWarning=document.querySelector('#movementWarning'),detailExamples={normal:'Opcional',missing:'Ex.: saída de 12 BQ em 12/08',correction:'Ex.: quantidade registada incorretamente',other:'Indique brevemente a razão'};movementReason.onchange=()=>{movementDetail.placeholder=detailExamples[movementReason.value];movementWarning.hidden=movementReason.value==='normal'};document.querySelector('#movementForm').onsubmit=e=>{e.preventDefault();closeModals();toast('Movimento guardado no histórico')};
document.querySelectorAll('.more').forEach(b=>b.onclick=e=>{e.stopPropagation();const m=b.closest('.machine').querySelector('.menu');document.querySelectorAll('.menu').forEach(x=>x!==m&&x.classList.remove('open'));m.classList.toggle('open')});document.addEventListener('click',()=>document.querySelectorAll('.menu').forEach(m=>m.classList.remove('open')));
document.querySelectorAll('[data-act]').forEach(b=>b.onclick=e=>{e.stopPropagation();const line=b.closest('.machine').querySelector('.machine-name').textContent;toast(`${b.textContent}: linha ${line}`)});
document.querySelectorAll('.job-reference').forEach(link=>link.addEventListener('click',e=>{e.stopPropagation();window.location.href=`job-on.html?reference=${encodeURIComponent(link.dataset.job)}`}));
document.querySelectorAll('.machine').forEach(card=>card.addEventListener('click',e=>{if(e.target.closest('.more,.menu,.job-reference'))return;const refs=[...card.querySelectorAll('.machine-ref')];if(!refs.length){toast('Linha livre — use Adicionar');return}if(card.classList.contains('alert')){toast('Referências diferentes nesta linha. Remova uma referência para continuar.');return}rs.value=card.dataset.bq||'';document.querySelector('[data-view="registo"]').click();document.querySelector('#recordEmpty').classList.add('hidden');document.querySelector('#recordDetail').classList.remove('hidden');toast('Registo da boquilha aberto')}));
const filterLots=()=>{const q=document.querySelector('#lotSearch').value.toLowerCase().trim(),s=document.querySelector('#batchStatus').value;document.querySelectorAll('.lot').forEach(l=>{const stateOk=['all','current','open'].includes(s)||l.dataset.state===s;l.hidden=!l.dataset.search.includes(q)||!stateOk})};document.querySelector('#lotSearch').oninput=filterLots;document.querySelector('#batchStatus').onchange=filterLots;document.querySelector('#newLotBatches').onclick=()=>{document.querySelector('[data-view="registo"]').click();showInlineLot()};
const activeDays=[1,4,5,8,11,12,14,18,21,25,27,29];const cal=document.querySelector('#calendar');for(let i=0;i<5;i++)cal.insertAdjacentHTML('beforeend','<span class="day blank"></span>');for(let d=1;d<=31;d++)cal.insertAdjacentHTML('beforeend',`<button class="day ${activeDays.includes(d)?'has':''} ${d===14?'selected':''}">${d}</button>`);cal.onclick=e=>{if(!e.target.matches('.day'))return;cal.querySelectorAll('.day').forEach(x=>x.classList.remove('selected'));e.target.classList.add('selected');document.querySelector('.movement-title h3').textContent=`Movimentos de ${e.target.textContent} de agosto`;toast(activeDays.includes(+e.target.textContent)?'Movimentos do dia carregados':'Sem movimentos neste dia')};
const edits=document.querySelector('#editMovement'),del=document.querySelector('#deleteMovement');document.querySelectorAll('.movement:not(.head)').forEach(m=>{m.onclick=()=>{document.querySelectorAll('.movement').forEach(x=>x.classList.remove('selected'));m.classList.add('selected');edits.disabled=del.disabled=false};m.ondblclick=()=>{rs.value=m.querySelector('strong').textContent;document.querySelector('[data-view="registo"]').click();document.querySelector('#recordEmpty').classList.add('hidden');document.querySelector('#recordDetail').classList.remove('hidden');toast(`Lote ${m.children[1].textContent} aberto no Registo`)}});edits.onclick=()=>toast('Abrir correção do movimento selecionado');del.onclick=()=>toast('Confirmação necessária antes de eliminar');
const historyRows=[...document.querySelectorAll('.movement[data-type]')],historyText=document.querySelector('#historyText'),historyType=document.querySelector('#historyType'),historyRepairer=document.querySelector('#historyRepairer'),historyState=document.querySelector('#historyState'),historyDateFilter=document.querySelector('#historyDateFilter');function filterHistory(){const q=historyText.value.toLowerCase().trim(),type=historyType.value,repairer=historyRepairer.value,state=historyState.value;let n=0;historyRows.forEach(r=>{const show=r.dataset.text.includes(q)&&(!type||r.dataset.type===type)&&(!repairer||r.dataset.repairer===repairer)&&(state==='all'||r.dataset.state===state);r.hidden=!show;if(show)n++});document.querySelector('#historyCount').textContent=`${n} movimento(s) · Página 1 de 1`;edits.disabled=del.disabled=true} [historyText,historyType,historyRepairer,historyState,historyDateFilter].forEach(e=>e.addEventListener(e.tagName==='INPUT'?'input':'change',filterHistory));document.querySelector('#clearFilters').onclick=()=>{historyText.value='';historyType.value='';historyRepairer.value='';historyState.value='current';historyDateFilter.value='';filterHistory()};document.querySelector('#clearHistoryDate').onclick=()=>{historyDateFilter.value='';document.querySelector('.movement-title h3').textContent='Movimentos de todos os dias';cal.querySelectorAll('.day').forEach(x=>x.classList.remove('selected'));filterHistory()};
document.querySelector('#closeLot').onclick=()=>{if(confirm('Fechar este lote e guardar o resumo final no Histórico?'))toast('Lote fechado — snapshot histórico criado')};
</script>
</body></html>

```
## END FILE CONTENT

---

# FILE 024

## Source Path
`job-on.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Job On — Portal DMO
EXTERNAL CSS (0): 
SCRIPT SRC (0): 
<header>: 0
<nav>: 0
<aside>: 0
<main>: 0
<section>: 0
<footer>: 0
<form>: 0
UNIQUE IDS (0): 
DATA-* ATTRIBUTES (0): 
<button: 0
<input: 0
<select: 0
<textarea: 0
<table: 0
<a: 1
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Job On — Portal DMO</title>
  <meta http-equiv="refresh" content="0;url=job-on-v48-folha-producao.html">
</head>
<body>
  <p><a href="job-on-v48-folha-producao.html">Abrir a folha de produção Job On</a></p>
</body>
</html>

```
## END FILE CONTENT

---

# FILE 025

## Source Path
`job-on-v48-folha-producao.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Job On — Folha de produção
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 1
<main>: 1
<section>: 10
<footer>: 0
<form>: 0
UNIQUE IDS (33): planeamento, calendar, newJob, jobList, createPanel, closeCreate, cancelCreate, createDraft, folha, backToPlanning, jobSheet, jobStartDate, jobEndDate, editSheet, saveSheet, sheetMode, imagePreview, jobImage, inventoryPicker, inventoryPickerTitle, toolFamilyFilter, addCalRow, calRows, piClampMaterial, historico, definicoes, catalogFamily, catalogField, newCatalogOption, addCatalogOption, catalogRows, editCatalogOption, disableCatalogOption
DATA-* ATTRIBUTES (4): can-edit-jobon, can-confirm-verifications, view, catalog
<button: 43
<input: 85
<select: 21
<textarea: 11
<table: 6
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: calendar, syncJobDatesToCalendar
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Job On — Folha de produção</title>
  <link rel="stylesheet" href="dmo-design-system.css">
  <style>
    :root{--b:#3c73a8;--dark:#193046;--soft:#f1f6fa;--line:#d9e6f2;--page:#f6f9fc;--text:#172d42;--muted:#64778a;--warn:#a97943;--warn-soft:#f7f0e7;--green:#527c72;--green-soft:#e5f0eb;--red:#9a625d;--shadow:0 8px 24px rgba(25,48,70,.06)}
    *{box-sizing:border-box}body{margin:0;background:var(--page);color:var(--text);font:14px/1.4 Inter,"Segoe UI",sans-serif}button,input,select,textarea{font:inherit}
    .header{height:76px;display:flex;align-items:center;gap:13px;padding:10px 28px;background:#fff;border-bottom:1px solid var(--line)}.logo{width:44px;height:44px;border-radius:50%}.brand h1{margin:0;font-size:18px}.brand p,.muted{margin:3px 0 0;color:var(--muted);font-size:11px}.user{margin-left:auto;padding-left:18px;border-left:1px solid var(--line);text-align:right}.user strong,.user span{display:block}.user span{font-size:11px;color:var(--muted)}
    .tabs{height:52px;display:flex;gap:26px;padding:0 28px;background:#fff;border-bottom:1px solid var(--line)}.tab{border:0;border-bottom:3px solid transparent;background:none;color:var(--muted);font-weight:750;cursor:pointer}.tab.active{color:var(--b);border-color:var(--b)}.settings{margin-left:auto}
    .main{max-width:1500px;margin:auto;padding:22px 26px}.view{display:none}.view.active{display:block}.page-title{margin:0 0 14px}.page-title.with-action{display:flex;align-items:flex-end;justify-content:space-between;gap:12px}.page-title h2{margin:0;font-size:23px}.card{background:#fff;border:1px solid var(--line);border-radius:12px;box-shadow:var(--shadow)}.btn{min-height:36px;padding:7px 12px;border:1px solid var(--b);border-radius:8px;background:var(--b);color:#fff;font-weight:750;cursor:pointer}.btn:hover,.btn:focus-visible{background:#fff;color:var(--b);outline:none}.btn.danger{border-color:var(--red);background:var(--red)}.btn.danger:hover{background:#fff;color:var(--red)}.btn.icon{width:36px;padding:0}.btn:disabled{border-color:#cbd5df;background:#cbd5df;color:#fff;cursor:not-allowed}
    .planner{display:grid;grid-template-columns:300px minmax(0,1fr);gap:14px;align-items:start}.calendar{padding:14px}.cal-head{display:flex;align-items:center;justify-content:space-between}.cal-head .btn{min-height:32px;width:32px}.week,.days{display:grid;grid-template-columns:repeat(7,1fr);gap:4px}.week{margin:10px 0 6px;color:var(--muted);font-size:9px;font-weight:800;text-align:center}.day{position:relative;min-height:32px;border:1px solid var(--line);border-radius:6px;background:#fff;color:var(--text);font-size:11px;font-weight:750;cursor:pointer}.day:hover{border-color:var(--b);background:var(--page)}.day.in-range{border-color:#afc7de;background:#e8eff7}.day.range-start,.day.range-end,.day.selected{border-color:var(--b);background:var(--b);color:#fff}.day.has:after{content:"";position:absolute;left:50%;bottom:3px;width:3px;height:3px;border-radius:50%;background:var(--warn)}.day.blank{visibility:hidden}
    .day-card{padding:14px}.card-head{display:flex;align-items:flex-start;justify-content:space-between;gap:12px}.card-head h3{margin:0}.card-head-actions{display:flex;gap:8px}.filters{display:grid;grid-template-columns:minmax(220px,2fr) 130px 90px auto;gap:8px;align-items:end;margin-top:12px;padding:10px;background:var(--soft);border:1px solid var(--line);border-radius:9px}.filters>.btn{min-height:40px}.field label{display:block;margin-bottom:5px;color:var(--muted);font-size:10px;font-weight:750}.field input,.field select,.field textarea{width:100%;min-height:40px;padding:8px 10px;border:1px solid var(--line);border-radius:8px;background:#fff;color:var(--text);outline:none}.field input:focus,.field select:focus,.field textarea:focus{border-color:var(--b);box-shadow:0 0 0 3px rgba(60,115,168,.13)}
    .table-wrap{overflow:auto;border:1px solid var(--line);border-radius:9px;margin-top:10px}table{width:100%;border-collapse:collapse;white-space:nowrap}th{padding:9px 10px;background:var(--soft);color:var(--muted);font-size:9px;text-align:left;text-transform:uppercase}td{padding:10px;border-top:1px solid var(--line)}tr[data-row]{cursor:pointer}tr[data-row]:hover{background:var(--page)}tr[data-row].selected{background:#d9e6f2}.pill{display:inline-flex;padding:3px 8px;border-radius:99px;background:#e8eff7;color:#315d88;font-size:9px;font-weight:800}.pill.good{background:var(--green-soft);color:var(--green)}.pill.warn{background:var(--warn-soft);color:var(--warn)}.list-foot{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-top:10px;color:var(--muted);font-size:10px}.pager{display:flex;align-items:center;gap:6px}.pager .btn{width:36px;padding:0}
    .create-panel{display:none;margin-top:12px;padding:14px;border:1px solid var(--line);border-radius:10px;background:#fff}.create-panel.open{display:block}.create-grid{display:grid;grid-template-columns:minmax(220px,2fr) 120px 90px 90px 90px 150px;gap:8px;align-items:end}.source-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-top:12px}.source{padding:11px;border:1px solid var(--line);border-radius:8px;background:#fff;color:var(--text);text-align:left;cursor:pointer}.source:hover,.source.selected{border-color:var(--b);background:#e8eff7}.source span{display:block;margin-top:3px;color:var(--muted);font-size:10px}.actions{display:flex;justify-content:flex-end;gap:8px;margin-top:12px;padding-top:12px;border-top:1px solid var(--line)}
    .sheet{margin-top:16px}.sheet-head{position:sticky;top:0;z-index:10;padding:13px 16px}.context{display:grid;grid-template-columns:135px 135px 90px minmax(130px,1.4fr) 85px 62px 55px 72px 86px 80px 105px;gap:8px}.context div{padding:8px 9px;border-left:3px solid var(--line);border-radius:7px;background:var(--soft);opacity:.72}.context span{display:block;color:var(--muted);font-size:8px;text-transform:uppercase}.context input,.context select{width:100%;min-height:26px;padding:3px 5px;border:1px solid var(--line);border-radius:6px;background:#fff;color:var(--text);font-weight:800}.context .key-context{border-left-color:var(--b);background:#e8eff7;opacity:1}.context .key-context span{color:#315d88}.context .key-context input,.context .key-context select{font-size:14px;color:var(--dark)}.sheet-toolbar{display:flex;gap:8px;flex-wrap:wrap;margin-top:10px}.visual-row{display:grid;grid-template-columns:minmax(0,1fr) 260px;gap:10px;align-items:stretch;margin-top:12px}.alert{margin:0;padding:10px 12px;border-left:4px solid var(--warn);border-radius:8px;background:var(--warn-soft);font-size:11px}.image-box{display:flex;align-items:center;gap:10px;padding:9px 10px;border:1px solid var(--line);border-radius:9px;background:#fff}.image-preview{width:58px;height:58px;display:grid;place-items:center;flex:0 0 auto;border:1px dashed #bdd3e8;border-radius:7px;background:var(--soft);color:var(--muted);font-size:9px;text-align:center;overflow:hidden}.image-preview img{width:100%;height:100%;object-fit:contain}.image-box strong,.image-box small{display:block}.image-box small{margin-top:2px;color:var(--muted);font-size:9px}.image-upload{margin-top:6px;min-height:30px;padding:4px 8px;font-size:10px}
    .visual-row+.sheet-body{margin-top:16px}.sheet-body{padding:14px}.section-bar{display:flex;align-items:center;justify-content:space-between;margin:2px 0 10px;padding:8px 10px;border-left:4px solid var(--b);border-radius:7px;background:var(--soft)}.section-bar h3{margin:0;font-size:13px}.tool-grid{display:grid;grid-template-columns:repeat(4,minmax(0,1fr));gap:10px;align-items:start}.tool{border:1px solid var(--line);border-radius:10px;background:#fff;overflow:hidden}.tool.wide{grid-column:span 2}.tool-title{display:flex;align-items:center;justify-content:space-between;padding:9px 11px;background:var(--soft);border-bottom:1px solid var(--line)}.tool-title h4{margin:0;font-size:17px;color:var(--dark)}.tool-body{padding:10px}.mini-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:7px}.mini-grid .full{grid-column:1/-1}.mini-grid .field input,.mini-grid .field select{min-height:34px;padding:6px 8px;font-size:12px}.mini-grid textarea{min-height:72px!important;resize:vertical}.measure-table{width:100%;white-space:normal}.measure-table th,.measure-table td{padding:6px 7px}.measure-table input,.measure-table select{width:100%;min-height:30px;border:1px solid var(--line);border-radius:6px;padding:4px 6px;background:#fff}.quantity{display:grid;grid-template-columns:1fr 1fr;gap:7px}.quantity div{padding:7px 8px;border-radius:7px;background:var(--soft)}.quantity span{display:block;color:var(--muted);font-size:8px}.quantity input{width:100%;min-height:25px;padding:2px 4px;border:1px solid var(--line);border-radius:5px;background:#fff;color:var(--text);font-size:12px;font-weight:800}
    .tool-grid{grid-template-columns:repeat(4,minmax(0,1fr))}.tool.priority{border-color:#98b9da;box-shadow:0 5px 16px rgba(25,48,70,.07)}.tool.priority .tool-title{border-left:4px solid var(--b);background:#e8eff7}.tool.priority .tool-title h4{font-size:20px}.tool.priority .mini-grid>.field:first-child input,.tool.priority .mini-grid>.field:nth-child(2) select{border-color:#98b9da;background:#f3f7fb;color:var(--dark);font-weight:800}.tool.priority .quantity div{border-left:3px solid var(--b);background:#e8eff7}.tool.priority .quantity input{font-size:14px;color:var(--dark)}.tool.secondary{opacity:.86}.sheet-mode{margin-left:auto;border:1px solid transparent;transition:background-color .16s ease,border-color .16s ease,color .16s ease}.sheet:not(.editing) .sheet-mode{border-color:#c8d9e8;background:#e7eff6;color:#315d88}.edit-only{display:none!important}.sheet.editing .edit-only{display:inline-flex!important}body[data-can-edit-jobon="false"] .responsible-only{display:none!important}.sheet:not(.editing) .tool input,.sheet:not(.editing) .tool select,.sheet:not(.editing) .tool textarea,.sheet:not(.editing) .notes textarea,.sheet:not(.editing) .context input,.sheet:not(.editing) .context select{border-color:transparent;background:transparent;box-shadow:none;color:var(--text);pointer-events:none}body[data-can-confirm-verifications="false"] .checks input{pointer-events:none}.sheet:not(.editing) .tool select,.sheet:not(.editing) .context select{appearance:none}.sheet:not(.editing) .field label{color:#718397}.sheet:not(.editing) .image-upload{display:none}.sheet.editing .sheet-mode{border-color:#d8c4ad;background:#f0e8df;color:#76583c}.tool-title-actions{display:flex;align-items:center;gap:6px}.btn.compact{min-height:30px;padding:4px 8px;font-size:10px}.inventory-picker{display:none;margin:12px 0;padding:12px}.sheet.editing .inventory-picker.open{display:block}.inventory-filters{display:grid;grid-template-columns:2fr 100px 150px 150px auto;gap:8px;align-items:end}.inventory-picker .table-wrap{margin-top:10px}.inventory-source{margin:8px 0 0;color:var(--muted);font-size:10px}.lower-grid{display:grid;grid-template-columns:1.35fr 1fr;gap:10px;margin-top:10px}.checks{display:grid;gap:7px}.check{display:grid;grid-template-columns:20px 1fr auto;gap:8px;align-items:center;padding:8px;border-radius:7px;background:var(--soft)}.check input{width:16px;height:16px;accent-color:var(--b)}.check small{display:block;color:var(--muted)}.notes textarea{min-height:138px}.history-box{margin-top:10px}.empty{display:grid;min-height:180px;place-items:center;color:var(--muted);text-align:center;border:1px dashed #bdd3e8;border-radius:10px}
    @media(max-width:1100px){.planner{grid-template-columns:280px 1fr}.tool-grid{grid-template-columns:repeat(2,1fr)}.context{grid-template-columns:repeat(4,1fr)}.create-grid{grid-template-columns:repeat(3,1fr)}}
    @media(max-width:720px){.header{padding:10px 14px}.user{display:none}.tabs{padding:0 12px;gap:16px;overflow:auto}.main{padding:14px 12px}.planner,.lower-grid{grid-template-columns:1fr}.calendar{max-width:330px}.filters,.create-grid,.context,.tool-grid,.inventory-filters{grid-template-columns:1fr}.tool.wide{grid-column:auto}.source-grid{grid-template-columns:1fr}.card-head{flex-direction:column}.card-head-actions{width:100%}.card-head-actions .btn{flex:1}.sheet-head{position:static}}
  </style>
</head>
<body data-can-edit-jobon="true" data-can-confirm-verifications="true">
  <header class="header"><img class="logo" src="logo_recolored(1).png" alt="BA"><div class="brand"><h1>Job On</h1><p>Planeamento e preparação da produção</p></div><div class="user"><strong>Ana Martins</strong><span>Responsável</span></div></header>
  <nav class="tabs"><button class="tab" data-view="planeamento">Planeamento</button><button class="tab active" data-view="folha">Job On</button><button class="tab" data-view="historico">Histórico</button><button class="tab settings responsible-only" data-view="definicoes">Definições</button></nav>
  <main class="main">
    <section class="view" id="planeamento">
      <div class="page-title"><h2>Planeamento de Job On</h2><p class="muted">Escolha o dia; selecione um registo ou abra-o com duplo clique.</p></div>
      <div class="planner">
        <aside class="card calendar"><div class="cal-head"><button class="btn icon" aria-label="Mês anterior">‹</button><strong>Agosto 2026</strong><button class="btn icon" aria-label="Mês seguinte">›</button></div><div class="week"><span>SEG</span><span>TER</span><span>QUA</span><span>QUI</span><span>SEX</span><span>SÁB</span><span>DOM</span></div><div class="days" id="calendar"></div></aside>
        <section class="card day-card">
          <div class="card-head"><div><h3>Job Ons de 18 de agosto</h3><p class="muted">O botão de criação pertence a este contexto e usa o dia selecionado.</p></div><div class="card-head-actions"><button class="btn responsible-only" id="newJob">Criar Job On</button></div></div>
          <div class="filters"><div class="field"><label>Referência ou produção</label><input placeholder="Pesquisar"></div><div class="field"><label>Máquina</label><select><option>Todas</option><option>B1</option><option>B2</option><option>B3</option><option>C1</option><option>C2</option><option>C3</option></select></div><div class="field"><label>Linhas</label><select><option>20</option><option>40</option><option>60</option></select></div><button class="btn">Limpar</button></div>
          <div class="table-wrap"><table id="jobList"><thead><tr><th>Data</th><th>Referência</th><th>Produção</th><th>Máquina</th><th>Estado</th><th>Preparação</th></tr></thead><tbody><tr data-row class="selected"><td>18/08/2026</td><td><strong>5774T173</strong></td><td>202603</td><td>B1</td><td><span class="pill">Planeado</span></td><td><span class="pill warn">2 verificações</span></td></tr><tr data-row><td>18/08/2026</td><td><strong>9389T194</strong></td><td>202601</td><td>B3</td><td><span class="pill good">Em fabrico</span></td><td>Completo</td></tr></tbody></table></div>
          <div class="list-foot"><span>2 registos · Página 1 de 1</span><div class="pager"><button class="btn icon" disabled>‹</button><span>1 / 1</span><button class="btn icon" disabled>›</button></div></div>
          <div class="create-panel" id="createPanel"><div class="card-head"><div><h3>Criar Job On para 18/08/2026</h3><p class="muted">A data vem do calendário; os restantes valores identificam o novo fabrico.</p></div><button class="btn icon" id="closeCreate">×</button></div><div class="create-grid"><div class="field"><label>Referência</label><input value="5774T173"></div><div class="field"><label>Produção</label><input value="202604"></div><div class="field"><label>Máquina</label><select><option>B1</option><option>B2</option><option>B3</option><option>C1</option><option>C2</option><option>C3</option></select></div><div class="field"><label>Secções</label><input value="12"></div><div class="field"><label>Gota</label><input value="2"></div><div class="field"><label>Data</label><input type="date" value="2026-08-18"></div></div><div class="source-grid"><button class="source selected"><strong>Duplicar anterior</strong><span>5774T173 · Produção 202512 · B1</span></button><button class="source"><strong>Escolher histórico</strong><span>Selecionar outro Job On desta Referência</span></button><button class="source"><strong>Novo em branco</strong><span>Folha completa sem ferramentas associadas</span></button></div><div class="actions"><button class="btn" id="cancelCreate">Cancelar</button><button class="btn" id="createDraft">Criar rascunho</button></div></div>
        </section>
      </div>

    </section>
    <section class="view active" id="folha">
      <div class="page-title with-action"><div><h2>Folha Job On</h2><p class="muted">Produção 202603 · Referência 5774T173 · Máquina B1</p></div><button class="btn" id="backToPlanning">Voltar ao planeamento</button></div>
      <section class="sheet" id="jobSheet">
        <div class="card sheet-head"><div class="context"><div class="key-context"><span>Data início</span><input id="jobStartDate" type="date" value="2026-08-18" aria-label="Data de início"></div><div class="key-context"><span>Data fim</span><input id="jobEndDate" type="date" value="2026-08-21" aria-label="Data de fim"></div><div class="key-context"><span>Máquina/Linha</span><select aria-label="Máquina ou linha"><option selected>B1</option><option>B2</option><option>B3</option><option>C1</option><option>C2</option><option>C3</option></select></div><div><span>Referência</span><input value="5774T173" aria-label="Referência"></div><div><span>Produção</span><input value="202603" aria-label="Produção"></div><div><span>Secções</span><input type="number" value="12" min="0" aria-label="Secções"></div><div><span>Gota</span><input type="number" value="2" min="0" aria-label="Gota"></div><div><span>Tipo</span><input value="3" aria-label="Tipo"></div><div><span>Paragem</span><input value="" placeholder="—" aria-label="Paragem"></div><div><span>Peso</span><input type="number" value="145" step="0.01" aria-label="Peso"></div><div><span>Processo</span><select aria-label="Processo"><option selected>NNPB</option><option>PS</option></select></div></div><div class="sheet-toolbar"><button class="btn responsible-only" id="editSheet">Editar folha</button><button class="btn responsible-only" id="saveSheet" hidden>Guardar nova revisão</button><button class="btn edit-only responsible-only">Alterar data</button><button class="btn edit-only responsible-only">Duplicar</button><button class="btn">Comparar</button><button class="btn">Exportar</button><span class="pill sheet-mode" id="sheetMode">Modo consulta</span></div></div>
        <div class="visual-row"><div class="alert"><strong>Preparação pendente:</strong> verificar gargalo no MF e confirmar pernos nos fundos antes do fabrico.</div><div class="image-box"><div class="image-preview" id="imagePreview">Sem imagem</div><div><strong>Imagem do artigo</strong><small>Pré-visualização associada ao contexto aberto.</small><label class="btn image-upload" for="jobImage">Carregar imagem</label><input id="jobImage" type="file" accept="image/*" hidden></div></div></div>
        <div class="card sheet-body">
          <div class="section-bar"><h3>Folha de ferramentas</h3><span class="muted">Snapshot integral desta produção; em edição todos os campos podem ser alterados.</span></div>
          <section class="card inventory-picker edit-only" id="inventoryPicker" aria-label="Selecionar ferramenta do Job On">
            <div class="card-head"><div><h3 id="inventoryPickerTitle">Alterar CM associado</h3><p class="muted">Lista live filtrada pela Referência da ferramenta e pela Máquina B1. A associação só muda depois de confirmar e guardar o Job On.</p></div><button class="btn compact">Associar lote selecionado</button></div>
            <div class="inventory-filters"><div class="field"><label>Referência ou nome técnico</label><input value="5447" placeholder="Pesquisar"></div><div class="field"><label>Família</label><select id="toolFamilyFilter"><option>CM</option><option>MF</option><option>BQ</option></select></div><div class="field"><label>Localização</label><select><option>Todas</option><option>Armazém</option><option>Produção</option><option>Reparação</option></select></div><div class="field"><label>Estado</label><select><option>Todos</option><option>Novo</option><option>Reparado</option><option>Por reparar</option></select></div><button class="btn">Limpar</button></div>
            <div class="table-wrap"><table><thead><tr><th>Referência</th><th>Lote</th><th>Nome técnico</th><th>Máquina</th><th>Posição</th><th>Contexto</th><th>Estado</th><th>% uso</th></tr></thead><tbody><tr data-row class="selected"><td><strong>5447</strong></td><td>3</td><td>Contra-molde 5447</td><td>B1</td><td>2421</td><td>Armazém</td><td><span class="pill warn">Por reparar</span></td><td>38%</td></tr><tr data-row><td><strong>5447</strong></td><td>4</td><td>Contra-molde 5447</td><td>B1</td><td>—</td><td>Reparação externa</td><td><span class="pill">Fora para reparar</span></td><td>22%</td></tr><tr data-row><td><strong>5447</strong></td><td>6</td><td>Contra-molde 5447</td><td>B1</td><td>1840</td><td>Armazém</td><td><span class="pill good">Reparado</span></td><td>14%</td></tr></tbody></table></div>
            <p class="inventory-source">Posição/contexto: Armazém. Estado técnico e % de uso: domínio da ferramenta. Selecionar não cria uma Saída nem reserva a ferramenta.</p>
          </section>
          <div class="tool-grid">
            <article class="tool wide"><div class="tool-title"><h4>MP</h4><span class="pill good">Completo</span></div><div class="tool-body"><div class="mini-grid"><div class="field"><label>Referência</label><input value="5809"></div><div class="field"><label>Lote</label><select><option>01</option><option>04</option></select></div><div class="field"><label>Tipo</label><input value="6 1/4\""></div><div class="field"><label>Diâmetro pata</label><input value="35,00"></div><div class="field"><label>Diâmetro gargalo</label><input value="43,00"></div><div class="field"><label>Diâmetro exterior</label><input value="152,20"></div><div class="field"><label>Folgas</label><input value="0,19 / 0 / +0,03"></div><div class="field"><label>Adaptador</label><select><option>N/A</option></select></div><div class="field"><label>Inversão</label><input value="82,55"></div><div class="field"><label>Parafuso</label><select><option>N/A</option></select></div><div class="field"><label>3.ª almofada</label><select><option>N/A</option></select></div><div class="field"><label>Reparador</label><input value="MOLDIN"></div><div class="field"><label>Utilização</label><input value="30,00%"></div><div class="quantity"><div><span>Stock</span><strong>18</strong></div><div><span>Em máquina</span><strong>12</strong></div></div><div class="field full"><label>Notas MP</label><textarea>CX rodar justa 1/4 de volta. Verificar diâmetro exterior.</textarea></div></div></div></article>
            <article class="tool wide"><div class="tool-title"><h4>MF</h4><span class="pill warn">Verificar</span></div><div class="tool-body"><div class="mini-grid"><div class="field"><label>Referência</label><input value="5810"></div><div class="field"><label>Lote</label><select><option>01</option><option>04</option><option>13</option></select></div><div class="field"><label>Tipo</label><input value="6 1/4\""></div><div class="field"><label>Diâmetro corpo</label><input value="70 / 69,40"></div><div class="field"><label>Diâmetro gargalo</label><input value="43,40"></div><div class="field"><label>Diâmetro exterior</label><input value="152,20"></div><div class="field"><label>Folgas</label><input value="0,18 / 0 / +0,03"></div><div class="field"><label>Fundo final</label><input value="5810"></div><div class="field"><label>Adaptador</label><select><option>N/A</option></select></div><div class="field"><label>Inversão</label><input value="82,55"></div><div class="field"><label>Parafuso</label><select><option>N/A</option></select></div><div class="field"><label>Reparador</label><input value="BA"></div><div class="field"><label>Utilização</label><input value="12,00%"></div><div class="quantity"><div><span>Stock</span><strong>16</strong></div><div><span>Em máquina</span><strong>12</strong></div></div><div class="field full"><label>Notas MF</label><textarea>Tratar gravações. Bolear rebaixo do molde final.</textarea></div></div></div></article>
            <article class="tool"><div class="tool-title"><h4>BQ</h4><span class="pill good">Disponível</span></div><div class="tool-body"><div class="mini-grid"><div class="field"><label>Referência</label><input value="T173"></div><div class="field"><label>Lote</label><select><option>24/33</option><option>25/08</option></select></div><div class="field"><label>Utilização</label><input value="45,00%"></div><div class="quantity"><div><span>Stock</span><strong>192</strong></div><div><span>Necessárias</span><strong>144</strong></div></div><div class="field full"><label>Notas BQ</label><textarea>Garantir folga AN/BQ entre 0,04–0,06 mm.</textarea></div></div></div></article>
            <article class="tool"><div class="tool-title"><h4>PU</h4><span class="pill">Informação</span></div><div class="tool-body"><div class="mini-grid"><div class="field"><label>Referência</label><input value="5809"></div><div class="field"><label>Versão</label><input value="AP5809PA8L-B"></div><div class="quantity full"><div><span>Stock</span><strong>12</strong></div><div><span>Em máquina</span><strong>12</strong></div></div><div class="field full"><label>Notas PU</label><textarea>Procedimento NNPB. Confirmar marcação.</textarea></div></div></div></article>
            <article class="tool wide"><div class="tool-title"><h4>CAL</h4><div class="tool-title-actions"><span class="pill good">Completo</span><button class="btn compact edit-only" id="addCalRow" type="button">Adicionar linha</button></div></div><div class="tool-body"><table class="measure-table"><thead><tr><th>Elemento</th><th>Valor</th><th>Qtd. máquina</th><th class="edit-only">Ação</th></tr></thead><tbody id="calRows"><tr><td><input value="Bucha marcada" aria-label="Elemento CAL"></td><td><input value="5809" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr><tr><td><input value="CS P interna" aria-label="Elemento CAL"></td><td><input value="56,4 × 43,3" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr><tr><td><input value="Tampão" aria-label="Elemento CAL"></td><td><input value="34,95" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr><tr><td><input value="Inversão MF" aria-label="Elemento CAL"></td><td><input value="75,20 mm" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr><tr><td><input value="Inversão MP" aria-label="Elemento CAL"></td><td><input value="32,60 mm" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr><tr><td><input value="Nível" aria-label="Elemento CAL"></td><td><input value="37,60 mm" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr><tr><td><input value="Cabeça de sopro" aria-label="Elemento CAL"></td><td><input value="56,4 × 43,3" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr><tr><td><input value="Pinças" aria-label="Elemento CAL"></td><td><input value="P 43,9 / M 43,3" aria-label="Valor CAL"></td><td><input value="3" aria-label="Quantidade em máquina"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr></tbody></table></div></article>
            <article class="tool"><div class="tool-title"><h4>AN</h4><span class="pill">Informação</span></div><div class="tool-body"><div class="mini-grid"><div class="field full"><label>Referência</label><input value="T173"></div><div class="field full"><label>Notas AN</label><textarea>Chanfro. Garantir folga AN/BQ.</textarea></div></div></div></article>
            <article class="tool"><div class="tool-title"><h4>ARR</h4><span class="pill">Informação</span></div><div class="tool-body"><div class="mini-grid"><div class="field full"><label>Referência</label><input value="5809"></div><div class="field full"><label>Notas ARR</label><textarea>AR5809PA8L roscado 30 mm.</textarea></div></div></div></article>
            <article class="tool"><div class="tool-title"><h4>PI</h4><span class="pill good">Completo</span></div><div class="tool-body"><div class="mini-grid"><div class="field"><label>Pinças</label><select id="piClampMaterial" data-catalog="PI.clamp_material"><option>Latão</option><option>Grafite</option></select></div><div class="field"><label>Diâmetro</label><input value="44,00"></div><div class="field full"><label>Notas PI</label><textarea>Boleadas.</textarea></div></div></div></article>
            <article class="tool"><div class="tool-title"><h4>CS</h4><span class="pill">Informação</span></div><div class="tool-body"><div class="mini-grid"><div class="field"><label>Referência</label><input value="T173"></div><div class="field"><label>Furos</label><input value="4 × 4"></div><div class="field full"><label>Tubo</label><input value="À face"></div><div class="quantity full"><div><span>Stock</span><strong>9</strong></div><div><span>Em máquina</span><strong>3</strong></div></div><div class="field full"><label>Notas CS</label><textarea>Confirmar tubo à face.</textarea></div></div></div></article>
            <article class="tool"><div class="tool-title"><h4>TP</h4><span class="pill">Informação</span></div><div class="tool-body"><div class="mini-grid"><div class="field"><label>Diâmetro PS</label><input value="34,95"></div><div class="field"><label>Referência</label><input value="T173"></div><div class="field full"><label>Bacia PS</label><input value="6,70"></div><div class="quantity full"><div><span>Stock</span><strong>7</strong></div><div><span>Em máquina</span><strong>3</strong></div></div><div class="field full"><label>Notas TP</label><textarea>Maquinados.</textarea></div></div></div></article>
            <article class="tool"><div class="tool-title"><h4>FO</h4><span class="pill">Por preencher</span></div><div class="tool-body"><div class="mini-grid"><div class="field full"><label>Tipo</label><input placeholder="Introduzir tipo"></div><div class="quantity full"><div><span>Stock</span><strong>—</strong></div><div><span>Em máquina</span><strong>—</strong></div></div><div class="field full"><label>Notas FO</label><textarea placeholder="Observações"></textarea></div></div></div></article>
          </div>
          <div class="lower-grid"><section><div class="section-bar"><h3>Verificações</h3><span class="pill warn">2 pendentes</span></div><p class="muted">Confirmações manuais deste Job On. Só mudam de estado depois de o check ser guardado.</p><div class="checks"><label class="check"><input type="checkbox"><span><strong>MF · Verificar gargalo</strong><small>Regra da ficha MF · Por fabrico · Lote 01</small></span><span class="pill warn">Pendente</span></label><label class="check"><input type="checkbox"><span><strong>MF · Meter pernos nos fundos</strong><small>Regra da ficha MF · Uma vez no lote · Lote 01</small></span><span class="pill warn">Pendente</span></label><label class="check"><input type="checkbox" checked><span><strong>MP · Confirmar folga</strong><small>Confirmada manualmente por João Silva · 17/08/2026 14:32</small></span><span class="pill good">Confirmada</span></label></div></section><section class="notes"><div class="section-bar"><h3>Notas gerais</h3></div><div class="field"><textarea>Auditar código de pontos e gravações. Preparar boquilhas lavadas para entrar em máquina. Confirmar concordância MF–FF.</textarea></div></section></div>
          <section class="history-box"><div class="section-bar"><h3>Histórico desta produção</h3><span class="muted">Alterações, verificações e movimentos relacionados.</span></div><div class="table-wrap"><table><thead><tr><th>Data/hora</th><th>Evento</th><th>Ferramenta</th><th>Detalhe</th><th>Utilizador</th></tr></thead><tbody><tr><td>17/08 · 14:32</td><td>Verificação confirmada manualmente</td><td>MP · 5809 · Lote 01</td><td>Folga confirmada no Job On</td><td>João Silva</td></tr><tr><td>16/08 · 09:18</td><td>Rascunho criado</td><td>Job On</td><td>Duplicado da produção 202512</td><td>Rui Costa</td></tr></tbody></table></div></section>
        </div>
      </section>
    </section>
    <section class="view" id="historico">
      <div class="page-title"><h2>Histórico por Referência e Produção</h2><p class="muted">Filtre a Referência para ver todas as Produções. Um clique seleciona; duplo clique abre a revisão corrente e o respetivo histórico.</p></div>
      <div class="card day-card">
        <div class="filters" style="grid-template-columns:1.5fr 120px 110px 135px 135px 90px auto"><div class="field"><label>Referência</label><input value="7080C002" placeholder="Pesquisar Referência"></div><div class="field"><label>Produção</label><input placeholder="Todas"></div><div class="field"><label>Máquina</label><select><option>Todas</option><option>B1</option><option>B3</option><option>C3</option></select></div><div class="field"><label>Desde</label><input type="date"></div><div class="field"><label>Até</label><input type="date"></div><div class="field"><label>Linhas</label><select><option>20</option><option>40</option><option>60</option></select></div><button class="btn">Limpar</button></div>
        <div class="table-wrap"><table><thead><tr><th>Data início</th><th>Data fim</th><th>Referência</th><th>Produção</th><th>Máquina</th><th>Revisão corrente</th><th>Total revisões</th><th>Estado</th></tr></thead><tbody><tr data-row class="selected"><td>17/08/2026</td><td>20/08/2026</td><td><strong>7080C002</strong></td><td>202602</td><td>C3</td><td>3</td><td>3</td><td><span class="pill">Planeado</span></td></tr><tr data-row><td>08/04/2026</td><td>12/04/2026</td><td><strong>7080C002</strong></td><td>202601</td><td>C3</td><td>2</td><td>2</td><td><span class="pill good">Fechado</span></td></tr></tbody></table></div>
        <div class="list-foot"><span>2 Produções desta Referência · Página 1 de 1</span><div class="pager"><button class="btn icon" disabled>‹</button><span>1 / 1</span><button class="btn icon" disabled>›</button></div></div>
      </div>
    </section>
    <section class="view" id="definicoes">
      <div class="page-title"><h2>Definições do Job On</h2><p class="muted">Gestão dos valores disponíveis nos campos configuráveis.</p></div>
      <div class="card day-card">
        <div class="card-head"><div><h3>Opções dos campos</h3><p class="muted">As opções são partilhadas pelos novos Job Ons. Desativar não altera folhas nem revisões antigas.</p></div></div>
        <div class="filters" style="grid-template-columns:150px 190px minmax(220px,1fr) auto"><div class="field"><label>Família</label><select id="catalogFamily"><option>PI</option><option>MP</option><option>MF</option><option>BQ</option><option>PU</option><option>CS</option><option>TP</option><option>FO</option></select></div><div class="field"><label>Campo</label><select id="catalogField"><option value="clamp_material">Pinças — material</option></select></div><div class="field"><label>Nova opção</label><input id="newCatalogOption" placeholder="Ex.: Aço tratado"></div><button class="btn" id="addCatalogOption" type="button">Adicionar opção</button></div>
        <div class="table-wrap"><table><thead><tr><th>Ordem</th><th>Opção</th><th>Estado</th><th>Utilização</th></tr></thead><tbody id="catalogRows"><tr data-option-row class="selected"><td>1</td><td><strong>Latão</strong></td><td><span class="pill good">Ativa</span></td><td>Disponível em novos registos</td></tr><tr data-option-row><td>2</td><td><strong>Grafite</strong></td><td><span class="pill good">Ativa</span></td><td>Disponível em novos registos</td></tr></tbody></table></div>
        <div class="list-foot"><span>2 opções · Um clique seleciona</span><div class="pager"><button class="btn" id="editCatalogOption" type="button">Editar selecionada</button><button class="btn danger" id="disableCatalogOption" type="button">Desativar selecionada</button></div></div>
      </div>
    </section>
  </main>
  <script>
    const qs=(s,r=document)=>r.querySelector(s),qsa=(s,r=document)=>[...r.querySelectorAll(s)];
    const openView=id=>{qsa('.tab').forEach(x=>x.classList.toggle('active',x.dataset.view===id));qsa('.view').forEach(x=>x.classList.toggle('active',x.id===id));window.scrollTo({top:0,behavior:'smooth'})};
    qsa('.tab').forEach(tab=>tab.onclick=()=>openView(tab.dataset.view));
    const cal=qs('#calendar');for(let i=0;i<5;i++)cal.insertAdjacentHTML('beforeend','<button class="day blank"></button>');for(let d=1;d<=31;d++)cal.insertAdjacentHTML('beforeend',`<button class="day ${[6,10,14,18,21,25,28].includes(d)?'has':''} ${d===18?'selected':''}">${d}</button>`);
    const syncJobDatesToCalendar=()=>{const start=qs('#jobStartDate').value,end=qs('#jobEndDate').value;const startDay=start.startsWith('2026-08-')?Number(start.slice(-2)):null,endDay=end.startsWith('2026-08-')?Number(end.slice(-2)):null;qsa('.day:not(.blank)',cal).forEach(day=>{const value=Number(day.textContent);day.classList.toggle('in-range',Boolean(startDay&&endDay&&value>=startDay&&value<=endDay));day.classList.toggle('range-start',value===startDay);day.classList.toggle('range-end',value===endDay)})};qs('#jobStartDate').onchange=syncJobDatesToCalendar;qs('#jobEndDate').onchange=syncJobDatesToCalendar;syncJobDatesToCalendar();
    cal.onclick=e=>{if(!e.target.matches('.day:not(.blank)'))return;qsa('.day',cal).forEach(x=>x.classList.remove('selected'));e.target.classList.add('selected');qs('.day-card h3').textContent=`Job Ons de ${e.target.textContent} de agosto`;};
    qs('#newJob').onclick=()=>qs('#createPanel').classList.add('open');qs('#closeCreate').onclick=qs('#cancelCreate').onclick=()=>qs('#createPanel').classList.remove('open');
    qsa('.source').forEach(x=>x.onclick=()=>{qsa('.source').forEach(y=>y.classList.remove('selected'));x.classList.add('selected')});
    qsa('tr[data-row]').forEach(row=>{row.onclick=()=>{qsa('tr[data-row]').forEach(x=>x.classList.remove('selected'));row.classList.add('selected')};row.ondblclick=()=>openView('folha')});
    qs('#createDraft').onclick=()=>{qs('#createPanel').classList.remove('open');openView('folha')};
    qs('#backToPlanning').onclick=()=>openView('planeamento');
    const toolGrid=qs('.tool-grid');qsa('.tool',toolGrid).forEach(tool=>{const title=qs('h4',tool);const priority=['MP','MF','BQ'].includes(title.textContent);tool.classList.add(priority?'priority':'secondary');if(priority){const action=document.createElement('button');action.className='btn compact edit-only';action.textContent='Alterar';action.type='button';action.onclick=()=>{const family=title.textContent==='MP'?'CM':title.textContent;qs('#toolFamilyFilter').value=family;qs('#inventoryPickerTitle').textContent=`Alterar ${family} associado`;qs('#inventoryPicker').classList.add('open');qs('#inventoryPicker').scrollIntoView({behavior:'smooth',block:'center'})};qs('.tool-title',tool).appendChild(action)}});
    qsa('.quantity',toolGrid).forEach(group=>qsa(':scope > div',group).forEach(cell=>{const value=qs('strong',cell);if(!value)return;const input=document.createElement('input');input.type='number';input.min='0';input.step='1';input.value=value.textContent.trim()==='—'?'':value.textContent.trim();input.placeholder='—';input.setAttribute('aria-label',`${qs('span',cell).textContent} ${qs('h4',cell.closest('.tool')).textContent}`);value.replaceWith(input)}));
    qs('#jobImage').onchange=e=>{const file=e.target.files[0];if(!file)return;const preview=qs('#imagePreview');preview.innerHTML='';const img=document.createElement('img');img.src=URL.createObjectURL(file);img.alt='Imagem do artigo';preview.appendChild(img)};
    const sheet=qs('#jobSheet'),editSheet=qs('#editSheet'),saveSheet=qs('#saveSheet'),sheetMode=qs('#sheetMode'),canEditJobOn=document.body.dataset.canEditJobon==='true';editSheet.onclick=()=>{if(!canEditJobOn)return;const editing=sheet.classList.toggle('editing');editSheet.textContent=editing?'Cancelar edição':'Editar folha';saveSheet.hidden=!editing;sheetMode.textContent=editing?'Modo edição':'Modo consulta';if(!editing)qs('#inventoryPicker').classList.remove('open')};saveSheet.onclick=()=>{if(!canEditJobOn)return;sheet.classList.remove('editing');qs('#inventoryPicker').classList.remove('open');editSheet.textContent='Editar folha';saveSheet.hidden=true;sheetMode.textContent='Modo consulta'};
    const calRows=qs('#calRows');qs('#addCalRow').onclick=()=>calRows.insertAdjacentHTML('beforeend','<tr><td><input aria-label="Elemento CAL" placeholder="Novo elemento"></td><td><input aria-label="Valor CAL" placeholder="Valor"></td><td><input aria-label="Quantidade em máquina" placeholder="0"></td><td class="edit-only"><button class="btn compact danger cal-remove" type="button">Remover</button></td></tr>');calRows.onclick=e=>{if(e.target.matches('.cal-remove')&&sheet.classList.contains('editing'))e.target.closest('tr').remove()};
    const catalogRows=qs('#catalogRows'),catalogSelect=qs('#piClampMaterial'),catalogInput=qs('#newCatalogOption'),catalogSave=qs('#addCatalogOption');let editingCatalogRow=null;const selectCatalogRow=row=>{qsa('[data-option-row]',catalogRows).forEach(item=>item.classList.toggle('selected',item===row))};catalogRows.onclick=e=>{const row=e.target.closest('[data-option-row]');if(row)selectCatalogRow(row)};catalogSave.onclick=()=>{const label=catalogInput.value.trim();if(!label)return;const duplicate=qsa('strong',catalogRows).some(item=>item.textContent.toLocaleLowerCase('pt-PT')===label.toLocaleLowerCase('pt-PT')&&item.closest('tr')!==editingCatalogRow);if(duplicate){catalogInput.focus();return}if(editingCatalogRow){const current=qs('strong',editingCatalogRow).textContent;qs('strong',editingCatalogRow).textContent=label;const option=qsa('option',catalogSelect).find(item=>item.value===current);if(option){option.value=label;option.textContent=label}editingCatalogRow=null;catalogSave.textContent='Adicionar opção'}else{const order=qsa('[data-option-row]',catalogRows).length+1;catalogRows.insertAdjacentHTML('beforeend',`<tr data-option-row><td>${order}</td><td><strong></strong></td><td><span class="pill good">Ativa</span></td><td>Disponível em novos registos</td></tr>`);const row=catalogRows.lastElementChild;qs('strong',row).textContent=label;catalogSelect.add(new Option(label,label));selectCatalogRow(row)}catalogInput.value=''};qs('#editCatalogOption').onclick=()=>{const row=qs('[data-option-row].selected',catalogRows);if(!row)return;editingCatalogRow=row;catalogInput.value=qs('strong',row).textContent;catalogSave.textContent='Guardar alteração';catalogInput.focus()};qs('#disableCatalogOption').onclick=()=>{const row=qs('[data-option-row].selected',catalogRows);if(!row)return;const label=qs('strong',row).textContent;qs('.pill',row).className='pill';qs('.pill',row).textContent='Inativa';row.lastElementChild.textContent='Mantida apenas no histórico';qsa('option',catalogSelect).find(option=>option.value===label)?.remove()};
  </script>
</body>
</html>

```
## END FILE CONTENT

---

# FILE 026

## Source Path
`login.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Entrar — Portal DMO
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (0): 
<header>: 0
<nav>: 0
<aside>: 0
<main>: 1
<section>: 2
<footer>: 0
<form>: 1
UNIQUE IDS (6): loginForm, email, password, passwordToggle, submitButton, formMessage
DATA-* ATTRIBUTES (0): 
<button: 2
<input: 2
<select: 0
<textarea: 0
<table: 0
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html><html lang="pt-PT"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>Entrar — Portal DMO</title><link rel="stylesheet" href="dmo-design-system.css"><style>
body{min-height:100vh;display:grid;place-items:center;padding:28px}.login-shell{width:min(1120px,100%);min-height:650px;display:grid;grid-template-columns:38% 62%;overflow:hidden}.identity{position:relative;padding:50px 44px;background:linear-gradient(155deg,var(--dmo-brand-050),#c7d9eb);display:flex;flex-direction:column}.brand-logo{width:78px;height:78px;border-radius:50%;object-fit:cover;margin-bottom:22px}.eyebrow{font-size:12px;font-weight:850;letter-spacing:.14em;color:var(--dmo-brand-600)}.identity h1{margin:8px 0 4px;font-size:32px;color:var(--dmo-brand-950)}.identity p{margin:0;color:var(--dmo-brand-800)}.identity-foot{margin-top:auto;color:var(--dmo-muted);font-size:11px}.identity-foot strong{display:block;margin-top:12px;color:var(--dmo-brand-700)}.login-side{display:flex;align-items:center;justify-content:center;padding:58px}.login-form{width:min(410px,100%)}.login-form h2{font-size:28px;margin:0 0 6px}.login-form>.lead{margin:0 0 18px;color:var(--dmo-muted)}.notice{display:flex;gap:10px;background:var(--dmo-brand-050);border-left:4px solid var(--dmo-brand-600);padding:11px 13px;border-radius:8px;margin-bottom:22px}.notice strong{display:block;font-size:12px}.notice span{font-size:11px;color:var(--dmo-muted)}.login-form .dmo-field{margin-bottom:15px}.password-wrap{position:relative}.password-wrap input{padding-right:78px}.password-toggle{position:absolute;right:7px;top:5px;min-height:30px;padding:4px 9px}.submit{width:100%;min-height:44px;margin-top:4px}.form-message{display:none;margin-top:12px;padding:10px 12px;border-radius:8px;background:var(--dmo-danger-soft);color:var(--dmo-danger);font-size:12px}.form-message.show{display:block}
@media(max-width:760px){body{padding:0}.login-shell{min-height:100vh;grid-template-columns:1fr;border:0;border-radius:0}.identity{padding:22px 24px;display:grid;grid-template-columns:auto 1fr;gap:14px;align-items:center}.brand-logo{width:56px;height:56px;margin:0}.identity .copy h1{font-size:22px;margin:2px 0}.identity .copy p,.identity-foot{display:none}.login-side{padding:36px 24px;align-items:start}}
</style></head><body><main class="login-shell dmo-card"><section class="identity"><img class="brand-logo" src="logo_recolored(1).png" alt="BA Glass"><div class="copy"><div class="eyebrow">BA GLASS</div><h1>Portal DMO</h1><p>Aplicações do Departamento DMO</p></div><div class="identity-foot">Acesso reservado a utilizadores autorizados.<strong>Portal DMO · Desenvolvido por Diogo Oliveira · 2026</strong></div></section><section class="login-side"><form class="login-form" id="loginForm"><h2>Entrar</h2><p class="lead">Introduza as suas credenciais para aceder ao Portal DMO.</p><div class="notice"><b>ⓘ</b><div><strong>Ambiente de testes</strong><span>Os dados introduzidos podem não representar produção.</span></div></div><div class="dmo-field"><label for="email">Email</label><input id="email" type="email" autocomplete="username" placeholder="nome@empresa.com" required></div><div class="dmo-field"><label for="password">Palavra-passe</label><div class="password-wrap"><input id="password" type="password" autocomplete="current-password" required><button class="dmo-button password-toggle" id="passwordToggle" type="button">Mostrar</button></div></div><button class="dmo-button submit" id="submitButton">Entrar</button><div class="form-message" id="formMessage" role="alert">Preencha o email e a palavra-passe.</div></form></section></main><script>
const password=document.querySelector('#password'),toggle=document.querySelector('#passwordToggle'),form=document.querySelector('#loginForm'),submit=document.querySelector('#submitButton'),message=document.querySelector('#formMessage');toggle.onclick=()=>{const show=password.type==='password';password.type=show?'text':'password';toggle.textContent=show?'Ocultar':'Mostrar'};form.onsubmit=e=>{e.preventDefault();if(!form.checkValidity()){message.classList.add('show');return}message.classList.remove('show');submit.disabled=true;submit.textContent='A entrar…';setTimeout(()=>{submit.disabled=false;submit.textContent='Entrar';location.href='job-on.html'},700)};
</script></body></html>

```
## END FILE CONTENT

---

# FILE 027

## Source Path
`moldes.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Moldes — Portal DMO
EXTERNAL CSS (0): 
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 10
<footer>: 0
<form>: 0
UNIQUE IDS (16): registo, newList, listForm, formType, saveList, rep-cm-42, rep-mf-41, correct, ferramentas, lot-cm-1, historico, historyList, hist-cm-1, historyCorrect, definicoes, toast
DATA-* ATTRIBUTES (3): view, type, id
<button: 23
<input: 8
<select: 13
<textarea: 0
<table: 4
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Moldes — Portal DMO</title>
    <style>
      :root {
        --b: #3c73a8;
        --bd: #193046;
        --bs: #e8eff7;
        --page: #f6f9fc;
        --sub: #f1f6fa;
        --line: #d9e6f2;
        --text: #172d42;
        --muted: #64778a;
        --green: #527c72;
        --green-s: #e5f0eb;
        --warn: #a97943;
        --warn-s: #f7f0e7;
        --red: #9a625d;
        --disabled: #cbd5df;
        --r: 12px;
        --shadow: 0 8px 24px rgba(25, 48, 70, 0.06);
      }
      * {
        box-sizing: border-box;
      }
      body {
        margin: 0;
        background: var(--page);
        color: var(--text);
        font:
          14px/1.45 Inter,
          "Segoe UI",
          sans-serif;
      }
      button,
      input,
      select,
      textarea {
        font: inherit;
      }
      .header {
        min-height: 76px;
        display: flex;
        align-items: center;
        gap: 13px;
        padding: 10px 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .logo {
        width: 44px;
        height: 44px;
        object-fit: contain;
        border-radius: 50%;
      }
      .head h1 {
        margin: 0;
        font-size: 18px;
      }
      .head p,
      .muted {
        margin: 3px 0 0;
        color: var(--muted);
        font-size: 11px;
      }
      .user {
        margin-left: auto;
        padding-left: 18px;
        text-align: right;
        border-left: 1px solid var(--line);
      }
      .user strong,
      .user span {
        display: block;
      }
      .user span {
        font-size: 11px;
        color: var(--muted);
      }
      .tabs {
        height: 52px;
        display: flex;
        gap: 25px;
        padding: 0 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .tab {
        border: 0;
        border-bottom: 3px solid transparent;
        background: none;
        color: var(--muted);
        font-weight: 750;
        cursor: pointer;
      }
      .tab.active {
        color: var(--b);
        border-color: var(--b);
      }
      .tab.settings {
        margin-left: auto;
      }
      .main {
        max-width: 1360px;
        margin: auto;
        padding: 26px;
      }
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }
      .page-head,
      .panel-head,
      .footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
      }
      .page-head {
        align-items: end;
        margin-bottom: 16px;
      }
      .page-head h2,
      .panel-head h3 {
        margin: 0;
      }
      .card {
        padding: 18px;
        background: #fff;
        border: 1px solid var(--line);
        border-radius: var(--r);
        box-shadow: var(--shadow);
      }
      .btn {
        min-height: 36px;
        padding: 7px 12px;
        border: 1px solid var(--b);
        border-radius: 8px;
        background: var(--b);
        color: #fff;
        font-weight: 750;
        cursor: pointer;
      }
      .btn:hover,
      .btn:focus-visible {
        background: #fff;
        color: var(--b);
        outline: none;
      }
      .btn:disabled {
        border-color: var(--disabled);
        background: var(--disabled);
        color: #fff;
        cursor: not-allowed;
      }
      .btn.icon {
        width: 36px;
        padding: 0;
      }
      .type-switch {
        display: flex;
        gap: 8px;
      }
      .type-switch .btn {
        background: #fff;
        color: var(--b);
      }
      .type-switch .btn:hover,
      .type-switch .btn:focus-visible,
      .type-switch .btn.active {
        background: var(--b);
        color: #fff;
      }
      .production-grid {
        display: grid;
        grid-template-columns: repeat(6, minmax(0, 1fr));
        gap: 8px;
      }
      .production-card {
        min-width: 0;
        padding: 11px;
        border: 1px solid var(--b);
        border-radius: 9px;
        background: var(--b);
        color: #fff;
        text-align: left;
        cursor: pointer;
      }
      .production-card:hover,
      .production-card.active {
        background: #fff;
        color: var(--b);
      }
      .production-card strong,
      .production-card span,
      .production-card small {
        display: block;
      }
      .production-card strong {
        font-size: 13px;
      }
      .production-card span {
        margin-top: 3px;
        font-weight: 750;
      }
      .production-card small {
        margin-top: 2px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        opacity: 0.82;
      }
      .search {
        display: grid;
        grid-template-columns: 2fr 150px 150px 100px auto;
        gap: 9px;
        align-items: end;
        margin-top: 16px;
      }
      .field label {
        display: block;
        margin-bottom: 6px;
        color: var(--muted);
        font-size: 11px;
        font-weight: 750;
      }
      .field input,
      .field select,
      .field textarea {
        width: 100%;
        min-height: 40px;
        padding: 9px 11px;
        border: 1px solid var(--line);
        border-radius: 8px;
        background: #fff;
        color: var(--text);
      }
      .search > .btn,
      .filters > .btn {
        min-height: 40px;
      }
      .context {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 8px;
        margin-top: 16px;
      }
      .context div {
        padding: 11px;
        border-left: 3px solid var(--b);
        border-radius: 8px;
        background: var(--sub);
      }
      .context span {
        display: block;
        color: var(--muted);
        font-size: 9px;
      }
      .context strong {
        font-size: 14px;
      }
      .actions {
        display: flex;
        justify-content: flex-end;
        gap: 8px;
        margin-top: 14px;
      }
      .inline {
        display: none;
        margin-top: 16px;
      }
      .inline.open {
        display: block;
      }
      .form-grid {
        display: grid;
        grid-template-columns: 2fr 120px 120px 150px;
        gap: 10px;
        align-items: end;
      }
      .table-wrap {
        overflow: auto;
        margin-top: 14px;
        border: 1px solid var(--line);
        border-radius: 10px;
      }
      table {
        width: 100%;
        border-collapse: collapse;
        white-space: nowrap;
      }
      th {
        padding: 10px 12px;
        background: var(--sub);
        color: var(--muted);
        font-size: 10px;
        text-align: left;
        text-transform: uppercase;
      }
      td {
        padding: 11px 12px;
        border-top: 1px solid var(--line);
      }
      tr[data-dmo-row] {
        cursor: pointer;
      }
      tr[data-dmo-row]:hover {
        background: var(--page);
      }
      tr.selected {
        background: var(--line);
      }
      .pill {
        display: inline-flex;
        padding: 4px 8px;
        border-radius: 99px;
        background: var(--bs);
        color: #315d88;
        font-size: 10px;
        font-weight: 800;
      }
      .pill.green {
        background: var(--green-s);
        color: var(--green);
      }
      .footer {
        padding-top: 11px;
        color: var(--muted);
        font-size: 11px;
      }
      .pager {
        display: flex;
        align-items: center;
        gap: 7px;
      }
      .filters {
        display: grid;
        grid-template-columns: 2fr repeat(4, 150px) 100px auto;
        gap: 9px;
        align-items: end;
      }
      .split {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 16px;
      }
      .toast {
        position: fixed;
        right: 22px;
        bottom: 22px;
        padding: 11px 15px;
        border-radius: 9px;
        background: #0f1d2a;
        color: #fff;
        opacity: 0;
        transform: translateY(50px);
        transition: 0.2s;
      }
      .toast.show {
        opacity: 1;
        transform: none;
      }
      @media (max-width: 950px) {
        .production-grid {
          grid-template-columns: repeat(3, minmax(0, 1fr));
        }
        .filters {
          grid-template-columns: repeat(3, 1fr);
        }
        .split {
          grid-template-columns: 1fr;
        }
      }
      @media (max-width: 650px) {
        .header {
          padding: 10px 14px;
        }
        .user {
          display: none;
        }
        .tabs {
          padding: 0 12px;
          gap: 14px;
          overflow: auto;
        }
        .tab {
          white-space: nowrap;
        }
        .tab.settings {
          margin-left: 0;
        }
        .main {
          padding: 16px 12px;
        }
        .production-grid {
          grid-template-columns: repeat(2, minmax(0, 1fr));
        }
        .search,
        .form-grid,
        .filters,
        .context {
          grid-template-columns: 1fr 1fr;
        }
        .search .btn,
        .filters .btn {
          grid-column: 1/-1;
        }
        .table-wrap table {
          min-width: 920px;
        }
        .footer {
          align-items: flex-start;
          flex-direction: column;
        }
        .pager {
          align-self: flex-end;
        }
        .actions {
          display: grid;
          grid-template-columns: 1fr 1fr;
        }
        .actions .btn {
          min-height: 44px;
        }
      }
    </style>
  </head>
  <body>
    <header class="header">
      <img class="logo" src="logo_recolored(1).png" alt="BA" />
      <div class="head">
        <h1>Moldes</h1>
        <p>Contra moldes e Moldes finais</p>
      </div>
      <div class="user">
        <strong>Ana Martins</strong><span>Responsável de moldes</span>
      </div>
    </header>
    <nav class="tabs">
      <button class="tab active" data-view="registo">Registo</button
      ><button class="tab" data-view="ferramentas">Ferramentas</button
      ><button class="tab" data-view="historico">Histórico</button
      ><button class="tab settings" data-view="definicoes">Definições</button>
    </nav>
    <main class="main">
      <section class="view active" id="registo">
        <div class="page-head">
          <div>
            <h2>Preparar reparação de Moldes</h2>
            <p class="muted">Crie antecipadamente a lista de ferramentas para uma produção futura.</p>
          </div>
          <div class="type-switch">
            <button class="btn active" data-type="CM">Contra moldes</button
            ><button class="btn" data-type="MF">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="search">
            <div class="field">
              <label>Procurar Referência, lote ou número individual</label><input placeholder="Ex.: 5774T173, lote 01 ou CM 34" />
            </div>
            <div class="field">
              <label>Período</label><select><option>Próximos 30 dias</option><option>Próximos 60 dias</option></select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
                <option>C2</option>
                <option>C3</option>
              </select>
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Pesquisar</button>
          </div>
          <div class="actions">
            <button class="btn" id="newList">Criar lista de reparação</button>
          </div>
          <div class="card inline" id="listForm">
            <div class="panel-head">
              <div>
                <h3>Nova lista de reparação · <span id="formType">CM</span></h3>
                <p class="muted">A produção, Referência e máquina vêm do planeamento selecionado.</p>
              </div>
              <button class="btn icon" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="field"><label>Produção prevista</label><select><option>202604 · 5774T173 · B1 · 22/08</option><option>202605 · 9389T194 · B3 · 24/08</option></select></div>
              <div class="field">
                <label>Reparador</label><select><option>Externo A</option><option>Externo B</option></select>
              </div>
              <div class="field">
                <label>Ferramentas</label><input value="15 selecionadas" readonly />
              </div>
              <div class="field">
                <label>Enviar até</label><input type="date" value="2026-08-19" />
              </div>
            </div>
            <div class="actions">
              <button class="btn" id="saveList">Criar lista</button>
            </div>
          </div>
        </section>
        <section class="card" style="margin-top: 16px">
          <div class="panel-head">
            <div>
              <h3>Listas de reparação programadas</h3>
              <p class="muted">Um clique seleciona; duplo clique abre a lista completa.</p>
            </div>
            <div class="field" style="width: 100px">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Lista</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Produção</th><th>Linha</th><th>Início produção</th><th>Reparador</th><th>Itens</th><th>Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="rep-cm-42"><td><strong>REP-CM-0042</strong></td><td>CM</td><td><strong>5774T173</strong></td><td>202604</td><td>B1</td><td>22/08</td><td>Externo A</td><td>15</td><td><span class="pill">A retirar</span></td></tr>
                <tr data-dmo-row data-id="rep-mf-41"><td><strong>REP-MF-0041</strong></td><td>MF</td><td><strong>9389T194</strong></td><td>202605</td><td>B3</td><td>24/08</td><td>Externo B</td><td>12</td><td><span class="pill green">Enviado</span></td></tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 listas · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" disabled id="correct">Editar lista</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="ferramentas">
        <div class="page-head">
          <div>
            <h2>Ferramentas e lotes</h2>
            <p class="muted">
              CM e MF usam listas separadas, com o mesmo comportamento canónico.
            </p>
          </div>
          <div class="type-switch">
            <button class="btn active">Contra moldes</button
            ><button class="btn">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote ou número</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Estado</label
              ><select>
                <option>Todos</option>
                <option>Produção</option>
                <option>Armazém</option>
                <option>Reparação</option>
              </select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>Todas</option>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
              </select>
            </div>
            <div class="field">
              <label>Lote</label><input placeholder="Todos" />
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>Tipo</th>
                  <th>Quantidade</th>
                  <th>Em produção</th>
                  <th>Armazém</th>
                  <th>Reparação</th>
                  <th>Máquinas</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="lot-cm-1">
                  <td><strong>5774T173</strong></td>
                  <td>01</td>
                  <td>CM</td>
                  <td>96</td>
                  <td>24</td>
                  <td>60</td>
                  <td>12</td>
                  <td>B1 · B3</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 lote · Página 1 de 1</span>
            <div class="pager">
              <button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="historico">
        <div class="page-head">
          <div>
            <h2>Histórico de Moldes</h2>
            <p class="muted">Consulta e comparação de movimentos CM/MF.</p>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote, número ou operador</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>Todos</option>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Movimento</label
              ><select>
                <option>Todos</option>
                <option>Entrada</option>
                <option>Saída</option>
              </select>
            </div>
            <div class="field"><label>Desde</label><input type="date" /></div>
            <div class="field"><label>Até</label><input type="date" /></div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list id="historyList">
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>N.º</th>
                  <th>Movimento</th>
                  <th>Origem</th>
                  <th>Destino</th>
                  <th>Linha</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="hist-cm-1">
                  <td>18/08 · 14:32</td>
                  <td>CM</td>
                  <td>5774T173</td>
                  <td>01</td>
                  <td>34</td>
                  <td>Entrada</td>
                  <td>Produção</td>
                  <td>Armazém</td>
                  <td>B1</td>
                  <td>João Silva</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 movimento · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" id="historyCorrect" disabled>
                Corrigir movimento</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="definicoes">
        <div class="page-head">
          <div>
            <h2>Definições de Moldes</h2>
            <p class="muted">Reparadores e associações por tipo e máquina.</p>
          </div>
        </div>
        <div class="split">
          <section class="card">
            <div class="panel-head">
              <h3>Reparadores</h3>
              <button class="btn">Adicionar reparador</button>
            </div>
            <div class="table-wrap">
              <table data-dmo-list>
                <thead>
                  <tr>
                    <th>Nome</th>
                    <th>Tipo</th>
                    <th>Máquinas</th>
                    <th>Estado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr data-dmo-row>
                    <td>Externo A</td>
                    <td>CM</td>
                    <td>B1 · B3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                  <tr data-dmo-row>
                    <td>Externo B</td>
                    <td>MF</td>
                    <td>C1 · C3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
          <section class="card">
            <div class="panel-head"><h3>Regra de separação</h3></div>
            <p>
              Contra moldes e Moldes finais podem partilhar uma Referência de
              produção, mas mantêm:
            </p>
            <ul>
              <li>tipos e IDs diferentes;</li>
              <li>lotes e números individuais próprios;</li>
              <li>movimentos e históricos separados;</li>
              <li>reparadores permitidos independentes.</li>
            </ul>
          </section>
        </div>
      </section>
    </main>
    <div class="toast" id="toast"></div>
    <script>
      const $ = (s) => document.querySelector(s),
        $$ = (s) => [...document.querySelectorAll(s)],
        say = (t) => {
          const x = $("#toast");
          x.textContent = t;
          x.classList.add("show");
          setTimeout(() => x.classList.remove("show"), 2200);
        };
      $$(".tab").forEach(
        (t) =>
          (t.onclick = () => {
            $$(".tab,.view").forEach((x) => x.classList.remove("active"));
            t.classList.add("active");
            $("#" + t.dataset.view).classList.add("active");
          }),
      );
      $$("[data-type]").forEach(
        (b) =>
          (b.onclick = () => {
            $$("[data-type]").forEach((x) => x.classList.remove("active"));
            b.classList.add("active");
            $("#formType").textContent = b.dataset.type;
          }),
      );
      $("#newList").onclick = () => $("#listForm").classList.add("open");
      $$("[data-close]").forEach(
        (b) =>
          (b.onclick = () => b.closest(".inline").classList.remove("open")),
      );
      $("#saveList").onclick = () => {
        $("#listForm").classList.remove("open");
        say("Lista de reparação preparada");
      };
      document.addEventListener("click", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) {
          const table = row.closest("[data-dmo-list]");
          table
            .querySelectorAll("[data-dmo-row]")
            .forEach((x) => x.classList.remove("selected"));
          row.classList.add("selected");
          $("#correct").disabled = false;
          if (table.id === "historyList") $("#historyCorrect").disabled = false;
        }
      });
      document.addEventListener("dblclick", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) say("Abrir detalhe " + row.dataset.id);
      });
    </script>
  </body>
</html>

```
## END FILE CONTENT

---

# FILE 028

## Source Path
`moldes-v42-listas.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Moldes — Portal DMO
EXTERNAL CSS (0): 
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 10
<footer>: 0
<form>: 0
UNIQUE IDS (16): registo, newList, listForm, formType, saveList, rep-cm-42, rep-mf-41, correct, ferramentas, lot-cm-1, historico, historyList, hist-cm-1, historyCorrect, definicoes, toast
DATA-* ATTRIBUTES (3): view, type, id
<button: 23
<input: 8
<select: 13
<textarea: 0
<table: 4
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Moldes — Portal DMO</title>
    <style>
      :root {
        --b: #3c73a8;
        --bd: #193046;
        --bs: #e8eff7;
        --page: #f6f9fc;
        --sub: #f1f6fa;
        --line: #d9e6f2;
        --text: #172d42;
        --muted: #64778a;
        --green: #527c72;
        --green-s: #e5f0eb;
        --warn: #a97943;
        --warn-s: #f7f0e7;
        --red: #9a625d;
        --disabled: #cbd5df;
        --r: 12px;
        --shadow: 0 8px 24px rgba(25, 48, 70, 0.06);
      }
      * {
        box-sizing: border-box;
      }
      body {
        margin: 0;
        background: var(--page);
        color: var(--text);
        font:
          14px/1.45 Inter,
          "Segoe UI",
          sans-serif;
      }
      button,
      input,
      select,
      textarea {
        font: inherit;
      }
      .header {
        min-height: 76px;
        display: flex;
        align-items: center;
        gap: 13px;
        padding: 10px 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .logo {
        width: 44px;
        height: 44px;
        object-fit: contain;
        border-radius: 50%;
      }
      .head h1 {
        margin: 0;
        font-size: 18px;
      }
      .head p,
      .muted {
        margin: 3px 0 0;
        color: var(--muted);
        font-size: 11px;
      }
      .user {
        margin-left: auto;
        padding-left: 18px;
        text-align: right;
        border-left: 1px solid var(--line);
      }
      .user strong,
      .user span {
        display: block;
      }
      .user span {
        font-size: 11px;
        color: var(--muted);
      }
      .tabs {
        height: 52px;
        display: flex;
        gap: 25px;
        padding: 0 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .tab {
        border: 0;
        border-bottom: 3px solid transparent;
        background: none;
        color: var(--muted);
        font-weight: 750;
        cursor: pointer;
      }
      .tab.active {
        color: var(--b);
        border-color: var(--b);
      }
      .tab.settings {
        margin-left: auto;
      }
      .main {
        max-width: 1360px;
        margin: auto;
        padding: 26px;
      }
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }
      .page-head,
      .panel-head,
      .footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
      }
      .page-head {
        align-items: end;
        margin-bottom: 16px;
      }
      .page-head h2,
      .panel-head h3 {
        margin: 0;
      }
      .card {
        padding: 18px;
        background: #fff;
        border: 1px solid var(--line);
        border-radius: var(--r);
        box-shadow: var(--shadow);
      }
      .btn {
        min-height: 36px;
        padding: 7px 12px;
        border: 1px solid var(--b);
        border-radius: 8px;
        background: var(--b);
        color: #fff;
        font-weight: 750;
        cursor: pointer;
      }
      .btn:hover,
      .btn:focus-visible {
        background: #fff;
        color: var(--b);
        outline: none;
      }
      .btn:disabled {
        border-color: var(--disabled);
        background: var(--disabled);
        color: #fff;
        cursor: not-allowed;
      }
      .btn.icon {
        width: 36px;
        padding: 0;
      }
      .type-switch {
        display: flex;
        gap: 8px;
      }
      .type-switch .btn {
        background: #fff;
        color: var(--b);
      }
      .type-switch .btn:hover,
      .type-switch .btn:focus-visible,
      .type-switch .btn.active {
        background: var(--b);
        color: #fff;
      }
      .production-grid {
        display: grid;
        grid-template-columns: repeat(6, minmax(0, 1fr));
        gap: 8px;
      }
      .production-card {
        min-width: 0;
        padding: 11px;
        border: 1px solid var(--b);
        border-radius: 9px;
        background: var(--b);
        color: #fff;
        text-align: left;
        cursor: pointer;
      }
      .production-card:hover,
      .production-card.active {
        background: #fff;
        color: var(--b);
      }
      .production-card strong,
      .production-card span,
      .production-card small {
        display: block;
      }
      .production-card strong {
        font-size: 13px;
      }
      .production-card span {
        margin-top: 3px;
        font-weight: 750;
      }
      .production-card small {
        margin-top: 2px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        opacity: 0.82;
      }
      .search {
        display: grid;
        grid-template-columns: 2fr 150px 150px 100px auto;
        gap: 9px;
        align-items: end;
        margin-top: 16px;
      }
      .field label {
        display: block;
        margin-bottom: 6px;
        color: var(--muted);
        font-size: 11px;
        font-weight: 750;
      }
      .field input,
      .field select,
      .field textarea {
        width: 100%;
        min-height: 40px;
        padding: 9px 11px;
        border: 1px solid var(--line);
        border-radius: 8px;
        background: #fff;
        color: var(--text);
      }
      .search > .btn,
      .filters > .btn {
        min-height: 40px;
      }
      .context {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 8px;
        margin-top: 16px;
      }
      .context div {
        padding: 11px;
        border-left: 3px solid var(--b);
        border-radius: 8px;
        background: var(--sub);
      }
      .context span {
        display: block;
        color: var(--muted);
        font-size: 9px;
      }
      .context strong {
        font-size: 14px;
      }
      .actions {
        display: flex;
        justify-content: flex-end;
        gap: 8px;
        margin-top: 14px;
      }
      .inline {
        display: none;
        margin-top: 16px;
      }
      .inline.open {
        display: block;
      }
      .form-grid {
        display: grid;
        grid-template-columns: 2fr 120px 120px 150px;
        gap: 10px;
        align-items: end;
      }
      .table-wrap {
        overflow: auto;
        margin-top: 14px;
        border: 1px solid var(--line);
        border-radius: 10px;
      }
      table {
        width: 100%;
        border-collapse: collapse;
        white-space: nowrap;
      }
      th {
        padding: 10px 12px;
        background: var(--sub);
        color: var(--muted);
        font-size: 10px;
        text-align: left;
        text-transform: uppercase;
      }
      td {
        padding: 11px 12px;
        border-top: 1px solid var(--line);
      }
      tr[data-dmo-row] {
        cursor: pointer;
      }
      tr[data-dmo-row]:hover {
        background: var(--page);
      }
      tr.selected {
        background: var(--line);
      }
      .pill {
        display: inline-flex;
        padding: 4px 8px;
        border-radius: 99px;
        background: var(--bs);
        color: #315d88;
        font-size: 10px;
        font-weight: 800;
      }
      .pill.green {
        background: var(--green-s);
        color: var(--green);
      }
      .footer {
        padding-top: 11px;
        color: var(--muted);
        font-size: 11px;
      }
      .pager {
        display: flex;
        align-items: center;
        gap: 7px;
      }
      .filters {
        display: grid;
        grid-template-columns: 2fr repeat(4, 150px) 100px auto;
        gap: 9px;
        align-items: end;
      }
      .split {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 16px;
      }
      .toast {
        position: fixed;
        right: 22px;
        bottom: 22px;
        padding: 11px 15px;
        border-radius: 9px;
        background: #0f1d2a;
        color: #fff;
        opacity: 0;
        transform: translateY(50px);
        transition: 0.2s;
      }
      .toast.show {
        opacity: 1;
        transform: none;
      }
      @media (max-width: 950px) {
        .production-grid {
          grid-template-columns: repeat(3, minmax(0, 1fr));
        }
        .filters {
          grid-template-columns: repeat(3, 1fr);
        }
        .split {
          grid-template-columns: 1fr;
        }
      }
      @media (max-width: 650px) {
        .header {
          padding: 10px 14px;
        }
        .user {
          display: none;
        }
        .tabs {
          padding: 0 12px;
          gap: 14px;
          overflow: auto;
        }
        .tab {
          white-space: nowrap;
        }
        .tab.settings {
          margin-left: 0;
        }
        .main {
          padding: 16px 12px;
        }
        .production-grid {
          grid-template-columns: repeat(2, minmax(0, 1fr));
        }
        .search,
        .form-grid,
        .filters,
        .context {
          grid-template-columns: 1fr 1fr;
        }
        .search .btn,
        .filters .btn {
          grid-column: 1/-1;
        }
        .table-wrap table {
          min-width: 920px;
        }
        .footer {
          align-items: flex-start;
          flex-direction: column;
        }
        .pager {
          align-self: flex-end;
        }
        .actions {
          display: grid;
          grid-template-columns: 1fr 1fr;
        }
        .actions .btn {
          min-height: 44px;
        }
      }
    </style>
  </head>
  <body>
    <header class="header">
      <img class="logo" src="logo_recolored(1).png" alt="BA" />
      <div class="head">
        <h1>Moldes</h1>
        <p>Contra moldes e Moldes finais</p>
      </div>
      <div class="user">
        <strong>Ana Martins</strong><span>Responsável de moldes</span>
      </div>
    </header>
    <nav class="tabs">
      <button class="tab active" data-view="registo">Registo</button
      ><button class="tab" data-view="ferramentas">Ferramentas</button
      ><button class="tab" data-view="historico">Histórico</button
      ><button class="tab settings" data-view="definicoes">Definições</button>
    </nav>
    <main class="main">
      <section class="view active" id="registo">
        <div class="page-head">
          <div>
            <h2>Preparar reparação de Moldes</h2>
            <p class="muted">Crie antecipadamente a lista de ferramentas para uma produção futura.</p>
          </div>
          <div class="type-switch">
            <button class="btn active" data-type="CM">Contra moldes</button
            ><button class="btn" data-type="MF">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="search">
            <div class="field">
              <label>Procurar Referência, lote ou número individual</label><input placeholder="Ex.: 5774T173, lote 01 ou CM 34" />
            </div>
            <div class="field">
              <label>Período</label><select><option>Próximos 30 dias</option><option>Próximos 60 dias</option></select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
                <option>C2</option>
                <option>C3</option>
              </select>
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Pesquisar</button>
          </div>
          <div class="actions">
            <button class="btn" id="newList">Criar lista de reparação</button>
          </div>
          <div class="card inline" id="listForm">
            <div class="panel-head">
              <div>
                <h3>Nova lista de reparação · <span id="formType">CM</span></h3>
                <p class="muted">A produção, Referência e máquina vêm do planeamento selecionado.</p>
              </div>
              <button class="btn icon" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="field"><label>Produção prevista</label><select><option>202604 · 5774T173 · B1 · 22/08</option><option>202605 · 9389T194 · B3 · 24/08</option></select></div>
              <div class="field">
                <label>Reparador</label><select><option>Externo A</option><option>Externo B</option></select>
              </div>
              <div class="field">
                <label>Ferramentas</label><input value="15 selecionadas" readonly />
              </div>
              <div class="field">
                <label>Enviar até</label><input type="date" value="2026-08-19" />
              </div>
            </div>
            <div class="actions">
              <button class="btn" id="saveList">Criar lista</button>
            </div>
          </div>
        </section>
        <section class="card" style="margin-top: 16px">
          <div class="panel-head">
            <div>
              <h3>Listas de reparação programadas</h3>
              <p class="muted">Um clique seleciona; duplo clique abre a lista completa.</p>
            </div>
            <div class="field" style="width: 100px">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Lista</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Produção</th><th>Linha</th><th>Início produção</th><th>Reparador</th><th>Itens</th><th>Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="rep-cm-42"><td><strong>REP-CM-0042</strong></td><td>CM</td><td><strong>5774T173</strong></td><td>202604</td><td>B1</td><td>22/08</td><td>Externo A</td><td>15</td><td><span class="pill">A retirar</span></td></tr>
                <tr data-dmo-row data-id="rep-mf-41"><td><strong>REP-MF-0041</strong></td><td>MF</td><td><strong>9389T194</strong></td><td>202605</td><td>B3</td><td>24/08</td><td>Externo B</td><td>12</td><td><span class="pill green">Enviado</span></td></tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 listas · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" disabled id="correct">Editar lista</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="ferramentas">
        <div class="page-head">
          <div>
            <h2>Ferramentas e lotes</h2>
            <p class="muted">
              CM e MF usam listas separadas, com o mesmo comportamento canónico.
            </p>
          </div>
          <div class="type-switch">
            <button class="btn active">Contra moldes</button
            ><button class="btn">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote ou número</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Estado</label
              ><select>
                <option>Todos</option>
                <option>Produção</option>
                <option>Armazém</option>
                <option>Reparação</option>
              </select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>Todas</option>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
              </select>
            </div>
            <div class="field">
              <label>Lote</label><input placeholder="Todos" />
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>Tipo</th>
                  <th>Quantidade</th>
                  <th>Em produção</th>
                  <th>Armazém</th>
                  <th>Reparação</th>
                  <th>Máquinas</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="lot-cm-1">
                  <td><strong>5774T173</strong></td>
                  <td>01</td>
                  <td>CM</td>
                  <td>96</td>
                  <td>24</td>
                  <td>60</td>
                  <td>12</td>
                  <td>B1 · B3</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 lote · Página 1 de 1</span>
            <div class="pager">
              <button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="historico">
        <div class="page-head">
          <div>
            <h2>Histórico de Moldes</h2>
            <p class="muted">Consulta e comparação de movimentos CM/MF.</p>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote, número ou operador</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>Todos</option>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Movimento</label
              ><select>
                <option>Todos</option>
                <option>Entrada</option>
                <option>Saída</option>
              </select>
            </div>
            <div class="field"><label>Desde</label><input type="date" /></div>
            <div class="field"><label>Até</label><input type="date" /></div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list id="historyList">
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>N.º</th>
                  <th>Movimento</th>
                  <th>Origem</th>
                  <th>Destino</th>
                  <th>Linha</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="hist-cm-1">
                  <td>18/08 · 14:32</td>
                  <td>CM</td>
                  <td>5774T173</td>
                  <td>01</td>
                  <td>34</td>
                  <td>Entrada</td>
                  <td>Produção</td>
                  <td>Armazém</td>
                  <td>B1</td>
                  <td>João Silva</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 movimento · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" id="historyCorrect" disabled>
                Corrigir movimento</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="definicoes">
        <div class="page-head">
          <div>
            <h2>Definições de Moldes</h2>
            <p class="muted">Reparadores e associações por tipo e máquina.</p>
          </div>
        </div>
        <div class="split">
          <section class="card">
            <div class="panel-head">
              <h3>Reparadores</h3>
              <button class="btn">Adicionar reparador</button>
            </div>
            <div class="table-wrap">
              <table data-dmo-list>
                <thead>
                  <tr>
                    <th>Nome</th>
                    <th>Tipo</th>
                    <th>Máquinas</th>
                    <th>Estado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr data-dmo-row>
                    <td>Externo A</td>
                    <td>CM</td>
                    <td>B1 · B3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                  <tr data-dmo-row>
                    <td>Externo B</td>
                    <td>MF</td>
                    <td>C1 · C3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
          <section class="card">
            <div class="panel-head"><h3>Regra de separação</h3></div>
            <p>
              Contra moldes e Moldes finais podem partilhar uma Referência de
              produção, mas mantêm:
            </p>
            <ul>
              <li>tipos e IDs diferentes;</li>
              <li>lotes e números individuais próprios;</li>
              <li>movimentos e históricos separados;</li>
              <li>reparadores permitidos independentes.</li>
            </ul>
          </section>
        </div>
      </section>
    </main>
    <div class="toast" id="toast"></div>
    <script>
      const $ = (s) => document.querySelector(s),
        $$ = (s) => [...document.querySelectorAll(s)],
        say = (t) => {
          const x = $("#toast");
          x.textContent = t;
          x.classList.add("show");
          setTimeout(() => x.classList.remove("show"), 2200);
        };
      $$(".tab").forEach(
        (t) =>
          (t.onclick = () => {
            $$(".tab,.view").forEach((x) => x.classList.remove("active"));
            t.classList.add("active");
            $("#" + t.dataset.view).classList.add("active");
          }),
      );
      $$("[data-type]").forEach(
        (b) =>
          (b.onclick = () => {
            $$("[data-type]").forEach((x) => x.classList.remove("active"));
            b.classList.add("active");
            $("#formType").textContent = b.dataset.type;
          }),
      );
      $("#newList").onclick = () => $("#listForm").classList.add("open");
      $$("[data-close]").forEach(
        (b) =>
          (b.onclick = () => b.closest(".inline").classList.remove("open")),
      );
      $("#saveList").onclick = () => {
        $("#listForm").classList.remove("open");
        say("Lista de reparação preparada");
      };
      document.addEventListener("click", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) {
          const table = row.closest("[data-dmo-list]");
          table
            .querySelectorAll("[data-dmo-row]")
            .forEach((x) => x.classList.remove("selected"));
          row.classList.add("selected");
          $("#correct").disabled = false;
          if (table.id === "historyList") $("#historyCorrect").disabled = false;
        }
      });
      document.addEventListener("dblclick", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) say("Abrir detalhe " + row.dataset.id);
      });
    </script>
  </body>
</html>

```
## END FILE CONTENT

---

# FILE 029

## Source Path
`moldes-v43-alinhado.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Moldes — Portal DMO
EXTERNAL CSS (0): 
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 10
<footer>: 0
<form>: 0
UNIQUE IDS (16): registo, newList, listForm, formType, saveList, rep-cm-42, rep-mf-41, correct, ferramentas, lot-cm-1, historico, historyList, hist-cm-1, historyCorrect, definicoes, toast
DATA-* ATTRIBUTES (3): view, type, id
<button: 23
<input: 8
<select: 13
<textarea: 0
<table: 4
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Moldes — Portal DMO</title>
    <style>
      :root {
        --b: #3c73a8;
        --bd: #193046;
        --bs: #e8eff7;
        --page: #f6f9fc;
        --sub: #f1f6fa;
        --line: #d9e6f2;
        --text: #172d42;
        --muted: #64778a;
        --green: #527c72;
        --green-s: #e5f0eb;
        --warn: #a97943;
        --warn-s: #f7f0e7;
        --red: #9a625d;
        --disabled: #cbd5df;
        --r: 12px;
        --shadow: 0 8px 24px rgba(25, 48, 70, 0.06);
      }
      * {
        box-sizing: border-box;
      }
      body {
        margin: 0;
        background: var(--page);
        color: var(--text);
        font:
          14px/1.45 Inter,
          "Segoe UI",
          sans-serif;
      }
      button,
      input,
      select,
      textarea {
        font: inherit;
      }
      .header {
        min-height: 76px;
        display: flex;
        align-items: center;
        gap: 13px;
        padding: 10px 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .logo {
        width: 44px;
        height: 44px;
        object-fit: contain;
        border-radius: 50%;
      }
      .head h1 {
        margin: 0;
        font-size: 18px;
      }
      .head p,
      .muted {
        margin: 3px 0 0;
        color: var(--muted);
        font-size: 11px;
      }
      .user {
        margin-left: auto;
        padding-left: 18px;
        text-align: right;
        border-left: 1px solid var(--line);
      }
      .user strong,
      .user span {
        display: block;
      }
      .user span {
        font-size: 11px;
        color: var(--muted);
      }
      .tabs {
        height: 52px;
        display: flex;
        gap: 25px;
        padding: 0 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .tab {
        border: 0;
        border-bottom: 3px solid transparent;
        background: none;
        color: var(--muted);
        font-weight: 750;
        cursor: pointer;
      }
      .tab.active {
        color: var(--b);
        border-color: var(--b);
      }
      .tab.settings {
        margin-left: auto;
      }
      .main {
        max-width: 1360px;
        margin: auto;
        padding: 26px;
      }
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }
      .page-head,
      .panel-head,
      .footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
      }
      .page-head {
        align-items: end;
        margin-bottom: 16px;
      }
      .page-head h2,
      .panel-head h3 {
        margin: 0;
      }
      .card {
        padding: 18px;
        background: #fff;
        border: 1px solid var(--line);
        border-radius: var(--r);
        box-shadow: var(--shadow);
      }
      .btn {
        min-height: 36px;
        padding: 7px 12px;
        border: 1px solid var(--b);
        border-radius: 8px;
        background: var(--b);
        color: #fff;
        font-weight: 750;
        cursor: pointer;
      }
      .btn:hover,
      .btn:focus-visible {
        background: #fff;
        color: var(--b);
        outline: none;
      }
      .btn:disabled {
        border-color: var(--disabled);
        background: var(--disabled);
        color: #fff;
        cursor: not-allowed;
      }
      .btn.icon {
        width: 36px;
        padding: 0;
      }
      .type-switch {
        display: flex;
        gap: 8px;
      }
      .type-switch .btn {
        background: #fff;
        color: var(--b);
      }
      .type-switch .btn:hover,
      .type-switch .btn:focus-visible,
      .type-switch .btn.active {
        background: var(--b);
        color: #fff;
      }
      .production-grid {
        display: grid;
        grid-template-columns: repeat(6, minmax(0, 1fr));
        gap: 8px;
      }
      .production-card {
        min-width: 0;
        padding: 11px;
        border: 1px solid var(--b);
        border-radius: 9px;
        background: var(--b);
        color: #fff;
        text-align: left;
        cursor: pointer;
      }
      .production-card:hover,
      .production-card.active {
        background: #fff;
        color: var(--b);
      }
      .production-card strong,
      .production-card span,
      .production-card small {
        display: block;
      }
      .production-card strong {
        font-size: 13px;
      }
      .production-card span {
        margin-top: 3px;
        font-weight: 750;
      }
      .production-card small {
        margin-top: 2px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        opacity: 0.82;
      }
      .search {
        display: grid;
        grid-template-columns: 2fr 150px 150px 100px auto;
        gap: 9px;
        align-items: end;
        margin-top: 16px;
      }
      .field label {
        display: block;
        margin-bottom: 6px;
        color: var(--muted);
        font-size: 11px;
        font-weight: 750;
      }
      .field input,
      .field select,
      .field textarea {
        width: 100%;
        min-height: 40px;
        padding: 9px 11px;
        border: 1px solid var(--line);
        border-radius: 8px;
        background: #fff;
        color: var(--text);
      }
      .search > .btn,
      .filters > .btn {
        min-height: 40px;
      }
      .context {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 8px;
        margin-top: 16px;
      }
      .context div {
        padding: 11px;
        border-left: 3px solid var(--b);
        border-radius: 8px;
        background: var(--sub);
      }
      .context span {
        display: block;
        color: var(--muted);
        font-size: 9px;
      }
      .context strong {
        font-size: 14px;
      }
      .actions {
        display: flex;
        justify-content: flex-end;
        gap: 8px;
        margin-top: 14px;
      }
      .inline {
        display: none;
        margin-top: 16px;
      }
      .inline.open {
        display: block;
      }
      .form-grid {
        display: grid;
        grid-template-columns: 2fr 120px 120px 150px;
        gap: 10px;
        align-items: end;
      }
      .table-wrap {
        overflow: auto;
        margin-top: 14px;
        border: 1px solid var(--line);
        border-radius: 10px;
      }
      table {
        width: 100%;
        border-collapse: collapse;
        white-space: nowrap;
      }
      th {
        padding: 10px 12px;
        background: var(--sub);
        color: var(--muted);
        font-size: 10px;
        text-align: left;
        text-transform: uppercase;
      }
      td {
        padding: 11px 12px;
        border-top: 1px solid var(--line);
      }
      tr[data-dmo-row] {
        cursor: pointer;
      }
      tr[data-dmo-row]:hover {
        background: var(--page);
      }
      tr.selected {
        background: var(--line);
      }
      .pill {
        display: inline-flex;
        padding: 4px 8px;
        border-radius: 99px;
        background: var(--bs);
        color: #315d88;
        font-size: 10px;
        font-weight: 800;
      }
      .pill.green {
        background: var(--green-s);
        color: var(--green);
      }
      .footer {
        padding-top: 11px;
        color: var(--muted);
        font-size: 11px;
      }
      .pager {
        display: flex;
        align-items: center;
        gap: 7px;
      }
      .filters {
        display: grid;
        grid-template-columns: 2fr repeat(4, 150px) 100px auto;
        gap: 9px;
        align-items: end;
      }
      .split {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 16px;
      }
      .toast {
        position: fixed;
        right: 22px;
        bottom: 22px;
        padding: 11px 15px;
        border-radius: 9px;
        background: #0f1d2a;
        color: #fff;
        opacity: 0;
        transform: translateY(50px);
        transition: 0.2s;
      }
      .toast.show {
        opacity: 1;
        transform: none;
      }
      @media (max-width: 950px) {
        .production-grid {
          grid-template-columns: repeat(3, minmax(0, 1fr));
        }
        .filters {
          grid-template-columns: repeat(3, 1fr);
        }
        .split {
          grid-template-columns: 1fr;
        }
      }
      @media (max-width: 650px) {
        .header {
          padding: 10px 14px;
        }
        .user {
          display: none;
        }
        .tabs {
          padding: 0 12px;
          gap: 14px;
          overflow: auto;
        }
        .tab {
          white-space: nowrap;
        }
        .tab.settings {
          margin-left: 0;
        }
        .main {
          padding: 16px 12px;
        }
        .production-grid {
          grid-template-columns: repeat(2, minmax(0, 1fr));
        }
        .search,
        .form-grid,
        .filters,
        .context {
          grid-template-columns: 1fr 1fr;
        }
        .search .btn,
        .filters .btn {
          grid-column: 1/-1;
        }
        .table-wrap table {
          min-width: 920px;
        }
        .footer {
          align-items: flex-start;
          flex-direction: column;
        }
        .pager {
          align-self: flex-end;
        }
        .actions {
          display: grid;
          grid-template-columns: 1fr 1fr;
        }
        .actions .btn {
          min-height: 44px;
        }
      }
    </style>
  </head>
  <body>
    <header class="header">
      <img class="logo" src="logo_recolored(1).png" alt="BA" />
      <div class="head">
        <h1>Moldes</h1>
        <p>Contra moldes e Moldes finais</p>
      </div>
      <div class="user">
        <strong>Ana Martins</strong><span>Responsável de moldes</span>
      </div>
    </header>
    <nav class="tabs">
      <button class="tab active" data-view="registo">Registo</button
      ><button class="tab" data-view="ferramentas">Ferramentas</button
      ><button class="tab" data-view="historico">Histórico</button
      ><button class="tab settings" data-view="definicoes">Definições</button>
    </nav>
    <main class="main">
      <section class="view active" id="registo">
        <div class="page-head">
          <div>
            <h2>Preparar reparação de Moldes</h2>
            <p class="muted">Crie antecipadamente a lista de ferramentas para uma produção futura.</p>
          </div>
          <div class="type-switch">
            <button class="btn active" data-type="CM">Contra moldes</button
            ><button class="btn" data-type="MF">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="search">
            <div class="field">
              <label>Procurar Referência, lote ou número individual</label><input placeholder="Ex.: 5774T173, lote 01 ou CM 34" />
            </div>
            <div class="field">
              <label>Período</label><select><option>Próximos 30 dias</option><option>Próximos 60 dias</option></select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
                <option>C2</option>
                <option>C3</option>
              </select>
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Pesquisar</button>
          </div>
          <div class="actions">
            <button class="btn" id="newList">Criar lista de reparação</button>
          </div>
          <div class="card inline" id="listForm">
            <div class="panel-head">
              <div>
                <h3>Nova lista de reparação · <span id="formType">CM</span></h3>
                <p class="muted">A produção, Referência e máquina vêm do planeamento selecionado.</p>
              </div>
              <button class="btn icon" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="field"><label>Produção prevista</label><select><option>202604 · 5774T173 · B1 · 22/08</option><option>202605 · 9389T194 · B3 · 24/08</option></select></div>
              <div class="field">
                <label>Reparador</label><select><option>Externo A</option><option>Externo B</option></select>
              </div>
              <div class="field">
                <label>Ferramentas</label><input value="15 selecionadas" readonly />
              </div>
              <div class="field">
                <label>Enviar até</label><input type="date" value="2026-08-19" />
              </div>
            </div>
            <div class="actions">
              <button class="btn" id="saveList">Criar lista</button>
            </div>
          </div>
        </section>
        <section class="card" style="margin-top: 16px">
          <div class="panel-head">
            <div>
              <h3>Listas de reparação programadas</h3>
              <p class="muted">Um clique seleciona; duplo clique abre a lista completa.</p>
            </div>
            <div class="field" style="width: 100px">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Lista</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Produção</th><th>Linha</th><th>Início produção</th><th>Reparador</th><th>Itens</th><th>Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="rep-cm-42"><td><strong>REP-CM-0042</strong></td><td>CM</td><td><strong>5774T173</strong></td><td>202604</td><td>B1</td><td>22/08</td><td>Externo A</td><td>15</td><td><span class="pill">A retirar</span></td></tr>
                <tr data-dmo-row data-id="rep-mf-41"><td><strong>REP-MF-0041</strong></td><td>MF</td><td><strong>9389T194</strong></td><td>202605</td><td>B3</td><td>24/08</td><td>Externo B</td><td>12</td><td><span class="pill green">Enviado</span></td></tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 listas · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" disabled id="correct">Editar lista</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="ferramentas">
        <div class="page-head">
          <div>
            <h2>Ferramentas e lotes</h2>
            <p class="muted">
              CM e MF usam listas separadas, com o mesmo comportamento canónico.
            </p>
          </div>
          <div class="type-switch">
            <button class="btn active">Contra moldes</button
            ><button class="btn">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote ou número</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Estado</label
              ><select>
                <option>Todos</option>
                <option>Produção</option>
                <option>Armazém</option>
                <option>Reparação</option>
              </select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>Todas</option>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
              </select>
            </div>
            <div class="field">
              <label>Lote</label><input placeholder="Todos" />
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>Tipo</th>
                  <th>Quantidade</th>
                  <th>Em produção</th>
                  <th>Armazém</th>
                  <th>Reparação</th>
                  <th>Máquinas</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="lot-cm-1">
                  <td><strong>5774T173</strong></td>
                  <td>01</td>
                  <td>CM</td>
                  <td>96</td>
                  <td>24</td>
                  <td>60</td>
                  <td>12</td>
                  <td>B1 · B3</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 lote · Página 1 de 1</span>
            <div class="pager">
              <button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="historico">
        <div class="page-head">
          <div>
            <h2>Histórico de Moldes</h2>
            <p class="muted">Consulta e comparação de movimentos CM/MF.</p>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote, número ou operador</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>Todos</option>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Movimento</label
              ><select>
                <option>Todos</option>
                <option>Entrada</option>
                <option>Saída</option>
              </select>
            </div>
            <div class="field"><label>Desde</label><input type="date" /></div>
            <div class="field"><label>Até</label><input type="date" /></div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list id="historyList">
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>N.º</th>
                  <th>Movimento</th>
                  <th>Origem</th>
                  <th>Destino</th>
                  <th>Linha</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="hist-cm-1">
                  <td>18/08 · 14:32</td>
                  <td>CM</td>
                  <td>5774T173</td>
                  <td>01</td>
                  <td>34</td>
                  <td>Entrada</td>
                  <td>Produção</td>
                  <td>Armazém</td>
                  <td>B1</td>
                  <td>João Silva</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 movimento · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" id="historyCorrect" disabled>
                Corrigir movimento</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="definicoes">
        <div class="page-head">
          <div>
            <h2>Definições de Moldes</h2>
            <p class="muted">Reparadores e associações por tipo e máquina.</p>
          </div>
        </div>
        <div class="split">
          <section class="card">
            <div class="panel-head">
              <h3>Reparadores</h3>
              <button class="btn">Adicionar reparador</button>
            </div>
            <div class="table-wrap">
              <table data-dmo-list>
                <thead>
                  <tr>
                    <th>Nome</th>
                    <th>Tipo</th>
                    <th>Máquinas</th>
                    <th>Estado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr data-dmo-row>
                    <td>Externo A</td>
                    <td>CM</td>
                    <td>B1 · B3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                  <tr data-dmo-row>
                    <td>Externo B</td>
                    <td>MF</td>
                    <td>C1 · C3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
          <section class="card">
            <div class="panel-head"><h3>Regra de separação</h3></div>
            <p>
              Contra moldes e Moldes finais podem partilhar uma Referência de
              produção, mas mantêm:
            </p>
            <ul>
              <li>tipos e IDs diferentes;</li>
              <li>lotes e números individuais próprios;</li>
              <li>movimentos e históricos separados;</li>
              <li>reparadores permitidos independentes.</li>
            </ul>
          </section>
        </div>
      </section>
    </main>
    <div class="toast" id="toast"></div>
    <script>
      const $ = (s) => document.querySelector(s),
        $$ = (s) => [...document.querySelectorAll(s)],
        say = (t) => {
          const x = $("#toast");
          x.textContent = t;
          x.classList.add("show");
          setTimeout(() => x.classList.remove("show"), 2200);
        };
      $$(".tab").forEach(
        (t) =>
          (t.onclick = () => {
            $$(".tab,.view").forEach((x) => x.classList.remove("active"));
            t.classList.add("active");
            $("#" + t.dataset.view).classList.add("active");
          }),
      );
      $$("[data-type]").forEach(
        (b) =>
          (b.onclick = () => {
            $$("[data-type]").forEach((x) => x.classList.remove("active"));
            b.classList.add("active");
            $("#formType").textContent = b.dataset.type;
          }),
      );
      $("#newList").onclick = () => $("#listForm").classList.add("open");
      $$("[data-close]").forEach(
        (b) =>
          (b.onclick = () => b.closest(".inline").classList.remove("open")),
      );
      $("#saveList").onclick = () => {
        $("#listForm").classList.remove("open");
        say("Lista de reparação preparada");
      };
      document.addEventListener("click", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) {
          const table = row.closest("[data-dmo-list]");
          table
            .querySelectorAll("[data-dmo-row]")
            .forEach((x) => x.classList.remove("selected"));
          row.classList.add("selected");
          $("#correct").disabled = false;
          if (table.id === "historyList") $("#historyCorrect").disabled = false;
        }
      });
      document.addEventListener("dblclick", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) say("Abrir detalhe " + row.dataset.id);
      });
    </script>
  </body>
</html>

```
## END FILE CONTENT

---

# FILE 030

## Source Path
`moldes-v44-seletor-corrigido.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Moldes — Portal DMO
EXTERNAL CSS (0): 
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 10
<footer>: 0
<form>: 0
UNIQUE IDS (16): registo, newList, listForm, formType, saveList, rep-cm-42, rep-mf-41, correct, ferramentas, lot-cm-1, historico, historyList, hist-cm-1, historyCorrect, definicoes, toast
DATA-* ATTRIBUTES (3): view, type, id
<button: 23
<input: 8
<select: 13
<textarea: 0
<table: 4
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Moldes — Portal DMO</title>
    <style>
      :root {
        --b: #3c73a8;
        --bd: #193046;
        --bs: #e8eff7;
        --page: #f6f9fc;
        --sub: #f1f6fa;
        --line: #d9e6f2;
        --text: #172d42;
        --muted: #64778a;
        --green: #527c72;
        --green-s: #e5f0eb;
        --warn: #a97943;
        --warn-s: #f7f0e7;
        --red: #9a625d;
        --disabled: #cbd5df;
        --r: 12px;
        --shadow: 0 8px 24px rgba(25, 48, 70, 0.06);
      }
      * {
        box-sizing: border-box;
      }
      body {
        margin: 0;
        background: var(--page);
        color: var(--text);
        font:
          14px/1.45 Inter,
          "Segoe UI",
          sans-serif;
      }
      button,
      input,
      select,
      textarea {
        font: inherit;
      }
      .header {
        min-height: 76px;
        display: flex;
        align-items: center;
        gap: 13px;
        padding: 10px 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .logo {
        width: 44px;
        height: 44px;
        object-fit: contain;
        border-radius: 50%;
      }
      .head h1 {
        margin: 0;
        font-size: 18px;
      }
      .head p,
      .muted {
        margin: 3px 0 0;
        color: var(--muted);
        font-size: 11px;
      }
      .user {
        margin-left: auto;
        padding-left: 18px;
        text-align: right;
        border-left: 1px solid var(--line);
      }
      .user strong,
      .user span {
        display: block;
      }
      .user span {
        font-size: 11px;
        color: var(--muted);
      }
      .tabs {
        height: 52px;
        display: flex;
        gap: 25px;
        padding: 0 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .tab {
        border: 0;
        border-bottom: 3px solid transparent;
        background: none;
        color: var(--muted);
        font-weight: 750;
        cursor: pointer;
      }
      .tab.active {
        color: var(--b);
        border-color: var(--b);
      }
      .tab.settings {
        margin-left: auto;
      }
      .main {
        max-width: 1360px;
        margin: auto;
        padding: 26px;
      }
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }
      .page-head,
      .panel-head,
      .footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 12px;
      }
      .page-head {
        align-items: end;
        margin-bottom: 16px;
      }
      .page-head h2,
      .panel-head h3 {
        margin: 0;
      }
      .card {
        padding: 18px;
        background: #fff;
        border: 1px solid var(--line);
        border-radius: var(--r);
        box-shadow: var(--shadow);
      }
      .btn {
        min-height: 36px;
        padding: 7px 12px;
        border: 1px solid var(--b);
        border-radius: 8px;
        background: var(--b);
        color: #fff;
        font-weight: 750;
        cursor: pointer;
      }
      .btn:hover,
      .btn:focus-visible {
        background: #fff;
        color: var(--b);
        outline: none;
      }
      .btn:disabled {
        border-color: var(--disabled);
        background: var(--disabled);
        color: #fff;
        cursor: not-allowed;
      }
      .btn.icon {
        width: 36px;
        padding: 0;
      }
      .type-switch {
        display: flex;
        gap: 8px;
      }
      .type-switch .btn {
        background: #fff;
        color: var(--b);
      }
      .type-switch .btn:hover,
      .type-switch .btn:focus-visible,
      .type-switch .btn.active {
        background: var(--b);
        color: #fff;
      }
      .production-grid {
        display: grid;
        grid-template-columns: repeat(6, minmax(0, 1fr));
        gap: 8px;
      }
      .production-card {
        min-width: 0;
        padding: 11px;
        border: 1px solid var(--b);
        border-radius: 9px;
        background: var(--b);
        color: #fff;
        text-align: left;
        cursor: pointer;
      }
      .production-card:hover,
      .production-card.active {
        background: #fff;
        color: var(--b);
      }
      .production-card strong,
      .production-card span,
      .production-card small {
        display: block;
      }
      .production-card strong {
        font-size: 13px;
      }
      .production-card span {
        margin-top: 3px;
        font-weight: 750;
      }
      .production-card small {
        margin-top: 2px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        opacity: 0.82;
      }
      .search {
        display: grid;
        grid-template-columns: 2fr 150px 150px 100px auto;
        gap: 9px;
        align-items: end;
        margin-top: 16px;
      }
      .field label {
        display: block;
        margin-bottom: 6px;
        color: var(--muted);
        font-size: 11px;
        font-weight: 750;
      }
      .field input,
      .field select,
      .field textarea {
        width: 100%;
        min-height: 40px;
        padding: 9px 11px;
        border: 1px solid var(--line);
        border-radius: 8px;
        background: #fff;
        color: var(--text);
      }
      .search > .btn,
      .filters > .btn {
        min-height: 40px;
      }
      .context {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 8px;
        margin-top: 16px;
      }
      .context div {
        padding: 11px;
        border-left: 3px solid var(--b);
        border-radius: 8px;
        background: var(--sub);
      }
      .context span {
        display: block;
        color: var(--muted);
        font-size: 9px;
      }
      .context strong {
        font-size: 14px;
      }
      .actions {
        display: flex;
        justify-content: flex-end;
        gap: 8px;
        margin-top: 14px;
      }
      .inline {
        display: none;
        margin-top: 16px;
      }
      .inline.open {
        display: block;
      }
      .form-grid {
        display: grid;
        grid-template-columns: 2fr 120px 120px 150px;
        gap: 10px;
        align-items: end;
      }
      .table-wrap {
        overflow: auto;
        margin-top: 14px;
        border: 1px solid var(--line);
        border-radius: 10px;
      }
      table {
        width: 100%;
        border-collapse: collapse;
        white-space: nowrap;
      }
      th {
        padding: 10px 12px;
        background: var(--sub);
        color: var(--muted);
        font-size: 10px;
        text-align: left;
        text-transform: uppercase;
      }
      td {
        padding: 11px 12px;
        border-top: 1px solid var(--line);
      }
      tr[data-dmo-row] {
        cursor: pointer;
      }
      tr[data-dmo-row]:hover {
        background: var(--page);
      }
      tr.selected {
        background: var(--line);
      }
      .pill {
        display: inline-flex;
        padding: 4px 8px;
        border-radius: 99px;
        background: var(--bs);
        color: #315d88;
        font-size: 10px;
        font-weight: 800;
      }
      .pill.green {
        background: var(--green-s);
        color: var(--green);
      }
      .footer {
        padding-top: 11px;
        color: var(--muted);
        font-size: 11px;
      }
      .pager {
        display: flex;
        align-items: center;
        gap: 7px;
      }
      .filters {
        display: grid;
        grid-template-columns: 2fr repeat(4, 150px) 100px auto;
        gap: 9px;
        align-items: end;
      }
      .split {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 16px;
      }
      .toast {
        position: fixed;
        right: 22px;
        bottom: 22px;
        padding: 11px 15px;
        border-radius: 9px;
        background: #0f1d2a;
        color: #fff;
        opacity: 0;
        transform: translateY(50px);
        transition: 0.2s;
      }
      .toast.show {
        opacity: 1;
        transform: none;
      }
      @media (max-width: 950px) {
        .production-grid {
          grid-template-columns: repeat(3, minmax(0, 1fr));
        }
        .filters {
          grid-template-columns: repeat(3, 1fr);
        }
        .split {
          grid-template-columns: 1fr;
        }
      }
      @media (max-width: 650px) {
        .header {
          padding: 10px 14px;
        }
        .user {
          display: none;
        }
        .tabs {
          padding: 0 12px;
          gap: 14px;
          overflow: auto;
        }
        .tab {
          white-space: nowrap;
        }
        .tab.settings {
          margin-left: 0;
        }
        .main {
          padding: 16px 12px;
        }
        .production-grid {
          grid-template-columns: repeat(2, minmax(0, 1fr));
        }
        .search,
        .form-grid,
        .filters,
        .context {
          grid-template-columns: 1fr 1fr;
        }
        .search .btn,
        .filters .btn {
          grid-column: 1/-1;
        }
        .table-wrap table {
          min-width: 920px;
        }
        .footer {
          align-items: flex-start;
          flex-direction: column;
        }
        .pager {
          align-self: flex-end;
        }
        .actions {
          display: grid;
          grid-template-columns: 1fr 1fr;
        }
        .actions .btn {
          min-height: 44px;
        }
      }
    </style>
  </head>
  <body>
    <header class="header">
      <img class="logo" src="logo_recolored(1).png" alt="BA" />
      <div class="head">
        <h1>Moldes</h1>
        <p>Contra moldes e Moldes finais</p>
      </div>
      <div class="user">
        <strong>Ana Martins</strong><span>Responsável de moldes</span>
      </div>
    </header>
    <nav class="tabs">
      <button class="tab active" data-view="registo">Registo</button
      ><button class="tab" data-view="ferramentas">Ferramentas</button
      ><button class="tab" data-view="historico">Histórico</button
      ><button class="tab settings" data-view="definicoes">Definições</button>
    </nav>
    <main class="main">
      <section class="view active" id="registo">
        <div class="page-head">
          <div>
            <h2>Preparar reparação de Moldes</h2>
            <p class="muted">Crie antecipadamente a lista de ferramentas para uma produção futura.</p>
          </div>
          <div class="type-switch">
            <button class="btn active" data-type="CM">Contra moldes</button
            ><button class="btn" data-type="MF">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="search">
            <div class="field">
              <label>Procurar Referência, lote ou número individual</label><input placeholder="Ex.: 5774T173, lote 01 ou CM 34" />
            </div>
            <div class="field">
              <label>Período</label><select><option>Próximos 30 dias</option><option>Próximos 60 dias</option></select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
                <option>C2</option>
                <option>C3</option>
              </select>
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Pesquisar</button>
          </div>
          <div class="actions">
            <button class="btn" id="newList">Criar lista de reparação</button>
          </div>
          <div class="card inline" id="listForm">
            <div class="panel-head">
              <div>
                <h3>Nova lista de reparação · <span id="formType">CM</span></h3>
                <p class="muted">A produção, Referência e máquina vêm do planeamento selecionado.</p>
              </div>
              <button class="btn icon" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="field"><label>Produção prevista</label><select><option>202604 · 5774T173 · B1 · 22/08</option><option>202605 · 9389T194 · B3 · 24/08</option></select></div>
              <div class="field">
                <label>Reparador</label><select><option>Externo A</option><option>Externo B</option></select>
              </div>
              <div class="field">
                <label>Ferramentas</label><input value="15 selecionadas" readonly />
              </div>
              <div class="field">
                <label>Enviar até</label><input type="date" value="2026-08-19" />
              </div>
            </div>
            <div class="actions">
              <button class="btn" id="saveList">Criar lista</button>
            </div>
          </div>
        </section>
        <section class="card" style="margin-top: 16px">
          <div class="panel-head">
            <div>
              <h3>Listas de reparação programadas</h3>
              <p class="muted">Um clique seleciona; duplo clique abre a lista completa.</p>
            </div>
            <div class="field" style="width: 100px">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Lista</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Produção</th><th>Linha</th><th>Início produção</th><th>Reparador</th><th>Itens</th><th>Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="rep-cm-42"><td><strong>REP-CM-0042</strong></td><td>CM</td><td><strong>5774T173</strong></td><td>202604</td><td>B1</td><td>22/08</td><td>Externo A</td><td>15</td><td><span class="pill">A retirar</span></td></tr>
                <tr data-dmo-row data-id="rep-mf-41"><td><strong>REP-MF-0041</strong></td><td>MF</td><td><strong>9389T194</strong></td><td>202605</td><td>B3</td><td>24/08</td><td>Externo B</td><td>12</td><td><span class="pill green">Enviado</span></td></tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 listas · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" disabled id="correct">Editar lista</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="ferramentas">
        <div class="page-head">
          <div>
            <h2>Ferramentas e lotes</h2>
            <p class="muted">
              CM e MF usam listas separadas, com o mesmo comportamento canónico.
            </p>
          </div>
          <div class="type-switch">
            <button class="btn active">Contra moldes</button
            ><button class="btn">Moldes finais</button>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote ou número</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Estado</label
              ><select>
                <option>Todos</option>
                <option>Produção</option>
                <option>Armazém</option>
                <option>Reparação</option>
              </select>
            </div>
            <div class="field">
              <label>Máquina</label
              ><select>
                <option>Todas</option>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
              </select>
            </div>
            <div class="field">
              <label>Lote</label><input placeholder="Todos" />
            </div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>Tipo</th>
                  <th>Quantidade</th>
                  <th>Em produção</th>
                  <th>Armazém</th>
                  <th>Reparação</th>
                  <th>Máquinas</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="lot-cm-1">
                  <td><strong>5774T173</strong></td>
                  <td>01</td>
                  <td>CM</td>
                  <td>96</td>
                  <td>24</td>
                  <td>60</td>
                  <td>12</td>
                  <td>B1 · B3</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 lote · Página 1 de 1</span>
            <div class="pager">
              <button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="historico">
        <div class="page-head">
          <div>
            <h2>Histórico de Moldes</h2>
            <p class="muted">Consulta e comparação de movimentos CM/MF.</p>
          </div>
        </div>
        <section class="card">
          <div class="filters">
            <div class="field">
              <label>Referência, lote, número ou operador</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>Todos</option>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field">
              <label>Movimento</label
              ><select>
                <option>Todos</option>
                <option>Entrada</option>
                <option>Saída</option>
              </select>
            </div>
            <div class="field"><label>Desde</label><input type="date" /></div>
            <div class="field"><label>Até</label><input type="date" /></div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <div class="table-wrap">
            <table data-dmo-list id="historyList">
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Tipo</th>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>N.º</th>
                  <th>Movimento</th>
                  <th>Origem</th>
                  <th>Destino</th>
                  <th>Linha</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="hist-cm-1">
                  <td>18/08 · 14:32</td>
                  <td>CM</td>
                  <td>5774T173</td>
                  <td>01</td>
                  <td>34</td>
                  <td>Entrada</td>
                  <td>Produção</td>
                  <td>Armazém</td>
                  <td>B1</td>
                  <td>João Silva</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>1 movimento · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" id="historyCorrect" disabled>
                Corrigir movimento</button
              ><button class="btn icon" disabled>‹</button><span>1 / 1</span
              ><button class="btn icon" disabled>›</button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="definicoes">
        <div class="page-head">
          <div>
            <h2>Definições de Moldes</h2>
            <p class="muted">Reparadores e associações por tipo e máquina.</p>
          </div>
        </div>
        <div class="split">
          <section class="card">
            <div class="panel-head">
              <h3>Reparadores</h3>
              <button class="btn">Adicionar reparador</button>
            </div>
            <div class="table-wrap">
              <table data-dmo-list>
                <thead>
                  <tr>
                    <th>Nome</th>
                    <th>Tipo</th>
                    <th>Máquinas</th>
                    <th>Estado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr data-dmo-row>
                    <td>Externo A</td>
                    <td>CM</td>
                    <td>B1 · B3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                  <tr data-dmo-row>
                    <td>Externo B</td>
                    <td>MF</td>
                    <td>C1 · C3</td>
                    <td><span class="pill green">Ativo</span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
          <section class="card">
            <div class="panel-head"><h3>Regra de separação</h3></div>
            <p>
              Contra moldes e Moldes finais podem partilhar uma Referência de
              produção, mas mantêm:
            </p>
            <ul>
              <li>tipos e IDs diferentes;</li>
              <li>lotes e números individuais próprios;</li>
              <li>movimentos e históricos separados;</li>
              <li>reparadores permitidos independentes.</li>
            </ul>
          </section>
        </div>
      </section>
    </main>
    <div class="toast" id="toast"></div>
    <script>
      const $ = (s) => document.querySelector(s),
        $$ = (s) => [...document.querySelectorAll(s)],
        say = (t) => {
          const x = $("#toast");
          x.textContent = t;
          x.classList.add("show");
          setTimeout(() => x.classList.remove("show"), 2200);
        };
      $$(".tab").forEach(
        (t) =>
          (t.onclick = () => {
            $$(".tab,.view").forEach((x) => x.classList.remove("active"));
            t.classList.add("active");
            $("#" + t.dataset.view).classList.add("active");
          }),
      );
      $$("[data-type]").forEach(
        (b) =>
          (b.onclick = () => {
            $$("[data-type]").forEach((x) => x.classList.remove("active"));
            b.classList.add("active");
            $("#formType").textContent = b.dataset.type;
          }),
      );
      $("#newList").onclick = () => $("#listForm").classList.add("open");
      $$("[data-close]").forEach(
        (b) =>
          (b.onclick = () => b.closest(".inline").classList.remove("open")),
      );
      $("#saveList").onclick = () => {
        $("#listForm").classList.remove("open");
        say("Lista de reparação preparada");
      };
      document.addEventListener("click", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) {
          const table = row.closest("[data-dmo-list]");
          table
            .querySelectorAll("[data-dmo-row]")
            .forEach((x) => x.classList.remove("selected"));
          row.classList.add("selected");
          $("#correct").disabled = false;
          if (table.id === "historyList") $("#historyCorrect").disabled = false;
        }
      });
      document.addEventListener("dblclick", (e) => {
        const row = e.target.closest("[data-dmo-row]");
        if (row) say("Abrir detalhe " + row.dataset.id);
      });
    </script>
  </body>
</html>

```
## END FILE CONTENT

---

# FILE 031

## Source Path
`pegamentos.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Pegamentos — Controlo CM / Boquilha / Molde Final
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 3
<footer>: 1
<form>: 0
UNIQUE IDS (50): headerLogo, headerSub, registo, contextCard, ctx-jobon, referenceCombo, ctx-reference, referenceOptions, ctx-production, ctx-machine, noRecordCard, recordArea, f-jobon, f-reference, f-production, f-machine, f-date, f-filename, referenceFolderStatus, f-notes, componentsHost, f-ovalmax, f-gaptol, verificationHost, saveStatus, referencias, recSearch, recDateFrom, recDateTo, recordList, config, folderStatus, cfg-ovalmax, cfg-gaptol, printArea, lote-${c.key}, nom-${c.key}, min-${c.key}, max-${c.key}, util-${c.key}, rows-${c.key}, avgc-${c.key}, avg9-${c.key}, avgo-${c.key}, avgm-${c.key}, n-${c.key}, ref-${key}, oval-${key}-${row.id}, media-${key}-${row.id}, ${r.id}
DATA-* ATTRIBUTES (2): tab, id
<button: 15
<input: 24
<select: 2
<textarea: 1
<table: 2
<a: 0
MODAL/DIALOG refs: dialogs, modal, dialog, help-modal, help-dialog
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!DOCTYPE html>
<!-- Desenvolvido por Diogo Oliveira · 2026 · v1.9: tolerância dimensional = nominal ± 0,20 mm -->
<html lang="pt-PT">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Pegamentos — Controlo CM / Boquilha / Molde Final</title>
<link rel="stylesheet" href="dmo-design-system.css">
<style>
:root{--bg:#F2F7FC;--surface:#FFF;--primary:#0183D7;--primary-dark:#065C99;--accent:#0EA5E9;--accent-light:#DCEEFB;--text:#1F2937;--muted:#6B7280;--border:#D8E6F2;--danger:#B91C1C;--danger-light:#FEE2E2;--ok:#15803D;--ok-light:#DCFCE7;--warn:#B45309;--warn-light:#FFF7ED}
*{box-sizing:border-box}
body{margin:0;font-family:"Segoe UI",-apple-system,Helvetica,Arial,sans-serif;background:var(--bg);color:var(--text)}
header{display:flex;align-items:center;justify-content:space-between;gap:14px;padding:14px 20px;background:var(--surface);border-bottom:1px solid var(--border);flex-wrap:wrap}
header h1{font-size:18px;margin:0;color:var(--primary-dark)}
header .sub{font-size:13px;color:var(--muted);margin-top:2px}
.brand{display:flex;align-items:center;gap:12px}
.mark{width:42px;height:42px;border-radius:10px;background:linear-gradient(135deg,var(--primary),var(--accent));color:#fff;font-weight:800;display:grid;place-items:center;font-size:13px}
.logo{width:42px;height:42px;object-fit:contain;flex-shrink:0}
.print-logo{width:52px;height:52px;object-fit:contain}
nav.tabs{display:flex;align-items:stretch;overflow-x:auto;background:var(--surface);border-bottom:2px solid var(--border);padding:0 12px}
nav.tabs button{border:0;background:transparent;padding:12px 18px;font-size:14px;font-weight:600;color:var(--primary-dark);cursor:pointer;border-bottom:3px solid transparent;white-space:nowrap}
nav.tabs button.active{color:#fff;background:var(--primary);border-bottom-color:var(--primary)}
@media (hover:hover) and (pointer:fine){
  nav.tabs button:hover{background:var(--accent-light)}
  .btn:hover{background:var(--accent)}
  .btn.secondary:hover{background:var(--accent-light);color:var(--primary-dark)}
  .btn.danger:hover{background:#991B1B}
  tr:hover td{background:var(--accent-light)}
  .rec-item:hover{background:var(--accent-light)}
}
main{max-width:1180px;margin:0 auto;padding:20px;width:100%}
.tab-panel{display:none}.tab-panel.active{display:block}
.card{background:var(--surface);border:1px solid var(--border);border-radius:8px;padding:18px;margin-bottom:16px}
.card h2{font-size:15px;margin:0 0 12px;color:var(--primary-dark)}
.section-heading{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:12px;flex-wrap:wrap}
.section-heading h2{margin:0}
.field-row{display:flex;flex-wrap:wrap;gap:14px;margin-bottom:10px}
.field{display:flex;flex-direction:column;min-width:140px;flex:1 1 160px}
.field label{font-size:12px;color:var(--muted);margin-bottom:4px}
.field input,.field select,.field textarea{padding:8px 9px;border:1px solid var(--border);border-radius:5px;font-size:13px;font-family:inherit;width:100%}
.field textarea{min-height:60px;resize:vertical}
.btn{background:var(--primary);color:#fff;border:0;padding:9px 16px;border-radius:6px;font-size:13px;font-weight:600;cursor:pointer}
.btn.secondary{background:#fff;color:var(--primary);border:1px solid var(--primary)}
.btn.danger{background:var(--danger)}
.btn.small{padding:6px 10px;font-size:12px}
.btn:disabled{background:#B7C3CC;cursor:not-allowed}
.actions{display:flex;flex-wrap:wrap;gap:8px}
.comp-card{border-top:4px solid var(--primary)}
.comp-card.comp-boq{border-top-color:var(--accent)}
.comp-card.comp-mf{border-top-color:var(--primary-dark)}
.summary-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:10px;margin-top:10px}
.summary-item{border-left:3px solid var(--primary);background:#F7FAFC;padding:10px 12px;border-radius:4px}
.summary-item span{display:block;font-size:11px;color:var(--muted);margin-bottom:3px}
.summary-item strong{font-size:16px}
.gap-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:12px}
.gap-box{border:1px solid var(--border);border-radius:8px;padding:14px;background:#F8FBFE}
.gap-box .title{font-size:13px;font-weight:700;color:var(--primary-dark);margin-bottom:8px}
.gap-box .row{display:flex;justify-content:space-between;font-size:13px;margin-bottom:4px;color:var(--muted)}
.gap-box .row strong{color:var(--text)}
.pill{display:inline-block;border-radius:999px;padding:4px 10px;font-size:11px;font-weight:700}
.pill.ok{background:var(--ok-light);color:#166534}
.pill.warn{background:var(--warn-light);color:#9A3412}
.pill.critico{background:var(--danger-light);color:#991B1B}
.pill.pending{background:#E5E7EB;color:#374151}
.table-scroll{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:13px}
th,td{text-align:left;padding:6px 7px;border-bottom:1px solid var(--border)}
th{background:var(--primary);color:#fff;white-space:nowrap;font-size:12px}
td input{width:78px;padding:5px 6px;border:1px solid var(--border);border-radius:4px;font-size:13px}
td input.nr-input{width:56px}
.readonly-cell{font-weight:600;color:var(--muted)}
.readonly-cell.bad{color:var(--danger);font-weight:800}
.summary-item strong.bad{color:var(--danger)}
.field input[readonly]{background:#F3F7FA;color:var(--primary-dark);font-weight:700}
tfoot td{background:#F7FAFC;font-weight:700}
.row-del{border:0;background:transparent;color:var(--danger);cursor:pointer;font-size:16px;padding:0 6px}
.muted{color:var(--muted);font-size:12px}
.empty{padding:24px;text-align:center;color:var(--muted);border:1px dashed var(--border);border-radius:8px;background:#F8FAFC}
.search-results{margin-top:8px;border:1px solid var(--border);border-radius:6px;overflow:hidden}
.rec-item{padding:10px 12px;cursor:pointer;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center;background:#fff;gap:10px;flex-wrap:wrap}
.rec-item:last-child{border-bottom:0}
.rec-item .meta{font-size:12px;color:var(--muted)}
.folder-status{padding:10px 12px;border:1px solid var(--border);border-radius:7px;background:#F8FAFC;font-size:13px}
.folder-status strong{color:var(--primary-dark)}

.notice{padding:10px 12px;border-radius:7px;background:#FFF7ED;border:1px solid #FED7AA;color:#9A3412;font-size:12px;margin-top:10px}
.viz-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:12px;margin-top:14px}
.tolerance-viz{border:1px solid var(--border);border-radius:8px;background:#F8FBFE;padding:12px}
.tolerance-viz.compact{padding:10px}
.tolerance-viz .viz-title{font-size:13px;font-weight:700;color:var(--primary-dark);margin-bottom:8px}
.tolerance-viz .viz-meta{display:flex;flex-wrap:wrap;gap:10px;font-size:12px;color:var(--muted);margin-bottom:8px}
.tolerance-viz .viz-meta strong{color:var(--text)}
.tolerance-viz .viz-scale{position:relative;height:42px}
.tolerance-viz .viz-track{position:absolute;left:0;right:0;top:18px;height:6px;border-radius:999px;background:#D8E6F2}
.tolerance-viz .viz-band{position:absolute;top:15px;height:12px;border-radius:999px;background:#DCFCE7;border:1px solid #86EFAC}
.tolerance-viz .viz-nominal{position:absolute;top:10px;width:2px;height:22px;background:#065C99;border-radius:2px}
.tolerance-viz .viz-marker{position:absolute;top:4px;width:0;height:30px}
.tolerance-viz .viz-marker::before{content:"";position:absolute;left:-1px;top:9px;width:2px;height:21px;background:#0F172A;border-radius:2px}
.tolerance-viz .viz-marker::after{content:"";position:absolute;left:-6px;top:0;width:12px;height:12px;border-radius:50%;background:#0183D7;border:2px solid #fff;box-shadow:0 0 0 1px #94A3B8}
.tolerance-viz .viz-marker.bad::after{background:#B91C1C}
.tolerance-viz .viz-labels{display:flex;justify-content:space-between;gap:10px;font-size:11px;color:var(--muted);margin-top:2px}
.tolerance-viz .viz-legend{display:flex;justify-content:space-between;gap:10px;flex-wrap:wrap;font-size:12px;margin-top:8px}
.tolerance-viz .viz-legend span{color:var(--muted)}
.tolerance-viz .viz-legend strong{color:var(--text)}
.tolerance-viz .viz-legend .ok{color:#166534}
.tolerance-viz .viz-legend .bad{color:#991B1B}

.boundary-map-card{border:1px solid var(--border);border-radius:9px;background:#fff;padding:14px;margin-top:10px}
.boundary-map-card h3{margin:0 0 4px;color:var(--primary-dark);font-size:14px}
.boundary-map-card .boundary-help{font-size:12px;color:var(--muted);margin-bottom:10px}
.boundary-map-wrap{width:100%;overflow-x:auto;border:1px solid #D8E6F2;border-radius:7px;background:#FBFDFF}
.boundary-map-wrap svg{display:block;width:100%;min-width:760px;height:auto}
.boundary-map-status{margin-top:10px;padding:10px 12px;border-radius:7px;font-size:12px;border:1px solid #CBD5E1;background:#F8FAFC}
.boundary-map-status.ok{border-color:#86C89B;background:#ECFDF3;color:#166534}
.boundary-map-status.warn{border-color:#E8C978;background:#FFF9E8;color:#92400E}
.boundary-map-status.critico{border-color:#E8A3A3;background:#FFF1F1;color:#991B1B}
.boundary-map-legend{display:flex;gap:14px;flex-wrap:wrap;margin-top:9px;font-size:11px;color:var(--muted)}
.boundary-map-legend span{display:inline-flex;align-items:center;gap:5px}
.boundary-dot{width:10px;height:10px;border-radius:50%;display:inline-block}
.boundary-line{width:2px;height:14px;display:inline-block}
.boundary-diamond{width:9px;height:9px;display:inline-block;transform:rotate(45deg);border:2px solid #065C99;background:#fff}
.boundary-spacing-warning{margin-top:8px;color:#92400E;font-weight:600}

.toast{position:fixed;right:18px;bottom:18px;background:#fff;border:1px solid #CBD5E1;border-left:5px solid var(--primary);box-shadow:0 10px 28px rgba(15,23,42,.16);padding:12px 14px;border-radius:8px;font-size:14px;z-index:2000;max-width:320px}
@media(max-width:850px){main{padding:12px}}
@media(max-width:520px){header{align-items:flex-start}.actions .btn{flex:1 1 160px}}

/* Print */
#printArea{display:none}
@media print{
  *{-webkit-print-color-adjust:exact!important;print-color-adjust:exact!important}
  body.app-shell>*:not(#printArea){display:none!important}
  #printArea{display:block!important}
  .print-page{max-width:none;margin:0}
  .print-header{border-bottom:4px solid #0183D7;padding-bottom:10px;margin-bottom:14px}
  .print-header h1{margin:0;color:#065C99;font-size:22px}
  .print-header .sub{color:#6B7280;font-size:13px;margin-top:4px}
  .print-strip{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:14px 0}
  .print-strip .box{border-left:4px solid #0183D7;background:#F7FAFC;padding:10px 12px}
  .print-strip .box span{display:block;font-size:11px;color:#6B7280}
  .print-strip .box strong{font-size:16px}
  .print-section{font-size:14px;font-weight:700;color:#065C99;background:#EEF6FC;padding:6px 10px;margin:16px 0 6px}
  .print-table{width:100%;border-collapse:collapse;font-size:12px;margin-bottom:6px}
  .print-table th,.print-table td{border:1px solid #CBD5E1;padding:5px 7px;text-align:left}
  .print-table th{background:#0183D7;color:#fff}
  .print-gap{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin:8px 0 14px}
  .print-gap .box{border:1px solid #CBD5E1;padding:10px 12px;border-radius:6px}
  .print-pill{display:inline-block;border-radius:999px;padding:2px 9px;font-size:11px;font-weight:700}
  .print-pill.ok{background:#DCFCE7;color:#166534}
  .print-pill.warn{background:#FFF7ED;color:#9A3412}
  .print-pill.critico{background:#FEE2E2;color:#991B1B}
  .print-foot{margin-top:18px;font-size:11px;color:#6B7280}
  .print-top-alert{border:2px solid #B91C1C;background:#FEE2E2;color:#7F1D1D;font-weight:700;padding:10px 14px;border-radius:6px;margin:10px 0 16px}
  .print-top-ok{border:2px solid #15803D;background:#DCFCE7;color:#14532D;font-weight:700;padding:10px 14px;border-radius:6px;margin:10px 0 16px}
  .print-top-warn{border:2px solid #B45309;background:#FFF9E8;color:#92400E;font-weight:700;padding:10px 14px;border-radius:6px;margin:10px 0 16px}
  .print-warn{color:#92400E;font-weight:800}
  .print-bad{color:#B91C1C;font-weight:800}
  tr.print-bad-row{background:#FEF2F2}
  .print-alert{border:1px solid #FCA5A5;background:#FEF2F2;color:#7F1D1D;font-size:12px;padding:8px 10px;border-radius:5px;margin:6px 0 14px}
}


/* =========================================================
   BA UI BLUEPRINT v1 — STANDARD BUTTONS INSIDE APPLICATION PAGES
   Normal: white / blue text / blue border
   Hover: blue / white text
   Pressed: dark blue / white text
   Disabled: neutral grey
   Navigation tabs are intentionally excluded.
   ========================================================= */
:root{
  --ba-button-bg:#FFFFFF;
  --ba-button-text:var(--primary,#0183D7);
  --ba-button-border:var(--primary,#0183D7);
  --ba-button-hover:var(--primary,#0183D7);
  --ba-button-pressed:var(--primary-dark,#065C99);
  --ba-button-disabled-bg:#E5E7EB;
  --ba-button-disabled-text:#7B8794;
  --ba-button-disabled-border:#CBD5E1;
}

/* Standard action buttons inside page content, dialogs and menus */
main .btn,
.modal .btn,
.dialog .btn,
.help-modal .btn,
.help-dialog .btn,
.config-menu-panel .btn,
button.btn{
  background:var(--ba-button-bg)!important;
  color:var(--ba-button-text)!important;
  border:1px solid var(--ba-button-border)!important;
  box-shadow:none!important;
  transition:
    background-color .15s ease,
    color .15s ease,
    border-color .15s ease,
    transform .05s ease!important;
}

/* Remove previous semantic fills so every action starts neutral */
main .btn.secondary,
main .btn.danger,
main .btn.warn,
.modal .btn.secondary,
.modal .btn.danger,
.modal .btn.warn,
.dialog .btn.secondary,
.dialog .btn.danger,
.dialog .btn.warn,
.help-modal .btn.secondary,
.help-modal .btn.danger,
.help-modal .btn.warn,
button.btn.secondary,
button.btn.danger,
button.btn.warn{
  background:var(--ba-button-bg)!important;
  color:var(--ba-button-text)!important;
  border-color:var(--ba-button-border)!important;
  filter:none!important;
}

/* Desktop pointer feedback */
@media (hover:hover) and (pointer:fine){
  main .btn:not(:disabled):hover,
  .modal .btn:not(:disabled):hover,
  .dialog .btn:not(:disabled):hover,
  .help-modal .btn:not(:disabled):hover,
  .help-dialog .btn:not(:disabled):hover,
  .config-menu-panel .btn:not(:disabled):hover,
  button.btn:not(:disabled):hover{
    background:var(--ba-button-hover)!important;
    color:#FFFFFF!important;
    border-color:var(--ba-button-hover)!important;
    filter:none!important;
  }
}

/* Keyboard focus remains visible without looking permanently selected */
main .btn:not(:disabled):focus-visible,
.modal .btn:not(:disabled):focus-visible,
.dialog .btn:not(:disabled):focus-visible,
.help-modal .btn:not(:disabled):focus-visible,
.help-dialog .btn:not(:disabled):focus-visible,
.config-menu-panel .btn:not(:disabled):focus-visible,
button.btn:not(:disabled):focus-visible{
  background:var(--ba-button-bg)!important;
  color:var(--ba-button-text)!important;
  border-color:var(--ba-button-border)!important;
  outline:3px solid var(--accent-light,#DCEEFB)!important;
  outline-offset:2px!important;
}

/* Mouse/touch press */
main .btn:not(:disabled):active,
.modal .btn:not(:disabled):active,
.dialog .btn:not(:disabled):active,
.help-modal .btn:not(:disabled):active,
.help-dialog .btn:not(:disabled):active,
.config-menu-panel .btn:not(:disabled):active,
button.btn:not(:disabled):active{
  background:var(--ba-button-pressed)!important;
  color:#FFFFFF!important;
  border-color:var(--ba-button-pressed)!important;
  transform:translateY(1px)!important;
  outline:none!important;
}

/* Disabled */
main .btn:disabled,
.modal .btn:disabled,
.dialog .btn:disabled,
.help-modal .btn:disabled,
.help-dialog .btn:disabled,
.config-menu-panel .btn:disabled,
button.btn:disabled{
  background:var(--ba-button-disabled-bg)!important;
  color:var(--ba-button-disabled-text)!important;
  border-color:var(--ba-button-disabled-border)!important;
  cursor:not-allowed!important;
  transform:none!important;
}

/* Navigation tabs keep their own selected-page behavior */
nav.tabs button{
  box-shadow:none;
}

.app-credit{
  max-width:1180px;
  margin:0 auto;
  padding:4px 20px 18px;
  text-align:right;
  color:var(--muted);
  font-size:11px;
}
@media(max-width:850px){
  .app-credit{padding:4px 12px 16px;text-align:center}
}





#printArea,
  #printArea *{
    -webkit-print-color-adjust:exact!important;
    print-color-adjust:exact!important;
    color-adjust:exact!important;
    box-sizing:border-box!important;
  }

  #printArea{
    width:100%!important;
    max-width:100%!important;
    padding:0!important;
    margin:0!important;
    overflow:visible!important;
  }

  .print-page,
  .page{
    width:100%!important;
    max-width:100%!important;
    margin:0!important;
    padding:0!important;
    overflow:visible!important;
  }

  .print-header{
    border-bottom:2px solid #0183D7!important;
    padding-bottom:7px!important;
    margin-bottom:9px!important;
  }

  .print-header h1,
  .print-title{
    color:#065C99!important;
  }

  .print-section{
    background:#EAF4FB!important;
    color:#065C99!important;
    border-left:4px solid #0183D7!important;
    padding:5px 8px!important;
    margin:10px 0 5px!important;
  }

  .print-strip,
  .print-mold-strip,
  .print-gap{
    gap:6px!important;
    margin:7px 0 9px!important;
  }

  .print-strip .box,
  .print-mold-item,
  .print-gap .box{
    background:#F7FBFE!important;
    border-color:#B9D7EB!important;
    padding:6px 8px!important;
  }

  .print-table,
  table.print-table{
    width:100%!important;
    max-width:100%!important;
    table-layout:fixed!important;
    border-collapse:collapse!important;
    font-size:9.5px!important;
    margin-bottom:5px!important;
  }

  .print-table th,
  .print-table td{
    padding:3px 4px!important;
    overflow-wrap:anywhere!important;
    word-break:break-word!important;
  }

  .print-table th{
    background:#DCEEFB!important;
    color:#065C99!important;
    border-color:#8CBFDF!important;
  }

  .print-pill.ok,
  .print-status-approved,
  .print-top-ok{
    background:#ECFDF3!important;
    color:#166534!important;
    border-color:#86C89B!important;
  }

  .print-pill.warn,
  .print-status-pending{
    background:#FFF9E8!important;
    color:#92400E!important;
    border-color:#E8C978!important;
  }

  .print-pill.critico,
  .print-top-alert,
  .print-alert,
  tr.print-bad-row{
    background:#FFF1F1!important;
    color:#991B1B!important;
    border-color:#E8A3A3!important;
  }

  .print-foot{
    margin-top:8px!important;
    font-size:9.5px!important;
  }

  img{
    max-width:100%!important;
  }
}


/* =========================================================
   BA PRINT BLUEPRINT v2 - VERIFIED A4 LANDSCAPE
   Colour is carried by text and borders, not only backgrounds.
   ========================================================= */
@page { size: A4 landscape; margin: 7mm; }

/* Printable report appearance on screen and in saved HTML files */
#printArea .print-section,
.print-page .print-section,
.page .print-section {
  background: #FFFFFF !important;
  color: #065C99 !important;
  border: 1px solid #8CBFDF !important;
  border-left: 5px solid #0183D7 !important;
  border-radius: 3px !important;
}
#printArea .print-table th,
.print-page .print-table th,
.page .print-table th {
  background: #FFFFFF !important;
  color: #065C99 !important;
  border-top: 2px solid #0183D7 !important;
  border-bottom: 2px solid #0183D7 !important;
}

@media print {
  html, body {
    width: auto !important;
    min-width: 0 !important;
    height: auto !important;
    margin: 0 !important;
    padding: 0 !important;
    background: #FFFFFF !important;
  }

  body.app-shell > *:not(#printArea) { display: none !important; }
  #printArea, #printArea * {
    visibility: visible !important;
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
    color-adjust: exact !important;
    box-sizing: border-box !important;
  }
  #printArea {
    display: block !important;
    position: static !important;
    inset: auto !important;
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
    overflow: visible !important;
  }

  /* Standalone saved report files do not always contain #printArea. */
  body > .page,
  body > .print-page,
  #printArea > .page,
  #printArea > .print-page,
  .page,
  .print-page {
    display: block !important;
    position: static !important;
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
    border: 0 !important;
    box-shadow: none !important;
    overflow: visible !important;
  }

  .print-header,
  .head {
    break-inside: avoid !important;
    page-break-inside: avoid !important;
    border-bottom: 2px solid #0183D7 !important;
    padding: 0 0 6px !important;
    margin: 0 0 7px !important;
  }
  .print-title,
  .print-header h1,
  .head h1,
  h1 {
    color: #065C99 !important;
  }

  .print-section,
  .page h2,
  .print-page h2 {
    break-after: avoid !important;
    page-break-after: avoid !important;
    background: #FFFFFF !important;
    color: #065C99 !important;
    border: 1px solid #8CBFDF !important;
    border-left: 5px solid #0183D7 !important;
    padding: 4px 7px !important;
    margin: 8px 0 4px !important;
    font-size: 10.5px !important;
    line-height: 1.2 !important;
  }

  .print-mold-strip,
  .mold-strip,
  .print-strip,
  .summary-grid,
  .print-gap {
    gap: 5px !important;
    margin: 5px 0 7px !important;
    break-inside: avoid !important;
    page-break-inside: avoid !important;
  }
  .print-mold-item,
  .mold-item,
  .print-strip .box,
  .summary-card,
  .summary-item,
  .print-gap .box {
    min-width: 0 !important;
    padding: 5px 6px !important;
    background: #FFFFFF !important;
    border: 1px solid #B9D7EB !important;
    border-left: 3px solid #0183D7 !important;
  }

  .table-scroll {
    overflow: visible !important;
    width: 100% !important;
  }
  table,
  .print-table,
  .comparison-table {
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    table-layout: fixed !important;
    border-collapse: collapse !important;
    font-size: 8.5px !important;
    line-height: 1.18 !important;
    margin: 0 0 4px !important;
  }
  th, td,
  .print-table th, .print-table td,
  .comparison-table th, .comparison-table td {
    padding: 2.5px 3px !important;
    max-width: 0 !important;
    overflow-wrap: anywhere !important;
    word-break: normal !important;
    white-space: normal !important;
    vertical-align: top !important;
    border: 1px solid #B8C8D4 !important;
  }
  th,
  .print-table th,
  .comparison-table th {
    background: #FFFFFF !important;
    color: #065C99 !important;
    border-top: 2px solid #0183D7 !important;
    border-bottom: 2px solid #0183D7 !important;
    font-weight: 800 !important;
  }
  tr { break-inside: avoid !important; page-break-inside: avoid !important; }

  /* Statuses remain visibly coloured without requiring background printing. */
  .print-status-approved,
  .print-pill.ok,
  .pill.ok,
  .print-top-ok {
    background: #FFFFFF !important;
    color: #15803D !important;
    border: 1.5px solid #15803D !important;
  }
  .print-status-pending,
  .print-pill.warn,
  .pill.warn {
    background: #FFFFFF !important;
    color: #B45309 !important;
    border: 1.5px solid #B45309 !important;
  }
  .print-pill.critico,
  .pill.critico,
  .print-top-alert,
  .print-alert,
  .print-bad,
  .readonly-cell.bad,
  .delta-up {
    background: #FFFFFF !important;
    color: #B91C1C !important;
    border-color: #B91C1C !important;
  }
  .delta-down { color: #15803D !important; }
  tr.print-bad-row { background: #FFFFFF !important; border-left: 3px solid #B91C1C !important; }

  .boundary-map-card {
    width: 100% !important;
    max-width: 100% !important;
    margin: 4px 0 7px !important;
    padding: 6px !important;
    border: 1px solid #B9D7EB !important;
    break-inside: avoid !important;
    page-break-inside: avoid !important;
    overflow: hidden !important;
  }
  .boundary-map-card h3 {
    margin: 0 0 2px !important;
    font-size: 10px !important;
  }
  .boundary-map-card .boundary-help {
    margin-bottom: 4px !important;
    font-size: 8px !important;
    line-height: 1.15 !important;
  }
  .boundary-map-wrap {
    width: 100% !important;
    max-width: 100% !important;
    overflow: hidden !important;
    border: 1px solid #CBD5E1 !important;
  }
  .boundary-map-wrap svg {
    display: block !important;
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    height: auto !important;
    max-height: 112mm !important;
  }
  .boundary-map-legend {
    gap: 7px !important;
    margin-top: 3px !important;
    font-size: 7.5px !important;
    line-height: 1.1 !important;
  }
  .boundary-map-status {
    margin-top: 4px !important;
    padding: 4px 6px !important;
    font-size: 8px !important;
    line-height: 1.15 !important;
  }
  .boundary-spacing-warning {
    margin-top: 3px !important;
    font-size: 8px !important;
  }

  .print-foot, footer {
    margin-top: 6px !important;
    padding: 0 !important;
    font-size: 8.5px !important;
  }
  img { max-width: 100% !important; height: auto !important; }
  button, .decision, .no-print { display: none !important; }
}

/* DMO canonical layer — Pegamentos */
:root{--bg:#f4f7fa;--surface:#fff;--primary:#407caf;--primary-dark:#173f63;--accent:#7198ba;--accent-light:#e7eff6;--text:#102a43;--muted:#5e7388;--border:#cfdfec;--danger:#bf4b45;--danger-light:#f8e8e6;--ok:#278b63;--ok-light:#e8f4ee;--warn:#b7772d;--warn-light:#f8efe2}
body{font-family:Inter,"Segoe UI",Arial,sans-serif;background:var(--bg);color:var(--text)}
header{padding:12px 28px;box-shadow:0 1px 0 rgba(16,42,67,.04)}
header h1{font-size:20px;color:var(--text)}
.logo{width:44px;height:44px;border-radius:50%}
nav.tabs{padding:0 28px;border-bottom:1px solid var(--border)}
nav.tabs button{padding:13px 17px;color:#526b82;border-bottom-width:3px}
nav.tabs button.active{color:var(--primary);background:transparent;border-bottom-color:var(--primary)}
main{max-width:1440px;padding:22px 28px 48px}
.card{border-radius:14px;padding:20px;box-shadow:0 8px 22px rgba(38,72,102,.055)}
.card h2{font-size:16px;color:var(--text)}
.field input,.field select,.field textarea{min-height:42px;padding:9px 12px;border-radius:9px;background:#fff;color:var(--text)}
.field input:focus,.field select:focus,.field textarea:focus{outline:2px solid rgba(64,124,175,.18);border-color:var(--primary)}
.field input[readonly]{background:#edf3f8;color:var(--text)}
.btn{background:var(--primary);color:#fff;border:1px solid var(--primary);border-radius:10px;padding:9px 14px}
.btn.secondary{background:var(--primary);color:#fff;border-color:var(--primary)}
.btn.danger{background:var(--danger);border-color:var(--danger)}
@media (hover:hover) and (pointer:fine){
  nav.tabs button:hover{background:#edf3f8;color:var(--primary-dark)}
  .btn:hover,.btn.secondary:hover{background:#fff;color:var(--primary);border-color:var(--primary)}
  .btn.danger:hover{background:#fff;color:var(--danger);border-color:var(--danger)}
  tr:hover td{background:#edf3f8}
  .rec-item:hover{background:#edf3f8}
}
.context-card{border-left:4px solid var(--primary);padding:18px 20px}
.context-help{margin:4px 0 0}
.context-step{padding:5px 10px;border-radius:999px;background:var(--accent-light);color:var(--primary-dark);font-size:11px;font-weight:700}
.context-grid{display:grid;grid-template-columns:minmax(210px,1.3fr) minmax(190px,1.2fr) minmax(120px,.65fr) minmax(100px,.55fr) auto;gap:12px;align-items:end}
.context-submit{min-height:42px;white-space:nowrap}
.context-note{margin-top:14px;padding:10px 12px;border-radius:9px;background:#edf3f8;color:var(--muted);font-size:12px}
.context-combo{position:relative}.context-combo input{padding-right:42px}
.context-combo-toggle{position:absolute;right:5px;top:5px;width:32px;height:32px;border:0;border-radius:7px;background:transparent;color:var(--primary-dark);cursor:pointer;font-size:15px}
.context-options{position:absolute;z-index:40;top:calc(100% + 5px);left:0;right:0;padding:5px;background:#f8fbfd;border:1px solid var(--border);border-radius:10px;box-shadow:0 12px 28px rgba(16,42,67,.16);max-height:220px;overflow:auto}
.context-option{display:block;width:100%;padding:9px 10px;border:0;border-radius:7px;background:transparent;color:var(--text);text-align:left;font:inherit;cursor:pointer}
.context-option[aria-selected="true"]{background:#dce9f4;color:var(--primary-dark);font-weight:700}
.context-option-empty{padding:9px 10px;color:var(--muted);font-size:12px}
@media (hover:hover) and (pointer:fine){.context-combo-toggle:hover,.context-option:hover{background:var(--accent-light);color:var(--primary-dark)}}
.context-card.is-active{background:#fbfdff}
.context-card.is-active .context-note{display:none}
.compact-field{flex:0 1 150px}.machine-field{flex:0 1 110px}
.comp-card{border-top:0;border-left:4px solid var(--primary)}
.comp-card.comp-boq{border-left-color:#7198ba}.comp-card.comp-mf{border-left-color:#173f63}
th{background:#eaf1f7;color:#526b82;text-transform:uppercase;letter-spacing:.03em;font-size:10px}
th,td{padding:9px 10px}
.rec-item{transition:background .15s,border-color .15s}.rec-item.selected{background:#dce9f4;border-left:4px solid var(--primary)}
.filtered-source{margin:-2px 0 12px;color:var(--muted);font-size:11px}
.reference-filters{display:grid;grid-template-columns:minmax(240px,1.6fr) minmax(150px,.7fr) minmax(150px,.7fr) auto;gap:12px;align-items:end;margin:14px 0}
.reference-filters .btn{min-height:42px}
@media(max-width:780px){.context-grid{grid-template-columns:1fr 1fr}.context-jobon,.context-reference{grid-column:1/-1}.context-submit{width:100%}}
@media(max-width:520px){.context-grid{grid-template-columns:1fr}.context-jobon,.context-reference{grid-column:auto}}
</style>
</head>
<body class="app-shell">

<header class="dmo-app-header">
  <img class="dmo-app-header__logo" id="headerLogo" src="logo_recolored(1).png" alt="BA Glass">
  <div class="dmo-app-header__page">
    <h1>Pegamentos</h1>
    <p id="headerSub">Controlo dimensional · CM · BQ · MF</p>
  </div>
  <div class="dmo-app-header__user">
    <strong data-user-profile-name>João Silva</strong>
    <span data-user-profile-title>Metrologia</span>
  </div>
</header>

<nav class="tabs" role="tablist">
  <button type="button" class="active" data-tab="registo">Pegamentos</button>
  <button type="button" data-tab="referencias">Referências</button>
  <button type="button" data-tab="config">Configurações</button>
</nav>

<main>

<section id="registo" class="tab-panel active">
  <div id="contextCard" class="card context-card">
    <div class="section-heading">
      <div>
        <h2>Selecionar Job On</h2>
        <p class="muted context-help">O Job On fornece a produção e as instâncias exatas de CM, BQ e MF usadas nesta folha.</p>
      </div>
      <span class="context-step">1 · Contexto</span>
    </div>
    <div class="context-grid">
      <div class="field context-jobon">
        <label for="ctx-jobon">Job On / produção</label>
        <select id="ctx-jobon" onchange="applyJobOnContext()">
          <option value="">Selecionar Job On</option>
          <option value="JO-202601-B3">JO-202601-B3 · 202601 · 9389T194</option>
          <option value="JO-202603-B1">JO-202603-B1 · 202603 · 5774T173</option>
        </select>
      </div>
      <div class="field context-reference">
        <label for="ctx-reference">Referência</label>
        <div class="context-combo" id="referenceCombo">
          <input id="ctx-reference" placeholder="Herdada do Job On" readonly aria-readonly="true" autocomplete="off" aria-controls="referenceOptions" aria-expanded="false">
          <button type="button" class="context-combo-toggle" disabled aria-label="Referência herdada do Job On">▾</button>
          <div id="referenceOptions" class="context-options" role="listbox" hidden></div>
        </div>
      </div>
      <div class="field context-production">
        <label for="ctx-production">Produção</label>
        <input id="ctx-production" inputmode="numeric" maxlength="6" placeholder="Herdada" readonly aria-readonly="true">
      </div>
      <div class="field context-machine">
        <label for="ctx-machine">Máquina</label>
        <select id="ctx-machine" disabled aria-disabled="true">
          <option value="">Herdada</option>
          <option>B1</option><option>B2</option><option>B3</option>
          <option>C1</option><option>C2</option><option>C3</option>
        </select>
      </div>
      <button type="button" class="btn context-submit" onclick="startPegamentosSheet()">Abrir folha</button>
    </div>
    <div class="context-note"><strong>Fonte obrigatória:</strong> Referência, Produção, Máquina e lotes CM/BQ/MF vêm do Job On. Se faltar uma ferramenta, corrija o Job On antes de abrir a folha.</div>
  </div>
  <div id="noRecordCard" class="empty">Selecione um Job On completo para abrir a folha de Pegamentos.</div>
  <div id="recordArea" style="display:none">

    <div class="card">
      <div class="section-heading"><div><h2>Contexto ativo do Job On</h2><p class="muted context-help">A folha usa exatamente as ferramentas e lotes guardados neste Job On.</p></div>
        <div class="actions">
          <button class="btn secondary small" onclick="duplicateRecord()">Duplicar</button>
          <button class="btn danger small" onclick="deleteRecordConfirm()">Eliminar</button>
        </div>
      </div>
      <div class="field-row">
        <div class="field"><label>Job On</label><input id="f-jobon" readonly aria-readonly="true"></div>
        <div class="field"><label>Referência</label><input id="f-reference" readonly aria-readonly="true"></div>
        <div class="field compact-field"><label>Produção</label><input id="f-production" readonly aria-readonly="true"></div>
        <div class="field machine-field"><label>Máquina</label><input id="f-machine" readonly aria-readonly="true"></div>
        <div class="field"><label>Data</label><input id="f-date" type="date" oninput="onFieldChange()"></div>
      </div>
      <div class="field-row">
        <div class="field" style="flex:2 1 320px">
          <label>Nome do ficheiro — gerado automaticamente</label>
          <input id="f-filename" readonly aria-readonly="true">
        </div>
      </div>
      <div class="field-row">
        <div class="field" style="flex:1 1 100%">
          <label>Pasta resolvida do Job On/lote</label>
          <div id="referenceFolderStatus" class="folder-status">Capacidades / 5447T173</div>
          <p class="muted">O diretório principal vem das Definições e a subpasta foi definida na criação do lote no Peso. Não é alterada em Pegamentos.</p>
        </div>
      </div>
      <div class="field-row">
        <div class="field" style="flex:1 1 100%"><label>Observações</label><textarea id="f-notes" oninput="onFieldChange()"></textarea></div>
      </div>
    </div>

    <div id="componentsHost"></div>

    <div class="card">
      <div class="section-heading"><h2>Limites entre componentes (Pegamentos)</h2></div>
      <div class="field-row">
        <div class="field"><label>Ovalização máx. aceitável (mm)</label><input id="f-ovalmax" type="number" step="0.01" oninput="onToleranceChange()"></div>
        <div class="field" style="display:none"><label>Tolerância à folga nominal (mm)</label><input id="f-gaptol" type="number" step="0.01" oninput="onToleranceChange()"></div>
      </div>
      <div id="verificationHost"></div>
    </div>

    <div class="card">
      <h2>Relatório</h2><p class="muted">Pré-visualize e imprima a folha ou guarde-a em PDF.</p>
      <div class="actions">
        <button class="btn" onclick="printRecord()">Imprimir / Guardar PDF</button>
      </div>
      <div id="saveStatus" class="muted" style="margin-top:8px"></div>
    </div>

  </div>
</section>

<section id="referencias" class="tab-panel">
  <div class="card">
    <div class="section-heading"><h2>Histórico de Pegamentos</h2></div>
    <p class="muted">Um clique seleciona; duplo clique abre a folha associada.</p>
    <div class="reference-filters">
      <div class="field"><label for="recSearch">Job On, referência ou produção</label><input id="recSearch" type="search" placeholder="Pesquisar" oninput="renderRecordList()"></div>
      <div class="field"><label for="recDateFrom">Desde</label><input id="recDateFrom" type="date" oninput="renderRecordList()"></div>
      <div class="field"><label for="recDateTo">Até</label><input id="recDateTo" type="date" oninput="renderRecordList()"></div>
      <button type="button" class="btn" onclick="clearRecordFilters()">Limpar filtros</button>
    </div>
    <div id="recordList"></div>
  </div>
</section>

<section id="config" class="tab-panel">
  <div class="card">
    <h2>Pasta de relatórios</h2>
    <p class="muted">Escolha o diretório principal neste computador. A subpasta de cada lote é definida no Peso e reutilizada por Peso e Pegamentos (ex.: <strong>Capacidades / 5447T173</strong>).</p>
    <div id="folderStatus" class="folder-status">Nenhuma pasta principal selecionada.</div>
    <div class="actions" style="margin-top:12px">
      <button class="btn" onclick="chooseMainFolder()">Selecionar pasta principal</button>
    </div>
    <div class="notice">No Chrome ou Edge, o navegador pode pedir autorização para voltar a usar a pasta depois de reiniciar. A funcionalidade de gravação direta na pasta só funciona nestes navegadores; noutros, os ficheiros são descarregados normalmente.</div>
  </div>

  <div class="card">
    <h2>Valores por omissão</h2>
    <div class="field-row">
      <div class="field"><label>Ovalização máx. (mm)</label><input id="cfg-ovalmax" type="number" step="0.01"></div>
      <div class="field" style="display:none"><label>Tolerância à folga nominal (mm)</label><input id="cfg-gaptol" type="number" step="0.01"></div>
    </div>
    <div class="actions"><button class="btn" onclick="saveDefaults()">Guardar valores por omissão</button></div>
  </div>

</section>

</main>

<footer class="app-credit">Desenvolvido por Diogo Oliveira · 2026</footer>

<div id="printArea"></div>

<script>
/* ---------------- constants ---------------- */
const LOGO_DATA_URL = document.getElementById('headerLogo') ? document.getElementById('headerLogo').src : '';
const DB_KEY='pegamentosDB';
const APP_VERSION=4;
const NOMINAL_TOLERANCE=0.20;
const COMPONENTS=[
  {key:'cm', label:'CM — Contra-molde', bs:2},
  {key:'boq',label:'Boquilha',          bs:8},
  {key:'mf', label:'Molde final',       bs:14}
];
const GAPS=[
  {from:'cm', to:'boq', label:'CM → Boquilha'},
  {from:'boq',to:'mf',  label:'Boquilha → Molde final'}
];
const CONTEXT_REFERENCES=['9389T194','5447T173','9121T173'];
const JOB_ON_CONTEXTS={
  'JO-202601-B3':{reference:'9389T194',production:'202601',machine:'B3',subfolder:'5447T173',comp:{cm:{ref:'5447',lote:'4'},boq:{ref:'T194',lote:'12'},mf:{ref:'9389',lote:'26'}}},
  'JO-202603-B1':{reference:'5774T173',production:'202603',machine:'B1',subfolder:'5774T173',comp:{cm:{ref:'5809',lote:'01'},boq:{ref:'T173',lote:'24/33'},mf:{ref:'5810',lote:'01'}}}
};

let db=loadDB();
let activeId=db.activeId||null;
let selectedRecordId=null;
let mainDirHandle=null;
let currentTab='registo';

/* ---------------- persistence ---------------- */
function seedDB(){
  return {
    version:APP_VERSION,
    records:[],
    activeId:null,
    defaults:{ovalMax:0.20,gapTol:0.05},
    meta:{mainFolderName:''}
  };
}
function loadDB(){
  try{ return normalizeDB(JSON.parse(localStorage.getItem(DB_KEY))||seedDB()); }
  catch(e){ return seedDB(); }
}
function saveDB(){ localStorage.setItem(DB_KEY, JSON.stringify(db)); }
function normalizeDB(raw){
  const out = raw && typeof raw==='object' ? raw : seedDB();
  out.version = APP_VERSION;
  out.records = Array.isArray(out.records) ? out.records.filter(Boolean).map(normalizeRecord) : [];
  out.defaults = out.defaults && typeof out.defaults==='object' ? out.defaults : {};
  out.defaults.ovalMax = validNum(out.defaults.ovalMax, 0.20);
  out.defaults.gapTol = validNum(out.defaults.gapTol, 0.05);
  out.meta = out.meta && typeof out.meta==='object' ? out.meta : {};
  out.meta.mainFolderName = String(out.meta.mainFolderName||'');
  out.activeId = out.records.some(r=>r.id===out.activeId) ? out.activeId : null;
  return out;
}
function normalizeRecord(r){
  r.id = r.id || uid();
  r.jobOnId = String(r.jobOnId||'').trim();
  r.reference = String(r.reference||'').trim();
  r.machine = String(r.machine||'').toUpperCase().trim();
  r.production = String(r.production||'').replace(/\D/g,'').slice(0,6);
  if(!r.production){
    const oldProduction = String(r.filenameBase||'').match(/(?:^|_)(\d{6})(?:_|$)/);
    r.production = oldProduction ? oldProduction[1] : currentProduction();
  }
  r.filenameBase = buildFilenameBase(r.reference, r.production, r.jobOnId);
  r.referenceFolderName = String(r.referenceFolderName||'').trim();
  r.date = isDate(r.date) ? r.date : localDateString();
  r.notes = String(r.notes||'');
  r.createdAt = r.createdAt || nowISO();
  r.updatedAt = r.updatedAt || r.createdAt;
  r.tolerance = r.tolerance && typeof r.tolerance==='object' ? r.tolerance : {};
  r.tolerance.ovalMax = validNum(r.tolerance.ovalMax, db && db.defaults ? db.defaults.ovalMax : 0.20);
  r.tolerance.gapTol = validNum(r.tolerance.gapTol, db && db.defaults ? db.defaults.gapTol : 0.05);
  r.comp = r.comp && typeof r.comp==='object' ? r.comp : {};
  COMPONENTS.forEach(c=>{
    const cur = r.comp[c.key] && typeof r.comp[c.key]==='object' ? r.comp[c.key] : {};
    cur.ref = String(cur.ref||'').trim();
    cur.lote = String(cur.lote||'').trim();
    if(!cur.lote && cur.ref.includes('/')){
      const refParts = cur.ref.split('/');
      cur.ref = refParts[0].trim();
      cur.lote = refParts.slice(1).join('/').trim();
    }
    cur.util = cur.util==null||cur.util==='' ? null : validNum(cur.util,null);
    cur.nominal = cur.nominal==null||cur.nominal==='' ? null : validNum(cur.nominal,null);
    cur.rows = Array.isArray(cur.rows) ? cur.rows.filter(Boolean).map(normalizeRow) : [];
    if(cur.rows.length===0){ for(let i=0;i<4;i++) cur.rows.push(blankRow()); }
    r.comp[c.key] = cur;
  });
  return r;
}
function normalizeRow(row){
  let nr = row.nr==null ? '' : String(row.nr);
  if(nr.includes('/')) nr = nr.split('/')[0].trim();
  return {
    id: row.id || uid(),
    nr: nr.trim(),
    costura: row.costura==null||row.costura==='' ? '' : validNum(row.costura,''),
    noventa: row.noventa==null||row.noventa==='' ? '' : validNum(row.noventa,'')
  };
}
function blankRow(){ return {id:uid(), nr:'', costura:'', noventa:''}; }

/* ---------------- helpers ---------------- */
function uid(){ return Date.now().toString(36)+Math.random().toString(36).slice(2,8); }
function validNum(v,fallback){ if(v===''||v==null) return fallback; const n=Number(v); return Number.isFinite(n) ? n : fallback; }
function nowISO(){ return new Date().toISOString(); }
function localDateString(d=new Date()){ const y=d.getFullYear(),m=String(d.getMonth()+1).padStart(2,'0'),day=String(d.getDate()).padStart(2,'0'); return `${y}-${m}-${day}`; }
function currentProduction(d=new Date()){ return `${d.getFullYear()}${String(d.getMonth()+1).padStart(2,'0')}`; }
function buildFilenameBase(reference,production,jobOnId){
  const ref = safeName(reference||'sem_referencia');
  const prod = String(production||currentProduction()).replace(/\D/g,'').slice(0,6) || currentProduction();
  const job = safeName(jobOnId||'sem_job_on');
  return `Pegamentos_${prod}_${ref}_${job}`;
}
function updateAutomaticFilename(rec){
  rec.filenameBase = buildFilenameBase(rec.reference, rec.production, rec.jobOnId);
  const field=document.getElementById('f-filename');
  if(field) field.value = `${rec.filenameBase}_relatorio.pdf`;
}
function isDate(v){ return /^\d{4}-\d{2}-\d{2}$/.test(String(v||'')); }
function safeName(v){ return String(v||'').trim().replace(/[\/:*?"<>|]+/g,'_').replace(/\s+/g,'_') || 'sem_nome'; }
function escapeHtml(s){ return String(s??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c])); }
function fmtNum(v,dec=2){ return (v==null || !Number.isFinite(v)) ? '—' : v.toFixed(dec); }
function fmtSigned(v,dec=2){ if(v==null || !Number.isFinite(v)) return '—'; const s=v>=0?'+':''; return s+v.toFixed(dec); }
function nominalMin(nominal){ return nominal==null || !Number.isFinite(nominal) ? null : nominal - NOMINAL_TOLERANCE; }
function nominalMax(nominal){ return nominal==null || !Number.isFinite(nominal) ? null : nominal + NOMINAL_TOLERANCE; }
function toast(msg,type='info'){
  const t=document.createElement('div');
  t.className='toast';
  t.style.borderLeftColor = type==='error' ? 'var(--danger)' : type==='ok' ? 'var(--ok)' : 'var(--primary)';
  t.textContent=msg;
  document.body.appendChild(t);
  setTimeout(()=>t.remove(),3800);
}
function fail(e){ console.error(e); toast(e && e.message ? e.message : 'Ocorreu um erro.','error'); }
function renderReferenceOptions(){
  const input=document.getElementById('ctx-reference');
  const host=document.getElementById('referenceOptions');
  const q=(input.value||'').trim().toLowerCase();
  const values=CONTEXT_REFERENCES.filter(value=>!q||value.toLowerCase().includes(q));
  host.innerHTML=values.length?values.map(value=>`<button type="button" class="context-option" role="option" aria-selected="${input.value===value?'true':'false'}" onclick="selectContextReference('${value}')">${value}</button>`).join(''):'<div class="context-option-empty">Sem referências correspondentes.</div>';
  host.hidden=false; input.setAttribute('aria-expanded','true');
}
function openReferenceOptions(){ renderReferenceOptions(); }
function toggleReferenceOptions(){
  const host=document.getElementById('referenceOptions');
  if(host.hidden) renderReferenceOptions(); else closeReferenceOptions();
}
function closeReferenceOptions(){
  document.getElementById('referenceOptions').hidden=true;
  document.getElementById('ctx-reference').setAttribute('aria-expanded','false');
}
function selectContextReference(value){
  document.getElementById('ctx-reference').value=value;
  closeReferenceOptions();
  document.getElementById('ctx-production').focus();
}
function applyJobOnContext(){
  const jobOnId=document.getElementById('ctx-jobon').value;
  const context=JOB_ON_CONTEXTS[jobOnId];
  document.getElementById('ctx-reference').value=context?context.reference:'';
  document.getElementById('ctx-production').value=context?context.production:'';
  document.getElementById('ctx-machine').value=context?context.machine:'';
}
document.addEventListener('click',event=>{ if(!event.target.closest('#referenceCombo')) closeReferenceOptions(); });

/* ---------------- tabs ---------------- */
function showTab(id){
  currentTab=id;
  document.querySelectorAll('.tab-panel').forEach(p=>p.classList.toggle('active', p.id===id));
  document.querySelectorAll('nav.tabs button').forEach(b=>b.classList.toggle('active', b.dataset.tab===id));
  if(id==='referencias') renderRecordList();
  if(id==='config') renderConfig();
}
document.querySelector('nav.tabs').addEventListener('click', e=>{
  const b=e.target.closest('button[data-tab]');
  if(!b) return;
  showTab(b.dataset.tab);
});

/* ---------------- record CRUD ---------------- */
function newRecord(){
  const now = new Date();
  const yyyymm = `${now.getFullYear()}${String(now.getMonth()+1).padStart(2,'0')}`;
  const r = normalizeRecord({
    reference:'',
    production:yyyymm,
    date: localDateString(),
    tolerance:{ovalMax:db.defaults.ovalMax, gapTol:db.defaults.gapTol}
  });
  db.records.unshift(r);
  activeId=r.id; db.activeId=r.id;
  saveDB();
  showTab('registo');
  renderRecordArea();
  document.getElementById('ctx-reference').value='';
  document.getElementById('ctx-production').value=yyyymm;
  document.getElementById('ctx-machine').value='';
  document.getElementById('ctx-reference').focus();
  toast('Preencha o contexto para abrir a nova folha.','ok');
}
function getActive(){ return db.records.find(r=>r.id===activeId) || null; }
function openRecord(id){
  activeId=id; db.activeId=id; saveDB();
  showTab('registo');
  renderRecordArea();
}
function startPegamentosSheet(){
  const jobOnId=document.getElementById('ctx-jobon').value;
  const context=JOB_ON_CONTEXTS[jobOnId];
  const reference=document.getElementById('ctx-reference').value.trim();
  const production=document.getElementById('ctx-production').value.replace(/\D/g,'').slice(0,6);
  const machine=document.getElementById('ctx-machine').value;
  if(!jobOnId || !context || !reference || production.length!==6 || !machine || !context.comp.cm || !context.comp.boq || !context.comp.mf){
    toast('Selecione um Job On com CM, BQ e MF completos antes de abrir a folha.','error');
    return;
  }
  let rec=getActive();
  if(!rec){
    rec=normalizeRecord({jobOnId,reference,production,machine,referenceFolderName:context.subfolder,comp:context.comp,date:localDateString(),tolerance:{ovalMax:db.defaults.ovalMax,gapTol:db.defaults.gapTol}});
    db.records.unshift(rec); activeId=rec.id; db.activeId=rec.id;
  }
  rec.jobOnId=jobOnId; rec.reference=reference; rec.production=production; rec.machine=machine; rec.referenceFolderName=context.subfolder; rec.comp=JSON.parse(JSON.stringify(context.comp)); rec.updatedAt=nowISO();
  normalizeRecord(rec);
  updateAutomaticFilename(rec); saveDB(); renderRecordArea();
  document.getElementById('recordArea').scrollIntoView({behavior:'smooth',block:'start'});
}
function deleteRecordConfirm(){
  const rec=getActive(); if(!rec) return;
  if(!confirm(`Eliminar definitivamente a referência "${rec.reference||'(sem nome)'}"? Esta ação não pode ser desfeita.`)) return;
  db.records = db.records.filter(r=>r.id!==rec.id);
  activeId=null; db.activeId=null;
  saveDB();
  renderRecordArea();
  toast('Referência eliminada.','ok');
}
function duplicateRecord(){
  const rec=getActive(); if(!rec) return;
  const copy = JSON.parse(JSON.stringify(rec));
  copy.id = uid();
  copy.createdAt = nowISO(); copy.updatedAt = copy.createdAt;
  COMPONENTS.forEach(c=>{ copy.comp[c.key].rows.forEach(row=>row.id=uid()); });
  db.records.unshift(copy);
  activeId=copy.id; db.activeId=copy.id;
  saveDB();
  renderRecordArea();
  toast('Referência duplicada. O nome do ficheiro foi atualizado automaticamente.','ok');
}

/* ---------------- record area rendering ---------------- */
function renderRecordArea(){
  const rec=getActive();
  const ready=!!(rec && rec.jobOnId && rec.reference && /^\d{6}$/.test(rec.production) && rec.machine && rec.comp.cm.ref && rec.comp.boq.ref && rec.comp.mf.ref);
  document.getElementById('noRecordCard').style.display = ready ? 'none' : 'block';
  document.getElementById('recordArea').style.display = ready ? 'block' : 'none';
  document.getElementById('contextCard').classList.toggle('is-active',ready);
  document.getElementById('ctx-reference').value=rec ? rec.reference : '';
  document.getElementById('ctx-production').value=rec ? rec.production : currentProduction();
  document.getElementById('ctx-machine').value=rec ? rec.machine : '';
  document.getElementById('ctx-jobon').value=rec ? rec.jobOnId : '';
  document.getElementById('headerSub').textContent = rec ? `A editar: ${rec.reference || '(sem referência)'}` : 'Sobreposição CM · Boquilha · Molde Final';
  if(!ready) return;
  document.getElementById('f-jobon').value = rec.jobOnId;
  document.getElementById('f-reference').value = rec.reference;
  document.getElementById('f-production').value = rec.production;
  document.getElementById('f-machine').value = rec.machine;
  updateAutomaticFilename(rec);
  document.getElementById('f-date').value = rec.date;
  document.getElementById('f-notes').value = rec.notes;
  document.getElementById('f-ovalmax').value = rec.tolerance.ovalMax;
  document.getElementById('f-gaptol').value = rec.tolerance.gapTol;
  renderComponents(rec);
  renderVerification(rec);
  renderReferenceFolderStatus(rec);
  document.getElementById('saveStatus').textContent='';
}
function onFieldChange(){
  const rec=getActive(); if(!rec) return;
  rec.reference = document.getElementById('f-reference').value.trim();
  rec.production = document.getElementById('f-production').value.replace(/\D/g,'').slice(0,6);
  document.getElementById('f-production').value = rec.production;
  updateAutomaticFilename(rec);
  rec.date = document.getElementById('f-date').value || localDateString();
  rec.notes = document.getElementById('f-notes').value;
  rec.updatedAt = nowISO();
  document.getElementById('headerSub').textContent = `A editar: ${rec.reference || '(sem referência)'}`;
  saveDB();
}
function onToleranceChange(){
  const rec=getActive(); if(!rec) return;
  rec.tolerance.ovalMax = validNum(document.getElementById('f-ovalmax').value, 0.20);
  rec.tolerance.gapTol = validNum(document.getElementById('f-gaptol').value, 0.05);
  rec.updatedAt = nowISO();
  saveDB();
  renderComponents(rec);
  renderVerification(rec);
}

/* ---------------- component blocks ---------------- */
function renderComponents(rec){
  const host=document.getElementById('componentsHost');
  host.innerHTML = COMPONENTS.map(c=>componentCardHTML(rec,c)).join('');
  COMPONENTS.forEach(c=>refreshComponentCalc(rec,c.key));
}
function componentCardHTML(rec,c){
  const comp = rec.comp[c.key];
  const componentControl=componentControlHTML(c.key,comp.ref,rec.reference,rec.machine);
  return `
  <div class="card comp-card comp-${c.key}">
    <div class="section-heading"><h2>${escapeHtml(c.label)}</h2></div>
    <div class="field-row">
      <div class="field"><label>${c.key==='cm'?'CM':c.key==='boq'?'BQ':'MF'}</label>${componentControl}</div>
      <div class="field"><label>Lote</label><input id="lote-${c.key}" value="${escapeHtml(comp.lote||'')}" readonly aria-readonly="true"></div>
      <div class="field"><label>Nominal (mm)</label><input id="nom-${c.key}" type="number" step="0.01" value="${comp.nominal==null?'':comp.nominal}" oninput="onCompMeta('${c.key}')"></div>
      <div class="field"><label>Mínimo permitido (nominal − ${fmtNum(NOMINAL_TOLERANCE)} mm)</label><input id="min-${c.key}" value="${nominalMin(comp.nominal)==null?'':fmtNum(nominalMin(comp.nominal))}" readonly aria-readonly="true"></div>
      <div class="field"><label>Máximo permitido (nominal + ${fmtNum(NOMINAL_TOLERANCE)} mm)</label><input id="max-${c.key}" value="${nominalMax(comp.nominal)==null?'':fmtNum(nominalMax(comp.nominal))}" readonly aria-readonly="true"></div>
      <div class="field"><label>% Utilização</label><input id="util-${c.key}" type="number" step="0.01" value="${comp.util==null?'':comp.util}" oninput="onCompMeta('${c.key}')"></div>
    </div>
    <div class="filtered-source">${c.key==='cm'?'CM/lote herdados do Job On, com origem no Peso':c.key==='boq'?'BQ/lote herdados do Job On, com origem em Boquilhas/Reparação':'MF/lote herdados do Job On, com origem no domínio MF'} · ${escapeHtml(rec.jobOnId)} · ${escapeHtml(rec.reference)} / ${escapeHtml(rec.machine)}.</div>
    <div class="table-scroll">
      <table>
        <thead><tr><th>N.º</th><th>Costura</th><th>Contra costura</th><th>Ovalização</th><th>Média</th><th></th></tr></thead>
        <tbody id="rows-${c.key}"></tbody>
        <tfoot><tr>
          <td>AVG</td>
          <td id="avgc-${c.key}" class="readonly-cell">—</td>
          <td id="avg9-${c.key}" class="readonly-cell">—</td>
          <td id="avgo-${c.key}" class="readonly-cell">—</td>
          <td id="avgm-${c.key}" class="readonly-cell">—</td>
          <td></td>
        </tr></tfoot>
      </table>
    </div>
    <div class="actions" style="margin-top:10px">
      <button class="btn secondary small" onclick="addRow('${c.key}')">+ Linha</button>
      <span class="muted" id="n-${c.key}" style="align-self:center"></span>
    </div>
  </div>`;
}
const COMPONENT_CATALOG={
  cm:[{value:'9389',lote:'26',references:['9389T194'],machines:['B1','B3','C1']},{value:'5447',lote:'11',references:['5447T173'],machines:['B2','C1']}],
  boq:[{value:'T194',lote:'12',references:['9389T194'],machines:['B1','B3','C1']},{value:'T173',lote:'24/33',references:['5447T173','9121T173'],machines:['B2','C1']}]
};
function componentControlHTML(key,current,reference,machine){
  return `<input id="ref-${key}" value="${escapeHtml(current)}" readonly aria-readonly="true">`;
}
function onCompMeta(key){
  const rec=getActive(); if(!rec) return;
  const comp=rec.comp[key];
  comp.ref = document.getElementById(`ref-${key}`).value.trim();
  if(key!=='mf'){
    const source=(COMPONENT_CATALOG[key]||[]).find(x=>x.value===comp.ref && x.references.includes(rec.reference) && x.machines.includes(rec.machine));
    if(source) document.getElementById(`lote-${key}`).value=source.lote;
  }
  comp.lote = document.getElementById(`lote-${key}`).value.trim();
  comp.nominal = document.getElementById(`nom-${key}`).value==='' ? null : validNum(document.getElementById(`nom-${key}`).value,null);
  const minField = document.getElementById(`min-${key}`);
  const maxField = document.getElementById(`max-${key}`);
  if(minField) minField.value = nominalMin(comp.nominal)==null ? '' : fmtNum(nominalMin(comp.nominal));
  if(maxField) maxField.value = nominalMax(comp.nominal)==null ? '' : fmtNum(nominalMax(comp.nominal));
  comp.util = document.getElementById(`util-${key}`).value==='' ? null : validNum(document.getElementById(`util-${key}`).value,null);
  rec.updatedAt = nowISO();
  saveDB();
  renderVerification(rec);
}
function addRow(key){
  const rec=getActive(); if(!rec) return;
  rec.comp[key].rows.push(blankRow());
  rec.updatedAt = nowISO();
  saveDB();
  refreshComponentCalc(rec,key);
  renderVerification(rec);
}
function removeRow(key,rowId){
  const rec=getActive(); if(!rec) return;
  rec.comp[key].rows = rec.comp[key].rows.filter(r=>r.id!==rowId);
  if(rec.comp[key].rows.length===0) rec.comp[key].rows.push(blankRow());
  rec.updatedAt = nowISO();
  saveDB();
  refreshComponentCalc(rec,key);
  renderVerification(rec);
}
function onRowChange(key,rowId,field,el){
  const rec=getActive(); if(!rec) return;
  const row = rec.comp[key].rows.find(r=>r.id===rowId);
  if(!row) return;
  if(field==='costura' || field==='noventa'){
    row[field] = el.value==='' ? '' : validNum(el.value,'');
  } else {
    row[field] = el.value;
  }
  rec.updatedAt = nowISO();
  saveDB();
  refreshComponentCalc(rec,key,false);
  renderVerification(rec);
}

/* Recalculate a component. Rows are rebuilt only for add/remove/open actions.
   While typing, calculated cells update in place so focus is never lost. */
function refreshComponentCalc(rec,key,rebuildRows=true){
  const comp = rec.comp[key];
  const calc = calcComponent(comp, rec.tolerance.ovalMax);
  const tbody = document.getElementById(`rows-${key}`);
  if(tbody && rebuildRows){
    tbody.innerHTML = comp.rows.map((row,i)=>{
      const rc = calc.rows[i];
      const ovalClass = rc.oval!=null && Math.abs(rc.oval) > rec.tolerance.ovalMax ? 'readonly-cell bad' : 'readonly-cell';
      const mediaClass = rc.outsideNominalRange ? 'readonly-cell bad' : 'readonly-cell';
      return `<tr>
        <td><input class="nr-input" value="${escapeHtml(row.nr)}" oninput="onRowChange('${key}','${row.id}','nr',this)"></td>
        <td><input type="number" step="0.01" value="${row.costura===''?'':row.costura}" oninput="onRowChange('${key}','${row.id}','costura',this)"></td>
        <td><input type="number" step="0.01" value="${row.noventa===''?'':row.noventa}" oninput="onRowChange('${key}','${row.id}','noventa',this)"></td>
        <td id="oval-${key}-${row.id}" class="${ovalClass}">${rc.oval==null?'—':fmtSigned(rc.oval)}</td>
        <td id="media-${key}-${row.id}" class="${mediaClass}">${rc.media==null?'—':fmtNum(rc.media)}</td>
        <td><button class="row-del" title="Remover linha" onclick="removeRow('${key}','${row.id}')">×</button></td>
      </tr>`;
    }).join('');
  } else if(tbody) {
    comp.rows.forEach((row,i)=>{
      const rc=calc.rows[i];
      const oval=document.getElementById(`oval-${key}-${row.id}`);
      const media=document.getElementById(`media-${key}-${row.id}`);
      if(oval){
        oval.textContent=rc.oval==null?'—':fmtSigned(rc.oval);
        oval.className=rc.oval!=null && Math.abs(rc.oval)>rec.tolerance.ovalMax ? 'readonly-cell bad' : 'readonly-cell';
      }
      if(media){
        media.textContent=rc.media==null?'—':fmtNum(rc.media);
        media.className=rc.outsideNominalRange ? 'readonly-cell bad' : 'readonly-cell';
      }
    });
  }
  const set=(id,v)=>{ const el=document.getElementById(id); if(el) el.textContent=v; };
  set(`avgc-${key}`, fmtNum(calc.avgCostura));
  set(`avg9-${key}`, fmtNum(calc.avgNoventa));
  set(`avgo-${key}`, fmtSigned(calc.avgOval));
  set(`avgm-${key}`, fmtNum(calc.avgMedia));
  const avgMediaEl=document.getElementById(`avgm-${key}`);
  if(avgMediaEl) avgMediaEl.className=calc.avgOutsideNominalRange ? 'readonly-cell bad' : 'readonly-cell';
  set(`n-${key}`, `${calc.n} amostra(s)${calc.rangeViolations.length? ' · '+calc.rangeViolations.length+' fora do intervalo permitido' : ''}`);
}

/* ---------------- calculations ---------------- */
function calcComponent(comp, ovalMax){
  const min = nominalMin(comp.nominal);
  const max = nominalMax(comp.nominal);
  const rows = comp.rows.map(row=>{
    const c = row.costura===''?null:row.costura;
    const n = row.noventa===''?null:row.noventa;
    const oval = (c!=null && n!=null) ? (c-n) : null;
    const media = (c!=null && n!=null) ? (c+n)/2 : (c!=null ? c : (n!=null ? n : null));
    const outsideNominalRange = media!=null && min!=null && max!=null && (media < min || media > max);
    return {c,n,oval,media,outsideNominalRange};
  });
  const withData = rows.filter(r=>r.c!=null || r.n!=null);
  const costuraVals = rows.filter(r=>r.c!=null).map(r=>r.c);
  const noventaVals = rows.filter(r=>r.n!=null).map(r=>r.n);
  const avgCostura = costuraVals.length ? costuraVals.reduce((a,b)=>a+b,0)/costuraVals.length : null;
  const avgNoventa = noventaVals.length ? noventaVals.reduce((a,b)=>a+b,0)/noventaVals.length : null;
  const avgOval = (avgCostura!=null && avgNoventa!=null) ? (avgCostura-avgNoventa) : null;
  const avgMedia = (avgCostura!=null && avgNoventa!=null) ? (avgCostura+avgNoventa)/2 : (avgCostura!=null?avgCostura:avgNoventa);
  const violations = rows.map((r,i)=>({i,row:comp.rows[i],oval:r.oval})).filter(x=>x.oval!=null && Math.abs(x.oval) > ovalMax);
  const rangeViolations = rows.map((r,i)=>({i,row:comp.rows[i],media:r.media})).filter(x=>x.media!=null && min!=null && max!=null && (x.media < min || x.media > max));
  const avgOutsideNominalRange = avgMedia!=null && min!=null && max!=null && (avgMedia < min || avgMedia > max);
  return {rows, n:withData.length, avgCostura, avgNoventa, avgOval, avgMedia, violations, nominalMin:min, nominalMax:max, rangeViolations, avgOutsideNominalRange};
}
function calcRecord(rec){
  const comps = {};
  COMPONENTS.forEach(c=>{ comps[c.key] = calcComponent(rec.comp[c.key], rec.tolerance.ovalMax); });
  const gaps = GAPS.map(g=>{
    const fromComp = rec.comp[g.from], toComp = rec.comp[g.to];
    const fromCalc = comps[g.from], toCalc = comps[g.to];
    const nominalGap = (fromComp.nominal!=null && toComp.nominal!=null) ? (toComp.nominal - fromComp.nominal) : null;
    const measuredGap = (fromCalc.avgMedia!=null && toCalc.avgMedia!=null) ? (toCalc.avgMedia - fromCalc.avgMedia) : null;
    return {...g, nominalGap, measuredGap, status:(measuredGap!=null && nominalGap!=null)?'info':'pending'};
  });
  return {comps, gaps};
}

function clamp(v,min,max){ return Math.min(max, Math.max(min, v)); }
function toleranceGraphicHTML({title, nominal, min, max, measured, sampleCount=0, compact=false}){
  const hasNominal = nominal!=null && min!=null && max!=null;
  const hasMeasured = measured!=null;
  const tol = hasNominal ? Math.max(max-min, NOMINAL_TOLERANCE*2) : NOMINAL_TOLERANCE*2;
  const pad = Math.max(tol*0.35, 0.10);
  const domainMin = hasNominal ? min-pad : (hasMeasured ? measured-0.30 : 0);
  const domainMax = hasNominal ? max+pad : (hasMeasured ? measured+0.30 : 1);
  const span = Math.max(domainMax-domainMin, 0.0001);
  const toPct = value => clamp(((value-domainMin)/span)*100,0,100);
  const bandLeft = hasNominal ? toPct(min) : 0;
  const bandRight = hasNominal ? toPct(max) : 100;
  const nominalLeft = hasNominal ? toPct(nominal) : 50;
  const markerLeft = hasMeasured ? toPct(measured) : null;
  const within = hasMeasured && hasNominal ? (measured>=min && measured<=max) : null;
  const statusText = !hasMeasured ? 'Sem média medida' : within===null ? 'Sem intervalo nominal' : within ? 'Dentro do intervalo' : 'Fora do intervalo';
  const statusClass = within===false ? 'bad' : 'ok';
  return `<div class="tolerance-viz ${compact?'compact':''}">
    <div class="viz-title">${escapeHtml(title)} — visualização dimensional</div>
    <div class="viz-meta">
      <div><span>Nominal</span> <strong>${nominal==null?'—':fmtNum(nominal)} mm</strong></div>
      <div><span>Permitido</span> <strong>${min==null?'—':fmtNum(min)}–${max==null?'—':fmtNum(max)} mm</strong></div>
      <div><span>Média</span> <strong class="${within===false?'bad':''}">${measured==null?'—':fmtNum(measured)} mm</strong></div>
    </div>
    <div class="viz-scale">
      <div class="viz-track"></div>
      ${hasNominal ? `<div class="viz-band" style="left:${bandLeft}%;width:${Math.max(0,bandRight-bandLeft)}%"></div>` : ''}
      ${hasNominal ? `<div class="viz-nominal" style="left:${nominalLeft}%" title="Nominal"></div>` : ''}
      ${hasMeasured ? `<div class="viz-marker ${within===false?'bad':''}" style="left:${markerLeft}%" title="Média medida"></div>` : ''}
    </div>
    <div class="viz-labels">
      <span>${fmtNum(domainMin)} mm</span>
      <span>${fmtNum(domainMax)} mm</span>
    </div>
    <div class="viz-legend">
      <span>Faixa verde = intervalo permitido</span>
      <strong class="${statusClass}">${statusText}${sampleCount?` · ${sampleCount} amostra(s)`:''}</strong>
    </div>
  </div>`;
}


const COMPONENT_COLORS={cm:'#16A34A',boq:'#A855F7',mf:'#0891B2'};
function componentBoundaryLimits(key,rec){
  const own=rec.comp[key] ? rec.comp[key].nominal : null;
  const cm=rec.comp.cm.nominal, boq=rec.comp.boq.nominal, mf=rec.comp.mf.nominal;
  if(own==null) return {lower:null,upper:null,lowerLabel:'',upperLabel:''};
  if(key==='cm'){
    return {
      lower:own-NOMINAL_TOLERANCE,
      upper:boq!=null ? boq : own+NOMINAL_TOLERANCE,
      lowerLabel:'Limite mínimo do CM',
      upperLabel:boq!=null ? 'Nominal da Boquilha' : 'Limite máximo do CM'
    };
  }
  if(key==='boq'){
    return {
      lower:cm!=null ? cm : own-NOMINAL_TOLERANCE,
      upper:mf!=null ? mf : own+NOMINAL_TOLERANCE,
      lowerLabel:cm!=null ? 'Nominal do CM' : 'Limite mínimo da Boquilha',
      upperLabel:mf!=null ? 'Nominal do Molde final' : 'Limite máximo da Boquilha'
    };
  }
  return {
    lower:boq!=null ? boq : own-NOMINAL_TOLERANCE,
    upper:own+NOMINAL_TOLERANCE,
    lowerLabel:boq!=null ? 'Nominal da Boquilha' : 'Limite mínimo do Molde final',
    upperLabel:'Limite máximo do Molde final'
  };
}
function measurementBoundaryState(key,value,rec){
  const limits=componentBoundaryLimits(key,rec);
  if(value==null || limits.lower==null || limits.upper==null){
    return {status:'pending',boundary:null,amount:null,limits};
  }
  const eps=0.005;
  if(value < limits.lower-eps){
    return {status:'critico',boundary:limits.lowerLabel,amount:limits.lower-value,direction:'abaixo',limits};
  }
  if(value > limits.upper+eps){
    return {status:'critico',boundary:limits.upperLabel,amount:value-limits.upper,direction:'acima',limits};
  }
  if(Math.abs(value-limits.lower)<=eps){
    return {status:'warn',boundary:limits.lowerLabel,amount:0,direction:'no limite',limits};
  }
  if(Math.abs(value-limits.upper)<=eps){
    return {status:'warn',boundary:limits.upperLabel,amount:0,direction:'no limite',limits};
  }
  const lowerMargin=value-limits.lower;
  const upperMargin=limits.upper-value;
  return {
    status:'ok',
    boundary:lowerMargin<=upperMargin ? limits.lowerLabel : limits.upperLabel,
    amount:Math.min(lowerMargin,upperMargin),
    direction:'dentro',
    limits
  };
}
function boundaryAssessment(rec,comps){
  const points=[];
  COMPONENTS.forEach(c=>{
    const cc=comps[c.key];
    cc.rows.forEach((rowCalc,i)=>{
      if(rowCalc.media==null) return;
      const row=rec.comp[c.key].rows[i];
      points.push({
        key:c.key,
        component:c.label,
        nr:String(row.nr||i+1),
        value:rowCalc.media,
        state:measurementBoundaryState(c.key,rowCalc.media,rec)
      });
    });
  });
  const critical=points.filter(p=>p.state.status==='critico');
  const atBoundary=points.filter(p=>p.state.status==='warn');
  const spacings=[];
  if(rec.comp.cm.nominal!=null && rec.comp.boq.nominal!=null){
    spacings.push({label:'CM → Boquilha',value:rec.comp.boq.nominal-rec.comp.cm.nominal});
  }
  if(rec.comp.boq.nominal!=null && rec.comp.mf.nominal!=null){
    spacings.push({label:'Boquilha → Molde final',value:rec.comp.mf.nominal-rec.comp.boq.nominal});
  }
  const spacingWarnings=spacings.filter(s=>Math.abs(s.value-NOMINAL_TOLERANCE)>0.005);
  let status='pending';
  if(points.length) status=critical.length?'critico':atBoundary.length?'warn':'ok';
  return {points,critical,atBoundary,spacings,spacingWarnings,status};
}
function boundaryAssessmentText(assessment){
  if(!assessment.points.length) return 'Ainda não existem medições para representar.';
  if(assessment.critical.length){
    return `${assessment.critical.length} medição(ões) ultrapassaram uma linha limite.`;
  }
  if(assessment.atBoundary.length){
    return `${assessment.atBoundary.length} medição(ões) estão exatamente numa linha limite.`;
  }
  return 'Todas as medições permanecem dentro do corredor do respetivo componente.';
}
function boundaryMapHTML(rec,comps){
  const assessment=boundaryAssessment(rec,comps);
  const values=[];
  COMPONENTS.forEach(c=>{
    const nominal=rec.comp[c.key].nominal;
    if(nominal!=null) values.push(nominal);
    comps[c.key].rows.forEach(r=>{ if(r.media!=null) values.push(r.media); });
    const limits=componentBoundaryLimits(c.key,rec);
    if(limits.lower!=null) values.push(limits.lower);
    if(limits.upper!=null) values.push(limits.upper);
  });
  if(!values.length){
    return `<div class="boundary-map-card"><h3>Mapa de limites</h3><div class="empty">Introduza os valores nominais e as medições para gerar o gráfico.</div></div>`;
  }
  const tickStep=0.05;
  let domainMin=Math.floor((Math.min(...values)-0.04)/tickStep)*tickStep;
  let domainMax=Math.ceil((Math.max(...values)+0.04)/tickStep)*tickStep;
  if(domainMax-domainMin<0.60){
    const mid=(domainMin+domainMax)/2;
    domainMin=Math.floor((mid-0.30)/tickStep)*tickStep;
    domainMax=Math.ceil((mid+0.30)/tickStep)*tickStep;
  }
  const width=1000,height=310,left=115,right=35,top=58,bottom=52;
  const plotW=width-left-right;
  const laneY={cm:100,boq:165,mf:230};
  const x=value=>left+((value-domainMin)/(domainMax-domainMin))*plotW;
  const ticks=[];
  for(let v=domainMin,i=0;v<=domainMax+0.0001 && i<40;v+=tickStep,i++){
    ticks.push(Number(v.toFixed(4)));
  }
  const tickSvg=ticks.map(v=>`<line x1="${x(v).toFixed(1)}" y1="${top}" x2="${x(v).toFixed(1)}" y2="${height-bottom}" stroke="#E2E8F0" stroke-width="1"/><text x="${x(v).toFixed(1)}" y="${height-24}" text-anchor="middle" font-size="11" fill="#64748B">${fmtNum(v)}</text>`).join('');
  const laneSvg=COMPONENTS.map(c=>{
    const limits=componentBoundaryLimits(c.key,rec);
    const y=laneY[c.key];
    const band=limits.lower!=null&&limits.upper!=null
      ? `<rect x="${x(limits.lower).toFixed(1)}" y="${y-18}" width="${Math.max(0,x(limits.upper)-x(limits.lower)).toFixed(1)}" height="36" rx="8" fill="${COMPONENT_COLORS[c.key]}" opacity="0.10"/>`
      : '';
    return `${band}<line x1="${left}" y1="${y}" x2="${width-right}" y2="${y}" stroke="#CBD5E1" stroke-width="1.5"/><text x="${left-12}" y="${y+4}" text-anchor="end" font-size="13" font-weight="700" fill="#334155">${escapeHtml(componentShortLabel(c.key))}</text>`;
  }).join('');
  const nominalSvg=COMPONENTS.map(c=>{
    const n=rec.comp[c.key].nominal;
    if(n==null) return '';
    const px=x(n);
    return `<line x1="${px.toFixed(1)}" y1="${top-10}" x2="${px.toFixed(1)}" y2="${height-bottom}" stroke="${COMPONENT_COLORS[c.key]}" stroke-width="3"/><text x="${px.toFixed(1)}" y="22" text-anchor="middle" font-size="12" font-weight="700" fill="${COMPONENT_COLORS[c.key]}">${escapeHtml(componentShortLabel(c.key))}</text><text x="${px.toFixed(1)}" y="39" text-anchor="middle" font-size="11" fill="#475569">${fmtNum(n)} mm</text>`;
  }).join('');
  const outerLines=[];
  const cmLimits=componentBoundaryLimits('cm',rec);
  const mfLimits=componentBoundaryLimits('mf',rec);
  if(cmLimits.lower!=null){
    outerLines.push(`<line x1="${x(cmLimits.lower).toFixed(1)}" y1="${top}" x2="${x(cmLimits.lower).toFixed(1)}" y2="${height-bottom}" stroke="#94A3B8" stroke-width="2" stroke-dasharray="5 4"/><text x="${x(cmLimits.lower).toFixed(1)}" y="${height-6}" text-anchor="middle" font-size="10" fill="#64748B">CM − 0,20</text>`);
  }
  if(mfLimits.upper!=null){
    outerLines.push(`<line x1="${x(mfLimits.upper).toFixed(1)}" y1="${top}" x2="${x(mfLimits.upper).toFixed(1)}" y2="${height-bottom}" stroke="#94A3B8" stroke-width="2" stroke-dasharray="5 4"/><text x="${x(mfLimits.upper).toFixed(1)}" y="${height-6}" text-anchor="middle" font-size="10" fill="#64748B">MF + 0,20</text>`);
  }
  const pointSvg=COMPONENTS.map(c=>{
    const cc=comps[c.key];
    return cc.rows.map((r,i)=>{
      if(r.media==null) return '';
      const row=rec.comp[c.key].rows[i];
      const state=measurementBoundaryState(c.key,r.media,rec);
      const cy=laneY[c.key]+((i%5)-2)*5;
      const fill=state.status==='critico'?'#B91C1C':COMPONENT_COLORS[c.key];
      const stroke=state.status==='warn'?'#B45309':'#FFFFFF';
      const sw=state.status==='warn'?3:1.5;
      return `<circle cx="${x(r.media).toFixed(1)}" cy="${cy}" r="6" fill="${fill}" stroke="${stroke}" stroke-width="${sw}"><title>${escapeHtml(componentShortLabel(c.key))} ${escapeHtml(String(row.nr||i+1))}: ${fmtNum(r.media)} mm · ${state.status==='critico'?'Ultrapassou '+state.boundary:state.status==='warn'?'No limite de '+state.boundary:'Dentro dos limites'}</title></circle>`;
    }).join('');
  }).join('');
  const avgSvg=COMPONENTS.map(c=>{
    const avg=comps[c.key].avgMedia;
    if(avg==null) return '';
    const px=x(avg),py=laneY[c.key];
    const color=COMPONENT_COLORS[c.key];
    const pts=`${px.toFixed(1)},${(py-11).toFixed(1)} ${(px+11).toFixed(1)},${py.toFixed(1)} ${px.toFixed(1)},${(py+11).toFixed(1)} ${(px-11).toFixed(1)},${py.toFixed(1)}`;
    return `<polygon points="${pts}" fill="#FFFFFF" stroke="${color}" stroke-width="3"><title>Média ${escapeHtml(componentShortLabel(c.key))}: ${fmtNum(avg)} mm</title></polygon><text x="${px.toFixed(1)}" y="${py-18}" text-anchor="middle" font-size="10" font-weight="700" fill="${color}">${fmtNum(avg)}</text>`;
  }).join('');
  const issueDetails=[
    ...assessment.critical.map(p=>`${componentShortLabel(p.key)} ${p.nr}: ${fmtNum(p.value)} mm ultrapassou ${p.state.boundary} em ${fmtNum(p.state.amount)} mm`),
    ...assessment.atBoundary.map(p=>`${componentShortLabel(p.key)} ${p.nr}: ${fmtNum(p.value)} mm está no limite ${p.state.boundary}`)
  ];
  const spacingText=assessment.spacingWarnings.length
    ? `<div class="boundary-spacing-warning">Verificar nominais: ${assessment.spacingWarnings.map(s=>`${escapeHtml(s.label)} = ${fmtNum(s.value)} mm, esperado 0,20 mm`).join(' · ')}</div>`
    : '';
  return `<div class="boundary-map-card">
    <h3>Mapa de limites — lógica do ficheiro original</h3>
    <div class="boundary-help">Cada linha vertical é um nominal e funciona como fronteira para o componente vizinho. Um ponto só entra em alerta quando atinge ou ultrapassa a linha limite correspondente.</div>
    <div class="boundary-map-wrap">
      <svg viewBox="0 0 ${width} ${height}" role="img" aria-label="Mapa dimensional de CM, Boquilha e Molde final">
        ${tickSvg}${laneSvg}${outerLines.join('')}${nominalSvg}${pointSvg}${avgSvg}
      </svg>
    </div>
    <div class="boundary-map-legend">
      <span><i class="boundary-line" style="background:#16A34A"></i> Linhas verticais = nominais/fronteiras</span>
      <span><i class="boundary-dot" style="background:#0183D7"></i> Círculos = médias de cada medição</span>
      <span><i class="boundary-diamond"></i> Losango = média global</span>
      <span><i class="boundary-dot" style="background:#B91C1C"></i> Vermelho = fronteira ultrapassada</span>
    </div>
    <div class="boundary-map-status ${assessment.status}"><strong>${escapeHtml(boundaryAssessmentText(assessment))}</strong>${issueDetails.length?`<div style="margin-top:5px">${issueDetails.map(escapeHtml).join('<br>')}</div>`:''}${spacingText}</div>
  </div>`;
}

function componentShortLabel(key){
  if(key==='cm') return 'CM';
  if(key==='boq') return 'Boquilha';
  if(key==='mf') return 'Molde final';
  return key.toUpperCase();
}
function pairBoundaryInfo(g,rec,comps){
  const fromNom=rec.comp[g.from].nominal;
  const toNom=rec.comp[g.to].nominal;
  const fromMeasured=comps[g.from].avgMedia;
  const toMeasured=comps[g.to].avgMedia;
  const nominalSpace=(fromNom!=null && toNom!=null) ? toNom-fromNom : null;
  const currentSpace=(fromMeasured!=null && toMeasured!=null) ? toMeasured-fromMeasured : null;
  const fromDeviation=(fromNom!=null && fromMeasured!=null) ? fromMeasured-fromNom : null;
  const toDeviation=(toNom!=null && toMeasured!=null) ? toMeasured-toNom : null;
  const forwardMargin=(toNom!=null && fromMeasured!=null) ? toNom-fromMeasured : null;
  const reverseMargin=(fromNom!=null && toMeasured!=null) ? toMeasured-fromNom : null;
  const eps=0.005;
  const forwardCrossed=forwardMargin!=null && forwardMargin < -eps;
  const reverseCrossed=reverseMargin!=null && reverseMargin < -eps;
  const forwardAtBoundary=forwardMargin!=null && Math.abs(forwardMargin)<=eps;
  const reverseAtBoundary=reverseMargin!=null && Math.abs(reverseMargin)<=eps;
  let status='pending';
  let result='Dados insuficientes para calcular os limites entre os componentes.';
  if(forwardMargin!=null && reverseMargin!=null){
    if(forwardCrossed || reverseCrossed){
      status='critico';
      const parts=[];
      if(forwardCrossed) parts.push(`${componentShortLabel(g.from)} ultrapassou o limite de ${componentShortLabel(g.to)} em ${fmtNum(Math.abs(forwardMargin))} mm`);
      if(reverseCrossed) parts.push(`${componentShortLabel(g.to)} ultrapassou o limite de ${componentShortLabel(g.from)} em ${fmtNum(Math.abs(reverseMargin))} mm`);
      result=parts.join(' · ')+'.';
    }else if(forwardAtBoundary || reverseAtBoundary){
      status='warn';
      const parts=[];
      if(forwardAtBoundary) parts.push(`${componentShortLabel(g.from)} está no limite nominal de ${componentShortLabel(g.to)}`);
      if(reverseAtBoundary) parts.push(`${componentShortLabel(g.to)} está no limite nominal de ${componentShortLabel(g.from)}`);
      result=parts.join(' · ')+'.';
    }else{
      status='ok';
      if(forwardMargin<=reverseMargin){
        result=`${componentShortLabel(g.from)} ainda tem ${fmtNum(forwardMargin)} mm antes de atingir o limite de ${componentShortLabel(g.to)}.`;
      }else{
        result=`${componentShortLabel(g.to)} ainda tem ${fmtNum(reverseMargin)} mm antes de atingir o limite de ${componentShortLabel(g.from)}.`;
      }
    }
  }
  return {fromNom,toNom,fromMeasured,toMeasured,nominalSpace,currentSpace,fromDeviation,toDeviation,forwardMargin,reverseMargin,status,result};
}
function movementTowardText(key,deviation,otherKey,isFromSide){
  if(deviation==null) return 'Sem média disponível.';
  const own=componentShortLabel(key);
  const other=componentShortLabel(otherKey);
  if(Math.abs(deviation)<0.005) return `${own} está no nominal.`;
  const toward = isFromSide ? deviation>0 : deviation<0;
  const direction = toward ? `em direção a ${other}` : `afastado de ${other}`;
  const side = deviation>0 ? 'acima' : 'abaixo';
  return `${own}: ${fmtNum(Math.abs(deviation))} mm ${side} do nominal, ${direction}.`;
}
function boundaryMarginText(margin){
  if(margin==null) return '—';
  if(margin < -0.005) return `Ultrapassou ${fmtNum(Math.abs(margin))} mm`;
  if(Math.abs(margin)<=0.005) return 'No limite';
  return `${fmtNum(margin)} mm restantes`;
}

/* ---------------- verification panel ---------------- */
function pillHTML(status){
  const map={ok:['ok','Dentro da tolerância'],warn:['warn','Fora de tolerância'],critico:['critico','Fora de tolerância'],info:['pending','Informativo'],pending:['pending','Dados insuficientes']};
  const [cls,txt]=map[status]||map.pending;
  return `<span class="pill ${cls}">${txt}</span>`;
}
function renderVerification(rec){
  const host=document.getElementById('verificationHost');
  const {comps} = calcRecord(rec);
  const summary = COMPONENTS.map(c=>{
    const cc=comps[c.key];
    const limits=componentBoundaryLimits(c.key,rec);
    const avgState=measurementBoundaryState(c.key,cc.avgMedia,rec);
    const avgClass=avgState.status==='critico'?'bad':'';
    return `<div class="summary-item">
      <span>${escapeHtml(c.label)} — Média medida</span>
      <strong class="${avgClass}">${fmtNum(cc.avgMedia)} mm</strong>
      <span class="muted" style="margin-top:4px">Nominal: ${rec.comp[c.key].nominal==null?'—':fmtNum(rec.comp[c.key].nominal)} mm · Corredor: ${limits.lower==null?'—':fmtNum(limits.lower)}–${limits.upper==null?'—':fmtNum(limits.upper)} mm · ${cc.n} amostra(s)</span>
    </div>`;
  }).join('');
  host.innerHTML = `
    <div class="summary-grid">${summary}</div>
    ${boundaryMapHTML(rec,comps)}
  `;
}

/* ---------------- reference list ---------------- */
function renderRecordList(){
  const q = (document.getElementById('recSearch').value||'').toLowerCase().trim();
  const from=document.getElementById('recDateFrom')?.value||'';
  const to=document.getElementById('recDateTo')?.value||'';
  const host=document.getElementById('recordList');
  const list = db.records.filter(r=>(!q || r.jobOnId.toLowerCase().includes(q) || r.reference.toLowerCase().includes(q) || r.production.toLowerCase().includes(q) || r.filenameBase.toLowerCase().includes(q)) && (!from||r.date>=from) && (!to||r.date<=to));
  if(!list.length){ host.innerHTML='<div class="empty">Sem registos de Pegamentos para os filtros aplicados.</div>'; return; }
  host.innerHTML = `<div class="search-results" data-dmo-list>` + list.map(r=>{
    const {comps} = calcRecord(r);
    const hasData = COMPONENTS.some(c=>comps[c.key].n>0);
    const hasRangeViolation = COMPONENTS.some(c=>comps[c.key].rangeViolations.length>0);
    const worst = hasRangeViolation ? 'warn' : hasData ? 'ok' : 'pending';
    return `<div class="rec-item ${selectedRecordId===r.id?'selected':''}" data-dmo-row data-id="${r.id}" role="option" aria-selected="${selectedRecordId===r.id?'true':'false'}" tabindex="0" onclick="selectRecord('${r.id}')" ondblclick="openRecord('${r.id}')">
      <div><strong>${escapeHtml(r.reference||'(sem referência)')}</strong><div class="meta">${escapeHtml(r.jobOnId||'Sem Job On')} · ${escapeHtml(r.production)} · ${escapeHtml(r.machine||'—')} · ${escapeHtml(r.date)}</div></div>
      ${pillHTML(worst)}
    </div>`;
  }).join('') + `</div>`;
}
function selectRecord(id){
  selectedRecordId=id;
  renderRecordList();
}
function clearRecordFilters(){
  document.getElementById('recSearch').value='';
  document.getElementById('recDateFrom').value='';
  document.getElementById('recDateTo').value='';
  renderRecordList();
}

/* ---------------- folder handling (File System Access API) ---------------- */
function idbOpen(){ return new Promise((resolve,reject)=>{ const r=indexedDB.open('pegamentosFS',1); r.onupgradeneeded=()=>{ if(!r.result.objectStoreNames.contains('handles')) r.result.createObjectStore('handles'); }; r.onsuccess=()=>resolve(r.result); r.onerror=()=>reject(r.error); }); }
async function storeHandle(key,handle){ const idb=await idbOpen(); return new Promise((resolve,reject)=>{ const tx=idb.transaction('handles','readwrite'); tx.objectStore('handles').put(handle,key); tx.oncomplete=resolve; tx.onerror=()=>reject(tx.error); }); }
async function getHandle(key){ try{ const idb=await idbOpen(); return await new Promise((resolve,reject)=>{ const tx=idb.transaction('handles','readonly'); const r=tx.objectStore('handles').get(key); r.onsuccess=()=>resolve(r.result||null); r.onerror=()=>reject(r.error); }); }catch(e){ return null; } }
async function restoreMainFolderHandle(){
  mainDirHandle = await getHandle('mainDir');
  renderConfig();
  renderReferenceFolderStatus();
}
async function verifyPermission(handle,write=true,request=false){
  if(!handle) return false;
  const opts={mode:write?'readwrite':'read'};
  if((await handle.queryPermission(opts))==='granted') return true;
  if(!request) return false;
  return (await handle.requestPermission(opts))==='granted';
}
async function chooseMainFolder(){
  if(!window.showDirectoryPicker){ toast('Use o Chrome ou o Edge para escolher uma pasta.','error'); return; }
  try{
    const h = await window.showDirectoryPicker({mode:'readwrite'});
    if(!await verifyPermission(h,true,true)) throw new Error('Acesso à pasta não autorizado.');
    mainDirHandle = h;
    await storeHandle('mainDir', h);
    db.meta.mainFolderName = h.name;
    saveDB();
    renderConfig();
    renderReferenceFolderStatus();
    toast('Pasta principal configurada.','ok');
  }catch(e){ if(e.name!=='AbortError') fail(e); }
}
async function chooseReferenceFolder(){
  const rec=getActive(); if(!rec) return;
  if(!window.showDirectoryPicker){ toast('Use o Chrome ou o Edge para escolher uma pasta.','error'); return; }
  try{
    const h=await window.showDirectoryPicker({mode:'readwrite'});
    if(!await verifyPermission(h,true,true)) throw new Error('Acesso à pasta não autorizado.');
    await storeHandle(`referenceDir:${rec.id}`,h);
    rec.referenceFolderName=h.name;
    rec.updatedAt=nowISO();
    saveDB();
    await renderReferenceFolderStatus(rec);
    toast('Pasta desta referência configurada.','ok');
  }catch(e){ if(e.name!=='AbortError') fail(e); }
}
async function clearReferenceFolder(){
  const rec=getActive(); if(!rec) return;
  try{
    const idb=await idbOpen();
    await new Promise((resolve,reject)=>{
      const tx=idb.transaction('handles','readwrite');
      tx.objectStore('handles').delete(`referenceDir:${rec.id}`);
      tx.oncomplete=resolve;
      tx.onerror=()=>reject(tx.error);
    });
    rec.referenceFolderName='';
    saveDB();
    await renderReferenceFolderStatus(rec);
    toast('Será usada a pasta automática da referência.','ok');
  }catch(e){ fail(e); }
}
async function renderReferenceFolderStatus(rec=getActive()){
  const el=document.getElementById('referenceFolderStatus');
  if(!el || !rec) return;
  const root=mainDirHandle ? mainDirHandle.name : (db.meta.mainFolderName||'Diretório principal não autorizado');
  const subfolder=rec.referenceFolderName||safeName(rec.reference||'sem_referencia');
  el.innerHTML=`Pasta resolvida: <strong>${escapeHtml(root)} / ${escapeHtml(subfolder)}</strong>`;
}
async function ensureReferenceFolder(rec, request=false){
  if(!mainDirHandle) return null;
  if(!await verifyPermission(mainDirHandle,true,request)) return null;
  return mainDirHandle.getDirectoryHandle(safeName(rec.referenceFolderName||rec.reference||'sem_referencia'), {create:true});
}
async function writeFile(dir,name,blob){
  const fh = await dir.getFileHandle(name,{create:true});
  const w = await fh.createWritable();
  await w.write(blob);
  await w.close();
}
function renderConfig(){
  const el=document.getElementById('folderStatus');
  el.innerHTML = mainDirHandle
    ? `Pasta principal: <strong>${escapeHtml(mainDirHandle.name)}</strong>`
    : (db.meta.mainFolderName ? `Pasta configurada anteriormente (<strong>${escapeHtml(db.meta.mainFolderName)}</strong>) — clique em "Selecionar pasta principal" para reautorizar.` : 'Nenhuma pasta principal selecionada.');
  document.getElementById('cfg-ovalmax').value = db.defaults.ovalMax;
  document.getElementById('cfg-gaptol').value = db.defaults.gapTol;
}
function saveDefaults(){
  db.defaults.ovalMax = validNum(document.getElementById('cfg-ovalmax').value, 0.20);
  db.defaults.gapTol = validNum(document.getElementById('cfg-gaptol').value, 0.05);
  saveDB();
  toast('Valores por omissão guardados.','ok');
}

/* ---------------- downloads (fallback when no folder chosen) ---------------- */
function downloadBlob(name, blob){
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=name;
  a.click();
  setTimeout(()=>URL.revokeObjectURL(a.href),500);
}


function validateRecordIdentity(rec){
  if(!rec.jobOnId){
    toast('O registo tem de estar associado a um Job On.','error');
    return false;
  }
  if(!rec.reference){
    toast('Introduza a referência antes de criar o relatório.','error');
    document.getElementById('f-reference')?.focus();
    return false;
  }
  if(!/^\d{6}$/.test(rec.production||'')){
    toast('A produção deve ter 6 algarismos, por exemplo 202602.','error');
    document.getElementById('f-production')?.focus();
    return false;
  }
  if(!rec.comp.cm.ref || !rec.comp.cm.lote || !rec.comp.boq.ref || !rec.comp.boq.lote || !rec.comp.mf.ref || !rec.comp.mf.lote){
    toast('O Job On não contém todos os CM, BQ e MF necessários. Corrija o Job On.','error');
    return false;
  }
  updateAutomaticFilename(rec);
  return true;
}

/* ---------------- print report ---------------- */
function buildReportHTML(rec){
  const {comps} = calcRecord(rec);
  const assessment=boundaryAssessment(rec,comps);
  const compRows = COMPONENTS.map(c=>{
    const cc=comps[c.key], meta=rec.comp[c.key];
    const limits=componentBoundaryLimits(c.key,rec);
    const dataRows = cc.rows.map((rc,i)=>{
      const row=meta.rows[i];
      if(row.nr==='' && row.costura==='' && row.noventa==='') return '';
      const state=measurementBoundaryState(c.key,rc.media,rec);
      const mediaClass=state.status==='critico'?'print-bad':state.status==='warn'?'print-warn':'';
      const marker=state.status==='critico'?' ⚠':state.status==='warn'?' ◇':'';
      return `<tr${state.status==='critico'?' class="print-bad-row"':''}><td>${escapeHtml(String(row.nr))}</td><td>${row.costura===''?'—':fmtNum(row.costura)}</td><td>${row.noventa===''?'—':fmtNum(row.noventa)}</td><td>${rc.oval==null?'—':fmtSigned(rc.oval)}</td><td class="${mediaClass}">${rc.media==null?'—':fmtNum(rc.media)}${marker}</td></tr>`;
    }).join('');
    const componentIssues=assessment.points.filter(p=>p.key===c.key && (p.state.status==='critico'||p.state.status==='warn'));
    const issueNote=componentIssues.length
      ? `<div class="print-alert">${componentIssues.map(p=>`${p.state.status==='critico'?'⚠':'◇'} ${escapeHtml(componentShortLabel(p.key))} ${escapeHtml(p.nr)}: ${fmtNum(p.value)} mm — ${p.state.status==='critico'?`ultrapassou ${escapeHtml(p.state.boundary)} em ${fmtNum(p.state.amount)} mm`:`está no limite ${escapeHtml(p.state.boundary)}`}`).join('<br>')}</div>`
      : '';
    return `
    <div class="print-section">${escapeHtml(c.label)} — ${c.key==='cm'?'CM':c.key==='boq'?'BQ':'MF'} ${escapeHtml(meta.ref||'—')} · Lote ${escapeHtml(meta.lote||'—')} · Nominal ${meta.nominal==null?'—':fmtNum(meta.nominal)} mm · Corredor ${limits.lower==null?'—':fmtNum(limits.lower)}–${limits.upper==null?'—':fmtNum(limits.upper)} mm</div>
    <table class="print-table">
      <thead><tr><th>N.º</th><th>Costura</th><th>Contra costura</th><th>Ovalização</th><th>Média</th></tr></thead>
      <tbody>${dataRows || '<tr><td colspan="5">Sem medições registadas.</td></tr>'}</tbody>
      <tfoot><tr><td>AVG</td><td>${fmtNum(cc.avgCostura)}</td><td>${fmtNum(cc.avgNoventa)}</td><td>${fmtSigned(cc.avgOval)}</td><td>${fmtNum(cc.avgMedia)}</td></tr></tfoot>
    </table>
    ${issueNote}`;
  }).join('');
  let topAlert='';
  if(!assessment.points.length){
    topAlert=`<div class="print-top-ok">Ainda não existem medições para verificar.</div>`;
  }else if(assessment.critical.length){
    topAlert=`<div class="print-top-alert">⚠ ATENÇÃO — ${assessment.critical.length} medição(ões) ultrapassaram uma fronteira entre componentes.</div>`;
  }else if(assessment.atBoundary.length){
    topAlert=`<div class="print-top-warn">◇ ${assessment.atBoundary.length} medição(ões) estão exatamente numa fronteira.</div>`;
  }else{
    topAlert=`<div class="print-top-ok">✓ Todas as medições permanecem dentro do corredor do respetivo componente.</div>`;
  }
  return `<div class="print-page">
    <div class="print-header" style="display:flex;align-items:center;gap:14px">
      ${LOGO_DATA_URL ? `<img class="print-logo" src="${LOGO_DATA_URL}" alt="BA Glass">` : ''}
      <div>
        <h1>Pegamentos — ${escapeHtml(rec.reference||'(sem referência)')}</h1>
        <div class="sub">Job On ${escapeHtml(rec.jobOnId)} · Produção ${escapeHtml(rec.production)} · Máquina ${escapeHtml(rec.machine||'—')} · Ficheiro ${escapeHtml(rec.filenameBase)}_relatorio.pdf · Data ${escapeHtml(rec.date)}</div>
      </div>
    </div>
    <div class="print-strip">
      ${COMPONENTS.map(c=>{ const limits=componentBoundaryLimits(c.key,rec); return `<div class="box"><span>${escapeHtml(c.label)} — média medida</span><strong>${fmtNum(comps[c.key].avgMedia)} mm</strong><span>Nominal ${rec.comp[c.key].nominal==null?'—':fmtNum(rec.comp[c.key].nominal)} mm · Corredor ${limits.lower==null?'—':fmtNum(limits.lower)}–${limits.upper==null?'—':fmtNum(limits.upper)} mm</span></div>`; }).join('')}
    </div>
    ${topAlert}
    <div class="print-section">Mapa de limites</div>
    ${boundaryMapHTML(rec,comps)}
    ${compRows}
    ${rec.notes ? `<div class="print-section">Observações</div><div>${escapeHtml(rec.notes)}</div>` : ''}
    <div class="print-foot">Gerado em ${new Date().toLocaleString('pt-PT')}</div>
  </div>`;
}
function printRecord(){
  const rec=getActive(); if(!rec || !validateRecordIdentity(rec)) return;
  const area=document.getElementById('printArea');
  area.innerHTML = buildReportHTML(rec);
  requestAnimationFrame(()=>requestAnimationFrame(()=>window.print()));
}

/* ---------------- legacy prototype: printable HTML only ----------------
   Production implementation must generate the approved PDF described in the handoff. */
async function saveRecordToFolder(){
  const rec=getActive(); if(!rec || !validateRecordIdentity(rec)) return;
  const status=document.getElementById('saveStatus');
  const reportHtml = buildStandaloneReport(rec);
  updateAutomaticFilename(rec);
  const reportName = `${safeName(rec.filenameBase)}_relatorio.html`;

  if(!mainDirHandle){
    downloadBlob(reportName, new Blob([reportHtml],{type:'text/html;charset=utf-8'}));
    status.textContent = 'Relatório para impressão descarregado. Configure uma pasta em "Configurações" para o guardar diretamente.';
    toast('Ficheiro para impressão descarregado.','ok');
    return;
  }
  try{
    const dir = await ensureReferenceFolder(rec, true);
    if(!dir) throw new Error('A pasta precisa de autorização.');
    await writeFile(dir, reportName, new Blob([reportHtml],{type:'text/html;charset=utf-8'}));
    status.textContent = `Guardado em "${dir.name}" — ${reportName}.`;
    toast('Relatório para impressão guardado.','ok');
  }catch(e){
    status.textContent = 'Falha ao gravar na pasta. Volte a autorizar em Configurações.';
    fail(e);
  }
}

function buildStandaloneReport(rec){
  return `<!DOCTYPE html>
<html lang="pt-PT">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>${escapeHtml(rec.filenameBase||'Relatório de Pegamentos')}</title>
<style>
  :root{--primary:#0183D7;--primary-dark:#065C99;--text:#1F2937;--muted:#6B7280}
  *{box-sizing:border-box;-webkit-print-color-adjust:exact!important;print-color-adjust:exact!important}
  body{font-family:"Segoe UI",Arial,sans-serif;margin:0;background:#EAF2F8;color:var(--text)}
  .toolbar{position:sticky;top:0;display:flex;justify-content:center;gap:10px;padding:12px;background:#0F3554;z-index:5}
  .toolbar button{border:1px solid #fff;border-radius:6px;background:#fff;color:#065C99;padding:9px 16px;font-weight:700;cursor:pointer}
  .page{max-width:1050px;margin:24px auto;padding:28px;background:#fff;box-shadow:0 12px 35px rgba(15,23,42,.18)}
  .print-header{border-bottom:4px solid #0183D7;padding-bottom:10px;margin-bottom:14px}
  .print-header h1{margin:0;color:#065C99;font-size:22px}
  .print-header .sub{color:#6B7280;font-size:13px;margin-top:4px}
  .print-logo{width:52px;height:52px;object-fit:contain}
  .print-strip{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:14px 0}
  .print-strip .box{border-left:4px solid #0183D7;background:#F7FAFC;padding:10px 12px}
  .print-strip .box span{display:block;font-size:11px;color:#6B7280}
  .print-strip .box strong{font-size:16px}
  .print-section{font-size:14px;font-weight:700;color:#065C99;background:#DCEEFB;padding:7px 10px;margin:16px 0 6px}
  .print-table{width:100%;border-collapse:collapse;font-size:12px;margin-bottom:6px}
  .print-table th,.print-table td{border:1px solid #CBD5E1;padding:5px 7px;text-align:left}
  .print-table th{background:#0183D7;color:#fff}
  .print-gap{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;margin:8px 0 14px}
  .print-gap .box{border:1px solid #CBD5E1;padding:10px 12px;border-radius:6px;background:#F8FBFE}
  .print-pill{display:inline-block;border-radius:999px;padding:2px 9px;font-size:11px;font-weight:700}
  .print-pill.ok{background:#DCFCE7;color:#166534}
  .print-pill.warn{background:#FFF7ED;color:#9A3412}
  .print-pill.critico{background:#FEE2E2;color:#991B1B}
  .print-foot{margin-top:18px;font-size:11px;color:#6B7280}
  .print-top-alert{border:2px solid #B91C1C;background:#FEE2E2;color:#7F1D1D;font-weight:700;padding:10px 14px;border-radius:6px;margin:10px 0 16px}
  .print-top-ok{border:2px solid #15803D;background:#DCFCE7;color:#14532D;font-weight:700;padding:10px 14px;border-radius:6px;margin:10px 0 16px}
  .print-bad{color:#B91C1C;font-weight:800}
  tr.print-bad-row{background:#FEF2F2}
  .print-alert{border:1px solid #FCA5A5;background:#FEF2F2;color:#7F1D1D;font-size:12px;padding:8px 10px;border-radius:5px;margin:6px 0 14px}
  .print-top-warn{border:2px solid #B45309;background:#FFF9E8;color:#92400E;font-weight:700;padding:10px 14px;border-radius:6px;margin:10px 0 16px}
  .print-warn{color:#92400E;font-weight:800}
  .boundary-map-card{border:1px solid #D8E6F2;border-radius:9px;background:#fff;padding:14px;margin:10px 0 14px}
  .boundary-map-card h3{margin:0 0 4px;color:#065C99;font-size:14px}
  .boundary-map-card .boundary-help{font-size:12px;color:#6B7280;margin-bottom:10px}
  .boundary-map-wrap{width:100%;overflow-x:auto;border:1px solid #D8E6F2;border-radius:7px;background:#FBFDFF}
  .boundary-map-wrap svg{display:block;width:100%;min-width:760px;height:auto}
  .boundary-map-status{margin-top:10px;padding:10px 12px;border-radius:7px;font-size:12px;border:1px solid #CBD5E1;background:#F8FAFC}
  .boundary-map-status.ok{border-color:#86C89B;background:#ECFDF3;color:#166534}
  .boundary-map-status.warn{border-color:#E8C978;background:#FFF9E8;color:#92400E}
  .boundary-map-status.critico{border-color:#E8A3A3;background:#FFF1F1;color:#991B1B}
  .boundary-map-legend{display:flex;gap:14px;flex-wrap:wrap;margin-top:9px;font-size:11px;color:#6B7280}
  .boundary-map-legend span{display:inline-flex;align-items:center;gap:5px}
  .boundary-dot{width:10px;height:10px;border-radius:50%;display:inline-block}
  .boundary-line{width:2px;height:14px;display:inline-block}
  .boundary-diamond{width:9px;height:9px;display:inline-block;transform:rotate(45deg);border:2px solid #065C99;background:#fff}
  .boundary-spacing-warning{margin-top:8px;color:#92400E;font-weight:600}
  .viz-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin:8px 0 14px}
  .tolerance-viz{border:1px solid #CBD5E1;border-radius:8px;background:#F8FBFE;padding:10px 12px}
  .tolerance-viz .viz-title{font-size:13px;font-weight:700;color:#065C99;margin-bottom:8px}
  .tolerance-viz .viz-meta{display:flex;flex-wrap:wrap;gap:10px;font-size:12px;color:#6B7280;margin-bottom:8px}
  .tolerance-viz .viz-meta strong{color:#1F2937}
  .tolerance-viz .viz-scale{position:relative;height:42px}
  .tolerance-viz .viz-track{position:absolute;left:0;right:0;top:18px;height:6px;border-radius:999px;background:#D8E6F2}
  .tolerance-viz .viz-band{position:absolute;top:15px;height:12px;border-radius:999px;background:#DCFCE7;border:1px solid #86EFAC}
  .tolerance-viz .viz-nominal{position:absolute;top:10px;width:2px;height:22px;background:#065C99}
  .tolerance-viz .viz-marker{position:absolute;top:4px;width:0;height:30px}
  .tolerance-viz .viz-marker::before{content:"";position:absolute;left:-1px;top:9px;width:2px;height:21px;background:#0F172A}
  .tolerance-viz .viz-marker::after{content:"";position:absolute;left:-6px;top:0;width:12px;height:12px;border-radius:50%;background:#0183D7;border:2px solid #fff;box-shadow:0 0 0 1px #94A3B8}
  .tolerance-viz .viz-marker.bad::after{background:#B91C1C}
  .tolerance-viz .viz-labels{display:flex;justify-content:space-between;gap:10px;font-size:11px;color:#6B7280;margin-top:2px}
  .tolerance-viz .viz-legend{display:flex;justify-content:space-between;gap:10px;flex-wrap:wrap;font-size:12px;margin-top:8px}
  .tolerance-viz .viz-legend span{color:#6B7280}
  .tolerance-viz .viz-legend strong{color:#1F2937}
  .tolerance-viz .viz-legend .ok{color:#166534}
  .tolerance-viz .viz-legend .bad{color:#991B1B}
  @media print{
    body{background:#fff}
    .toolbar{display:none!important}
    .page{display:block!important;max-width:none!important;width:100%!important;margin:0!important;padding:0!important;box-shadow:none!important}
    .boundary-map-card{break-inside:avoid!important;page-break-inside:avoid!important;overflow:hidden!important;padding:6px!important;margin:4px 0 7px!important}
    .boundary-map-card h3{font-size:10px!important;margin:0 0 2px!important}
    .boundary-map-card .boundary-help{font-size:8px!important;line-height:1.15!important;margin-bottom:4px!important}
    .boundary-map-wrap{width:100%!important;max-width:100%!important;overflow:hidden!important}
    .boundary-map-wrap svg{width:100%!important;max-width:100%!important;min-width:0!important;height:auto!important;max-height:112mm!important}
    .boundary-map-legend{font-size:7.5px!important;gap:7px!important;margin-top:3px!important}
    .boundary-map-status{font-size:8px!important;line-height:1.15!important;padding:4px 6px!important;margin-top:4px!important}
  }
  @media(max-width:700px){
    .page{margin:0;padding:16px}
    .print-strip,.print-gap,.viz-grid{grid-template-columns:1fr}
  }



/* =========================================================
   BA PRINT BLUEPRINT v2 - VERIFIED A4 LANDSCAPE
   Colour is carried by text and borders, not only backgrounds.
   ========================================================= */
@page { size: A4 landscape; margin: 7mm; }

/* Printable report appearance on screen and in saved HTML files */
#printArea .print-section,
.print-page .print-section,
.page .print-section {
  background: #FFFFFF !important;
  color: #065C99 !important;
  border: 1px solid #8CBFDF !important;
  border-left: 5px solid #0183D7 !important;
  border-radius: 3px !important;
}
#printArea .print-table th,
.print-page .print-table th,
.page .print-table th {
  background: #FFFFFF !important;
  color: #065C99 !important;
  border-top: 2px solid #0183D7 !important;
  border-bottom: 2px solid #0183D7 !important;
}

@media print {
  html, body {
    width: auto !important;
    min-width: 0 !important;
    height: auto !important;
    margin: 0 !important;
    padding: 0 !important;
    background: #FFFFFF !important;
  }

  body.app-shell > *:not(#printArea) { display: none !important; }
  #printArea, #printArea * {
    visibility: visible !important;
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
    color-adjust: exact !important;
    box-sizing: border-box !important;
  }
  #printArea {
    display: block !important;
    position: static !important;
    inset: auto !important;
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
    overflow: visible !important;
  }

  /* Standalone saved report files do not always contain #printArea. */
  body > .page,
  body > .print-page,
  #printArea > .page,
  #printArea > .print-page,
  .page,
  .print-page {
    display: block !important;
    position: static !important;
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    margin: 0 !important;
    padding: 0 !important;
    border: 0 !important;
    box-shadow: none !important;
    overflow: visible !important;
  }

  .print-header,
  .head {
    break-inside: avoid !important;
    page-break-inside: avoid !important;
    border-bottom: 2px solid #0183D7 !important;
    padding: 0 0 6px !important;
    margin: 0 0 7px !important;
  }
  .print-title,
  .print-header h1,
  .head h1,
  h1 {
    color: #065C99 !important;
  }

  .print-section,
  .page h2,
  .print-page h2 {
    break-after: avoid !important;
    page-break-after: avoid !important;
    background: #FFFFFF !important;
    color: #065C99 !important;
    border: 1px solid #8CBFDF !important;
    border-left: 5px solid #0183D7 !important;
    padding: 4px 7px !important;
    margin: 8px 0 4px !important;
    font-size: 10.5px !important;
    line-height: 1.2 !important;
  }

  .print-mold-strip,
  .mold-strip,
  .print-strip,
  .summary-grid,
  .print-gap {
    gap: 5px !important;
    margin: 5px 0 7px !important;
    break-inside: avoid !important;
    page-break-inside: avoid !important;
  }
  .print-mold-item,
  .mold-item,
  .print-strip .box,
  .summary-card,
  .summary-item,
  .print-gap .box {
    min-width: 0 !important;
    padding: 5px 6px !important;
    background: #FFFFFF !important;
    border: 1px solid #B9D7EB !important;
    border-left: 3px solid #0183D7 !important;
  }

  .table-scroll {
    overflow: visible !important;
    width: 100% !important;
  }
  table,
  .print-table,
  .comparison-table {
    width: 100% !important;
    max-width: 100% !important;
    min-width: 0 !important;
    table-layout: fixed !important;
    border-collapse: collapse !important;
    font-size: 8.5px !important;
    line-height: 1.18 !important;
    margin: 0 0 4px !important;
  }
  th, td,
  .print-table th, .print-table td,
  .comparison-table th, .comparison-table td {
    padding: 2.5px 3px !important;
    max-width: 0 !important;
    overflow-wrap: anywhere !important;
    word-break: normal !important;
    white-space: normal !important;
    vertical-align: top !important;
    border: 1px solid #B8C8D4 !important;
  }
  th,
  .print-table th,
  .comparison-table th {
    background: #FFFFFF !important;
    color: #065C99 !important;
    border-top: 2px solid #0183D7 !important;
    border-bottom: 2px solid #0183D7 !important;
    font-weight: 800 !important;
  }
  tr { break-inside: avoid !important; page-break-inside: avoid !important; }

  /* Statuses remain visibly coloured without requiring background printing. */
  .print-status-approved,
  .print-pill.ok,
  .pill.ok,
  .print-top-ok {
    background: #FFFFFF !important;
    color: #15803D !important;
    border: 1.5px solid #15803D !important;
  }
  .print-status-pending,
  .print-pill.warn,
  .pill.warn {
    background: #FFFFFF !important;
    color: #B45309 !important;
    border: 1.5px solid #B45309 !important;
  }
  .print-pill.critico,
  .pill.critico,
  .print-top-alert,
  .print-alert,
  .print-bad,
  .readonly-cell.bad,
  .delta-up {
    background: #FFFFFF !important;
    color: #B91C1C !important;
    border-color: #B91C1C !important;
  }
  .delta-down { color: #15803D !important; }
  tr.print-bad-row { background: #FFFFFF !important; border-left: 3px solid #B91C1C !important; }

  .print-foot, footer {
    margin-top: 6px !important;
    padding: 0 !important;
    font-size: 8.5px !important;
  }
  img { max-width: 100% !important; height: auto !important; }
  button, .decision, .no-print { display: none !important; }
}

</style>
</head>
<body>
<div class="toolbar"><button onclick="window.print()">Imprimir / Guardar PDF</button></div>
<div class="page">${buildReportHTML(rec)}<div class="print-foot"><strong>Desenvolvido por Diogo Oliveira · 2026</strong></div></div>
</body>
</html>`;
}

/* ---------------- send ---------------- */
async function sendRecord(){
  const rec=getActive(); if(!rec) return;
  const {comps} = calcRecord(rec);
  const assessment=boundaryAssessment(rec,comps);
  const summaryLines = [
    `Pegamentos — ${rec.reference||'(sem referência)'}`,
    ...COMPONENTS.map(c=>{
      const meta=rec.comp[c.key];
      const limits=componentBoundaryLimits(c.key,rec);
      return `${c.label}: ${c.key==='cm'?'CM':c.key==='boq'?'BQ':'MF'} ${meta.ref||'—'} · Lote ${meta.lote||'—'} · média ${fmtNum(comps[c.key].avgMedia)} mm · nominal ${meta.nominal==null?'—':fmtNum(meta.nominal)} mm · corredor ${limits.lower==null?'—':fmtNum(limits.lower)}–${limits.upper==null?'—':fmtNum(limits.upper)} mm`;
    }),
    `Mapa de limites: ${boundaryAssessmentText(assessment)}`,
    ...assessment.critical.map(p=>`${componentShortLabel(p.key)} ${p.nr}: ${fmtNum(p.value)} mm ultrapassou ${p.state.boundary} em ${fmtNum(p.state.amount)} mm`),
    ...assessment.atBoundary.map(p=>`${componentShortLabel(p.key)} ${p.nr}: ${fmtNum(p.value)} mm está no limite ${p.state.boundary}`)
  ];
  const body = summaryLines.join('\n');
  if(navigator.share){
    try{
      await navigator.share({title:`Pegamentos — ${rec.reference}`,text:body});
      return;
    }catch(e){ if(e.name==='AbortError') return; }
  }
  window.location.href = `mailto:?subject=${encodeURIComponent('Pegamentos — '+(rec.reference||''))}&body=${encodeURIComponent(body)}`;
}

/* ---------------- database export/import ---------------- */
function exportDatabase(){
  downloadBlob(`pegamentos_backup_${localDateString()}.json`, new Blob([JSON.stringify(db,null,2)],{type:'application/json'}));
  toast('Cópia descarregada.','ok');
}
function importDatabasePrompt(){
  const inp=document.createElement('input');
  inp.type='file'; inp.accept='.json';
  inp.onchange=()=>{
    if(!inp.files || !inp.files[0]) return;
    const r=new FileReader();
    r.onload=()=>{
      try{
        const imported = normalizeDB(JSON.parse(r.result));
        if(!confirm(`Importar ${imported.records.length} referência(s) e substituir os dados atuais?`)) return;
        db = imported;
        activeId = db.activeId || (db.records[0] ? db.records[0].id : null);
        saveDB();
        renderRecordArea();
        renderRecordList();
        renderConfig();
        toast('Base de dados importada.','ok');
      }catch(e){ fail(e); }
    };
    r.readAsText(inp.files[0]);
  };
  inp.click();
}
function resetAll(){
  if(!confirm('Apagar definitivamente todas as referências guardadas neste navegador?')) return;
  db = seedDB();
  activeId = null;
  saveDB();
  renderRecordArea();
  renderRecordList();
  renderConfig();
  toast('Dados apagados.','ok');
}

/* ---------------- init ---------------- */
document.getElementById('recSearch') && document.getElementById('recSearch').addEventListener('input', renderRecordList);
window.addEventListener('afterprint', ()=>{ document.getElementById('printArea').innerHTML=''; });
renderRecordArea();
restoreMainFolderHandle();
</script>
</body>
</html>

```
## END FILE CONTENT

---

# FILE 032

## Source Path
`peso-operador.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Controlo de Peso e Volume — Operador
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (1): dmo-interactions.js
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 9
<footer>: 0
<form>: 0
UNIQUE IDS (38): new, editActiveRef, createRefFromNew, removeReading, addReading, readings, readingCount, calculate, sendApproval, refs, newRef, referenceList, 9389T194, refControls, editControl, compareControl, newControlRef, refEditor, editorTitle, closeEditor, history, historySearch, historyTypeFilter, historyTable, generateDoc, sendMail, comparisons, backToRefs, removeComparisonReading, addComparisonReading, comparisonReadings, comparisonReadingCount, comparisonPreview, calculateComparison, saveComparison, settings, sheet, toast
DATA-* ATTRIBUTES (4): view, id, status, kind
<button: 35
<input: 38
<select: 5
<textarea: 0
<table: 6
<a: 0
MODAL/DIALOG refs: dmo-modal-backdrop, dmo-modal, dmo-modal-head, dmo-modal-body, dmo-modal-foot
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Controlo de Peso e Volume — Operador</title>
<link rel="stylesheet" href="dmo-design-system.css">
<style>
.header{height:72px;background:#fff;border-bottom:1px solid var(--dmo-border);display:flex;align-items:center;padding:0 28px;gap:12px}.logo{width:42px;height:42px;border-radius:50%}.brand h1{font-size:17px;margin:0}.brand p,.user span{font-size:11px;color:var(--dmo-muted);margin:2px 0 0}.user{margin-left:auto;text-align:right}.user strong{display:block;font-size:12px}.tabs{height:50px;background:#fff;border-bottom:1px solid var(--dmo-border);display:flex;gap:26px;padding:0 28px}.tab{border:0;background:none;color:var(--dmo-muted);font-weight:700;border-bottom:3px solid transparent}.tab.active{color:var(--dmo-brand-600);border-color:var(--dmo-brand-600)}.settings{margin-left:auto}.main{max-width:1320px;margin:auto;padding:24px}.view{display:none}.view.active{display:block}.page-head{display:flex;align-items:end;justify-content:space-between;margin-bottom:16px}.page-head h2{font-size:23px;margin:0}.page-head p{color:var(--dmo-muted);margin:3px 0 0}.stack{display:grid;gap:14px}.card-pad{padding:18px}.section-head{display:flex;align-items:center;justify-content:space-between;margin-bottom:13px}.section-head h3{font-size:15px;margin:0}.grid4{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}.grid3{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}.control-grid{display:grid;grid-template-columns:minmax(190px,1.25fr) 105px 105px minmax(165px,.8fr) minmax(170px,1fr) 95px;gap:12px;align-items:end}.sap-grid{display:grid;grid-template-columns:210px 175px minmax(340px,1fr);gap:12px}.compact{max-width:150px}.reference-summary{display:grid;grid-template-columns:repeat(5,1fr);gap:10px}.summary-item{padding:10px;border-left:3px solid var(--dmo-brand-600);background:var(--dmo-subtle);border-radius:8px}.summary-item span{display:block;color:var(--dmo-muted);font-size:10px}.summary-item strong{font-size:13px}.readings{display:grid;grid-template-columns:repeat(4,1fr);gap:10px}.reading{display:grid;grid-template-columns:70px 1fr;gap:7px}.live{display:grid;grid-template-columns:repeat(4,1fr);gap:9px;margin-top:12px;padding:13px;border:1px solid var(--dmo-border);border-left:4px solid var(--dmo-brand-600);border-radius:10px}.live div{padding:9px;background:var(--dmo-subtle);border-radius:8px}.live span{display:block;font-size:10px;color:var(--dmo-muted)}.result-summary{display:grid;grid-template-columns:repeat(6,1fr);gap:8px;margin-bottom:12px}.result-summary div{background:var(--dmo-subtle);border-radius:8px;padding:10px}.result-summary span{display:block;color:var(--dmo-muted);font-size:10px}.result-summary strong{font-size:14px}.actions{display:flex;justify-content:flex-end;gap:8px;flex-wrap:wrap}.autosave{display:flex;align-items:center;gap:6px;color:var(--dmo-success);font-size:11px}.split{display:grid;grid-template-columns:410px 1fr;gap:12px}.select-table tr.selected{background:#d9e6f2}.compact-table th,.compact-table td{padding:8px 7px;font-size:11px}.reference-list{padding:14px}.reference-list .dmo-table{table-layout:fixed}.reference-list .dmo-table th,.reference-list .dmo-table td{overflow:hidden;text-overflow:ellipsis}.machine-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:8px}.machine-choice{min-height:48px;border:1px solid var(--dmo-border);border-radius:9px;background:var(--dmo-subtle);color:var(--dmo-brand-700);font-weight:800}.machine-choice.selected{background:var(--dmo-brand-600);border-color:var(--dmo-brand-600);color:#fff}.editor{display:none}.editor.open{display:block}.editor-sections{display:grid;gap:12px}.comparison-context{display:grid;grid-template-columns:repeat(5,1fr);gap:8px;margin-bottom:14px}.comparison-context div{padding:10px;background:var(--dmo-subtle);border-radius:8px}.comparison-context span{display:block;font-size:10px;color:var(--dmo-muted)}.sheet-context{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:13px}.hint{font-size:11px;color:var(--dmo-muted)}
@media(max-width:980px){.grid4,.result-summary{grid-template-columns:repeat(2,1fr)}.control-grid{grid-template-columns:1fr 105px 105px 165px}.sap-grid{grid-template-columns:190px 160px 1fr}.split{grid-template-columns:1fr}.reference-summary{grid-template-columns:repeat(3,1fr)}}@media(max-width:720px){.main{padding:14px}.grid4,.grid3,.control-grid,.sap-grid,.readings,.live,.reference-summary{grid-template-columns:1fr}.machine-grid{grid-template-columns:repeat(3,1fr)}}
</style>
<style>.reading-result{grid-column:1/-1;display:block;margin-top:-2px;padding:6px 8px;border-radius:7px;background:var(--dmo-brand-050);color:var(--dmo-brand-700);font-size:11px;font-weight:750}.reading-result.pending{color:var(--dmo-muted)}.comparison-actions{display:flex;justify-content:flex-end;gap:8px;margin-top:12px}</style>
</head>
<body>
<header class="header">
<img class="logo" src="logo_recolored(1).png" alt="BA">
<div class="brand">
<h1>Controlo de Peso e Volume</h1>
<p>Operador</p>
</div>
<div class="user">
<strong>João Silva</strong>
<span data-user-profile-title>Metrologia</span>
</div>
</header>
<nav class="tabs">
<button class="tab active" data-view="new">Novo controlo</button>
<button class="tab" data-view="refs">Referências</button>
<button class="tab" data-view="history">Histórico</button>
<button class="tab settings" data-view="settings">Definições</button>
</nav>
<main class="main">
<section class="view active" id="new">
<div class="page-head">
<div>
<h2>Novo controlo</h2>
<p>Preencha as leituras e envie o controlo para aprovação.</p>
</div>
<div class="autosave">● Guardado automaticamente</div>
</div>
<div class="stack">
<div class="dmo-card card-pad">
<div class="section-head">
<h3>Referência do controlo</h3>
<div class="actions">
<button class="dmo-button" id="editActiveRef">Editar referência</button>
<button class="dmo-button" id="createRefFromNew">Criar referência</button>
</div>
</div>
<div class="grid3">
<div class="dmo-field">
<label>Procurar referência</label>
<input placeholder="Ex.: 9121T173">
</div>
<div class="dmo-field">
<label>Medições anteriores aprovadas</label>
<select>
<option>Selecionar produção…</option>
<option>202512 · B3 · L24</option>
</select>
</div>
</div>
</div>
<div class="dmo-card card-pad">
<div class="section-head">
<h3>Referência ativa</h3>
</div>
<div class="reference-summary">
<div class="summary-item">
<span>Referência</span>
<strong>9389T194</strong>
</div>
<div class="summary-item">
<span>CM</span>
<strong>5447</strong>
</div>
<div class="summary-item">
<span>Lote CM</span>
<strong>4</strong>
</div>
<div class="summary-item">
<span>Boquilha</span>
<strong>T194</strong>
</div>
<div class="summary-item">
<span>Processo</span>
<strong>NNPB</strong>
</div>
<div class="summary-item">
<span>Máquina atual</span>
<strong>B3</strong>
</div>
</div>
</div>
<div class="dmo-card card-pad">
<div class="section-head">
<h3>Dados do controlo</h3>
</div>
<div class="comparison-context">
<div><span>Job On</span><strong>JO-202601-B3</strong></div><div><span>Produção</span><strong>202601</strong></div><div><span>CM</span><strong>5447</strong></div><div><span>Lote CM</span><strong>4</strong></div><div><span>Processo do lote</span><strong>NNPB</strong></div><div><span>Máquina</span><strong>B3</strong></div>
</div>
<p class="hint">Job On, Referência, Produção, Máquina e o CM/lote exatos vêm do Job On selecionado. O Processo vem desse lote criado no Peso. Estes dados não são editáveis no controlo.</p>
<div class="control-grid" style="grid-template-columns:165px 170px 95px;margin-top:12px">
<div class="dmo-field">
<label>Data de início</label>
<input type="date" value="2026-08-15">
</div>
<div class="dmo-field">
<label>Estado do molde</label>
<select>
<option>Novo</option>
<option>Reparado</option>
</select>
</div>
<div class="dmo-field">
<label>Temperatura (°C)</label>
<input type="number" value="20" min="0" max="99" step="1">
</div>
</div>
<hr style="border:0;border-top:1px solid var(--dmo-border);margin:18px 0">
<div class="section-head">
<h3>Leituras por CM</h3>
<div class="actions">
<button class="dmo-button" id="removeReading">Remover leitura</button>
<button class="dmo-button" id="addReading">Adicionar leitura</button>
</div>
</div>
<div class="readings" id="readings">
<div class="reading">
<div class="dmo-field">
<label>CM</label>
<input value="12">
</div>
<div class="dmo-field">
<label>Peso (g)</label>
<input value="152.43">
</div>
</div>
<div class="reading">
<div class="dmo-field">
<label>CM</label>
<input value="21">
</div>
<div class="dmo-field">
<label>Peso (g)</label>
<input value="152.43">
</div>
</div>
<div class="reading">
<div class="dmo-field">
<label>CM</label>
<input value="45">
</div>
<div class="dmo-field">
<label>Peso (g)</label>
<input value="151.43">
</div>
</div>
<div class="reading">
<div class="dmo-field">
<label>CM</label>
<input value="54">
</div>
<div class="dmo-field">
<label>Peso (g)</label>
<input value="151.43">
</div>
</div>
</div>
<div class="live">
<div>
<span>Leituras válidas</span>
<strong id="readingCount">4</strong>
</div>
<div>
<span>Média em água</span>
<strong>151,93 g</strong>
</div>
<div>
<span>Peso estimado em vidro</span>
<strong>230,97 g</strong>
</div>
<div>
<span>Diferença anterior</span>
<strong>+4,20 g</strong>
</div>
</div>
<hr style="border:0;border-top:1px solid var(--dmo-border);margin:18px 0">
<div class="sap-grid">
<div class="dmo-field">
<label>Fim da produção anterior (SAP)</label>
<input type="date">
</div>
<div class="dmo-field">
<label>Peso médio anterior (SAP)</label>
<input class="decimal-2" type="number" step="0.01">
</div>
<div class="dmo-field">
<label>Notas</label>
<input placeholder="Informação adicional relevante para este controlo">
</div>
</div>
</div>
<div class="dmo-card card-pad">
<div class="section-head">
<div>
<h3>Resultados</h3>
<p class="hint">A mesma estrutura é guardada e enviada para aprovação.</p>
</div>
</div>
<div class="result-summary">
<div>
<span>Densidade</span>
<strong>1,00 g/cm³</strong>
</div>
<div>
<span>Capacidade média</span>
<strong>151,93 cm³</strong>
</div>
<div>
<span>Peso médio</span>
<strong>230,97 g</strong>
</div>
<div>
<span>Peso nominal</span>
<strong>200 g</strong>
</div>
<div>
<span>Diferença</span>
<strong>+30,97 g</strong>
</div>
<div>
<span>Diferença %</span>
<strong>+15,49%</strong>
</div>
</div>
<div class="dmo-table-wrap">
<table class="dmo-table">
<thead>
<tr>
<th>CM</th>
<th>Peso em água</th>
<th>Capacidade</th>
<th>Desvio cm³</th>
<th>Desvio %</th>
<th>Peso estimado</th>
<th>Diferença anterior</th>
</tr>
</thead>
<tbody>
<tr>
<td>12</td>
<td>152,43</td>
<td>152,43</td>
<td>+0,50</td>
<td>+0,33%</td>
<td>231,41</td>
<td>+4,64</td>
</tr>
<tr>
<td>21</td>
<td>152,43</td>
<td>152,43</td>
<td>+0,50</td>
<td>+0,33%</td>
<td>231,41</td>
<td>+4,64</td>
</tr>
<tr>
<td>45</td>
<td>151,43</td>
<td>151,43</td>
<td>-0,50</td>
<td>-0,33%</td>
<td>230,53</td>
<td>+3,76</td>
</tr>
<tr>
<td>54</td>
<td>151,43</td>
<td>151,43</td>
<td>-0,50</td>
<td>-0,33%</td>
<td>230,53</td>
<td>+3,76</td>
</tr>
</tbody>
</table>
</div>
<div class="actions" style="margin-top:14px">
<button class="dmo-button" id="calculate">Calcular</button>
<button class="dmo-button" id="sendApproval">Enviar para aprovação</button>
</div>
</div>
</div>
</section>
<section class="view" id="refs">
<div class="page-head">
<div>
<h2>Referências</h2>
<p>Configuração e controlos associados a cada referência.</p>
</div>
</div>
<div class="split">
<div class="dmo-card card-pad reference-list">
<div class="section-head">
<h3>Lista de referências</h3>
<button class="dmo-button" id="newRef">Nova referência</button>
</div>
<div class="dmo-field">
<label>Pesquisar</label>
<input placeholder="Referência, CM, boquilha ou máquina">
</div>
<div class="dmo-table-wrap" style="margin-top:12px">
<table class="dmo-table select-table compact-table">
<thead>
<tr>
<th>Referência</th>
<th>CM</th>
<th>Boquilha</th>
<th>Processo</th>
<th>Peso</th>
</tr>
</thead>
<tbody data-dmo-list id="referenceList">
<tr class="selected" data-dmo-row data-id="9389T194" aria-selected="true">
<td>
<strong>9389T194</strong>
</td>
<td>9389</td>
<td>T194</td>
<td>NNPB</td>
<td>200</td>
</tr>
</tbody>
</table>
</div>
</div>
<div class="stack">
<div class="dmo-card card-pad">
<div class="section-head">
<div>
<h3>Controlos da referência ativa</h3>
<p class="hint">Um clique seleciona; duplo clique abre a folha.</p>
</div>
</div>
<div class="dmo-table-wrap">
<table class="dmo-table select-table compact-table" id="refControls">
<thead>
<tr>
<th>Data</th>
<th>Produção</th>
<th>Máquina</th>
<th>Lote</th>
<th>Peso</th>
<th>Revisão</th>
<th>Estado</th>
</tr>
</thead>
<tbody>
<tr data-row data-status="pending">
<td>15/08/2026</td>
<td>202601</td>
<td>B3</td>
<td>L26</td>
<td>155,98 g</td>
<td>1</td>
<td>
<span class="dmo-pill">Pendente</span>
</td>
</tr>
<tr data-row data-status="approved">
<td>10/07/2026</td>
<td>202512</td>
<td>B3</td>
<td>L24</td>
<td>151,78 g</td>
<td>2</td>
<td>
<span class="dmo-pill active">Aprovado</span>
</td>
</tr>
</tbody>
</table>
</div>
<div class="actions" style="margin-top:12px">
<button class="dmo-button" id="editControl" disabled>Editar controlo</button>
<button class="dmo-button" id="compareControl" disabled>Comparar</button>
<button class="dmo-button" id="newControlRef">Novo controlo deste Job On</button>
</div>
</div>
<div class="dmo-card card-pad editor" id="refEditor">
<div class="section-head">
<h3 id="editorTitle">Criar nova referência e primeiro lote</h3>
<button class="dmo-button" id="closeEditor">Fechar edição</button>
</div>
<div class="editor-sections">
<div class="grid3">
<div class="dmo-field">
<label>Referência base</label>
<input value="9389">
</div>
<div class="dmo-field">
<label>Número CM</label>
<input value="9389">
</div>
<div class="dmo-field">
<label>Neckring</label>
<input value="T194">
</div>
</div>
<div>
<h4 style="margin:0 0 4px">Dados do primeiro lote</h4>
<p class="hint" style="margin:0 0 10px">Processo e máquinas permitidas são definidos no lote do Peso e herdados pelo Job On, Novo controlo e Comparação.</p>
</div>
<div class="grid3">
<div class="dmo-field">
<label>Processo do lote</label>
<select>
<option>NNPB</option>
<option>PS</option>
</select>
</div>
<div class="dmo-field">
<label>Lote predefinido</label>
<input>
</div>
<div class="dmo-field">
<label>Peso nominal</label>
<input type="number" value="200">
</div>
</div>
<div class="grid3">
<div class="dmo-field" style="grid-column:span 2">
<label>Subpasta dos relatórios</label>
<input value="5447T173" placeholder="Ex.: 5447T173">
<small>Nome relativo criado dentro do diretório principal; não introduza um caminho absoluto.</small>
</div>
<div class="dmo-field">
<label>Caminho resolvido</label>
<input value="Capacidades / 5447T173" readonly aria-readonly="true">
</div>
</div>
<div class="dmo-field">
<label>Máquinas permitidas</label>
<p class="hint" style="margin:0 0 8px">Associe este lote às máquinas/linhas onde pode trabalhar. Esta associação já faz parte do registo operacional.</p>
<div class="machine-grid">
<button type="button" class="machine-choice selected">B1</button>
<button type="button" class="machine-choice">B2</button>
<button type="button" class="machine-choice selected">B3</button>
<button type="button" class="machine-choice selected">C1</button>
<button type="button" class="machine-choice">C2</button>
<button type="button" class="machine-choice">C3</button>
</div>
</div>
<div class="grid3">
<div class="dmo-field">
<label>Volume do punção</label>
<input type="number">
</div>
<div class="dmo-field">
<label>Volume da marisa</label>
<input type="number">
</div>
<div class="dmo-field">
<label>Volume do tampão</label>
<input type="number" readonly value="0">
</div>
</div>
<div class="dmo-field">
<label>Nota da alteração</label>
<input placeholder="Obrigatória ao alterar uma referência existente">
</div>
<div class="actions">
<button class="dmo-button">Guardar referência</button>
<button class="dmo-button">Ver histórico de alterações</button>
</div>
</div>
</div>
</div>
</div>
</section>
<section class="view" id="history">
<div class="page-head">
<div>
<h2>Histórico</h2>
<p>Controlos já enviados para aprovação.</p>
</div>
</div>
<div class="dmo-card card-pad">
<div class="grid4">
<div class="dmo-field">
<label>Pesquisar</label>
<input id="historySearch" placeholder="Referência, produção, lote ou máquina">
</div>
<div class="dmo-field">
<label>Estado</label>
<select>
<option>Todos</option>
<option>Pendente</option>
<option>Aprovado</option>
<option>Não aprovado</option>
</select>
</div>
<div class="dmo-field"><label>Tipo</label><select id="historyTypeFilter"><option value="all">Todos</option><option value="control">Registo de peso</option><option value="comparison">Comparação</option></select></div>
<div class="dmo-field">
<label>Desde</label>
<input type="date">
</div>
<div class="dmo-field">
<label>Até</label>
<input type="date">
</div>
</div>
<div class="dmo-table-wrap" style="margin-top:14px">
<table class="dmo-table select-table" id="historyTable">
<thead>
<tr>
<th>Data</th>
<th>Referência</th>
<th>Produção</th>
<th>Máquina</th>
<th>Lote</th>
<th>Peso</th>
<th>Revisão</th>
<th>Estado</th>
</tr>
</thead>
<tbody>
<tr data-row data-kind="control" data-status="approved">
<td>15/08/2026</td>
<td>9389T194</td>
<td>202601</td>
<td>B3</td>
<td>L26</td>
<td>230,97 g</td>
<td>2</td>
<td>
<span class="dmo-pill approved">Aprovado</span>
</td>
</tr>
<tr data-row data-kind="control" data-status="pending">
<td>14/08/2026</td>
<td>5447T173</td>
<td>202600</td>
<td>C1</td>
<td>L18</td>
<td>198,43 g</td>
<td>1</td>
<td>
<span class="dmo-pill pending">Pendente</span>
</td>
</tr>
<tr data-row data-kind="comparison" data-status="pending"><td>15/08/2026</td><td>9389T194</td><td>202512</td><td>B3</td><td>L24</td><td>225,68 g</td><td>Extra</td><td><span class="dmo-pill pending">Pendente</span></td></tr>
</tbody>
</table>
</div>
<div class="actions" style="margin-top:12px">
<button class="dmo-button" id="generateDoc" disabled>Gerar folha de produção</button>
<button class="dmo-button" id="sendMail" disabled>Enviar email para produção</button>
</div>
</div>
</section>
<section class="view" id="comparisons">
<div class="page-head"><div><h2>Comparar CM em produção</h2><p>Registe os CM atualmente montados e compare-os com o controlo aprovado selecionado.</p></div><button class="dmo-button" id="backToRefs">Voltar às referências</button></div>
<div class="stack">
<section class="dmo-card card-pad"><div class="section-head"><h3>Contexto ativo do Job On</h3></div><div class="reference-summary"><div class="summary-item"><span>Job On</span><strong>JO-202601-B3</strong></div><div class="summary-item"><span>Produção</span><strong>202601</strong></div><div class="summary-item"><span>Referência</span><strong>9389T194</strong></div><div class="summary-item"><span>CM</span><strong>5447</strong></div><div class="summary-item"><span>Lote CM</span><strong>4</strong></div><div class="summary-item"><span>Processo do lote</span><strong>NNPB</strong></div><div class="summary-item"><span>Máquina atual</span><strong>B3</strong></div></div></section>
<section class="dmo-card card-pad"><div class="section-head"><h3>Novo controlo aprovado deste Job On</h3><span class="dmo-pill active">Aprovado · Revisão 2</span></div><div class="comparison-context"><div><span>Job On</span><strong>JO-202601-B3</strong></div><div><span>Referência</span><strong>9389T194</strong></div><div><span>Produção</span><strong>202601</strong></div><div><span>CM</span><strong>5447</strong></div><div><span>Lote CM</span><strong>4</strong></div><div><span>Processo</span><strong>NNPB</strong></div><div><span>Máquina</span><strong>B3</strong></div></div><p class="hint">A Comparação mede os CM que já estão em produção e usa como base imutável o Novo controlo aprovado ligado a este Job On.</p></section>
<section class="dmo-card card-pad"><div class="section-head"><div><h3>CM atualmente em produção</h3><p class="hint">Introduza o CM e o peso medido. O peso do vidro é mostrado em tempo real por baixo de cada leitura.</p></div><div class="actions"><button class="dmo-button" id="removeComparisonReading">Remover leitura</button><button class="dmo-button" id="addComparisonReading">Adicionar leitura</button></div></div><div class="readings" id="comparisonReadings"><div class="reading"><div class="dmo-field"><label>CM</label><input type="number" value="34" step="1"></div><div class="dmo-field"><label>Peso (g)</label><input class="decimal-2" type="number" value="142.00" step="0.01"></div></div><div class="reading"><div class="dmo-field"><label>CM</label><input type="number" value="43" step="1"></div><div class="dmo-field"><label>Peso (g)</label><input class="decimal-2" type="number" value="151.40" step="0.01"></div></div></div><div class="live"><div><span>Leituras válidas</span><strong id="comparisonReadingCount">2</strong></div><div><span>Média em água</span><strong>146,70 g</strong></div><div><span>Peso estimado em vidro</span><strong>225,68 g</strong></div><div><span>Diferença para a base</span><strong>-5,29 g</strong></div></div><hr style="border:0;border-top:1px solid var(--dmo-border);margin:18px 0"><div class="sap-grid"><div class="dmo-field"><label>Data do registo da comparação</label><input type="date" value="2026-08-15"></div><div class="dmo-field"><label>Peso médio anterior (SAP)</label><input class="decimal-2" type="number" step="0.01"></div><div class="dmo-field"><label>Notas</label><input placeholder="Informação adicional relevante para esta comparação"></div></div></section>
<section class="dmo-card card-pad"><div class="section-head"><div><h3>Resultados da comparação</h3><p class="hint">Mesma estrutura do Novo controlo, comparada com o controlo aprovado usado como base.</p></div></div><div class="result-summary"><div><span>Densidade</span><strong>1,00 g/cm³</strong></div><div><span>Capacidade média</span><strong>55,65 cm³</strong></div><div><span>Peso médio</span><strong>225,68 g</strong></div><div><span>Média aprovada</span><strong>230,97 g</strong></div><div><span>Diferença</span><strong>-5,29 g</strong></div><div><span>Diferença %</span><strong>-2,29%</strong></div></div><div class="dmo-table-wrap"><table class="dmo-table compact-table"><thead><tr><th>CM</th><th>Peso em água</th><th>Peso do vidro</th><th>Capacidade atual</th><th>Média aprovada</th><th>Diferença peso</th><th>Diferença capacidade</th></tr></thead><tbody id="comparisonPreview"><tr><td>34</td><td>142,00 g</td><td>220,98 g</td><td>54,00 cm³</td><td>230,97 g</td><td>-9,99 g</td><td>-1,10 cm³</td></tr><tr><td>43</td><td>151,40 g</td><td>230,38 g</td><td>57,30 cm³</td><td>230,97 g</td><td>-0,59 g</td><td>+2,20 cm³</td></tr></tbody></table></div><div class="comparison-actions"><button class="dmo-button" id="calculateComparison">Calcular</button><button class="dmo-button" id="saveComparison">Enviar para aprovação</button></div></section>
</div>
</section>
<section class="view" id="settings">
<div class="stack">
<div class="dmo-card card-pad">
<div class="section-head"><div><h2>Diretório principal dos relatórios</h2><p class="hint">Os dados numéricos e o histórico ficam no servidor. Os PDFs aprovados são guardados neste computador.</p></div><button class="dmo-button">Selecionar diretório principal</button></div>
<div class="comparison-context"><div><span>Estado local</span><strong>Autorizado</strong></div><div><span>Diretório</span><strong>Capacidades</strong></div><div><span>Exemplo resolvido</span><strong>Capacidades / 5447T173</strong></div></div>
<p class="hint">A autorização pertence a este browser/computador e pode ter de ser renovada. A subpasta é definida quando se cria o lote.</p>
</div>
<div class="dmo-card card-pad">
<h2>Sincronização</h2>
<p class="hint">A sincronização dos registos estruturados com o servidor é automática. A interface apresenta separadamente o estado do servidor e o estado da gravação local do PDF.</p>
</div>
</div>
</section>
</main>
<div class="dmo-modal-backdrop" id="sheet">
<div class="dmo-modal" style="width:min(920px,100%)">
<div class="dmo-modal-head">
<div>
<h2>Folha de controlo</h2>
<p class="hint" style="margin:3px 0 0">9389T194 · Produção 202512 · B3 · Revisão 2</p>
</div>
<button class="dmo-button dmo-icon-button" data-close>×</button>
</div>
<div class="dmo-modal-body">
<div class="sheet-context">
<span class="dmo-pill active">Aprovado</span>
<span class="dmo-pill">Peso 151,78 g</span>
<span class="dmo-pill">Lote L24</span>
</div>
<div class="dmo-table-wrap">
<table class="dmo-table">
<thead>
<tr>
<th>CM</th>
<th>Capacidade</th>
<th>Desvio %</th>
<th>Peso estimado</th>
<th>Diferença anterior</th>
</tr>
</thead>
<tbody>
<tr>
<td>12</td>
<td>152,43</td>
<td>+0,33%</td>
<td>231,41</td>
<td>+4,64</td>
</tr>
<tr>
<td>45</td>
<td>151,43</td>
<td>-0,33%</td>
<td>230,53</td>
<td>+3,76</td>
</tr>
</tbody>
</table>
</div>
</div>
<div class="dmo-modal-foot">
<button class="dmo-button" data-close>Fechar</button>
<button class="dmo-button">Gerar folha</button>
<button class="dmo-button">Enviar para produção</button>
</div>
</div>
</div>
<div class="dmo-toast" id="toast">
</div>
<script>
const toast=document.querySelector('#toast');
const sheet=document.querySelector('#sheet');
const say=(text)=>{toast.textContent=text;toast.classList.add('show');setTimeout(()=>toast.classList.remove('show'),2200)};
const showView=(id,navId=id)=>{document.querySelectorAll('.view').forEach(x=>x.classList.remove('active'));document.querySelectorAll('.tab').forEach(x=>x.classList.toggle('active',x.dataset.view===navId));document.querySelector('#'+id).classList.add('active')};

document.querySelectorAll('.tab').forEach(button=>button.onclick=()=>showView(button.dataset.view));
document.querySelectorAll('.select-table tr[data-row]').forEach(row=>{
  row.onclick=()=>{
    row.closest('table').querySelectorAll('tr').forEach(x=>x.classList.remove('selected'));
    row.classList.add('selected');
    if(row.closest('#refControls')){document.querySelector('#editControl').disabled=false;document.querySelector('#compareControl').disabled=row.dataset.status!=='approved'}
    if(row.closest('#historyTable')){const approved=row.dataset.status==='approved';document.querySelector('#generateDoc').disabled=!approved;document.querySelector('#sendMail').disabled=!approved}
  };
  row.ondblclick=()=>sheet.classList.add('open');
});

function normaliseDecimal(input){if(input.value==='')return;const value=Number(String(input.value).replace(',','.'));if(Number.isFinite(value))input.value=value.toFixed(2)}
function formatPt(value){return Number(value).toFixed(2).replace('.',',')}
function calculateReadingPreview(reading){const weight=Number(reading.querySelectorAll('input')[1]?.value);const result=reading.querySelector('.reading-result');if(!result)return;if(!Number.isFinite(weight)){result.textContent='Peso do vidro: —';result.classList.add('pending');return}const glassWeight=weight+78.98;reading.dataset.glassWeight=glassWeight.toFixed(2);result.textContent=`Peso do vidro: ${formatPt(glassWeight)} g`;result.classList.remove('pending')}
function prepareReading(reading){const inputs=reading.querySelectorAll('input');if(inputs[1]){inputs[1].classList.add('decimal-2');inputs[1].step='0.01';if(!reading.querySelector('.reading-result'))reading.insertAdjacentHTML('beforeend','<small class="reading-result pending">Peso do vidro: —</small>');inputs[1].addEventListener('input',()=>calculateReadingPreview(reading));inputs[1].addEventListener('blur',()=>{normaliseDecimal(inputs[1]);calculateReadingPreview(reading)});calculateReadingPreview(reading)}}
function updateReadingCount(){document.querySelector('#readingCount').textContent=document.querySelectorAll('#readings .reading').length;document.querySelector('#comparisonReadingCount').textContent=document.querySelectorAll('#comparisonReadings .reading').length}
function addReading(containerId){const container=document.querySelector(containerId);const index=container.querySelectorAll('.reading').length+1;const reading=document.createElement('div');reading.className='reading';reading.innerHTML=`<div class="dmo-field"><label>CM</label><input type="number" step="1" placeholder="Ex.: ${index*10}"></div><div class="dmo-field"><label>Peso (g)</label><input class="decimal-2" type="number" step="0.01" placeholder="0,00"></div>`;container.appendChild(reading);prepareReading(reading);updateReadingCount()}
function removeReading(containerId){const items=document.querySelectorAll(`${containerId} .reading`);if(items.length<=1){say('É necessária pelo menos uma leitura');return}items[items.length-1].remove();updateReadingCount()}
document.querySelectorAll('.reading').forEach(prepareReading);
document.querySelectorAll('.decimal-2').forEach(input=>input.addEventListener('blur',()=>normaliseDecimal(input)));
document.querySelector('#addReading').onclick=()=>addReading('#readings');
document.querySelector('#removeReading').onclick=()=>removeReading('#readings');
document.querySelector('#addComparisonReading').onclick=()=>addReading('#comparisonReadings');
document.querySelector('#removeComparisonReading').onclick=()=>removeReading('#comparisonReadings');

document.querySelectorAll('[data-close]').forEach(button=>button.onclick=()=>sheet.classList.remove('open'));
document.querySelector('#newRef').onclick=document.querySelector('#createRefFromNew').onclick=document.querySelector('#editActiveRef').onclick=()=>{showView('refs');document.querySelector('#refEditor').classList.add('open')};
document.querySelector('#closeEditor').onclick=()=>document.querySelector('#refEditor').classList.remove('open');
document.querySelectorAll('.machine-choice').forEach(button=>button.onclick=()=>button.classList.toggle('selected'));
document.querySelector('#editControl').onclick=()=>{const row=document.querySelector('#refControls tr.selected');if(row?.dataset.status==='approved'){const reason=prompt('Justificação obrigatória para reabrir um controlo aprovado:');if(!reason)return;say('Revisão criada; nova aprovação necessária')}else say('Controlo aberto para edição')};
document.querySelector('#compareControl').onclick=()=>showView('comparisons','refs');
document.querySelector('#backToRefs').onclick=()=>showView('refs');
document.querySelector('#referenceList').addEventListener('dmo:list-select',event=>say(`Referência ${event.detail.id} selecionada`));
document.querySelector('#referenceList').addEventListener('dmo:list-open',event=>{document.querySelector('#historySearch').value=event.detail.id;showView('history');say(`Histórico filtrado por ${event.detail.id}`)});
document.querySelector('#historyTypeFilter').onchange=event=>{const type=event.target.value;document.querySelectorAll('#historyTable tr[data-row]').forEach(row=>row.hidden=type!=='all'&&row.dataset.kind!==type)};
document.querySelector('#newControlRef').onclick=()=>{showView('new');say('Novo controlo iniciado para 9389T194')};
document.querySelector('#calculate').onclick=()=>say('Resultados calculados com duas casas decimais');
document.querySelector('#sendApproval').onclick=()=>say('Controlo enviado para aprovação');
document.querySelector('#calculateComparison').onclick=()=>{const baseWeight=230.97,capacityByCm={34:54,43:57.3},capacityBase=55.1,rows=[...document.querySelectorAll('#comparisonReadings .reading')];document.querySelector('#comparisonPreview').innerHTML=rows.map(reading=>{calculateReadingPreview(reading);const inputs=reading.querySelectorAll('input'),cm=inputs[0].value||'—',water=Number(inputs[1].value),glass=Number(reading.dataset.glassWeight),capacity=capacityByCm[cm],weightDifference=Number.isFinite(glass)?glass-baseWeight:null,capacityDifference=Number.isFinite(capacity)?capacity-capacityBase:null;return `<tr><td>${cm}</td><td>${Number.isFinite(water)?formatPt(water)+' g':'—'}</td><td>${Number.isFinite(glass)?formatPt(glass)+' g':'—'}</td><td>${Number.isFinite(capacity)?formatPt(capacity)+' cm³':'—'}</td><td>${formatPt(baseWeight)} g</td><td>${Number.isFinite(weightDifference)?(weightDifference>=0?'+':'')+formatPt(weightDifference)+' g':'—'}</td><td>${Number.isFinite(capacityDifference)?(capacityDifference>=0?'+':'')+formatPt(capacityDifference)+' cm³':'—'}</td></tr>`}).join('');say('Comparação atualizada com o lote aprovado selecionado')};
document.querySelector('#saveComparison').onclick=()=>say('Comparação enviada para aprovação');
document.querySelector('#generateDoc').onclick=()=>say('Folha de produção gerada');
document.querySelector('#sendMail').onclick=()=>say('Email de produção preparado');
</script>
<script src="dmo-interactions.js">
</script>
</body>
</html>

```
## END FILE CONTENT

---

# FILE 033

## Source Path
`peso-responsavel.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Controlo de Peso e Volume — Responsável
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (1): dmo-interactions.js
<header>: 1
<nav>: 1
<aside>: 1
<main>: 1
<section>: 5
<footer>: 0
<form>: 0
UNIQUE IDS (20): calendar, allDates, selectedDate, approvalStatusFilter, approvalTypeFilter, approvalList, ctrl-202601, cmp-202512, controlDetail, reject, approve, emailPreview, sendApproved, comparisonDetail, keptCount, asideCount, pendingCount, comparisonReason, confirmComparison, toast
DATA-* ATTRIBUTES (5): id, kind, status, decision, date
<button: 14
<input: 2
<select: 2
<textarea: 2
<table: 2
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: data-dmo-calendar, dmo-calendar__head, dmo-calendar__week, dmo-calendar__grid, calendar, dmo-calendar__day
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Controlo de Peso e Volume — Responsável</title>
<link rel="stylesheet" href="dmo-design-system.css">
<style>
.header{height:72px;background:#fff;border-bottom:1px solid var(--dmo-border);display:flex;align-items:center;padding:0 28px;gap:12px}.logo{width:42px;height:42px;border-radius:50%}.brand h1{font-size:17px;margin:0}.brand p,.user span{font-size:11px;color:var(--dmo-muted);margin:2px 0 0}.user{margin-left:auto;text-align:right}.user strong{display:block;font-size:12px}.tabs{height:50px;background:#fff;border-bottom:1px solid var(--dmo-border);display:flex;padding:0 28px}.tabs button{border:0;background:none;color:var(--dmo-brand-600);font-weight:750;border-bottom:3px solid var(--dmo-brand-600)}.tabs .settings{margin-left:auto;color:var(--dmo-muted);border-color:transparent}.main{max-width:1320px;margin:auto;padding:24px}.page-head{margin-bottom:16px}.page-head h2{margin:0;font-size:23px}.page-head p{margin:3px 0 0;color:var(--dmo-muted)}.metrics{display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-bottom:14px}.metric{padding:14px 16px}.metric span{display:block;font-size:10px;font-weight:800;color:var(--dmo-muted);text-transform:uppercase}.metric strong{font-size:22px}.layout{display:grid;grid-template-columns:360px 1fr;gap:14px}.card-pad{padding:18px}.queue{margin-top:14px}.queue-filters{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:10px}.queue-item{position:relative;padding:12px;border:1px solid var(--dmo-border);border-radius:9px;margin-top:8px}.queue-item .dmo-pill{position:absolute;right:10px;top:10px}.queue-item small{display:block;color:var(--dmo-muted);margin-top:3px}.detail-head{display:flex;justify-content:space-between;gap:12px;margin-bottom:14px}.detail-head h3{margin:0}.detail-head p{margin:3px 0 0;color:var(--dmo-muted);font-size:11px}.section-title{margin:14px 0 10px;padding:8px 10px;border-left:4px solid var(--dmo-brand-600);border-radius:7px;background:var(--dmo-subtle);font-size:13px}.identity{display:grid;grid-template-columns:repeat(6,1fr);gap:8px}.identity div,.summary div{padding:10px;background:var(--dmo-subtle);border-radius:8px}.identity span,.summary span{display:block;font-size:10px;color:var(--dmo-muted)}.summary{display:grid;grid-template-columns:repeat(4,1fr);gap:8px}.summary strong{font-size:16px}.decision{margin-top:14px;padding:14px;background:var(--dmo-subtle);border-radius:10px}.decision .success{--button-color:#527c72}.decision .danger{--button-color:#9a625d}.actions{display:flex;justify-content:flex-end;gap:8px;margin-top:12px}.hint{font-size:11px;color:var(--dmo-muted)}.hidden{display:none}.cm-choice{display:flex;gap:6px}.cm-choice .dmo-button{min-height:30px;padding:4px 8px;font-size:11px}.cm-choice .success{--button-color:#527c72}.cm-choice .danger{--button-color:#9a625d}.cm-choice.has-decision .dmo-button:not(.chosen){opacity:.35}.cm-choice .chosen{box-shadow:0 0 0 3px rgba(60,115,168,.2)}.comparison-note{padding:10px 12px;border-left:4px solid var(--dmo-brand-600);background:var(--dmo-brand-050);border-radius:8px;margin-bottom:12px}@media(max-width:900px){.layout{grid-template-columns:1fr}.identity{grid-template-columns:repeat(3,1fr)}.summary{grid-template-columns:repeat(2,1fr)}}
.email-preview{display:none;margin-top:12px;padding:13px;background:#fff;border:1px solid var(--dmo-brand-200);border-left:4px solid var(--dmo-brand-600);border-radius:9px}.email-preview.open{display:block}.email-grid{display:grid;grid-template-columns:150px 1fr;gap:7px 12px;margin-top:10px}.email-grid span{color:var(--dmo-muted);font-size:11px}.email-grid strong{font-size:12px}.approved-message{margin-right:auto;color:var(--dmo-success);font-size:12px;font-weight:750}
</style>
</head>
<body>
<header class="header">
<img class="logo" src="logo_recolored(1).png" alt="BA">
<div class="brand">
<h1>Controlo de Peso e Volume</h1>
<p>Responsável</p>
</div>
<div class="user">
<strong>Ana Martins</strong>
<span data-user-profile-title>Responsável de qualidade</span>
</div>
</header>
<nav class="tabs">
<button>Aprovações</button>
<button class="settings">Definições</button>
</nav>
<main class="main">
<div class="page-head">
<h2>Aprovações</h2>
<p>Controlos recebidos para decisão.</p>
</div>
<section class="metrics">
<div class="dmo-card metric">
<span>Pendentes</span>
<strong>1</strong>
</div>
<div class="dmo-card metric">
<span>Aprovados</span>
<strong>0</strong>
</div>
<div class="dmo-card metric">
<span>Não aprovados</span>
<strong>0</strong>
</div>
</section>
<div class="layout">
<aside>
<section class="dmo-card card-pad" data-dmo-calendar>
<div class="dmo-calendar__head">
<button class="dmo-button dmo-icon-button" aria-label="Mês anterior">‹</button>
<strong>Agosto de 2026</strong>
<button class="dmo-button dmo-icon-button" aria-label="Mês seguinte">›</button>
</div>
<div class="dmo-calendar__week">
<span>SEG</span>
<span>TER</span>
<span>QUA</span>
<span>QUI</span>
<span>SEX</span>
<span>SÁB</span>
<span>DOM</span>
</div>
<div class="dmo-calendar__grid" id="calendar">
</div>
<button class="dmo-button" id="allDates" style="margin-top:12px">Mostrar todas as datas</button>
</section>
<section class="dmo-card card-pad queue">
<h3 style="margin:0">Controlos de <span id="selectedDate">15/08/2026</span>
</h3>
<p class="hint">Pesquise ou filtre os controlos recebidos.</p>
<div class="dmo-field">
<label>Pesquisar</label>
<input placeholder="Referência, CM, boquilha, lote ou máquina">
</div>
<div class="queue-filters"><div class="dmo-field">
<label>Estado</label>
<select id="approvalStatusFilter">
<option value="all">Todos</option>
<option value="pending">Pendentes</option>
<option value="approved">Aprovados</option>
<option value="rejected">Não aprovados</option>
</select>
</div><div class="dmo-field"><label>Tipo</label><select id="approvalTypeFilter"><option value="all">Todos</option><option value="control">Registo de peso</option><option value="comparison">Comparação</option></select></div></div>
<div data-dmo-list id="approvalList">
<article class="queue-item selected" data-dmo-row data-id="ctrl-202601" data-kind="control" data-status="pending" aria-selected="true">
<strong>9389T194 · 202601</strong>
<small>B3 · L26 · Revisão 1 · Peso 155,98 g</small>
<span class="dmo-pill pending">Pendente</span>
</article>
<article class="queue-item" data-dmo-row data-id="cmp-202512" data-kind="comparison" data-status="pending">
<strong>Comparação · 9389T194</strong>
<small>B3 · Base aprovada 202512 · 2 CM</small>
<span class="dmo-pill pending">Pendente</span>
</article>
</div>
</section>
</aside>
<section class="dmo-card card-pad" id="controlDetail">
<div class="detail-head">
<div>
<h3>9389T194</h3>
<p>Produção 202601 · Linha B3 · Lote 26 · Enviado em 15/08/2026 às 10:42</p>
</div>
<span class="dmo-pill pending">Pendente</span>
</div>
<h3 class="section-title">Identificação da produção</h3>
<div class="identity">
<div>
<span>Referência</span>
<strong>9389T194</strong>
</div>
<div>
<span>CM</span>
<strong>9389</strong>
</div>
<div>
<span>Boquilha / Neckring</span>
<strong>T194</strong>
</div>
<div>
<span>Máquina</span>
<strong>B3</strong>
</div>
<div>
<span>Lote</span>
<strong>26</strong>
</div>
<div>
<span>Processo</span>
<strong>NNPB</strong>
</div>
<div>
<span>Produção</span>
<strong>202601</strong>
</div>
<div>
<span>Estado do molde</span>
<strong>Novo</strong>
</div>
<div>
<span>Operador</span>
<strong>João Silva</strong>
</div>
<div>
<span>Data</span>
<strong>15/08/2026</strong>
</div>
<div>
<span>Revisão</span>
<strong>1</strong>
</div>
<div>
<span>Controlo anterior</span>
<strong>Não disponível</strong>
</div>
</div>
<h3 class="section-title">Comparação global de peso</h3>
<div class="summary">
<div>
<span>Peso médio atual</span>
<strong>155,98 g</strong>
</div>
<div>
<span>Média do controlo anterior</span>
<strong>—</strong>
</div>
<div>
<span>Média SAP da produção anterior</span>
<strong>121 g</strong>
</div>
<div>
<span>Peso nominal do desenho</span>
<strong>200 g</strong>
</div>
<div>
<span>Capacidade atual</span>
<strong>120,72 cm³</strong>
</div>
<div>
<span>Capacidade anterior</span>
<strong>—</strong>
</div>
<div>
<span>Diferença de capacidade</span>
<strong>—</strong>
</div>
</div>
<h3 class="section-title">Comparação por CM — peso do vidro e volume</h3>
<p class="hint">O peso de cada posição é calculado com CM + boquilha + punção. O volume confirma o equilíbrio das capacidades.</p>
<div class="dmo-table-wrap">
<table class="dmo-table">
<thead>
<tr>
<th>CM</th>
<th>Peso atual</th>
<th>Peso anterior</th>
<th>Diferença</th>
<th>Volume atual</th>
<th>Volume anterior</th>
<th>Diferença volume</th>
</tr>
</thead>
<tbody>
<tr>
<td>Leitura 1 · CM 12</td>
<td>155,07 g</td>
<td>—</td>
<td>—</td>
<td>120,34 cm³</td>
<td>—</td>
<td>—</td>
</tr>
<tr>
<td>Leitura 2 · CM 20</td>
<td>157,72 g</td>
<td>—</td>
<td>—</td>
<td>121,44 cm³</td>
<td>—</td>
<td>—</td>
</tr>
<tr>
<td>Leitura 3 · CM 41</td>
<td>153,63 g</td>
<td>—</td>
<td>—</td>
<td>119,74 cm³</td>
<td>—</td>
<td>—</td>
</tr>
<tr>
<td>Leitura 4 · CM 54</td>
<td>157,48 g</td>
<td>—</td>
<td>—</td>
<td>121,34 cm³</td>
<td>—</td>
<td>—</td>
</tr>
</tbody>
</table>
</div>
<h3 class="section-title">Referências e observações</h3>
<div class="dmo-field">
<label>Observações do operador</label>
<div style="padding:10px;background:var(--dmo-subtle);border-radius:8px">Sem observações</div>
</div>
<div class="decision">
<div class="dmo-field">
<label>Responsável</label>
<input value="Ana Martins" readonly>
</div>
<div class="dmo-field" style="margin-top:10px">
<label>Nota / alterações necessárias</label>
<textarea rows="2" placeholder="Obrigatória quando o controlo não é aprovado.">
</textarea>
</div>
<div class="actions">
<button class="dmo-button danger" id="reject">Não aprovar</button>
<button class="dmo-button success" id="approve">Aprovar</button>
</div>
<div class="email-preview" id="emailPreview">
<strong>Relatório aprovado — envio para produção</strong>
<p class="hint" style="margin:3px 0 0">A máquina B3 seleciona automaticamente o grupo Linha B. Confirme antes de enviar.</p>
<div class="email-grid"><span>Máquina</span><strong>B3</strong><span>Destinatários</span><strong>Moldesmg@baglass.com</strong><span>Assunto</span><strong>Controlo de Peso e Volume · 9389T194 · 202601 · B3 · L26</strong><span>Anexo</span><strong>9389T194_202601_B3_L26_R1.pdf</strong></div>
<div class="actions"><span class="approved-message">Aprovado por Ana Martins</span><button class="dmo-button" id="sendApproved">Enviar para produção</button></div>
</div>
</div>
</section>
<section class="dmo-card card-pad hidden" id="comparisonDetail">
<div class="detail-head"><div><h3>Comparação · 9389T194</h3><p>CM em produção comparados com o controlo aprovado 202512 · B3 · L24</p></div><span class="dmo-pill pending">Pendente</span></div>
<div class="comparison-note"><strong>Registo complementar</strong><div class="hint">Esta decisão não altera o controlo aprovado usado como base. Cria um registo adicional ligado à revisão aprovada.</div></div>
<h3 class="section-title">Base aprovada</h3><div class="identity"><div><span>Produção</span><strong>202512</strong></div><div><span>Lote</span><strong>L24</strong></div><div><span>Processo</span><strong>NNPB</strong></div><div><span>Máquina</span><strong>B3</strong></div><div><span>Média de peso</span><strong>145,20 g</strong></div><div><span>Média capacidade</span><strong>55,10 cm³</strong></div></div>
<h3 class="section-title">Decisão individual por CM</h3><div class="dmo-table-wrap"><table class="dmo-table"><thead><tr><th>CM</th><th>Peso atual</th><th>Capacidade atual</th><th>Peso aprovado</th><th>Capacidade aprovada</th><th>Diferença peso</th><th>Diferença capacidade</th><th>Decisão</th></tr></thead><tbody>
<tr data-cm-decision><td><strong>34</strong></td><td>142,00 g</td><td>54,00 cm³</td><td>145,20 g</td><td>55,10 cm³</td><td>-3,20 g</td><td>-1,10 cm³</td><td><div class="cm-choice"><button class="dmo-button success" data-decision="keep">Manter</button><button class="dmo-button danger" data-decision="aside">Colocar de parte</button></div></td></tr>
<tr data-cm-decision><td><strong>43</strong></td><td>151,40 g</td><td>57,30 cm³</td><td>145,20 g</td><td>55,10 cm³</td><td>+6,20 g</td><td>+2,20 cm³</td><td><div class="cm-choice"><button class="dmo-button success" data-decision="keep">Manter</button><button class="dmo-button danger" data-decision="aside">Colocar de parte</button></div></td></tr>
</tbody></table></div>
<div class="summary" style="margin-top:12px"><div><span>CM mantidos</span><strong id="keptCount">0</strong></div><div><span>CM colocados de parte</span><strong id="asideCount">0</strong></div><div><span>Sem decisão</span><strong id="pendingCount">2</strong></div></div>
<div class="dmo-field" style="margin-top:12px"><label>Justificação</label><textarea id="comparisonReason" rows="2" placeholder="Obrigatória quando pelo menos um CM é colocado de parte."></textarea></div><div class="actions"><button class="dmo-button" id="confirmComparison" disabled>Confirmar decisões</button></div>
</section>
</div>
</main>
<div class="dmo-toast" id="toast">
</div>
<script src="dmo-interactions.js">
</script>
<script>
const calendar=document.querySelector('#calendar');for(let i=0;i<35;i++){const day=i<31?i+1:'';const iso=day?`2026-08-${String(day).padStart(2,'0')}`:'';calendar.insertAdjacentHTML('beforeend',`<button class="dmo-calendar__day ${day===15?'selected has-record':day===14?'has-record':''}" data-date="${iso}" ${day?'':'disabled'}>${day}</button>`)}
const toast=document.querySelector('#toast'),say=text=>{toast.textContent=text;toast.classList.add('show');setTimeout(()=>toast.classList.remove('show'),2200)};
document.addEventListener('dmo:date-select',event=>document.querySelector('#selectedDate').textContent=event.detail.date.split('-').reverse().join('/'));
document.querySelector('#allDates').onclick=()=>{document.querySelectorAll('[data-dmo-calendar] .selected').forEach(x=>x.classList.remove('selected'));document.querySelector('#selectedDate').textContent='todas as datas'};
function filterApprovalList(){const type=document.querySelector('#approvalTypeFilter').value,status=document.querySelector('#approvalStatusFilter').value,rows=[...document.querySelectorAll('#approvalList [data-dmo-row]')];rows.forEach(row=>{row.hidden=!((type==='all'||row.dataset.kind===type)&&(status==='all'||row.dataset.status===status))});const selected=rows.find(row=>row.classList.contains('selected'));if(selected?.hidden){selected.classList.remove('selected');const first=rows.find(row=>!row.hidden);if(first)first.click();else{document.querySelector('#controlDetail').classList.add('hidden');document.querySelector('#comparisonDetail').classList.add('hidden')}}}
document.querySelector('#approvalTypeFilter').onchange=filterApprovalList;
document.querySelector('#approvalStatusFilter').onchange=filterApprovalList;
document.querySelector('#approvalList').addEventListener('dmo:list-select',event=>{const comparison=event.detail.row.dataset.kind==='comparison';document.querySelector('#controlDetail').classList.toggle('hidden',comparison);document.querySelector('#comparisonDetail').classList.toggle('hidden',!comparison)});
document.querySelector('#approvalList').addEventListener('dmo:list-open',event=>say(`Abrir registo ${event.detail.id}`));
document.querySelectorAll('[data-decision]').forEach(button=>button.onclick=()=>{const group=button.closest('.cm-choice');group.classList.add('has-decision');group.querySelectorAll('button').forEach(item=>item.classList.toggle('chosen',item===button));group.closest('[data-cm-decision]').dataset.decision=button.dataset.decision;updateDecisionSummary()});
function updateDecisionSummary(){const rows=[...document.querySelectorAll('[data-cm-decision]')],kept=rows.filter(row=>row.dataset.decision==='keep').length,aside=rows.filter(row=>row.dataset.decision==='aside').length,pending=rows.length-kept-aside;document.querySelector('#keptCount').textContent=kept;document.querySelector('#asideCount').textContent=aside;document.querySelector('#pendingCount').textContent=pending;document.querySelector('#confirmComparison').disabled=pending>0}
document.querySelector('#confirmComparison').onclick=()=>{const aside=Number(document.querySelector('#asideCount').textContent);if(aside&& !document.querySelector('#comparisonReason').value.trim()){say('Indique a justificação para os CM colocados de parte');return}say('Decisões individuais confirmadas; controlo aprovado preservado')};
document.querySelector('#approve').onclick=()=>{document.querySelector('#emailPreview').classList.add('open');document.querySelector('#approve').disabled=true;document.querySelector('#reject').disabled=true;say('Controlo aprovado — envio para Linha B preparado')};document.querySelector('#reject').onclick=()=>say('Controlo não aprovado — justificação registada');document.querySelector('#sendApproved').onclick=()=>say('Pedido de envio preparado para os destinatários da máquina B3');
</script>
</body>
</html>

```
## END FILE CONTENT

---

# FILE 034

## Source Path
`reparacao-externa-v1.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Reparação externa — Portal DMO
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (0): 
<header>: 0
<nav>: 0
<aside>: 0
<main>: 0
<section>: 0
<footer>: 0
<form>: 0
UNIQUE IDS (0): 
DATA-* ATTRIBUTES (0): 
<button: 0
<input: 0
<select: 0
<textarea: 0
<table: 0
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html><html lang="pt-PT"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>Reparação externa — Portal DMO</title><link rel="stylesheet" href="dmo-design-system.css"><style>
:root{--b:#3c73a8;--bd:#193046;--bs:#e8eff7;--page:#f6f9fc;--sub:#f1f6fa;--line:#d9e6f2;--text:#172d42;--muted:#64778a;--green:#527c72;--green-s:#e5f0eb;--warn:#a97943;--warn-s:#f7f0e7;--red:#9a625d;--disabled:#cbd5df;--r:12px;--shadow:0 8px 24px rgba(25,48,70,.06)}*{box-sizing:border-box}body{margin:0;background:var(--page);color:var(--text);font:14px/1.45 Inter,"Segoe UI",sans-serif}button,input,select,textarea{font:inherit}.header{min-height:76px;display:flex;align-items:center;gap:13px;padding:10px 28px;background:#fff;border-bottom:1px solid var(--line)}.logo{width:44px;height:44px;object-fit:contain;border-radius:50%}.head h1{margin:0;font-size:18px}.head p,.muted{margin:3px 0 0;color:var(--muted);font-size:11px}.user{margin-left:auto;padding-left:18px;text-align:right;border-left:1px solid var(--line)}.user strong,.user span{display:block}.user span{font-size:11px;color:var(--muted)}.tabs{height:52px;display:flex;gap:24px;padding:0 28px;background:#fff;border-bottom:1px solid var(--line);overflow:auto}.tab{border:0;border-bottom:3px solid transparent;background:none;color:var(--muted);font-weight:750;white-space:nowrap;cursor:pointer}.tab.active{color:var(--b);border-color:var(--b)}.tab.settings{margin-left:auto}.main{max-width:1360px;margin:auto;padding:26px}.view{display:none}.view.active{display:block}.page-head,.panel-head,.footer{display:flex;align-items:center;justify-content:space-between;gap:12px}.page-head{align-items:end;margin-bottom:16px}.page-head h2,.panel-head h3{margin:0}.card{padding:18px;background:#fff;border:1px solid var(--line);border-radius:var(--r);box-shadow:var(--shadow)}.btn{min-height:36px;padding:7px 12px;border:1px solid var(--b);border-radius:8px;background:var(--b);color:#fff;font-weight:750;cursor:pointer}.btn:hover,.btn:focus-visible{background:#fff;color:var(--b);outline:none}.btn:disabled{border-color:var(--disabled);background:var(--disabled);color:#fff;cursor:not-allowed}.btn.icon{width:36px;padding:0}.btn.danger{border-color:var(--red);background:var(--red)}.btn.danger:hover{background:#fff;color:var(--red)}.field label{display:block;margin-bottom:6px;color:var(--muted);font-size:11px;font-weight:750}.field input,.field select{width:100%;min-height:40px;padding:9px 11px;border:1px solid var(--line);border-radius:8px;background:#fff;color:var(--text)}.metrics{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:16px}.metric{padding:14px;border-left:4px solid var(--b);border-radius:9px;background:#fff;box-shadow:var(--shadow)}.metric span{display:block;color:var(--muted);font-size:10px}.metric strong{display:block;margin-top:3px;font-size:23px}.metric.warn{border-color:var(--warn)}.filters{display:grid;grid-template-columns:2fr 150px 150px 150px 100px auto;gap:9px;align-items:end}.table-wrap{overflow:auto;margin-top:14px;border:1px solid var(--line);border-radius:10px}table{width:100%;border-collapse:collapse;white-space:nowrap}th{padding:10px 12px;background:var(--sub);color:var(--muted);font-size:10px;text-align:left;text-transform:uppercase}td{padding:11px 12px;border-top:1px solid var(--line)}tr[data-dmo-row]{cursor:pointer}tr[data-dmo-row]:hover{background:var(--page)}tr.selected{background:var(--line)}.pill{display:inline-flex;padding:4px 8px;border-radius:99px;background:var(--bs);color:#315d88;font-size:10px;font-weight:800}.pill.green{background:var(--green-s);color:var(--green)}.pill.warn{background:var(--warn-s);color:var(--warn)}.progress{width:120px;height:7px;overflow:hidden;border-radius:9px;background:var(--line)}.progress i{display:block;height:100%;background:var(--b)}.footer{padding-top:11px;color:var(--muted);font-size:11px}.pager,.actions{display:flex;align-items:center;justify-content:flex-end;gap:8px}.inline{display:none;margin-top:16px}.inline.open{display:block}.form-grid{display:grid;grid-template-co
```
## END FILE CONTENT

---

# FILE 035

## Source Path
`reparacao-interna.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Reparação interna — Portal DMO
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (1): dmo-interactions.js
<header>: 1
<nav>: 1
<aside>: 1
<main>: 1
<section>: 5
<footer>: 0
<form>: 0
UNIQUE IDS (28): registo, initialState, activeForm, activeContextTitle, contextProduction, contextReference, contextJobOn, activeJobReference, activeJobMeta, toolNumber, review, toolNotice, confirmation, confirmType, confirmNumber, cancelReview, submitRepair, ri-1, ri-2, historico, historySearch, historyList, ri-3, correct, correction, closeCorrection, saveCorrection, toast
DATA-* ATTRIBUTES (6): reference, view, line, production, type, id
<button: 21
<input: 6
<select: 6
<textarea: 0
<table: 2
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Reparação interna — Portal DMO</title>
    <link rel="stylesheet" href="dmo-design-system.css" />
    <style>
      :root {
        --b: #3c73a8;
        --bd: #193046;
        --bs: #e8eff7;
        --page: #f6f9fc;
        --sub: #f1f6fa;
        --line: #d9e6f2;
        --text: #172d42;
        --muted: #64778a;
        --green: #527c72;
        --green-s: #e5f0eb;
        --warn: #a97943;
        --warn-s: #f7f0e7;
        --red: #9a625d;
        --red-s: #f3e9e7;
        --r: 12px;
        --shadow: 0 8px 24px rgba(25, 48, 70, 0.06);
      }
      * {
        box-sizing: border-box;
      }
      body {
        margin: 0;
        background: var(--page);
        color: var(--text);
        font:
          14px/1.45 Inter,
          "Segoe UI",
          sans-serif;
      }
      button,
      input,
      select,
      textarea {
        font: inherit;
      }
      .header {
        height: 76px;
        display: flex;
        align-items: center;
        gap: 13px;
        padding: 10px 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .logo {
        width: 44px;
        height: 44px;
        border-radius: 50%;
      }
      .head h1 {
        margin: 0;
        font-size: 18px;
      }
      .head p,
      .muted {
        margin: 3px 0 0;
        color: var(--muted);
        font-size: 11px;
      }
      .user {
        margin-left: auto;
        text-align: right;
        padding-left: 18px;
        border-left: 1px solid var(--line);
      }
      .user strong,
      .user span {
        display: block;
      }
      .user span {
        font-size: 11px;
        color: var(--muted);
      }
      .tabs {
        height: 52px;
        display: flex;
        gap: 26px;
        padding: 0 28px;
        background: #fff;
        border-bottom: 1px solid var(--line);
      }
      .tab {
        border: 0;
        border-bottom: 3px solid transparent;
        background: none;
        color: var(--muted);
        font-weight: 750;
        cursor: pointer;
      }
      .tab.active {
        color: var(--b);
        border-color: var(--b);
      }
      .main {
        max-width: 1320px;
        margin: auto;
        padding: 26px;
      }
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }
      .page-head {
        display: flex;
        align-items: end;
        justify-content: space-between;
        gap: 12px;
        margin-bottom: 16px;
      }
      .page-head h2 {
        margin: 0;
        font-size: 24px;
      }
      .card {
        background: #fff;
        border: 1px solid var(--line);
        border-radius: var(--r);
        box-shadow: var(--shadow);
      }
      .pad {
        padding: 18px;
      }
      .btn {
        min-height: 36px;
        padding: 7px 12px;
        border: 1px solid var(--b);
        border-radius: 8px;
        background: var(--b);
        color: #fff;
        font-weight: 750;
        cursor: pointer;
      }
      .btn:hover,
      .btn:focus-visible,
      .btn.active {
        background: #fff;
        color: var(--b);
        outline: none;
      }
      .btn:disabled {
        background: #cbd5df;
        border-color: #cbd5df;
        color: #fff;
        cursor: not-allowed;
      }
      .icon {
        width: 36px;
        padding: 0;
      }
      .field label {
        display: block;
        margin-bottom: 6px;
        color: var(--muted);
        font-size: 11px;
        font-weight: 750;
      }
      .field input,
      .field select,
      .field textarea {
        width: 100%;
        min-height: 40px;
        padding: 9px 11px;
        border: 1px solid var(--line);
        border-radius: 8px;
        background: #fff;
        color: var(--text);
        outline: none;
      }
      .field input:focus,
      .field select:focus,
      .field textarea:focus {
        border-color: var(--b);
        box-shadow: 0 0 0 3px #3c73a820;
      }
      .line-choice {
        display: grid;
        grid-template-columns: repeat(6, minmax(0, 1fr));
        gap: 8px;
      }
      .line-card {
        width: 100%;
        min-width: 0;
        min-height: 46px;
        padding: 7px 9px;
        text-align: left;
      }
      .line-card strong,
      .line-card small {
        display: block;
      }
      .line-card strong { font-size: 12px; }
      .line-card small {
        margin-top: 2px;
        overflow: hidden;
        font-size: 10px;
        font-weight: 650;
        text-overflow: ellipsis;
        white-space: nowrap;
        opacity: 0.84;
      }
      .line-card[data-reference="—"] {
        border-color: #ccd7e1;
        background: #eef2f6;
        color: #718293;
      }
      .line-card[data-reference="—"]:hover {
        background: #fff;
        color: #718293;
      }
      .flow {
        display: grid;
        grid-template-columns: minmax(0, 1fr);
        gap: 16px;
      }
      .flow > * {
        min-width: 0;
      }
      .step-title {
        font-size: 11px;
        font-weight: 850;
        color: var(--muted);
        text-transform: uppercase;
        letter-spacing: 0.04em;
        margin: 0 0 9px;
      }
      .context {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 8px;
        margin-top: 16px;
      }
      .context div {
        padding: 10px;
        border-left: 3px solid var(--b);
        border-radius: 8px;
        background: var(--sub);
      }
      .context span {
        display: block;
        color: var(--muted);
        font-size: 9px;
      }
      .context strong {
        font-size: 13px;
      }
      .register {
        display: grid;
        grid-template-columns: 170px 180px 1fr;
        gap: 12px;
        align-items: end;
        margin-top: 16px;
      }
      .type-choice {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 8px;
      }
      .confirmation {
        display: none;
        margin-top: 14px;
        padding: 12px;
        border-left: 4px solid var(--b);
        border-radius: 8px;
        background: var(--bs);
      }
      .confirmation.open {
        display: block;
      }
      .confirmation-grid {
        display: grid;
        grid-template-columns: repeat(5, 1fr);
        gap: 8px;
      }
      .confirmation span {
        display: block;
        color: var(--muted);
        font-size: 9px;
      }
      .actions {
        display: flex;
        justify-content: flex-end;
        gap: 8px;
        margin-top: 14px;
        padding-top: 14px;
        border-top: 1px solid var(--line);
      }
      .status {
        min-height: 210px;
        display: grid;
        place-items: center;
        text-align: center;
        padding: 24px;
        border: 1px dashed #bdd3e8;
        border-radius: 10px;
        color: var(--muted);
      }
      .status strong {
        display: block;
        color: var(--bd);
        font-size: 16px;
      }
      .status p {
        margin: 4px 0 0;
      }
      .recent {
        margin-top: 16px;
      }
      .panel-head {
        display: flex;
        align-items: start;
        justify-content: space-between;
        gap: 12px;
        margin-bottom: 12px;
      }
      .panel-head h3 {
        margin: 0;
      }
      .filters {
        display: grid;
        grid-template-columns: 2fr 110px 120px 150px 140px 100px auto;
        gap: 9px;
        align-items: end;
        padding: 12px;
        background: var(--sub);
        border: 1px solid var(--line);
        border-radius: 10px;
      }
      .table-wrap {
        overflow: auto;
        border: 1px solid var(--line);
        border-radius: 10px;
        margin-top: 12px;
      }
      table {
        width: 100%;
        border-collapse: collapse;
        white-space: nowrap;
      }
      th {
        padding: 10px 12px;
        background: var(--sub);
        color: var(--muted);
        font-size: 10px;
        text-align: left;
        text-transform: uppercase;
      }
      td {
        padding: 11px 12px;
        border-top: 1px solid var(--line);
      }
      tr[data-dmo-row] {
        cursor: pointer;
      }
      tr[data-dmo-row]:hover {
        background: var(--page);
      }
      tr.selected {
        background: var(--line);
      }
      .pill {
        display: inline-flex;
        padding: 4px 8px;
        border-radius: 99px;
        background: var(--bs);
        color: #315d88;
        font-size: 10px;
        font-weight: 800;
      }
      .pill.corrected {
        background: var(--warn-s);
        color: var(--warn);
      }
      .footer {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 10px;
        padding-top: 11px;
        color: var(--muted);
        font-size: 11px;
      }
      .pager {
        display: flex;
        align-items: center;
        gap: 7px;
      }
      .inline {
        display: none;
        margin-top: 14px;
      }
      .inline.open {
        display: block;
      }
      .correction-grid {
        display: grid;
        grid-template-columns: 120px 170px 180px 1fr;
        gap: 10px;
        align-items: end;
      }
      .notice {
        display: none;
        margin-top: 12px;
        padding: 11px;
        border-left: 4px solid var(--warn);
        border-radius: 8px;
        background: var(--warn-s);
        color: #77532f;
      }
      .notice.show {
        display: block;
      }
      .toast {
        position: fixed;
        z-index: 100;
        right: 22px;
        bottom: 22px;
        padding: 11px 15px;
        border-radius: 9px;
        background: #0f1d2a;
        color: #fff;
        opacity: 0;
        transform: translateY(50px);
        transition: 0.2s;
      }
      .toast.show {
        opacity: 1;
        transform: none;
      }
      .active-job {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-top: 16px;
        padding: 10px 12px;
        border: 1px solid var(--b);
        border-left-width: 4px;
        border-radius: 9px;
        background: var(--bs);
      }
      .active-job span {
        color: var(--muted);
        font-size: 10px;
        font-weight: 750;
      }
      .active-job strong {
        font-size: 16px;
        color: var(--bd);
      }
      .active-job small {
        margin-left: auto;
        color: var(--muted);
      }
      @media (max-width: 1000px) {
        .line-choice {
          grid-template-columns: repeat(3, minmax(0, 1fr));
        }
        .context,
        .confirmation-grid {
          grid-template-columns: repeat(3, 1fr);
        }
        .filters {
          grid-template-columns: repeat(3, 1fr);
        }
      }
      @media (max-width: 650px) {
        .active-job {
          align-items: flex-start;
          flex-direction: column;
          gap: 2px;
        }
        .active-job small {
          margin-left: 0;
        }
        .header {
          padding: 10px 14px;
        }
        .user {
          display: none;
        }
        .tabs {
          padding: 0 12px;
        }
        .main {
          padding: 16px 12px;
        }
        .line-choice {
          grid-template-columns: repeat(2, minmax(0, 1fr));
        }
        .register,
        .context,
        .confirmation-grid,
        .filters,
        .correction-grid {
          grid-template-columns: 1fr;
        }
        .table-wrap table {
          min-width: 900px;
        }
        .footer {
          align-items: flex-start;
          flex-direction: column;
        }
        .pager {
          align-self: flex-end;
        }
        .page-head {
          align-items: start;
        }
        .status {
          min-height: 140px;
        }
      }
    </style>
  </head>
  <body>
    <header class="header">
      <img class="logo" src="logo_recolored(1).png" alt="BA" />
      <div class="head">
        <h1>Reparação interna</h1>
        <p>Registo de intervenções CM e MF durante a produção</p>
      </div>
      <div class="user">
        <strong>João Silva</strong><span>Reparador de turno</span>
      </div>
    </header>
    <nav class="tabs">
      <button class="tab active" data-view="registo">Registo</button
      ><button class="tab" data-view="historico">Histórico</button>
    </nav>
    <main class="main">
      <section class="view active" id="registo">
        <div class="page-head">
          <div>
            <h2>Registar reparação</h2>
            <p class="muted">
              Escolha a produção; o contexto ativo é carregado automaticamente.
            </p>
          </div>
        </div>
        <div class="flow">
          <aside class="card pad">
            <p class="step-title">1 · Produção por linha</p>
            <div class="line-choice">
              <button class="btn line-card" data-line="B1" data-reference="5774T173" data-production="202603"><strong>B1</strong><small>5774T173</small></button>
              <button class="btn line-card" data-line="B2" data-reference="5810T188" data-production="202602"><strong>B2</strong><small>5810T188</small></button>
              <button class="btn line-card" data-line="B3" data-reference="9389T194" data-production="202601"><strong>B3</strong><small>9389T194</small></button>
              <button class="btn line-card" data-line="C1" data-reference="9121T173" data-production="202602"><strong>C1</strong><small>9121T173</small></button>
              <button class="btn line-card" data-line="C2" data-reference="—" data-production="—"><strong>C2</strong><small>Sem Job On ativo</small></button>
              <button class="btn line-card" data-line="C3" data-reference="5447T173" data-production="202601"><strong>C3</strong><small>5447T173</small></button>
            </div>
            <p class="muted" style="margin-top: 12px">
              As referências são obtidas dos Job On ativos.
            </p>
          </aside>
          <section class="card pad">
            <div id="initialState" class="status">
              <div>
                <strong>Selecione uma produção</strong>
                <p>Será mostrado o Job On ativo nesse contexto.</p>
              </div>
            </div>
            <div id="activeForm" hidden>
              <div class="panel-head">
                <div>
                  <p class="step-title">2 · Contexto automático</p>
                  <h3 id="activeContextTitle">Produção ativa na B1</h3>
                </div>
                <span class="pill">Atualizado agora</span>
              </div>
              <div class="context">
                <div><span>Produção</span><strong id="contextProduction">202603</strong></div>
                <div><span>Referência</span><strong id="contextReference">5774T173</strong></div>
                <div><span>Job On</span><strong id="contextJobOn">JO-202603-B1</strong></div>
                <div><span>Data</span><strong>18/08/2026</strong></div>
              </div>
              <div class="active-job">
                <span>REFERÊNCIA ATIVA DO JOB ON</span
                ><strong id="activeJobReference">5774T173</strong
                ><small id="activeJobMeta">Produção 202603 · B1</small>
              </div>
              <div class="register">
                <div>
                  <p class="step-title">3 · Tipo</p>
                  <div class="type-choice">
                    <button class="btn active" data-type="CM">CM</button
                    ><button class="btn" data-type="MF">MF</button>
                  </div>
                </div>
                <div class="field">
                  <label for="toolNumber">Número individual</label
                  ><input
                    id="toolNumber"
                    inputmode="numeric"
                    placeholder="Introduzir número"
                  />
                </div>
                <button class="btn" id="review" disabled>Rever registo</button>
              </div>
              <div class="notice" id="toolNotice">
                Número não encontrado no contexto selecionado. Confirme o tipo e
                o número.
              </div>
              <div class="confirmation" id="confirmation">
                <p class="step-title">Confirmar</p>
                <div class="confirmation-grid">
                  <div><span>Linha</span><strong>B1</strong></div>
                  <div><span>Produção</span><strong>202603</strong></div>
                  <div>
                    <span>Referência / lote</span><strong>5774T173 · 01</strong>
                  </div>
                  <div>
                    <span>Tipo</span><strong id="confirmType">CM</strong>
                  </div>
                  <div>
                    <span>N.º individual</span
                    ><strong id="confirmNumber">—</strong>
                  </div>
                </div>
                <div class="actions">
                  <button class="btn" id="cancelReview">Voltar</button
                  ><button class="btn" id="submitRepair">
                    Registar reparação
                  </button>
                </div>
              </div>
            </div>
          </section>
        </div>
        <section class="card pad recent">
          <div class="panel-head">
            <div>
              <h3>Últimos registos do turno</h3>
              <p class="muted">Confirmação rápida sem sair da página.</p>
            </div>
            <div class="field" style="width: 100px">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
          </div>
          <div class="table-wrap">
            <table data-dmo-list>
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Linha</th>
                  <th>Produção</th>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>Tipo</th>
                  <th>N.º individual</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="ri-1">
                  <td>18/08 · 14:32</td>
                  <td>B1</td>
                  <td>202603</td>
                  <td><strong>5774T173</strong></td>
                  <td>01</td>
                  <td>CM</td>
                  <td>34</td>
                  <td>João Silva</td>
                </tr>
                <tr data-dmo-row data-id="ri-2">
                  <td>18/08 · 14:18</td>
                  <td>B3</td>
                  <td>202601</td>
                  <td><strong>9389T194</strong></td>
                  <td>26</td>
                  <td>MF</td>
                  <td>12</td>
                  <td>João Silva</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 registos · Página 1 de 1</span>
            <div class="pager">
              <button class="btn icon" disabled aria-label="Página anterior">
                ‹</button
              ><span>1 / 1</span
              ><button class="btn icon" disabled aria-label="Página seguinte">
                ›
              </button>
            </div>
          </div>
        </section>
      </section>
      <section class="view" id="historico">
        <div class="page-head">
          <div>
            <h2>Histórico</h2>
            <p class="muted">
              Consulte os registos e selecione um para corrigir.
            </p>
          </div>
        </div>
        <section class="card pad">
          <div class="filters">
            <div class="field">
              <label>Referência, produção ou número</label
              ><input id="historySearch" placeholder="Pesquisar" />
            </div>
            <div class="field">
              <label>Linha</label
              ><select>
                <option>Todas</option>
                <option>B1</option>
                <option>B2</option>
                <option>B3</option>
                <option>C1</option>
                <option>C2</option>
                <option>C3</option>
              </select>
            </div>
            <div class="field">
              <label>Tipo</label
              ><select>
                <option>Todos</option>
                <option>CM</option>
                <option>MF</option>
              </select>
            </div>
            <div class="field"><label>Desde</label><input type="date" /></div>
            <div class="field"><label>Até</label><input type="date" /></div>
            <div class="field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="btn">Limpar</button>
          </div>
          <p class="muted" style="margin-top: 10px">
            Um clique seleciona; duplo clique abre o detalhe completo.
          </p>
          <div class="table-wrap">
            <table data-dmo-list id="historyList">
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Linha</th>
                  <th>Produção</th>
                  <th>Referência</th>
                  <th>Lote</th>
                  <th>Tipo</th>
                  <th>N.º individual</th>
                  <th>Operador</th>
                  <th>Estado</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="ri-1">
                  <td>18/08 · 14:32</td>
                  <td>B1</td>
                  <td>202603</td>
                  <td><strong>5774T173</strong></td>
                  <td>01</td>
                  <td>CM</td>
                  <td>34</td>
                  <td>João Silva</td>
                  <td>Atual</td>
                </tr>
                <tr data-dmo-row data-id="ri-2">
                  <td>18/08 · 14:18</td>
                  <td>B3</td>
                  <td>202601</td>
                  <td><strong>9389T194</strong></td>
                  <td>26</td>
                  <td>MF</td>
                  <td>12</td>
                  <td>João Silva</td>
                  <td><span class="pill corrected">Corrigido</span></td>
                </tr>
                <tr data-dmo-row data-id="ri-3">
                  <td>17/08 · 22:06</td>
                  <td>C1</td>
                  <td>202602</td>
                  <td><strong>9121T173</strong></td>
                  <td>11</td>
                  <td>CM</td>
                  <td>45</td>
                  <td>Ana Martins</td>
                  <td>Atual</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>3 registos · Página 1 de 1</span>
            <div class="pager">
              <button class="btn" id="correct" disabled>Corrigir registo</button>
              <button class="btn icon" disabled aria-label="Página anterior">
                ‹</button
              ><span>1 / 1</span
              ><button class="btn icon" disabled aria-label="Página seguinte">
                ›
              </button>
            </div>
          </div>
          <div class="card pad inline" id="correction">
            <div class="panel-head">
              <div>
                <h3>Corrigir registo selecionado</h3>
                <p class="muted">
                  O operador e a data/hora originais permanecem visíveis.
                </p>
              </div>
              <button class="btn icon" id="closeCorrection">×</button>
            </div>
            <div class="correction-grid">
              <div class="field">
                <label>Linha</label
                ><select>
                  <option>B3</option>
                  <option>B1</option>
                  <option>B2</option>
                  <option>C1</option>
                  <option>C2</option>
                  <option>C3</option>
                </select>
              </div>
              <div class="field">
                <label>Tipo</label
                ><select>
                  <option>MF</option>
                  <option>CM</option>
                </select>
              </div>
              <div class="field">
                <label>Número individual</label><input value="12" />
              </div>
              <div class="field">
                <label>Justificação da correção</label
                ><input placeholder="Indique o engano corrigido" />
              </div>
            </div>
            <div class="context">
              <div>
                <span>Operador original</span><strong>João Silva</strong>
              </div>
              <div>
                <span>Data/hora original</span><strong>18/08 · 14:18</strong>
              </div>
              <div><span>Produção original</span><strong>202601</strong></div>
              <div><span>Referência</span><strong>9389T194</strong></div>
              <div><span>Registo</span><strong>RI-000124</strong></div>
            </div>
            <div class="actions">
              <button class="btn" id="saveCorrection">Guardar correção</button>
            </div>
          </div>
        </section>
      </section>
    </main>
    <div class="toast" id="toast"></div>
    <script src="dmo-interactions.js"></script>
    <script>
      const $ = (s) => document.querySelector(s),
        $$ = (s) => [...document.querySelectorAll(s)],
        say = (t) => {
          const x = $("#toast");
          x.textContent = t;
          x.classList.add("show");
          setTimeout(() => x.classList.remove("show"), 2200);
        };
      $$(".tab").forEach(
        (t) =>
          (t.onclick = () => {
            $$(".tab,.view").forEach((x) => x.classList.remove("active"));
            t.classList.add("active");
            $("#" + t.dataset.view).classList.add("active");
          }),
      );
      $$("[data-line]").forEach(
        (b) =>
          (b.onclick = () => {
            $$("[data-line]").forEach((x) => x.classList.remove("active"));
            b.classList.add("active");
            const line = b.dataset.line;
            const reference = b.dataset.reference;
            const production = b.dataset.production;
            if (reference === "—") {
              $("#activeForm").hidden = true;
              $("#initialState").hidden = false;
              $("#initialState strong").textContent =
                "Sem Job On ativo na " + line;
              $("#initialState p").textContent =
                "Não é possível registar uma reparação sem contexto de produção.";
              return;
            }
            $("#initialState").hidden = true;
            $("#activeForm").hidden = false;
            $("#activeContextTitle").textContent =
              "Produção ativa na " + line;
            $("#contextProduction").textContent = production;
            $("#contextReference").textContent = reference;
            $("#contextJobOn").textContent =
              "JO-" + production + "-" + line;
            $("#activeJobReference").textContent = reference;
            $("#activeJobMeta").textContent =
              "Produção " + production + " · " + line;
            say("Contexto ativo carregado: " + reference);
          }),
      );
      $$("[data-type]").forEach(
        (b) =>
          (b.onclick = () => {
            $$("[data-type]").forEach((x) => x.classList.remove("active"));
            b.classList.add("active");
          }),
      );
      $("#toolNumber").oninput = () => {
        $("#review").disabled = !$("#toolNumber").value.trim();
        $("#toolNotice").classList.remove("show");
        $("#confirmation").classList.remove("open");
      };
      $("#review").onclick = () => {
        const n = $("#toolNumber").value.trim();
        if (n === "999") {
          $("#toolNotice").classList.add("show");
          return;
        }
        $("#confirmType").textContent = $("[data-type].active").dataset.type;
        $("#confirmNumber").textContent = n;
        $("#confirmation").classList.add("open");
      };
      $("#cancelReview").onclick = () =>
        $("#confirmation").classList.remove("open");
      $("#submitRepair").onclick = () => {
        $("#confirmation").classList.remove("open");
        $("#toolNumber").value = "";
        $("#review").disabled = true;
        $("#toolNumber").focus();
        say("Registo enviado; linha e tipo mantidos");
      };
      document.addEventListener("dmo:list-select", (e) => {
        if (e.target.id === "historyList") $("#correct").disabled = false;
      });
      document.addEventListener("dmo:list-open", (e) =>
        say("Abrir detalhe do registo " + e.detail.id),
      );
      $("#correct").onclick = () => $("#correction").classList.add("open");
      $("#closeCorrection").onclick = () =>
        $("#correction").classList.remove("open");
      $("#saveCorrection").onclick = () => {
        if (!$("#correction input[placeholder]").value.trim()) {
          say("Indique a justificação da correção");
          return;
        }
        $("#correction").classList.remove("open");
        say("Correção preparada; original preservado");
      };
    </script>
  </body>
</html>

```
## END FILE CONTENT

---

# FILE 036

## Source Path
`reparacao-v2.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Reparação — Portal DMO
EXTERNAL CSS (0): 
SCRIPT SRC (0): 
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 8
<footer>: 0
<form>: 0
UNIQUE IDS (9): areaMenu, menu, openBoquilhas, openMoldes, registo, changeArea, envios, historico, definicoes
DATA-* ATTRIBUTES (2): view, type
<button: 10
<input: 0
<select: 0
<textarea: 0
<table: 0
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html><html lang="pt-PT"><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>Reparação — Portal DMO</title><style>
:root{--b:#3c73a8;--bd:#193046;--page:#f6f9fc;--sub:#f1f6fa;--line:#d9e6f2;--text:#172d42;--muted:#64778a;--disabled:#cbd5df;--r:12px;--shadow:0 8px 24px rgba(25,48,70,.06)}*{box-sizing:border-box}body{margin:0;background:var(--page);color:var(--text);font:14px/1.45 Inter,"Segoe UI",sans-serif}button,input,select{font:inherit}.header{min-height:76px;display:flex;align-items:center;gap:13px;padding:10px 28px;background:#fff;border-bottom:1px solid var(--line)}.logo{width:44px;height:44px;object-fit:contain;border-radius:50%}.head h1{margin:0;font-size:18px}.head p,.muted{margin:3px 0 0;color:var(--muted);font-size:11px}.user{margin-left:auto;padding-left:18px;text-align:right;border-left:1px solid var(--line)}.user strong,.user span{display:block}.user span{font-size:11px;color:var(--muted)}.nav{height:52px;display:flex;align-items:center;gap:24px;padding:0 28px;background:#fff;border-bottom:1px solid var(--line)}.menu-wrap{position:relative}.menu-trigger,.tab,.btn{min-height:36px;padding:7px 12px;border:1px solid var(--b);border-radius:8px;background:var(--b);color:#fff;font-weight:750;cursor:pointer}.menu-trigger:hover,.btn:hover,.btn:focus-visible{background:#fff;color:var(--b)}.tab{height:52px;border:0;border-bottom:3px solid transparent;border-radius:0;background:none;color:var(--muted)}.tab.active{color:var(--b);border-color:var(--b)}.settings{margin-left:auto}.menu{display:none;position:absolute;z-index:20;top:43px;left:0;min-width:210px;padding:6px;border:1px solid var(--line);border-radius:10px;background:#f7fafc;box-shadow:0 10px 24px rgba(15,29,42,.18)}.menu.open{display:block}.menu button{width:100%;padding:9px 10px;border:0;border-radius:7px;background:transparent;color:var(--text);text-align:left;cursor:pointer}.menu button:hover{background:#d9e6f2}.menu small{display:block;margin-top:2px;color:var(--muted)}main{max-width:1320px;margin:auto;padding:26px}.page-head{display:flex;align-items:end;justify-content:space-between;gap:12px;margin-bottom:16px}.page-head h2{margin:0}.card{padding:18px;background:#fff;border:1px solid var(--line);border-radius:var(--r);box-shadow:var(--shadow)}.context{display:flex;align-items:center;justify-content:space-between;gap:12px;padding:14px 16px;border-left:4px solid var(--b);border-radius:9px;background:var(--sub)}.context strong{font-size:16px}.type-select{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-top:16px}.type{padding:18px;border:1px solid var(--line);border-radius:10px;background:#fff}.type h3{margin:0}.type p{min-height:38px;color:var(--muted);font-size:11px}.type .btn{min-width:130px}.view{display:none}.view.active{display:block}.empty{display:grid;min-height:260px;place-items:center;text-align:center;border:1px dashed #bdd3e8;border-radius:10px;color:var(--muted)}.notice{margin-top:16px;padding:12px;border-left:4px solid #a97943;border-radius:8px;background:#f7f0e7;color:#77532f}@media(max-width:650px){.header{padding:10px 14px}.user{display:none}.nav{padding:0 12px;gap:10px;overflow:visible}.tab{padding:7px 6px}.settings{margin-left:0}main{padding:16px 12px}.type-select{grid-template-columns:1fr}.context{align-items:flex-start;flex-direction:column}}
</style></head><body><header class="header"><img class="logo" src="logo_recolored(1).png" alt="BA"><div class="head"><h1>Reparação</h1><p>Boquilhas e Moldes</p></div><div class="user"><strong>Ana Martins</strong><span>Responsável de reparação</span></div></header><nav class="nav"><div class="menu-wrap"><button class="menu-trigger" id="areaMenu">Menu · Moldes ▾</button><div class="menu" id="menu"><button id="openBoquilhas"><strong>Boquilhas</strong><small>Abre o módulo BQ existente</small></button><button id="openMoldes"><strong>Moldes</strong><small>Contra moldes e Moldes finais</small></button></div></div><button class="tab active" data-view="registo">Registo</button><button class="tab" data-view="envios">Envios</button><button class="tab" data-view="historico">Histórico</button><button class="tab settings" data-view="definicoes">Definições</button></nav><main>
<section class="view active" id="registo"><div class="page-head"><div><h2>Moldes</h2><p class="muted">Escolha o tipo de ferramenta sem combinar os respetivos registos.</p></div></div><section class="card"><div class="context"><div><span class="muted">ÁREA ATIVA</span><br><strong>Moldes</strong></div><button class="btn" id="changeArea">Mudar área</button></div><div class="type-select"><article class="type"><h3>Contra moldes</h3><p>Ferramentas CM individuais, lotes, reparadores e ciclo externo próprios.</p><button class="btn" data-type="CM">Abrir Contra moldes</button></article><article class="type"><h3>Moldes finais</h3><p>Ferramentas MF individuais, mantidas separadas dos Contra moldes.</p><button class="btn" data-type="MF">Abrir Moldes finais</button></article></div><div class="notice">Boquilhas não é recriada nesta página. A opção do menu abre diretamente <strong>boquilhas.html</strong>, preservando o design e o comportamento já aprovados.</div></section></section>
<section class="view" id="envios"><div class="page-head"><div><h2>Envios de Moldes</h2><p class="muted">Saídas programadas e retornos de CM ou MF.</p></div></div><section class="card"><div class="empty"><div><strong>Selecione Contra moldes ou Moldes finais</strong><p>Os envios são filtrados pelo tipo escolhido no Registo.</p></div></div></section></section>
<section class="view" id="historico"><div class="page-head"><div><h2>Histórico de Moldes</h2><p class="muted">Consulta dos ciclos externos de CM e MF.</p></div></div><section class="card"><div class="empty"><div><strong>Histórico separado por tipo</strong><p>Um clique seleciona; duplo clique abre o detalhe.</p></div></div></section></section>
<section class="view" id="definicoes"><div class="page-head"><div><h2>Definições de Moldes</h2><p class="muted">Reparadores permitidos por CM/MF e máquina.</p></div></div><section class="card"><div class="empty"><div><strong>Associações de reparadores</strong><p>Alterações não modificam envios históricos.</p></div></div></section></section>
</main><script>
const $=s=>document.querySelector(s),$$=s=>[...document.querySelectorAll(s)];$('#areaMenu').onclick=e=>{e.stopPropagation();$('#menu').classList.toggle('open')};$('#changeArea').onclick=()=>$('#menu').classList.add('open');document.onclick=()=>$('#menu').classList.remove('open');$('#menu').onclick=e=>e.stopPropagation();$('#openBoquilhas').onclick=()=>location.href='boquilhas.html';$('#openMoldes').onclick=()=>$('#menu').classList.remove('open');$$('.tab').forEach(t=>t.onclick=()=>{$$('.tab,.view').forEach(x=>x.classList.remove('active'));t.classList.add('active');$('#'+t.dataset.view).classList.add('active')});$$('[data-type]').forEach(b=>b.onclick=()=>alert('Abrir área '+b.dataset.type+' — implementação usa o domínio existente desse tipo.'));
</script></body></html>

```
## END FILE CONTENT

---

# FILE 037

## Source Path
`tampoes.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Tampões — Portal DMO
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (1): dmo-interactions.js
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 13
<footer>: 0
<form>: 0
UNIQUE IDS (33): registo, findConfig, filledBalance, emptyBalance, quantityForm, quantityTitle, balanceType, quantity, saveQuantity, stateForm, targetState, stateQuantity, statePreview, saveState, transformForm, saveTransform, tp-1, tp-2, consulta, cfg-1, cfg-2, planSelected, planeamento, plan-1, historico, historyList, correctMovement, opcoes, value-1, value-2, value-3, value-4, toast
DATA-* ATTRIBUTES (3): view, action, id
<button: 31
<input: 15
<select: 18
<textarea: 0
<table: 6
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Tampões — Portal DMO</title>
    <link rel="stylesheet" href="dmo-design-system.css" />
    <style>
      :root{--dmo-brand-950:#0f1d2a;--dmo-brand-700:#315d88;--dmo-brand-600:#3c73a8;--dmo-brand-200:#bdd3e8;--dmo-brand-100:#d9e6f2;--dmo-brand-050:#e8eff7;--dmo-page:#f6f9fc;--dmo-card:#fff;--dmo-subtle:#f1f6fa;--dmo-border:#d9e6f2;--dmo-text:#172d42;--dmo-muted:#64778a;--dmo-success:#527c72;--dmo-success-soft:#e5f0eb;--dmo-warning:#a97943;--dmo-warning-soft:#f7f0e7;--dmo-disabled:#cbd5df;--dmo-r-control:8px;--dmo-r-card:12px;--dmo-shadow:0 8px 24px rgba(25,48,70,.06)}
      *{box-sizing:border-box}body{margin:0;background:var(--dmo-page);color:var(--dmo-text);font:14px/1.45 Inter,"Segoe UI",sans-serif}button,input,select,textarea{font:inherit}
      .dmo-button{min-height:36px;padding:7px 12px;border:1px solid var(--dmo-brand-600);border-radius:var(--dmo-r-control);background:var(--dmo-brand-600);color:#fff;font-weight:700;cursor:pointer;transition:.15s ease}.dmo-button:hover,.dmo-button:focus-visible{background:#fff;color:var(--dmo-brand-600);outline:none}.dmo-button:disabled{border-color:var(--dmo-disabled);background:var(--dmo-disabled);color:#fff;cursor:not-allowed}.dmo-icon-button{width:36px;padding:0;display:grid;place-items:center}
      .dmo-field label{display:block;margin-bottom:6px;color:var(--dmo-muted);font-size:11px;font-weight:750}.dmo-field input,.dmo-field select,.dmo-field textarea{width:100%;min-height:40px;padding:9px 11px;border:1px solid var(--dmo-border);border-radius:var(--dmo-r-control);background:#fff;color:var(--dmo-text);outline:none}.dmo-field input:focus,.dmo-field select:focus,.dmo-field textarea:focus{border-color:var(--dmo-brand-600);box-shadow:0 0 0 3px rgba(60,115,168,.13)}
      .dmo-card{background:#fff;border:1px solid var(--dmo-border);border-radius:var(--dmo-r-card);box-shadow:var(--dmo-shadow)}.dmo-table-wrap{overflow:auto;border:1px solid var(--dmo-border);border-radius:10px}.dmo-table{width:100%;border-collapse:collapse;white-space:nowrap}.dmo-table th{padding:11px 12px;background:var(--dmo-subtle);color:var(--dmo-muted);font-size:10px;text-align:left;text-transform:uppercase}.dmo-table td{padding:11px 12px;border-top:1px solid var(--dmo-border)}.dmo-table tr[data-dmo-row]{cursor:pointer}.dmo-table tr[data-dmo-row]:hover{background:var(--dmo-page)}.dmo-table tr.selected{background:var(--dmo-brand-100)}
      .dmo-app-header{min-height:76px;display:flex;align-items:center;gap:13px;padding:10px 28px;background:#fff;border-bottom:1px solid var(--dmo-border)}.dmo-app-header__logo{width:44px;height:44px;object-fit:contain;border-radius:50%;flex:0 0 auto}.dmo-app-header__page{min-width:0}.dmo-app-header__page h1{margin:0;font-size:18px;line-height:1.2}.dmo-app-header__page p{margin:3px 0 0;color:var(--dmo-muted);font-size:11px}.dmo-app-header__user{margin-left:auto;min-width:150px;padding-left:18px;text-align:right;border-left:1px solid var(--dmo-border)}.dmo-app-header__user strong,.dmo-app-header__user span{display:block}.dmo-app-header__user strong{font-size:12px}.dmo-app-header__user span{margin-top:2px;color:var(--dmo-muted);font-size:11px}
      .dmo-toast{position:fixed;right:22px;bottom:22px;z-index:100;padding:11px 15px;border-radius:9px;background:var(--dmo-brand-950);color:#fff;opacity:0;transform:translateY(50px);transition:.2s}.dmo-toast.show{opacity:1;transform:none}
      .tabs {
        height: 52px;
        display: flex;
        gap: 26px;
        padding: 0 28px;
        background: #fff;
        border-bottom: 1px solid var(--dmo-border);
      }
      .tab {
        border: 0;
        border-bottom: 3px solid transparent;
        background: none;
        color: var(--dmo-muted);
        font-weight: 750;
        cursor: pointer;
      }
      .tab.active {
        color: var(--dmo-brand-600);
        border-color: var(--dmo-brand-600);
      }
      .tab.options {
        margin-left: auto;
      }
      main {
        max-width: 1320px;
        margin: auto;
        padding: 26px;
      }
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }
      .head {
        display: flex;
        align-items: end;
        justify-content: space-between;
        gap: 12px;
        margin-bottom: 16px;
      }
      .head h2,
      .panel h3 {
        margin: 0;
      }
      .muted {
        margin: 4px 0 0;
        color: var(--dmo-muted);
        font-size: 11px;
      }
      .panel {
        padding: 18px;
      }
      .filters {
        display: grid;
        grid-template-columns: 180px 180px 110px auto;
        gap: 10px;
        align-items: end;
      }
      .summary {
        display: grid;
        grid-template-columns: minmax(220px, 1fr) repeat(2, 180px);
        gap: 12px;
        margin-top: 16px;
      }
      .summary > div {
        padding: 14px;
        border-left: 4px solid var(--dmo-brand-600);
        border-radius: 9px;
        background: var(--dmo-subtle);
      }
      .summary span {
        display: block;
        color: var(--dmo-muted);
        font-size: 10px;
      }
      .summary strong {
        display: block;
        margin-top: 3px;
        font-size: 24px;
      }
      .summary .configuration strong {
        font-size: 17px;
      }
      .actions {
        display: flex;
        justify-content: flex-end;
        gap: 8px;
        margin-top: 14px;
      }
      .inline {
        display: none;
        margin-top: 16px;
      }
      .inline.open {
        display: block;
      }
      .form-grid {
        display: grid;
        grid-template-columns: minmax(260px, 1fr) 180px 130px;
        gap: 10px;
        align-items: end;
      }
      .transform {
        grid-template-columns: 1fr 110px 160px 160px;
      }
      .notice {
        margin-top: 12px;
        padding: 11px;
        border-left: 4px solid var(--dmo-warning);
        border-radius: 8px;
        background: var(--dmo-warning-soft);
        color: #77532f;
      }
      .table-card {
        margin-top: 16px;
        padding: 18px;
      }
      .table-head,
      .footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
      }
      .table-head h3 {
        margin: 0;
      }
      .footer {
        padding-top: 11px;
        color: var(--dmo-muted);
        font-size: 11px;
      }
      .pager {
        display: flex;
        align-items: center;
        gap: 7px;
      }
      .history-filters {
        display: grid;
        grid-template-columns: 2fr repeat(4, 150px) 100px auto;
        gap: 9px;
        align-items: end;
      }
      .state {
        display: inline-flex;
        padding: 4px 8px;
        border-radius: 99px;
        background: var(--dmo-brand-050);
        color: var(--dmo-brand-700);
        font-size: 10px;
        font-weight: 800;
      }
      .state.plan {
        background: var(--dmo-warning-soft);
        color: var(--dmo-warning);
      }
      .options-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 16px;
      }
      .field-row {
        display: grid;
        grid-template-columns: 1fr 100px 90px 90px;
        gap: 9px;
        align-items: end;
        margin-top: 12px;
      }
      .empty {
        padding: 32px;
        text-align: center;
        border: 1px dashed var(--dmo-brand-200);
        border-radius: 10px;
        color: var(--dmo-muted);
      }
      @media (max-width: 900px) {
        .summary {
          grid-template-columns: 1fr 1fr;
        }
        .configuration {
          grid-column: 1/-1;
        }
        .history-filters {
          grid-template-columns: repeat(3, 1fr);
        }
        .options-grid {
          grid-template-columns: 1fr;
        }
      }
      @media (max-width: 620px) {
        .dmo-app-header__page p {
          display: none;
        }
        .tabs {
          padding: 0 12px;
          gap: 14px;
          overflow: auto;
        }
        .tab {
          white-space: nowrap;
        }
        .tab.options {
          margin-left: 0;
        }
        main {
          padding: 16px 12px;
        }
        .head {
          align-items: start;
        }
        .filters,
        .form-grid,
        .transform,
        .history-filters,
        .field-row {
          grid-template-columns: 1fr 1fr;
        }
        .filters .dmo-button,
        .history-filters .dmo-button {
          grid-column: 1/-1;
        }
        .summary {
          grid-template-columns: 1fr 1fr;
        }
        .summary > div {
          padding: 12px;
        }
        .summary strong {
          font-size: 22px;
        }
        .actions {
          display: grid;
          grid-template-columns: 1fr 1fr;
        }
        .actions .dmo-button {
          min-height: 44px;
        }
        .dmo-table {
          min-width: 860px;
        }
        .table-head {
          align-items: flex-start;
        }
        .footer {
          align-items: flex-start;
          flex-direction: column;
        }
        .pager {
          align-self: flex-end;
        }
      }
    </style>
  </head>
  <body>
    <header class="dmo-app-header">
      <img class="dmo-app-header__logo" src="logo_recolored(1).png" alt="BA" />
      <div class="dmo-app-header__page">
        <h1>Tampões</h1>
        <p>Quantidades, configurações técnicas e planeamento</p>
      </div>
      <div class="dmo-app-header__user">
        <strong>João Silva</strong><span>Operador</span>
      </div>
    </header>
    <nav class="tabs">
      <button class="tab active" data-view="registo">Registo</button
      ><button class="tab" data-view="consulta">Consulta</button
      ><button class="tab" data-view="planeamento">Planeamento</button
      ><button class="tab" data-view="historico">Histórico</button
      ><button class="tab options" data-view="opcoes">Opções</button>
    </nav>
    <main>
      <section class="view active" id="registo">
        <div class="head">
          <div>
            <h2>Registar quantidades</h2>
            <p class="muted">
              Selecione uma configuração e altere apenas o saldo necessário.
            </p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="filters">
            <div class="dmo-field">
              <label>Diâmetro (mm)</label><select>
                <option>28,95</option>
                <option>30,00</option>
                <option>32,00</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Calote (mm)</label><select>
                <option>4,00</option>
                <option>7,00</option>
                <option>10,00</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="dmo-button" id="findConfig">Pesquisar</button>
          </div>
          <div class="summary">
            <div class="configuration">
              <span>CONFIGURAÇÃO SELECIONADA</span
              ><strong>Ø 28,95 mm · Calote 4,00 mm</strong>
            </div>
            <div>
              <span>ENCHIDOS</span><strong id="filledBalance">28</strong>
            </div>
            <div>
              <span>POR ENCHER</span><strong id="emptyBalance">5</strong>
            </div>
          </div>
          <div class="actions">
            <button class="dmo-button" data-action="add">Adicionar</button
            ><button class="dmo-button" data-action="remove">Remover</button
            ><button class="dmo-button" data-action="state">
              Alterar estado
            </button
            ><button class="dmo-button" data-action="transform">
              Alterar configuração
            </button>
          </div>
          <div class="dmo-card panel inline" id="quantityForm">
            <div class="head">
              <div>
                <h3 id="quantityTitle">Adicionar quantidade</h3>
                <p class="muted">
                  O novo saldo só é apresentado depois da confirmação.
                </p>
              </div>
              <button class="dmo-button dmo-icon-button" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="dmo-field">
                <label>Configuração</label
                ><input value="Ø 28,95 mm · Calote 4,00 mm" readonly />
              </div>
              <div class="dmo-field">
                <label>Saldo</label
                ><select id="balanceType">
                  <option>Enchidos</option>
                  <option>Por encher</option>
                </select>
              </div>
              <div class="dmo-field">
                <label>Quantidade</label
                ><input id="quantity" type="number" min="1" value="1" />
              </div>
            </div>
            <div class="actions">
              <button class="dmo-button" id="saveQuantity">
                Confirmar movimento
              </button>
            </div>
          </div>
          <div class="dmo-card panel inline" id="stateForm">
            <div class="head">
              <div><h3>Alterar estado</h3><p class="muted">Transfere a quantidade entre Por encher e Enchidos.</p></div>
              <button class="dmo-button dmo-icon-button" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="dmo-field"><label>Configuração</label><input value="Ø 28,95 mm · Calote 4,00 mm" readonly /></div>
              <div class="dmo-field"><label>Novo estado</label><select id="targetState"><option>Enchidos</option><option>Por encher</option></select></div>
              <div class="dmo-field"><label>Quantidade</label><input id="stateQuantity" type="number" min="1" value="1" /></div>
            </div>
            <div class="notice" id="statePreview">Serão retirados de Por encher e adicionados a Enchidos.</div>
            <div class="actions"><button class="dmo-button" id="saveState">Confirmar alteração</button></div>
          </div>
          <div class="dmo-card panel inline" id="transformForm">
            <div class="head">
              <div>
                <h3>Alterar configuração</h3>
                <p class="muted">
                  Transfere uma quantidade; não edita a configuração de origem.
                </p>
              </div>
              <button class="dmo-button dmo-icon-button" data-close>×</button>
            </div>
            <div class="form-grid transform">
              <div class="dmo-field">
                <label>Origem</label
                ><input value="Ø 28,95 mm · Calote 4,00 mm" readonly />
              </div>
              <div class="dmo-field">
                <label>Quantidade</label
                ><input type="number" min="1" value="25" />
              </div>
              <div class="dmo-field">
                <label>Novo diâmetro</label><select><option>28,95</option><option>30,00</option><option>32,00</option></select>
              </div>
              <div class="dmo-field">
                <label>Nova calote</label><select><option>7,00</option><option>4,00</option><option>10,00</option></select>
              </div>
            </div>
            <div class="notice">
              Destino previsto: Ø 28,95 mm · Calote 7,00 mm. A origem e o
              destino são atualizados na mesma operação.
            </div>
            <div class="actions">
              <button class="dmo-button" id="saveTransform">
                Confirmar transformação
              </button>
            </div>
          </div>
        </section>
        <section class="dmo-card table-card">
          <div class="table-head">
            <div>
              <h3>Últimos movimentos</h3>
              <p class="muted">
                Um clique seleciona; duplo clique abre o detalhe.
              </p>
            </div>
          </div>
          <div class="dmo-table-wrap">
            <table class="dmo-table" data-dmo-list>
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Configuração</th>
                  <th>Movimento</th>
                  <th>Saldo</th>
                  <th>Quantidade</th>
                  <th>Antes</th>
                  <th>Depois</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="tp-1">
                  <td>18/08 · 14:40</td>
                  <td>Ø 28,95 · 4,00 mm</td>
                  <td>Adicionar</td>
                  <td>Enchidos</td>
                  <td>8</td>
                  <td>20</td>
                  <td>28</td>
                  <td>João Silva</td>
                </tr>
                <tr data-dmo-row data-id="tp-2">
                  <td>18/08 · 10:12</td>
                  <td>Ø 28,95 · 4,00 → 7,00 mm</td>
                  <td>Alterar configuração</td>
                  <td>Enchidos</td>
                  <td>25</td>
                  <td>45</td>
                  <td>20</td>
                  <td>Ana Martins</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 movimentos · Página 1 de 1</span>
            <div class="pager">
              <button class="dmo-button dmo-icon-button" disabled>‹</button
              ><span>1 / 1</span
              ><button class="dmo-button dmo-icon-button" disabled>›</button>
            </div>
          </div>
        </section>
      </section>

      <section class="view" id="consulta">
        <div class="head">
          <div>
            <h2>Consulta</h2>
            <p class="muted">Disponibilidade atual por configuração técnica.</p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="filters">
            <div class="dmo-field">
              <label>Diâmetro (mm)</label><select><option>Todos</option><option>28,95</option><option>30,00</option><option>32,00</option></select>
            </div>
            <div class="dmo-field">
              <label>Calote (mm)</label><select><option>Todas</option><option>4,00</option><option>7,00</option><option>10,00</option></select>
            </div>
            <div class="dmo-field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="dmo-button">Limpar filtros</button>
          </div>
          <div class="dmo-table-wrap" style="margin-top: 14px">
            <table class="dmo-table" data-dmo-list>
              <thead>
                <tr>
                  <th>Diâmetro</th>
                  <th>Calote</th>
                  <th>Enchidos</th>
                  <th>Por encher</th>
                  <th>Total</th>
                  <th>Último movimento</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="cfg-1">
                  <td><strong>28,95 mm</strong></td>
                  <td>4,00 mm</td>
                  <td>28</td>
                  <td>5</td>
                  <td>33</td>
                  <td>18/08 · 14:40</td>
                </tr>
                <tr data-dmo-row data-id="cfg-2">
                  <td><strong>28,95 mm</strong></td>
                  <td>7,00 mm</td>
                  <td>25</td>
                  <td>0</td>
                  <td>25</td>
                  <td>18/08 · 10:12</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 configurações · Página 1 de 1</span>
            <div class="pager">
              <button class="dmo-button" id="planSelected" disabled>
                Planear</button
              ><button class="dmo-button dmo-icon-button" disabled>‹</button
              ><span>1 / 1</span
              ><button class="dmo-button dmo-icon-button" disabled>›</button>
            </div>
          </div>
        </section>
      </section>

      <section class="view" id="planeamento">
        <div class="head">
          <div>
            <h2>Planeamento</h2>
            <p class="muted">
              Necessidades previstas; não reserva nem altera quantidades.
            </p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="form-grid">
            <div class="dmo-field">
              <label>Configuração</label
              ><select>
                <option>Ø 28,95 mm · Calote 4,00 mm</option>
                <option>Ø 28,95 mm · Calote 7,00 mm</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Quantidade necessária</label
              ><input type="number" value="40" />
            </div>
            <div class="dmo-field">
              <label>Data prevista</label
              ><input type="date" value="2026-08-22" />
            </div>
          </div>
          <div class="summary">
            <div class="configuration">
              <span>NECESSIDADE</span><strong>40 tampões</strong>
            </div>
            <div><span>ENCHIDOS DISPONÍVEIS</span><strong>28</strong></div>
            <div><span>EM FALTA</span><strong>12</strong></div>
          </div>
          <div class="actions">
            <button class="dmo-button">Guardar planeamento</button>
          </div>
        </section>
        <section class="dmo-card table-card">
          <div class="table-head"><h3>Planos ativos</h3></div>
          <div class="dmo-table-wrap">
            <table class="dmo-table" data-dmo-list>
              <thead>
                <tr>
                  <th>Data prevista</th>
                  <th>Configuração</th>
                  <th>Necessário</th>
                  <th>Disponível</th>
                  <th>Diferença</th>
                  <th>Estado</th>
                  <th>Autor</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="plan-1">
                  <td>22/08/2026</td>
                  <td>Ø 28,95 · 4,00 mm</td>
                  <td>40</td>
                  <td>28</td>
                  <td>-12</td>
                  <td><span class="state plan">Aberto</span></td>
                  <td>João Silva</td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>
      </section>

      <section class="view" id="historico">
        <div class="head">
          <div>
            <h2>Histórico</h2>
            <p class="muted">Movimentos físicos e transformações auditáveis.</p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="history-filters">
            <div class="dmo-field">
              <label>Configuração ou operador</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="dmo-field">
              <label>Movimento</label
              ><select>
                <option>Todos</option>
                <option>Adicionar</option>
                <option>Remover</option>
                <option>Alterar estado</option>
                <option>Alterar configuração</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Saldo</label
              ><select>
                <option>Todos</option>
                <option>Enchidos</option>
                <option>Por encher</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Desde</label><input type="date" />
            </div>
            <div class="dmo-field"><label>Até</label><input type="date" /></div>
            <div class="dmo-field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="dmo-button">Limpar</button>
          </div>
          <div class="dmo-table-wrap" style="margin-top: 14px">
            <table class="dmo-table" data-dmo-list id="historyList">
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Origem</th>
                  <th>Destino</th>
                  <th>Movimento</th>
                  <th>Saldo</th>
                  <th>Qtd.</th>
                  <th>Antes</th>
                  <th>Depois</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="tp-1">
                  <td>18/08 · 14:40</td>
                  <td>Ø 28,95 · 4,00</td>
                  <td>—</td>
                  <td>Adicionar</td>
                  <td>Enchidos</td>
                  <td>8</td>
                  <td>20</td>
                  <td>28</td>
                  <td>João Silva</td>
                </tr>
                <tr data-dmo-row data-id="tp-2">
                  <td>18/08 · 10:12</td>
                  <td>Ø 28,95 · 4,00</td>
                  <td>Ø 28,95 · 7,00</td>
                  <td>Alterar configuração</td>
                  <td>Enchidos</td>
                  <td>25</td>
                  <td>45</td>
                  <td>20</td>
                  <td>Ana Martins</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 movimentos · Página 1 de 1</span>
            <div class="pager">
              <button class="dmo-button" id="correctMovement" disabled>
                Corrigir movimento</button
              ><button class="dmo-button dmo-icon-button" disabled>‹</button
              ><span>1 / 1</span
              ><button class="dmo-button dmo-icon-button" disabled>›</button>
            </div>
          </div>
        </section>
      </section>

      <section class="view" id="opcoes">
        <div class="head">
          <div>
            <h2>Opções</h2>
            <p class="muted">
              Campos numéricos usados para descrever e comparar configurações.
            </p>
          </div>
        </div>
        <div class="options-grid">
          <section class="dmo-card panel">
            <div class="table-head">
              <h3>Campos comparáveis</h3>
              <button class="dmo-button">Adicionar campo</button>
            </div>
            <div class="field-row">
              <div class="dmo-field">
                <label>Nome</label><input value="Diâmetro" />
              </div>
              <div class="dmo-field">
                <label>Unidade</label><input value="mm" />
              </div>
              <div class="dmo-field">
                <label>Decimais</label
                ><select>
                  <option>2</option>
                </select>
              </div>
              <div class="dmo-field">
                <label>Estado</label
                ><select>
                  <option>Ativo</option>
                </select>
              </div>
            </div>
            <div class="field-row">
              <div class="dmo-field">
                <label>Nome</label><input value="Calote" />
              </div>
              <div class="dmo-field">
                <label>Unidade</label><input value="mm" />
              </div>
              <div class="dmo-field">
                <label>Decimais</label
                ><select>
                  <option>2</option>
                </select>
              </div>
              <div class="dmo-field">
                <label>Estado</label
                ><select>
                  <option>Ativo</option>
                </select>
              </div>
            </div>
            <div class="actions">
              <button class="dmo-button">Guardar campos</button>
            </div>
            <div class="table-head" style="margin-top: 22px">
              <div><h3>Valores disponíveis</h3><p class="muted">Estes valores alimentam os dropdowns do módulo.</p></div>
              <button class="dmo-button">Adicionar valor</button>
            </div>
            <div class="dmo-table-wrap" style="margin-top: 12px">
              <table class="dmo-table" data-dmo-list>
                <thead><tr><th>Campo</th><th>Valor</th><th>Unidade</th><th>Estado</th></tr></thead>
                <tbody>
                  <tr data-dmo-row data-id="value-1"><td>Diâmetro</td><td>28,95</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                  <tr data-dmo-row data-id="value-2"><td>Diâmetro</td><td>30,00</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                  <tr data-dmo-row data-id="value-3"><td>Calote</td><td>4,00</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                  <tr data-dmo-row data-id="value-4"><td>Calote</td><td>7,00</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                </tbody>
              </table>
            </div>
          </section>
          <section class="dmo-card panel">
            <div class="table-head">
              <h3>Configurações</h3>
              <button class="dmo-button">Nova configuração</button>
            </div>
            <p class="muted">
              As configurações usam os campos ativos. Desativar não elimina
              movimentos anteriores.
            </p>
            <div class="dmo-table-wrap" style="margin-top: 14px">
              <table class="dmo-table" data-dmo-list>
                <thead>
                  <tr>
                    <th>Diâmetro</th>
                    <th>Calote</th>
                    <th>Estado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr data-dmo-row data-id="cfg-1">
                    <td>28,95 mm</td>
                    <td>4,00 mm</td>
                    <td><span class="state">Ativa</span></td>
                  </tr>
                  <tr data-dmo-row data-id="cfg-2">
                    <td>28,95 mm</td>
                    <td>7,00 mm</td>
                    <td><span class="state">Ativa</span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
        </div>
      </section>
    </main>
    <div class="dmo-toast" id="toast"></div>
    <script src="dmo-interactions.js"></script>
    <script>
      const $ = (s) => document.querySelector(s),
        $$ = (s) => [...document.querySelectorAll(s)];
      const say = (t) => {
        const x = $("#toast");
        x.textContent = t;
        x.classList.add("show");
        setTimeout(() => x.classList.remove("show"), 2200);
      };
      $$(".tab").forEach(
        (t) =>
          (t.onclick = () => {
            $$(".tab,.view").forEach((x) => x.classList.remove("active"));
            t.classList.add("active");
            $("#" + t.dataset.view).classList.add("active");
          }),
      );
      $$("[data-action]").forEach(
        (b) =>
          (b.onclick = () => {
            $$(".inline").forEach((x) => x.classList.remove("open"));
            if (b.dataset.action === "transform")
              $("#transformForm").classList.add("open");
            else if (b.dataset.action === "state")
              $("#stateForm").classList.add("open");
            else {
              $("#quantityTitle").textContent =
                (b.dataset.action === "add" ? "Adicionar" : "Remover") +
                " quantidade";
              $("#quantityForm").dataset.mode = b.dataset.action;
              $("#quantityForm").classList.add("open");
            }
          }),
      );
      $$("[data-close]").forEach(
        (b) =>
          (b.onclick = () => b.closest(".inline").classList.remove("open")),
      );
      $("#saveQuantity").onclick = () => {
        const q = Math.max(1, Number($("#quantity").value) || 1),
          id =
            $("#balanceType").value === "Enchidos"
              ? "#filledBalance"
              : "#emptyBalance",
          sign = $("#quantityForm").dataset.mode === "remove" ? -1 : 1,
          next = Number($(id).textContent) + sign * q;
        if (next < 0) {
          say("Quantidade superior ao saldo disponível");
          return;
        }
        $(id).textContent = next;
        $("#quantityForm").classList.remove("open");
        say("Movimento preparado");
      };
      $("#saveTransform").onclick = () => {
        $("#transformForm").classList.remove("open");
        say("Transformação preparada");
      };
      $("#targetState").onchange = () => {
        const target = $("#targetState").value;
        $("#statePreview").textContent =
          target === "Enchidos"
            ? "Serão retirados de Por encher e adicionados a Enchidos."
            : "Serão retirados de Enchidos e adicionados a Por encher.";
      };
      $("#saveState").onclick = () => {
        const quantity = Math.max(1, Number($("#stateQuantity").value) || 1);
        const toFilled = $("#targetState").value === "Enchidos";
        const source = toFilled ? $("#emptyBalance") : $("#filledBalance");
        const target = toFilled ? $("#filledBalance") : $("#emptyBalance");
        if (Number(source.textContent) < quantity) {
          say("Quantidade superior ao saldo de origem");
          return;
        }
        source.textContent = Number(source.textContent) - quantity;
        target.textContent = Number(target.textContent) + quantity;
        $("#stateForm").classList.remove("open");
        say("Alteração de estado preparada");
      };
      document.addEventListener("dmo:list-select", (e) => {
        if (e.target.id === "historyList")
          $("#correctMovement").disabled = false;
        if (e.target.closest && e.target.closest("#consulta"))
          $("#planSelected").disabled = false;
      });
      document.addEventListener("dmo:list-open", (e) =>
        say("Abrir detalhe " + e.detail.id),
      );
      $("#correctMovement").onclick = () => say("Abrir correção auditável");
    </script>
  </body>
</html>

```
## END FILE CONTENT

---

# FILE 038

## Source Path
`tampoes-v38-standalone.html`

## File Type
`.HTML`

### HTML STRUCTURE EXTRACTION
```
PAGE TITLE: Tampões — Portal DMO
EXTERNAL CSS (1): dmo-design-system.css
SCRIPT SRC (1): dmo-interactions.js
<header>: 1
<nav>: 1
<aside>: 0
<main>: 1
<section>: 13
<footer>: 0
<form>: 0
UNIQUE IDS (33): registo, findConfig, filledBalance, emptyBalance, quantityForm, quantityTitle, balanceType, quantity, saveQuantity, stateForm, targetState, stateQuantity, statePreview, saveState, transformForm, saveTransform, tp-1, tp-2, consulta, cfg-1, cfg-2, planSelected, planeamento, plan-1, historico, historyList, correctMovement, opcoes, value-1, value-2, value-3, value-4, toast
DATA-* ATTRIBUTES (3): view, action, id
<button: 31
<input: 15
<select: 18
<textarea: 0
<table: 6
<a: 0
MODAL/DIALOG refs: 
CALENDAR refs: 
```

### ORIGINAL HTML

## BEGIN FILE CONTENT
```html
<!doctype html>
<html lang="pt-PT">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Tampões — Portal DMO</title>
    <link rel="stylesheet" href="dmo-design-system.css" />
    <style>
      :root{--dmo-brand-950:#0f1d2a;--dmo-brand-700:#315d88;--dmo-brand-600:#3c73a8;--dmo-brand-200:#bdd3e8;--dmo-brand-100:#d9e6f2;--dmo-brand-050:#e8eff7;--dmo-page:#f6f9fc;--dmo-card:#fff;--dmo-subtle:#f1f6fa;--dmo-border:#d9e6f2;--dmo-text:#172d42;--dmo-muted:#64778a;--dmo-success:#527c72;--dmo-success-soft:#e5f0eb;--dmo-warning:#a97943;--dmo-warning-soft:#f7f0e7;--dmo-disabled:#cbd5df;--dmo-r-control:8px;--dmo-r-card:12px;--dmo-shadow:0 8px 24px rgba(25,48,70,.06)}
      *{box-sizing:border-box}body{margin:0;background:var(--dmo-page);color:var(--dmo-text);font:14px/1.45 Inter,"Segoe UI",sans-serif}button,input,select,textarea{font:inherit}
      .dmo-button{min-height:36px;padding:7px 12px;border:1px solid var(--dmo-brand-600);border-radius:var(--dmo-r-control);background:var(--dmo-brand-600);color:#fff;font-weight:700;cursor:pointer;transition:.15s ease}.dmo-button:hover,.dmo-button:focus-visible{background:#fff;color:var(--dmo-brand-600);outline:none}.dmo-button:disabled{border-color:var(--dmo-disabled);background:var(--dmo-disabled);color:#fff;cursor:not-allowed}.dmo-icon-button{width:36px;padding:0;display:grid;place-items:center}
      .dmo-field label{display:block;margin-bottom:6px;color:var(--dmo-muted);font-size:11px;font-weight:750}.dmo-field input,.dmo-field select,.dmo-field textarea{width:100%;min-height:40px;padding:9px 11px;border:1px solid var(--dmo-border);border-radius:var(--dmo-r-control);background:#fff;color:var(--dmo-text);outline:none}.dmo-field input:focus,.dmo-field select:focus,.dmo-field textarea:focus{border-color:var(--dmo-brand-600);box-shadow:0 0 0 3px rgba(60,115,168,.13)}
      .dmo-card{background:#fff;border:1px solid var(--dmo-border);border-radius:var(--dmo-r-card);box-shadow:var(--dmo-shadow)}.dmo-table-wrap{overflow:auto;border:1px solid var(--dmo-border);border-radius:10px}.dmo-table{width:100%;border-collapse:collapse;white-space:nowrap}.dmo-table th{padding:11px 12px;background:var(--dmo-subtle);color:var(--dmo-muted);font-size:10px;text-align:left;text-transform:uppercase}.dmo-table td{padding:11px 12px;border-top:1px solid var(--dmo-border)}.dmo-table tr[data-dmo-row]{cursor:pointer}.dmo-table tr[data-dmo-row]:hover{background:var(--dmo-page)}.dmo-table tr.selected{background:var(--dmo-brand-100)}
      .dmo-app-header{min-height:76px;display:flex;align-items:center;gap:13px;padding:10px 28px;background:#fff;border-bottom:1px solid var(--dmo-border)}.dmo-app-header__logo{width:44px;height:44px;object-fit:contain;border-radius:50%;flex:0 0 auto}.dmo-app-header__page{min-width:0}.dmo-app-header__page h1{margin:0;font-size:18px;line-height:1.2}.dmo-app-header__page p{margin:3px 0 0;color:var(--dmo-muted);font-size:11px}.dmo-app-header__user{margin-left:auto;min-width:150px;padding-left:18px;text-align:right;border-left:1px solid var(--dmo-border)}.dmo-app-header__user strong,.dmo-app-header__user span{display:block}.dmo-app-header__user strong{font-size:12px}.dmo-app-header__user span{margin-top:2px;color:var(--dmo-muted);font-size:11px}
      .dmo-toast{position:fixed;right:22px;bottom:22px;z-index:100;padding:11px 15px;border-radius:9px;background:var(--dmo-brand-950);color:#fff;opacity:0;transform:translateY(50px);transition:.2s}.dmo-toast.show{opacity:1;transform:none}
      .tabs {
        height: 52px;
        display: flex;
        gap: 26px;
        padding: 0 28px;
        background: #fff;
        border-bottom: 1px solid var(--dmo-border);
      }
      .tab {
        border: 0;
        border-bottom: 3px solid transparent;
        background: none;
        color: var(--dmo-muted);
        font-weight: 750;
        cursor: pointer;
      }
      .tab.active {
        color: var(--dmo-brand-600);
        border-color: var(--dmo-brand-600);
      }
      .tab.options {
        margin-left: auto;
      }
      main {
        max-width: 1320px;
        margin: auto;
        padding: 26px;
      }
      .view {
        display: none;
      }
      .view.active {
        display: block;
      }
      .head {
        display: flex;
        align-items: end;
        justify-content: space-between;
        gap: 12px;
        margin-bottom: 16px;
      }
      .head h2,
      .panel h3 {
        margin: 0;
      }
      .muted {
        margin: 4px 0 0;
        color: var(--dmo-muted);
        font-size: 11px;
      }
      .panel {
        padding: 18px;
      }
      .filters {
        display: grid;
        grid-template-columns: 180px 180px 110px auto;
        gap: 10px;
        align-items: end;
      }
      .summary {
        display: grid;
        grid-template-columns: minmax(220px, 1fr) repeat(2, 180px);
        gap: 12px;
        margin-top: 16px;
      }
      .summary > div {
        padding: 14px;
        border-left: 4px solid var(--dmo-brand-600);
        border-radius: 9px;
        background: var(--dmo-subtle);
      }
      .summary span {
        display: block;
        color: var(--dmo-muted);
        font-size: 10px;
      }
      .summary strong {
        display: block;
        margin-top: 3px;
        font-size: 24px;
      }
      .summary .configuration strong {
        font-size: 17px;
      }
      .actions {
        display: flex;
        justify-content: flex-end;
        gap: 8px;
        margin-top: 14px;
      }
      .inline {
        display: none;
        margin-top: 16px;
      }
      .inline.open {
        display: block;
      }
      .form-grid {
        display: grid;
        grid-template-columns: minmax(260px, 1fr) 180px 130px;
        gap: 10px;
        align-items: end;
      }
      .transform {
        grid-template-columns: 1fr 110px 160px 160px;
      }
      .notice {
        margin-top: 12px;
        padding: 11px;
        border-left: 4px solid var(--dmo-warning);
        border-radius: 8px;
        background: var(--dmo-warning-soft);
        color: #77532f;
      }
      .table-card {
        margin-top: 16px;
        padding: 18px;
      }
      .table-head,
      .footer {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 10px;
      }
      .table-head h3 {
        margin: 0;
      }
      .footer {
        padding-top: 11px;
        color: var(--dmo-muted);
        font-size: 11px;
      }
      .pager {
        display: flex;
        align-items: center;
        gap: 7px;
      }
      .history-filters {
        display: grid;
        grid-template-columns: 2fr repeat(4, 150px) 100px auto;
        gap: 9px;
        align-items: end;
      }
      .state {
        display: inline-flex;
        padding: 4px 8px;
        border-radius: 99px;
        background: var(--dmo-brand-050);
        color: var(--dmo-brand-700);
        font-size: 10px;
        font-weight: 800;
      }
      .state.plan {
        background: var(--dmo-warning-soft);
        color: var(--dmo-warning);
      }
      .options-grid {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 16px;
      }
      .field-row {
        display: grid;
        grid-template-columns: 1fr 100px 90px 90px;
        gap: 9px;
        align-items: end;
        margin-top: 12px;
      }
      .empty {
        padding: 32px;
        text-align: center;
        border: 1px dashed var(--dmo-brand-200);
        border-radius: 10px;
        color: var(--dmo-muted);
      }
      @media (max-width: 900px) {
        .summary {
          grid-template-columns: 1fr 1fr;
        }
        .configuration {
          grid-column: 1/-1;
        }
        .history-filters {
          grid-template-columns: repeat(3, 1fr);
        }
        .options-grid {
          grid-template-columns: 1fr;
        }
      }
      @media (max-width: 620px) {
        .dmo-app-header__page p {
          display: none;
        }
        .tabs {
          padding: 0 12px;
          gap: 14px;
          overflow: auto;
        }
        .tab {
          white-space: nowrap;
        }
        .tab.options {
          margin-left: 0;
        }
        main {
          padding: 16px 12px;
        }
        .head {
          align-items: start;
        }
        .filters,
        .form-grid,
        .transform,
        .history-filters,
        .field-row {
          grid-template-columns: 1fr 1fr;
        }
        .filters .dmo-button,
        .history-filters .dmo-button {
          grid-column: 1/-1;
        }
        .summary {
          grid-template-columns: 1fr 1fr;
        }
        .summary > div {
          padding: 12px;
        }
        .summary strong {
          font-size: 22px;
        }
        .actions {
          display: grid;
          grid-template-columns: 1fr 1fr;
        }
        .actions .dmo-button {
          min-height: 44px;
        }
        .dmo-table {
          min-width: 860px;
        }
        .table-head {
          align-items: flex-start;
        }
        .footer {
          align-items: flex-start;
          flex-direction: column;
        }
        .pager {
          align-self: flex-end;
        }
      }
    </style>
  </head>
  <body>
    <header class="dmo-app-header">
      <img class="dmo-app-header__logo" src="logo_recolored(1).png" alt="BA" />
      <div class="dmo-app-header__page">
        <h1>Tampões</h1>
        <p>Quantidades, configurações técnicas e planeamento</p>
      </div>
      <div class="dmo-app-header__user">
        <strong>João Silva</strong><span>Operador</span>
      </div>
    </header>
    <nav class="tabs">
      <button class="tab active" data-view="registo">Registo</button
      ><button class="tab" data-view="consulta">Consulta</button
      ><button class="tab" data-view="planeamento">Planeamento</button
      ><button class="tab" data-view="historico">Histórico</button
      ><button class="tab options" data-view="opcoes">Opções</button>
    </nav>
    <main>
      <section class="view active" id="registo">
        <div class="head">
          <div>
            <h2>Registar quantidades</h2>
            <p class="muted">
              Selecione uma configuração e altere apenas o saldo necessário.
            </p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="filters">
            <div class="dmo-field">
              <label>Diâmetro (mm)</label><select>
                <option>28,95</option>
                <option>30,00</option>
                <option>32,00</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Calote (mm)</label><select>
                <option>4,00</option>
                <option>7,00</option>
                <option>10,00</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="dmo-button" id="findConfig">Pesquisar</button>
          </div>
          <div class="summary">
            <div class="configuration">
              <span>CONFIGURAÇÃO SELECIONADA</span
              ><strong>Ø 28,95 mm · Calote 4,00 mm</strong>
            </div>
            <div>
              <span>ENCHIDOS</span><strong id="filledBalance">28</strong>
            </div>
            <div>
              <span>POR ENCHER</span><strong id="emptyBalance">5</strong>
            </div>
          </div>
          <div class="actions">
            <button class="dmo-button" data-action="add">Adicionar</button
            ><button class="dmo-button" data-action="remove">Remover</button
            ><button class="dmo-button" data-action="state">
              Alterar estado
            </button
            ><button class="dmo-button" data-action="transform">
              Alterar configuração
            </button>
          </div>
          <div class="dmo-card panel inline" id="quantityForm">
            <div class="head">
              <div>
                <h3 id="quantityTitle">Adicionar quantidade</h3>
                <p class="muted">
                  O novo saldo só é apresentado depois da confirmação.
                </p>
              </div>
              <button class="dmo-button dmo-icon-button" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="dmo-field">
                <label>Configuração</label
                ><input value="Ø 28,95 mm · Calote 4,00 mm" readonly />
              </div>
              <div class="dmo-field">
                <label>Saldo</label
                ><select id="balanceType">
                  <option>Enchidos</option>
                  <option>Por encher</option>
                </select>
              </div>
              <div class="dmo-field">
                <label>Quantidade</label
                ><input id="quantity" type="number" min="1" value="1" />
              </div>
            </div>
            <div class="actions">
              <button class="dmo-button" id="saveQuantity">
                Confirmar movimento
              </button>
            </div>
          </div>
          <div class="dmo-card panel inline" id="stateForm">
            <div class="head">
              <div><h3>Alterar estado</h3><p class="muted">Transfere a quantidade entre Por encher e Enchidos.</p></div>
              <button class="dmo-button dmo-icon-button" data-close>×</button>
            </div>
            <div class="form-grid">
              <div class="dmo-field"><label>Configuração</label><input value="Ø 28,95 mm · Calote 4,00 mm" readonly /></div>
              <div class="dmo-field"><label>Novo estado</label><select id="targetState"><option>Enchidos</option><option>Por encher</option></select></div>
              <div class="dmo-field"><label>Quantidade</label><input id="stateQuantity" type="number" min="1" value="1" /></div>
            </div>
            <div class="notice" id="statePreview">Serão retirados de Por encher e adicionados a Enchidos.</div>
            <div class="actions"><button class="dmo-button" id="saveState">Confirmar alteração</button></div>
          </div>
          <div class="dmo-card panel inline" id="transformForm">
            <div class="head">
              <div>
                <h3>Alterar configuração</h3>
                <p class="muted">
                  Transfere uma quantidade; não edita a configuração de origem.
                </p>
              </div>
              <button class="dmo-button dmo-icon-button" data-close>×</button>
            </div>
            <div class="form-grid transform">
              <div class="dmo-field">
                <label>Origem</label
                ><input value="Ø 28,95 mm · Calote 4,00 mm" readonly />
              </div>
              <div class="dmo-field">
                <label>Quantidade</label
                ><input type="number" min="1" value="25" />
              </div>
              <div class="dmo-field">
                <label>Novo diâmetro</label><select><option>28,95</option><option>30,00</option><option>32,00</option></select>
              </div>
              <div class="dmo-field">
                <label>Nova calote</label><select><option>7,00</option><option>4,00</option><option>10,00</option></select>
              </div>
            </div>
            <div class="notice">
              Destino previsto: Ø 28,95 mm · Calote 7,00 mm. A origem e o
              destino são atualizados na mesma operação.
            </div>
            <div class="actions">
              <button class="dmo-button" id="saveTransform">
                Confirmar transformação
              </button>
            </div>
          </div>
        </section>
        <section class="dmo-card table-card">
          <div class="table-head">
            <div>
              <h3>Últimos movimentos</h3>
              <p class="muted">
                Um clique seleciona; duplo clique abre o detalhe.
              </p>
            </div>
          </div>
          <div class="dmo-table-wrap">
            <table class="dmo-table" data-dmo-list>
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Configuração</th>
                  <th>Movimento</th>
                  <th>Saldo</th>
                  <th>Quantidade</th>
                  <th>Antes</th>
                  <th>Depois</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="tp-1">
                  <td>18/08 · 14:40</td>
                  <td>Ø 28,95 · 4,00 mm</td>
                  <td>Adicionar</td>
                  <td>Enchidos</td>
                  <td>8</td>
                  <td>20</td>
                  <td>28</td>
                  <td>João Silva</td>
                </tr>
                <tr data-dmo-row data-id="tp-2">
                  <td>18/08 · 10:12</td>
                  <td>Ø 28,95 · 4,00 → 7,00 mm</td>
                  <td>Alterar configuração</td>
                  <td>Enchidos</td>
                  <td>25</td>
                  <td>45</td>
                  <td>20</td>
                  <td>Ana Martins</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 movimentos · Página 1 de 1</span>
            <div class="pager">
              <button class="dmo-button dmo-icon-button" disabled>‹</button
              ><span>1 / 1</span
              ><button class="dmo-button dmo-icon-button" disabled>›</button>
            </div>
          </div>
        </section>
      </section>

      <section class="view" id="consulta">
        <div class="head">
          <div>
            <h2>Consulta</h2>
            <p class="muted">Disponibilidade atual por configuração técnica.</p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="filters">
            <div class="dmo-field">
              <label>Diâmetro (mm)</label><select><option>Todos</option><option>28,95</option><option>30,00</option><option>32,00</option></select>
            </div>
            <div class="dmo-field">
              <label>Calote (mm)</label><select><option>Todas</option><option>4,00</option><option>7,00</option><option>10,00</option></select>
            </div>
            <div class="dmo-field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="dmo-button">Limpar filtros</button>
          </div>
          <div class="dmo-table-wrap" style="margin-top: 14px">
            <table class="dmo-table" data-dmo-list>
              <thead>
                <tr>
                  <th>Diâmetro</th>
                  <th>Calote</th>
                  <th>Enchidos</th>
                  <th>Por encher</th>
                  <th>Total</th>
                  <th>Último movimento</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="cfg-1">
                  <td><strong>28,95 mm</strong></td>
                  <td>4,00 mm</td>
                  <td>28</td>
                  <td>5</td>
                  <td>33</td>
                  <td>18/08 · 14:40</td>
                </tr>
                <tr data-dmo-row data-id="cfg-2">
                  <td><strong>28,95 mm</strong></td>
                  <td>7,00 mm</td>
                  <td>25</td>
                  <td>0</td>
                  <td>25</td>
                  <td>18/08 · 10:12</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 configurações · Página 1 de 1</span>
            <div class="pager">
              <button class="dmo-button" id="planSelected" disabled>
                Planear</button
              ><button class="dmo-button dmo-icon-button" disabled>‹</button
              ><span>1 / 1</span
              ><button class="dmo-button dmo-icon-button" disabled>›</button>
            </div>
          </div>
        </section>
      </section>

      <section class="view" id="planeamento">
        <div class="head">
          <div>
            <h2>Planeamento</h2>
            <p class="muted">
              Necessidades previstas; não reserva nem altera quantidades.
            </p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="form-grid">
            <div class="dmo-field">
              <label>Configuração</label
              ><select>
                <option>Ø 28,95 mm · Calote 4,00 mm</option>
                <option>Ø 28,95 mm · Calote 7,00 mm</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Quantidade necessária</label
              ><input type="number" value="40" />
            </div>
            <div class="dmo-field">
              <label>Data prevista</label
              ><input type="date" value="2026-08-22" />
            </div>
          </div>
          <div class="summary">
            <div class="configuration">
              <span>NECESSIDADE</span><strong>40 tampões</strong>
            </div>
            <div><span>ENCHIDOS DISPONÍVEIS</span><strong>28</strong></div>
            <div><span>EM FALTA</span><strong>12</strong></div>
          </div>
          <div class="actions">
            <button class="dmo-button">Guardar planeamento</button>
          </div>
        </section>
        <section class="dmo-card table-card">
          <div class="table-head"><h3>Planos ativos</h3></div>
          <div class="dmo-table-wrap">
            <table class="dmo-table" data-dmo-list>
              <thead>
                <tr>
                  <th>Data prevista</th>
                  <th>Configuração</th>
                  <th>Necessário</th>
                  <th>Disponível</th>
                  <th>Diferença</th>
                  <th>Estado</th>
                  <th>Autor</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="plan-1">
                  <td>22/08/2026</td>
                  <td>Ø 28,95 · 4,00 mm</td>
                  <td>40</td>
                  <td>28</td>
                  <td>-12</td>
                  <td><span class="state plan">Aberto</span></td>
                  <td>João Silva</td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>
      </section>

      <section class="view" id="historico">
        <div class="head">
          <div>
            <h2>Histórico</h2>
            <p class="muted">Movimentos físicos e transformações auditáveis.</p>
          </div>
        </div>
        <section class="dmo-card panel">
          <div class="history-filters">
            <div class="dmo-field">
              <label>Configuração ou operador</label
              ><input placeholder="Pesquisar" />
            </div>
            <div class="dmo-field">
              <label>Movimento</label
              ><select>
                <option>Todos</option>
                <option>Adicionar</option>
                <option>Remover</option>
                <option>Alterar estado</option>
                <option>Alterar configuração</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Saldo</label
              ><select>
                <option>Todos</option>
                <option>Enchidos</option>
                <option>Por encher</option>
              </select>
            </div>
            <div class="dmo-field">
              <label>Desde</label><input type="date" />
            </div>
            <div class="dmo-field"><label>Até</label><input type="date" /></div>
            <div class="dmo-field">
              <label>Linhas</label
              ><select>
                <option>20</option>
                <option>40</option>
                <option>60</option>
              </select>
            </div>
            <button class="dmo-button">Limpar</button>
          </div>
          <div class="dmo-table-wrap" style="margin-top: 14px">
            <table class="dmo-table" data-dmo-list id="historyList">
              <thead>
                <tr>
                  <th>Data/hora</th>
                  <th>Origem</th>
                  <th>Destino</th>
                  <th>Movimento</th>
                  <th>Saldo</th>
                  <th>Qtd.</th>
                  <th>Antes</th>
                  <th>Depois</th>
                  <th>Operador</th>
                </tr>
              </thead>
              <tbody>
                <tr data-dmo-row data-id="tp-1">
                  <td>18/08 · 14:40</td>
                  <td>Ø 28,95 · 4,00</td>
                  <td>—</td>
                  <td>Adicionar</td>
                  <td>Enchidos</td>
                  <td>8</td>
                  <td>20</td>
                  <td>28</td>
                  <td>João Silva</td>
                </tr>
                <tr data-dmo-row data-id="tp-2">
                  <td>18/08 · 10:12</td>
                  <td>Ø 28,95 · 4,00</td>
                  <td>Ø 28,95 · 7,00</td>
                  <td>Alterar configuração</td>
                  <td>Enchidos</td>
                  <td>25</td>
                  <td>45</td>
                  <td>20</td>
                  <td>Ana Martins</td>
                </tr>
              </tbody>
            </table>
          </div>
          <div class="footer">
            <span>2 movimentos · Página 1 de 1</span>
            <div class="pager">
              <button class="dmo-button" id="correctMovement" disabled>
                Corrigir movimento</button
              ><button class="dmo-button dmo-icon-button" disabled>‹</button
              ><span>1 / 1</span
              ><button class="dmo-button dmo-icon-button" disabled>›</button>
            </div>
          </div>
        </section>
      </section>

      <section class="view" id="opcoes">
        <div class="head">
          <div>
            <h2>Opções</h2>
            <p class="muted">
              Campos numéricos usados para descrever e comparar configurações.
            </p>
          </div>
        </div>
        <div class="options-grid">
          <section class="dmo-card panel">
            <div class="table-head">
              <h3>Campos comparáveis</h3>
              <button class="dmo-button">Adicionar campo</button>
            </div>
            <div class="field-row">
              <div class="dmo-field">
                <label>Nome</label><input value="Diâmetro" />
              </div>
              <div class="dmo-field">
                <label>Unidade</label><input value="mm" />
              </div>
              <div class="dmo-field">
                <label>Decimais</label
                ><select>
                  <option>2</option>
                </select>
              </div>
              <div class="dmo-field">
                <label>Estado</label
                ><select>
                  <option>Ativo</option>
                </select>
              </div>
            </div>
            <div class="field-row">
              <div class="dmo-field">
                <label>Nome</label><input value="Calote" />
              </div>
              <div class="dmo-field">
                <label>Unidade</label><input value="mm" />
              </div>
              <div class="dmo-field">
                <label>Decimais</label
                ><select>
                  <option>2</option>
                </select>
              </div>
              <div class="dmo-field">
                <label>Estado</label
                ><select>
                  <option>Ativo</option>
                </select>
              </div>
            </div>
            <div class="actions">
              <button class="dmo-button">Guardar campos</button>
            </div>
            <div class="table-head" style="margin-top: 22px">
              <div><h3>Valores disponíveis</h3><p class="muted">Estes valores alimentam os dropdowns do módulo.</p></div>
              <button class="dmo-button">Adicionar valor</button>
            </div>
            <div class="dmo-table-wrap" style="margin-top: 12px">
              <table class="dmo-table" data-dmo-list>
                <thead><tr><th>Campo</th><th>Valor</th><th>Unidade</th><th>Estado</th></tr></thead>
                <tbody>
                  <tr data-dmo-row data-id="value-1"><td>Diâmetro</td><td>28,95</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                  <tr data-dmo-row data-id="value-2"><td>Diâmetro</td><td>30,00</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                  <tr data-dmo-row data-id="value-3"><td>Calote</td><td>4,00</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                  <tr data-dmo-row data-id="value-4"><td>Calote</td><td>7,00</td><td>mm</td><td><span class="state">Ativo</span></td></tr>
                </tbody>
              </table>
            </div>
          </section>
          <section class="dmo-card panel">
            <div class="table-head">
              <h3>Configurações</h3>
              <button class="dmo-button">Nova configuração</button>
            </div>
            <p class="muted">
              As configurações usam os campos ativos. Desativar não elimina
              movimentos anteriores.
            </p>
            <div class="dmo-table-wrap" style="margin-top: 14px">
              <table class="dmo-table" data-dmo-list>
                <thead>
                  <tr>
                    <th>Diâmetro</th>
                    <th>Calote</th>
                    <th>Estado</th>
                  </tr>
                </thead>
                <tbody>
                  <tr data-dmo-row data-id="cfg-1">
                    <td>28,95 mm</td>
                    <td>4,00 mm</td>
                    <td><span class="state">Ativa</span></td>
                  </tr>
                  <tr data-dmo-row data-id="cfg-2">
                    <td>28,95 mm</td>
                    <td>7,00 mm</td>
                    <td><span class="state">Ativa</span></td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
        </div>
      </section>
    </main>
    <div class="dmo-toast" id="toast"></div>
    <script src="dmo-interactions.js"></script>
    <script>
      const $ = (s) => document.querySelector(s),
        $$ = (s) => [...document.querySelectorAll(s)];
      const say = (t) => {
        const x = $("#toast");
        x.textContent = t;
        x.classList.add("show");
        setTimeout(() => x.classList.remove("show"), 2200);
      };
      $$(".tab").forEach(
        (t) =>
          (t.onclick = () => {
            $$(".tab,.view").forEach((x) => x.classList.remove("active"));
            t.classList.add("active");
            $("#" + t.dataset.view).classList.add("active");
          }),
      );
      $$("[data-action]").forEach(
        (b) =>
          (b.onclick = () => {
            $$(".inline").forEach((x) => x.classList.remove("open"));
            if (b.dataset.action === "transform")
              $("#transformForm").classList.add("open");
            else if (b.dataset.action === "state")
              $("#stateForm").classList.add("open");
            else {
              $("#quantityTitle").textContent =
                (b.dataset.action === "add" ? "Adicionar" : "Remover") +
                " quantidade";
              $("#quantityForm").dataset.mode = b.dataset.action;
              $("#quantityForm").classList.add("open");
            }
          }),
      );
      $$("[data-close]").forEach(
        (b) =>
          (b.onclick = () => b.closest(".inline").classList.remove("open")),
      );
      $("#saveQuantity").onclick = () => {
        const q = Math.max(1, Number($("#quantity").value) || 1),
          id =
            $("#balanceType").value === "Enchidos"
              ? "#filledBalance"
              : "#emptyBalance",
          sign = $("#quantityForm").dataset.mode === "remove" ? -1 : 1,
          next = Number($(id).textContent) + sign * q;
        if (next < 0) {
          say("Quantidade superior ao saldo disponível");
          return;
        }
        $(id).textContent = next;
        $("#quantityForm").classList.remove("open");
        say("Movimento preparado");
      };
      $("#saveTransform").onclick = () => {
        $("#transformForm").classList.remove("open");
        say("Transformação preparada");
      };
      $("#targetState").onchange = () => {
        const target = $("#targetState").value;
        $("#statePreview").textContent =
          target === "Enchidos"
            ? "Serão retirados de Por encher e adicionados a Enchidos."
            : "Serão retirados de Enchidos e adicionados a Por encher.";
      };
      $("#saveState").onclick = () => {
        const quantity = Math.max(1, Number($("#stateQuantity").value) || 1);
        const toFilled = $("#targetState").value === "Enchidos";
        const source = toFilled ? $("#emptyBalance") : $("#filledBalance");
        const target = toFilled ? $("#filledBalance") : $("#emptyBalance");
        if (Number(source.textContent) < quantity) {
          say("Quantidade superior ao saldo de origem");
          return;
        }
        source.textContent = Number(source.textContent) - quantity;
        target.textContent = Number(target.textContent) + quantity;
        $("#stateForm").classList.remove("open");
        say("Alteração de estado preparada");
      };
      document.addEventListener("dmo:list-select", (e) => {
        if (e.target.id === "historyList")
          $("#correctMovement").disabled = false;
        if (e.target.closest && e.target.closest("#consulta"))
          $("#planSelected").disabled = false;
      });
      document.addEventListener("dmo:list-open", (e) =>
        say("Abrir detalhe " + e.detail.id),
      );
      $("#correctMovement").onclick = () => say("Abrir correção auditável");
    </script>
  </body>
</html>

```
## END FILE CONTENT

---

# FILE 039

## Source Path
`dmo-design-system.css`

## File Type
`.CSS`

### CSS CONTRACT EXTRACTION
```
CSS VARIABLES/TOKENS (0):
MEDIA QUERIES (2): @media(max-width:720px) | @media(max-width:600px)
SELECTORS (80): :root ; * ; body ; button ; input ; select ; textarea ; .dmo-button ; .dmo-button:hover ; .dmo-button:focus-visible ; .dmo-button.danger ; .dmo-button.success ; .dmo-button:disabled ; .dmo-icon-button ; .dmo-field label ; .dmo-field input ; .dmo-field select ; .dmo-field textarea ; .dmo-field input:focus ; .dmo-field select:focus ; .dmo-field textarea:focus ; .dmo-card ; .dmo-pill ; .dmo-pill.active ; .dmo-pill.inactive ; .dmo-table-wrap ; .dmo-table ; .dmo-table th ; .dmo-table td ; .dmo-table tr[data-row] ; .dmo-table tr[data-row]:hover ; .dmo-table tr.selected ; .dmo-modal-backdrop ; .dmo-modal-backdrop.open ; .dmo-modal ; .dmo-modal-head ; .dmo-modal-foot ; .dmo-modal-head h2 ; .dmo-modal-head .dmo-icon-button ; .dmo-modal-body ; .dmo-toast ; .dmo-toast.show ; /* Canonical control sizing. Context wins over the base button size. */
.filters > .btn ; .filters > .dmo-button ; .search > .btn ; .search > .dmo-button ; .history-filters > .btn ; .history-filters > .dmo-button ; .dmo-filter-row > .btn ; .dmo-filter-row > .dmo-button ; .pager > .btn.icon ; .pager > .dmo-icon-button ; .dmo-pagination > .btn.icon ; .dmo-pagination > .dmo-icon-button ; /* Canonical selectable lists */
[data-dmo-list] [data-dmo-row] ; [data-dmo-list] [data-dmo-row]:hover ; [data-dmo-list] [data-dmo-row]:focus-visible ; [data-dmo-list] [data-dmo-row].selected ; /* Canonical calendar */
.dmo-calendar__head ; .dmo-calendar__week ; .dmo-calendar__grid ; .dmo-calendar__day ; .dmo-calendar__day:hover ; .dmo-calendar__day.has-record::after ; .dmo-calendar__day.selected ; .dmo-calendar__day.selected::after ; .dmo-calendar__day:disabled ; /* Canonical record states: shared by weight controls and comparisons */
.dmo-pill.pending ; .dmo-pill.approved ; .dmo-pill.rejected ; .dmo-pill.record-type ; /* Canonical application header */
.dmo-app-header ; .dmo-app-header__logo ; .dmo-app-header__page ; .dmo-app-header__page h1 ; .dmo-app-header__page p ; .dmo-app-header__user ; .dmo-app-header__user strong ; .dmo-app-header__user span ; .dmo-app-header
```

### ORIGINAL CSS

## BEGIN FILE CONTENT
```css
:root{--dmo-brand-950:#0f1d2a;--dmo-brand-900:#193046;--dmo-brand-800:#234463;--dmo-brand-700:#315d88;--dmo-brand-600:#3c73a8;--dmo-brand-500:#568dc3;--dmo-brand-200:#bdd3e8;--dmo-brand-100:#d9e6f2;--dmo-brand-050:#e8eff7;--dmo-page:#f6f9fc;--dmo-card:#fff;--dmo-subtle:#f1f6fa;--dmo-border:#d9e6f2;--dmo-text:#172d42;--dmo-muted:#64778a;--dmo-success:#527c72;--dmo-success-soft:#e5f0eb;--dmo-warning:#a97943;--dmo-warning-soft:#f7f0e7;--dmo-danger:#9a625d;--dmo-danger-soft:#f3e9e7;--dmo-disabled:#cbd5df;--dmo-r-control:8px;--dmo-r-card:12px;--dmo-r-modal:16px;--dmo-shadow:0 8px 24px rgba(25,48,70,.06);--dmo-fast:150ms ease}
:root{--dmo-brand-300:#98b9da;--dmo-surface-page:var(--dmo-page);--dmo-surface-card:var(--dmo-card);--dmo-surface-subtle:var(--dmo-subtle);--dmo-text-muted:var(--dmo-muted);--dmo-text-on-color:#fff;--dmo-radius-control:var(--dmo-r-control);--dmo-radius-card:var(--dmo-r-card);--dmo-radius-modal:var(--dmo-r-modal);--dmo-radius-pill:999px;--dmo-control-height:40px;--dmo-control-height-compact:34px;--dmo-sidebar-width:276px;--dmo-header-height:76px;--dmo-tabs-height:52px;--dmo-shadow-card:var(--dmo-shadow);--dmo-shadow-menu:0 10px 24px rgba(15,29,42,.22);--dmo-shadow-modal:0 25px 70px rgba(15,29,42,.35);--dmo-transition-fast:var(--dmo-fast);--dmo-space-1:4px;--dmo-space-2:8px;--dmo-space-3:12px;--dmo-space-4:16px;--dmo-space-5:20px;--dmo-space-6:24px;--dmo-space-8:32px}
*{box-sizing:border-box}body{margin:0;background:var(--dmo-page);color:var(--dmo-text);font:14px/1.45 Inter,ui-sans-serif,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif}button,input,select,textarea{font:inherit}.dmo-button{min-height:36px;padding:7px 12px;border:1px solid var(--button-color,var(--dmo-brand-600));border-radius:var(--dmo-r-control);background:var(--button-color,var(--dmo-brand-600));color:#fff;font-weight:700;cursor:pointer;transition:background var(--dmo-fast),color var(--dmo-fast),border-color var(--dmo-fast)}.dmo-button:hover,.dmo-button:focus-visible{background:#fff;color:var(--button-color,var(--dmo-brand-600));outline:none}.dmo-button.danger{--button-color:var(--dmo-danger)}.dmo-button.success{--button-color:var(--dmo-success)}.dmo-button:disabled{--button-color:var(--dmo-disabled);cursor:not-allowed}.dmo-icon-button{width:36px;padding:0;display:grid;place-items:center}.dmo-field label{display:block;margin-bottom:6px;color:var(--dmo-muted);font-size:11px;font-weight:750}.dmo-field input,.dmo-field select,.dmo-field textarea{width:100%;min-height:40px;border:1px solid var(--dmo-border);border-radius:var(--dmo-r-control);background:#fff;color:var(--dmo-text);padding:9px 11px;outline:none}.dmo-field input:focus,.dmo-field select:focus,.dmo-field textarea:focus{border-color:var(--dmo-brand-600);box-shadow:0 0 0 3px rgba(60,115,168,.13)}.dmo-card{background:var(--dmo-card);border:1px solid var(--dmo-border);border-radius:var(--dmo-r-card);box-shadow:var(--dmo-shadow)}.dmo-pill{display:inline-flex;align-items:center;padding:4px 8px;border-radius:999px;background:var(--dmo-brand-050);color:var(--dmo-brand-700);font-size:10px;font-weight:800}.dmo-pill.active{background:var(--dmo-success-soft);color:var(--dmo-success)}.dmo-pill.inactive{background:#eef1f4;color:var(--dmo-muted)}.dmo-table-wrap{overflow:auto;border:1px solid var(--dmo-border);border-radius:10px}.dmo-table{width:100%;border-collapse:collapse;white-space:nowrap}.dmo-table th{background:var(--dmo-subtle);color:var(--dmo-muted);font-size:10px;text-transform:uppercase;text-align:left;padding:11px 12px}.dmo-table td{padding:11px 12px;border-top:1px solid var(--dmo-border)}.dmo-table tr[data-row]{cursor:pointer}.dmo-table tr[data-row]:hover{background:var(--dmo-page)}.dmo-table tr.selected{background:#d9e6f2}.dmo-modal-backdrop{display:none;position:fixed;z-index:80;inset:0;background:rgba(15,29,42,.68);padding:24px;align-items:center;justify-content:center}.dmo-modal-backdrop.open{display:flex}.dmo-modal{width:min(560px,100%);background:#fff;border-radius:var(--dmo-r-modal);box-shadow:0 25px 70px rgba(15,29,42,.35)}.dmo-modal-head,.dmo-modal-foot{display:flex;align-items:center;padding:18px 20px;border-bottom:1px solid var(--dmo-border)}.dmo-modal-head h2{margin:0;font-size:18px}.dmo-modal-head .dmo-icon-button{margin-left:auto}.dmo-modal-body{padding:20px}.dmo-modal-foot{border:0;border-top:1px solid var(--dmo-border);justify-content:flex-end;gap:8px}.dmo-toast{position:fixed;right:22px;bottom:22px;z-index:100;background:var(--dmo-brand-950);color:#fff;padding:11px 15px;border-radius:9px;box-shadow:var(--dmo-shadow);opacity:0;transform:translateY(50px);transition:.2s}.dmo-toast.show{opacity:1;transform:none}

/* Canonical control sizing. Context wins over the base button size. */
.filters > .btn,
.filters > .dmo-button,
.search > .btn,
.search > .dmo-button,
.history-filters > .btn,
.history-filters > .dmo-button,
.dmo-filter-row > .btn,
.dmo-filter-row > .dmo-button {
  min-height: var(--dmo-control-height);
}
.pager > .btn.icon,
.pager > .dmo-icon-button,
.dmo-pagination > .btn.icon,
.dmo-pagination > .dmo-icon-button {
  width: 36px;
  min-width: 36px;
  min-height: 36px;
  height: 36px;
  padding: 0;
}
@media(max-width:720px){.dmo-modal-backdrop{padding:12px}.dmo-table{min-width:760px}}

/* Canonical selectable lists */
[data-dmo-list] [data-dmo-row]{cursor:pointer;transition:background var(--dmo-fast),border-color var(--dmo-fast)}
[data-dmo-list] [data-dmo-row]:hover{background:var(--dmo-page)}
[data-dmo-list] [data-dmo-row]:focus-visible{outline:3px solid rgba(60,115,168,.2);outline-offset:1px}
[data-dmo-list] [data-dmo-row].selected{background:var(--dmo-brand-100);border-color:var(--dmo-brand-600)}

/* Canonical calendar */
.dmo-calendar__head{display:flex;align-items:center;justify-content:space-between;gap:8px}
.dmo-calendar__week,.dmo-calendar__grid{display:grid;grid-template-columns:repeat(7,minmax(0,1fr));gap:5px}
.dmo-calendar__week{margin-top:12px;color:var(--dmo-muted);font-size:10px;font-weight:800;text-align:center}
.dmo-calendar__grid{margin-top:7px}
.dmo-calendar__day{position:relative;min-height:38px;border:1px solid var(--dmo-border);border-radius:7px;background:#fff;color:var(--dmo-text);font-weight:700;cursor:pointer}
.dmo-calendar__day:hover{border-color:var(--dmo-brand-600);background:var(--dmo-page)}
.dmo-calendar__day.has-record::after{content:"";position:absolute;left:50%;bottom:4px;width:4px;height:4px;border-radius:50%;background:var(--dmo-warning);transform:translateX(-50%)}
.dmo-calendar__day.selected{background:var(--dmo-brand-600);border-color:var(--dmo-brand-600);color:#fff}
.dmo-calendar__day.selected::after{background:#fff}
.dmo-calendar__day:disabled{visibility:hidden}

/* Canonical record states: shared by weight controls and comparisons */
.dmo-pill.pending{background:#e7eef5;color:#315d88}
.dmo-pill.approved{background:#e5f0eb;color:#3f7765}
.dmo-pill.rejected{background:#f3e9e7;color:#9a625d}
.dmo-pill.record-type{background:#edf2f6;color:#536b80}

/* Canonical application header */
.dmo-app-header{min-height:var(--dmo-header-height);display:flex;align-items:center;gap:13px;padding:10px 28px;background:var(--dmo-card);border-bottom:1px solid var(--dmo-border)}
.dmo-app-header__logo{width:44px;height:44px;object-fit:contain;border-radius:50%;flex:0 0 auto}
.dmo-app-header__page{min-width:0}
.dmo-app-header__page h1{margin:0;color:var(--dmo-text);font-size:18px;line-height:1.2;font-weight:800}
.dmo-app-header__page p{margin:3px 0 0;color:var(--dmo-muted);font-size:11px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.dmo-app-header__user{margin-left:auto;min-width:150px;padding-left:18px;text-align:right;border-left:1px solid var(--dmo-border)}
.dmo-app-header__user strong{display:block;color:var(--dmo-text);font-size:12px;line-height:1.3}
.dmo-app-header__user span{display:block;margin-top:2px;color:var(--dmo-muted);font-size:11px;line-height:1.3}
@media(max-width:600px){.dmo-app-header{padding:10px 14px}.dmo-app-header__logo{width:38px;height:38px}.dmo-app-header__page h1{font-size:15px}.dmo-app-header__page p{max-width:180px}.dmo-app-header__user{min-width:0;padding-left:10px}.dmo-app-header__user strong{font-size:11px}}

```
## END FILE CONTENT

---

# FILE 040

## Source Path
`dmo-interactions.js`

## File Type
`.JS`

### INTERACTION EXTRACTION
```
FUNCTION/USAGE (3): function rows, function selectRow, => {
EVENT TYPES (2): click, dblclick
QUERY SELECTORS: [data-dmo-row] ; [data-dmo-list] ; [data-dmo-calendar] ; [data-date]
DATASET READS: id, date
CustomEvent: 4
dispatchEvent: 4
DOMContentLoaded: 0
window.onload: 0
```

### ORIGINAL JAVASCRIPT

## BEGIN FILE CONTENT
```js
(function () {
  const SELECTED = "selected";

  function rows(list) {
    return list.querySelectorAll("[data-dmo-row]");
  }

  function selectRow(list, row) {
    rows(list).forEach((item) => {
      item.classList.toggle(SELECTED, item === row);
      item.setAttribute("aria-selected", item === row ? "true" : "false");
    });
    list.dispatchEvent(new CustomEvent("dmo:list-select", {
      bubbles: true,
      detail: { id: row.dataset.id || null, row }
    }));
  }

  document.querySelectorAll("[data-dmo-list]").forEach((list) => {
    list.setAttribute("role", "listbox");
    rows(list).forEach((row) => {
      row.setAttribute("role", "option");
      row.tabIndex = 0;
      row.addEventListener("click", () => selectRow(list, row));
      row.addEventListener("dblclick", () => list.dispatchEvent(new CustomEvent("dmo:list-open", {
        bubbles: true,
        detail: { id: row.dataset.id || null, row }
      })));
    });
  });

  document.querySelectorAll("[data-dmo-calendar]").forEach((calendar) => {
    calendar.addEventListener("click", (event) => {
      const day = event.target.closest("[data-date]");
      if (!day || day.disabled) return;
      calendar.querySelectorAll("[data-date]").forEach((item) => {
        item.classList.toggle(SELECTED, item === day);
        item.setAttribute("aria-pressed", item === day ? "true" : "false");
      });
      calendar.dispatchEvent(new CustomEvent("dmo:date-select", {
        bubbles: true,
        detail: { date: day.dataset.date }
      }));
    });
  });
})();

```
## END FILE CONTENT

---

# FILE 041

## Source Path
`logo_recolored(1).png`

## File Type
`BINARY ASSET`

## BEGIN FILE CONTENT

ASSET:
`logo_recolored(1).png`
- filename: 
- type: PNG image
- size (bytes): 38205
- embedded: NO (binary not embedded)
- referenced by: login.html, header/shell (logo da marca) conforme uso visual
- apparent usage: logótipo da aplicação (marca)

## END FILE CONTENT

---
# CONSOLIDATED COMPONENT INDEX

- **Shell** — FILE 004, FILE 005, FILE 007, FILE 009, FILE 014, FILE 020, FILE 021, FILE 022, FILE 025, FILE 026, FILE 031, FILE 032, FILE 033, FILE 034, FILE 035, FILE 037, FILE 038
- **Header** — FILE 007, FILE 021, FILE 022, FILE 023, FILE 025, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 035, FILE 036, FILE 037, FILE 038
- **Sidebar** — FILE 005, FILE 007, FILE 023, FILE 039
- **Navigation** — FILE 021, FILE 022, FILE 023, FILE 025, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 035, FILE 036, FILE 037, FILE 038
- **Buttons** — FILE 005, FILE 021, FILE 022, FILE 026, FILE 032, FILE 033, FILE 037, FILE 038, FILE 039
- **Icon Buttons** — FILE 005, FILE 021, FILE 022, FILE 032, FILE 033, FILE 037, FILE 038, FILE 039
- **Inputs** — (não detetado por marca no conteúdo)
- **Textareas** — FILE 022, FILE 023, FILE 025, FILE 031, FILE 033
- **Selects** — FILE 021, FILE 022, FILE 023, FILE 025, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 035, FILE 037, FILE 038
- **Checkbox** — FILE 022, FILE 023, FILE 025
- **Radio** — (não detetado por marca no conteúdo)
- **Date Input** — FILE 021, FILE 023, FILE 025, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 035, FILE 037, FILE 038
- **Date Picker** — (não detetado por marca no conteúdo)
- **Calendar** — FILE 007, FILE 015, FILE 022, FILE 033, FILE 039, FILE 040
- **Cards** — FILE 004, FILE 021, FILE 022, FILE 026, FILE 032, FILE 033, FILE 037, FILE 038, FILE 039
- **Lists** — FILE 007, FILE 013, FILE 015, FILE 016, FILE 022, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 035, FILE 037, FILE 038, FILE 039, FILE 040
- **List Rows** — FILE 007, FILE 013, FILE 015, FILE 016, FILE 022, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 034, FILE 035, FILE 037, FILE 038, FILE 039, FILE 040
- **Tables** — FILE 021, FILE 022, FILE 025, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 035, FILE 037, FILE 038
- **Filters** — FILE 005, FILE 007, FILE 021, FILE 022, FILE 023, FILE 025, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 034, FILE 035, FILE 037, FILE 038, FILE 039
- **Search** — FILE 005, FILE 007, FILE 021, FILE 022, FILE 023, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 035, FILE 039
- **Tabs** — FILE 007, FILE 021, FILE 022, FILE 032, FILE 033, FILE 037, FILE 038, FILE 039
- **Badges** — FILE 005
- **Status** — FILE 005, FILE 010, FILE 022, FILE 023, FILE 031, FILE 032, FILE 033, FILE 035
- **Alerts** — FILE 001, FILE 003, FILE 004, FILE 005, FILE 007, FILE 011, FILE 013, FILE 017, FILE 020, FILE 022, FILE 023, FILE 025, FILE 026, FILE 031, FILE 036
- **Toasts** — FILE 005, FILE 007, FILE 021, FILE 022, FILE 023, FILE 027, FILE 028, FILE 029, FILE 030, FILE 031, FILE 032, FILE 033, FILE 035, FILE 037, FILE 038, FILE 039
- **Modals** — FILE 001, FILE 003, FILE 004, FILE 005, FILE 007, FILE 008, FILE 021, FILE 022, FILE 023, FILE 031, FILE 032, FILE 039
- **Confirmation Dialog** — FILE 001, FILE 002, FILE 003, FILE 004, FILE 005, FILE 006, FILE 007, FILE 008, FILE 009, FILE 010, FILE 011, FILE 012, FILE 013, FILE 014, FILE 015, FILE 016, FILE 017, FILE 018, FILE 019, FILE 020, FILE 021, FILE 022, FILE 023, FILE 025, FILE 031, FILE 033, FILE 035, FILE 037, FILE 038
- **Context Menu** — (não detetado por marca no conteúdo)
- **Dropdown** — FILE 004, FILE 005, FILE 007, FILE 008, FILE 010, FILE 011, FILE 012, FILE 019, FILE 037, FILE 038
- **Tooltip** — FILE 005
- **Pagination** — FILE 005, FILE 039
- **Empty State** — FILE 004, FILE 005, FILE 022, FILE 023, FILE 025, FILE 031, FILE 036, FILE 037, FILE 038
- **Loading State** — FILE 004, FILE 005, FILE 011
- **Error State** — FILE 004, FILE 005, FILE 031
- **Page Header** — (não detetado por marca no conteúdo)
- **Section Header** — (não detetado por marca no conteúdo)
- **Forms** — FILE 021, FILE 022, FILE 023, FILE 026
- **Action Bar** — (não detetado por marca no conteúdo)
- **History Entry** — FILE 005, FILE 007, FILE 022, FILE 023, FILE 025, FILE 027, FILE 028, FILE 029, FILE 030, FILE 032, FILE 035, FILE 037, FILE 038, FILE 039
- **User/Profile** — FILE 005, FILE 007, FILE 015, FILE 021, FILE 031, FILE 032, FILE 033

# PAGE / MODULE INDEX

- **Login**
    - `login.html` FILE 026
    - `docs/PORTAL_LOGIN_ADMIN_HANDOFF.md` 
- **Admin**
    - `admin.html` FILE 021
    - `docs/PORTAL_LOGIN_ADMIN_HANDOFF.md` 
- **Shell**
    - `dmo-design-system.css` FILE 039
    - `dmo-interactions.js` FILE 040
    - `docs/DESIGN_IMPLEMENTATION_CONTRACT.md` 
    - `docs/DMO_DESIGN_SYSTEM.md` 
- **Peso Operador**
    - `peso-operador.html` FILE 032
    - `docs/PESO_INTERFACE_HANDOFF.md` 
- **Peso Responsável**
    - `peso-responsavel.html` FILE 033
    - `docs/PESO_INTERFACE_HANDOFF.md` 
- **Peso Comparação**
    - `peso-operador.html` FILE 032
    - `docs/PESO_INTERFACE_HANDOFF.md` 
- **Boquilhas**
    - `boquilhas.html` FILE 023
    - `docs/BOQUILHAS_INTERFACE_BEHAVIOR.md` 
- **Job On**
    - `job-on.html` FILE 024
    - `job-on-v48-folha-producao.html` FILE 025
    - `docs/JOB_ON_DESIGN_BRIEF.md` 
    - `docs/JOB_ON_VERIFICACOES_DESIGN_BRIEF.md` 
    - `docs/JOB_ON_DATA_MODEL.md` 
- **CM**
    - `moldes.html` FILE 027
    - `moldes-v42-listas.html` FILE 028
    - `moldes-v43-alinhado.html` FILE 029
    - `moldes-v44-seletor-corrigido.html` FILE 030
    - `docs/REPARACAO_EXTERNA_DESIGN_BRIEF.md` 
- **MF**
    - `moldes.html` FILE 027
    - `moldes-v42-listas.html` FILE 028
    - `moldes-v43-alinhado.html` FILE 029
    - `moldes-v44-seletor-corrigido.html` FILE 030
    - `docs/REPARACAO_EXTERNA_DESIGN_BRIEF.md` 
- **Warehouse/Armazém**
    - `armazem.html` FILE 022
    - `docs/ARMAZEM_DESIGN_BRIEF.md` 
- **Internal Repair**
    - `reparacao-interna.html` FILE 035
    - `reparacao-v2.html` FILE 036
    - `docs/REPARACAO_INTERNA_DESIGN_BRIEF.md` 
- **External Repair**
    - `reparacao-externa-v1.html` FILE 034
    - `docs/REPARACAO_EXTERNA_DESIGN_BRIEF.md` 
- **Pegamentos**
    - `pegamentos.html` FILE 031
    - `docs/PEGAMENTOS_INTERFACE_HANDOFF.md` 
- **Tampões**
    - `tampoes.html` FILE 037
    - `tampoes-v38-standalone.html` FILE 038
    - `docs/TAMPOES_DESIGN_BRIEF.md` 
- **Tool Creation**
    - `docs/FERRAMENTAS_REGISTO_DESIGN_BRIEF.md` 
    - `moldes.html` FILE 027
- **History**
    - `docs/MODULE_UI_HANDOFF_TEMPLATE.md` 
    - `docs/DMO_DESIGN_SYSTEM.md` 

Documentos de apoio/metadados: `README.md`, `docs/HANDOFF_INDEX.md`, `docs/DESIGN_INPUT_EXTRACTION.md`, `docs/AUDITORIA_GLOBAL_HANDOFF.md`, `docs/CODER_IMPLEMENTATION_HANDOFF.md`, `docs/DESIGN_IMPLEMENTATION_CONTRACT.md`.

# MOCKUP TO COMPONENT MAP

- **admin.html**
    - GLOBAL dmo-* components used (18): dmo-border, dmo-brand-600, dmo-button, dmo-card, dmo-design-system, dmo-field, dmo-icon-button, dmo-modal, dmo-modal-backdrop, dmo-modal-body, dmo-modal-foot, dmo-modal-head, dmo-muted, dmo-pill, dmo-surface-soft, dmo-table, dmo-table-wrap, dmo-toast
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **armazem.html**
    - GLOBAL dmo-* components used (39): dmo-app-header, dmo-border, dmo-brand-050, dmo-brand-100, dmo-brand-200, dmo-brand-500, dmo-brand-600, dmo-brand-700, dmo-brand-900, dmo-brand-950, dmo-button, dmo-calendar, dmo-card, dmo-danger, dmo-danger-soft, dmo-design-system, dmo-disabled, dmo-fast, dmo-field, dmo-icon-button, dmo-interactions, dmo-list, dmo-muted, dmo-page, dmo-pill, dmo-r-card, dmo-r-control, dmo-r-modal, dmo-row, dmo-shadow, dmo-subtle, dmo-success, dmo-success-soft, dmo-table, dmo-table-wrap, dmo-text, dmo-toast, dmo-warning, dmo-warning-soft
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **boquilhas.html**
    - GLOBAL dmo-* components used (0): 
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **job-on.html**
    - GLOBAL dmo-* components used (0): 
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **job-on-v48-folha-producao.html**
    - GLOBAL dmo-* components used (1): dmo-design-system
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **login.html**
    - GLOBAL dmo-* components used (12): dmo-brand-050, dmo-brand-600, dmo-brand-700, dmo-brand-800, dmo-brand-950, dmo-button, dmo-card, dmo-danger, dmo-danger-soft, dmo-design-system, dmo-field, dmo-muted
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **moldes.html**
    - GLOBAL dmo-* components used (2): dmo-list, dmo-row
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **moldes-v42-listas.html**
    - GLOBAL dmo-* components used (2): dmo-list, dmo-row
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **moldes-v43-alinhado.html**
    - GLOBAL dmo-* components used (2): dmo-list, dmo-row
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **moldes-v44-seletor-corrigido.html**
    - GLOBAL dmo-* components used (2): dmo-list, dmo-row
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **pegamentos.html**
    - GLOBAL dmo-* components used (4): dmo-app-header, dmo-design-system, dmo-list, dmo-row
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **peso-operador.html**
    - GLOBAL dmo-* components used (24): dmo-border, dmo-brand-050, dmo-brand-600, dmo-brand-700, dmo-button, dmo-card, dmo-design-system, dmo-field, dmo-icon-button, dmo-interactions, dmo-list, dmo-modal, dmo-modal-backdrop, dmo-modal-body, dmo-modal-foot, dmo-modal-head, dmo-muted, dmo-pill, dmo-row, dmo-subtle, dmo-success, dmo-table, dmo-table-wrap, dmo-toast
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **peso-responsavel.html**
    - GLOBAL dmo-* components used (20): dmo-border, dmo-brand-050, dmo-brand-200, dmo-brand-600, dmo-button, dmo-calendar, dmo-card, dmo-design-system, dmo-field, dmo-icon-button, dmo-interactions, dmo-list, dmo-muted, dmo-pill, dmo-row, dmo-subtle, dmo-success, dmo-table, dmo-table-wrap, dmo-toast
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **reparacao-externa-v1.html**
    - GLOBAL dmo-* components used (2): dmo-design-system, dmo-row
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **reparacao-interna.html**
    - GLOBAL dmo-* components used (4): dmo-design-system, dmo-interactions, dmo-list, dmo-row
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **reparacao-v2.html**
    - GLOBAL dmo-* components used (0): 
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **tampoes.html**
    - GLOBAL dmo-* components used (31): dmo-app-header, dmo-border, dmo-brand-050, dmo-brand-100, dmo-brand-200, dmo-brand-600, dmo-brand-700, dmo-brand-950, dmo-button, dmo-card, dmo-design-system, dmo-disabled, dmo-field, dmo-icon-button, dmo-interactions, dmo-list, dmo-muted, dmo-page, dmo-r-card, dmo-r-control, dmo-row, dmo-shadow, dmo-subtle, dmo-success, dmo-success-soft, dmo-table, dmo-table-wrap, dmo-text, dmo-toast, dmo-warning, dmo-warning-soft
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).
- **tampoes-v38-standalone.html**
    - GLOBAL dmo-* components used (31): dmo-app-header, dmo-border, dmo-brand-050, dmo-brand-100, dmo-brand-200, dmo-brand-600, dmo-brand-700, dmo-brand-950, dmo-button, dmo-card, dmo-design-system, dmo-disabled, dmo-field, dmo-icon-button, dmo-interactions, dmo-list, dmo-muted, dmo-page, dmo-r-card, dmo-r-control, dmo-row, dmo-shadow, dmo-subtle, dmo-success, dmo-success-soft, dmo-table, dmo-table-wrap, dmo-text, dmo-toast, dmo-warning, dmo-warning-soft
    - composition: ver ORIGINAL HTML na secção de ficheiro respetiva (composição específica do módulo preservada integralmente).

# DESIGN CONTRADICTION REGISTER

Apenas design/interação. Baseado nas divergências detetadas entre ficheiros (principalmente `docs/DESIGN_IMPLEMENTATION_CONTRACT.md` §18, `dmo-design-system.css` e mockups HTML).

| ID | Topic | Source A | Source B | Difference | Severity |
|---|---|---|---|---|---|
| DC-01 | Altura de botão | Design System: compact 34 / normal 40 | `dmo-design-system.css` `.dmo-button` min 36; CODER: 36 standard | fixar API: compact 34, default 36/40 a decidir; um token por size | HIGH |
| DC-02 | Hover/active | filled → hover invertido | alguns mockups têm `.btn` próprios e variantes de tom | usar state machine §5.2; proibir brightness | MEDIUM |
| DC-03 | Enter em lista | RESOLVIDO: sem atalho de teclado específico do BA DMO; clique seleciona, duplo clique abre | antigas propostas (`Enter`/`Ctrl+Enter`/`Espaço`) removidas | contrato único confirmado | RESOLVED |
| DC-04 | Calendário | componente canónico documentado/Boquilhas | Peso apresenta variante visual | um único componente | HIGH |
| DC-05 | Inputs | contrato global 40px | HTML locais com alturas/paddings diferentes | Field global 40; compact só por variante | MEDIUM |
| DC-06 | Card radius/shadow | 12px + shadow token | mockups redefinem radius/shadow localmente | usar Card global | MEDIUM |
| DC-07 | CSS architecture | tokens/componentes globais | 17 HTML com `<style>` + inline styles; 6 sem CSS global | não copiar CSS dos mockups; reconstruir | HIGH |
| DC-08 | Moldes tabs vs botões | tabs representam áreas | mockup mostra `Contra moldes`/`Moldes finais` como botões | usar Tabs globais / segmented selector | MEDIUM |
| DC-09 | Page width | princípio largura disponível | alguns mockups centralizados estreitos | container fluido/max-width + gutters | MEDIUM |
| DC-10 | Sidebar | side panel fixo contextual | não existe sidebar global noutros módulos | separar App Navigation de Context Sidebar | MEDIUM |
| DC-11 | Header naming | header usa título da página | mockups misturam app/module title | dois níveis: module identity (header) / view title (content) | MEDIUM |
| DC-12 | Job On tool label | exemplos usam `MP` | discussões referem `CM` como prioritário | `FUNCTIONAL INPUT REQUIRED`; visual aceita label configurada | MEDIUM |
| DC-13 | Modal confirmations | contrato proíbe APIs nativas | vários mockups usam `confirm/prompt/alert` | Confirmation Dialog/Modal global | HIGH |
| DC-14 | Dropdown | contrato pede custom styled/searchable | alguns HTML usam select/datalist/browser menu | Select nativo estilizado (curto); Custom Dropdown (contextual/pesquisa) | MEDIUM |
| DC-15 | Pegamentos | brief remove base local/ações duplicadas | HTML ainda contém código legacy | brief prevalece; não portar legacy UI | MEDIUM |
| DC-16 | Versioned mockups | ficheiros canónicos coexistem com versões v38/v42/v43/v44 | — | manter entrada canónica; versões = evidência histórica | LOW |

# DESIGN GAPS

Classificação de gaps de design. Não inclui DB/domain. Base: `docs/DESIGN_IMPLEMENTATION_CONTRACT.md` §19/§22.

### P0 — blocks design foundation

1. tokens exatos de typography/line-height/letter-spacing
2. escala de z-index/layers
3. page width e gutters responsivos
4. tamanho default do Button (36 ou 40)
5. teclado de row (RESOLVIDO: sem atalho específico do BA DMO; clique seleciona, duplo clique abre)
6. border/focus tokens + regras de reduced motion
7. declarar `dmo-design-system` como única fonte visual (impedir legacy/inline/local component CSS)

### P1 — blocks reusable component

1. Calendar completo (today, disabled/outside month, loading/error, responsive)
2. Field completo (required, helper, error, readonly, async/loading)
3. Custom Dropdown pesquisável e Select curto
4. Modal/Confirmation com focus trap e size variants
5. Loading, Empty e Error components
6. Tooltip e icon system
7. Sidebar/drawer responsiva
8. User/Profile menu e logout visual
9. History Entry/detail
10. Sorting contract para Table
11. Page Header, Action Bar e Detail Panel canónicos

### P2 — blocks module implementation

1. Peso deve adotar o Calendar canónico
2. Shell precisa do module switcher e account interaction
3. CM/MF precisam de handoff visual individual ou validação de que o brief conjunto basta
4. Pegamentos precisa de mockup canónico limpo (sem áreas legacy)
5. Tool creation precisa de mockup próprio consolidado
6. History transversal precisa de composição canónica
7. Boquilhas/Admin/Armazém precisam de substituir confirmações nativas no mockup final
8. Job On precisa de validação funcional da nomenclatura MP/CM

### P3 — cosmetic / can defer

1. afinação da intensidade de sombras
2. motion normal de menus/modais
3. truncation/tooltip em referências longas
4. microcopy uniforme de empty states
5. refinamento de densidade ultra-wide
6. animação de cartões expansíveis

# DESIGN IMPLEMENTATION READINESS

| Area | Status | Missing |
|---|---|---|
| Design tokens | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| CSS architecture | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Typography | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Buttons | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Fields | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Forms | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Lists | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Tables | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Filters | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Tabs | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Calendar | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Modal | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Feedback | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Shell | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Page anatomy | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Responsive | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Login | READY | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Admin | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Peso Operador | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Peso Responsável | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Comparação | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Boquilhas | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Job On | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Warehouse | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Internal Repair | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| External Repair | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Pegamentos | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Tampões | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| Tool Creation | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |
| History | PARTIAL | ver DESIGN GAPS / CONSOLIDATED COMPONENT INDEX |

> Nota: `MODULE DESIGN COVERAGE: 1/17 READY` (Login) segundo o `docs/DESIGN_IMPLEMENTATION_CONTRACT.md` — todos os restantes módulos `PARTIAL`, sobretudo por dependência de componentes globais por fechar.

# DESIGN IMPLEMENTATION CONTRACT SUMMARY

Consolidado apenas do que resulta claramente das fontes. Onde existir conflito não resolvido: marcado `UNRESOLVED DESIGN CONFLICT`.

### Canonical token system

- Paleta de marca V1 `#3c73a8` (brand-600); texto `#172d42`; muted `#64778a`; página `#f6f9fc`; card `#fff`; subtle `#f1f6fa`; border `#d9e6f2`; success/warning/danger moderados.
- Typography: Inter + fallback; corpos usando `--dmo-text`/muted; decimais máx. 2 casas em apresentação.
- Spacing 4/8/12/16/20/24/32; radius control 8 / card 12 / pill; field height 40px.
- `UNRESOLVED DESIGN CONFLICT`: tokens de typography/line-height/letter-spacing, layers/z-index, page width/gutters, button default size, pressed token (P0 do contract).

### Canonical CSS architecture

- Ordem obrigatória: GLOBAL TOKENS → GLOBAL COMPONENTS → GLOBAL LAYOUT/SHELL → MODULE COMPOSITION.
- Sem `<style>` de design por página; sem `style="..."` de design; sem `site.css` legacy a competir; um único componente por peça.
- Exceções raras, nomeadas, documentadas e testadas.

### Canonical component system

- Um componente universal implementado uma vez; components reassemble global (dmo-design-system.css) como fonte.
- Módulo CSS só compõe (grid, ordem, larguras); nunca redefine cor/raio/sombra/tipografia/botão/field/table/modal/calendar/feedback.

### Canonical interaction behavior

- Listas: um clique seleciona, duplo clique abre, ações dependentes fora da lista; `data-dmo-list`/`data-dmo-row` como contrato.
- Botões: filled em repouso, invertidos no hover/focus; sem brightness; danger usa vermelho moderado.
- Estados: default/hover/focus/active/selected/disabled/loading/success/warning/error; focus visible independente de hover.
- Modais/confirmação substituem APIs nativas (`confirm/prompt/alert`).
- Listas: um clique seleciona, duplo clique abre; não existe atalho de teclado específico do BA DMO.

### Canonical calendar behavior

- Semana começa segunda; mês/ano centrado; setas; seleção por clique filtra; "Mostrar todas as datas" remove filtro; um componente único partilhado por Boquilhas/Peso/JobOn.

### Canonical list/table behavior

- Um clique seleciona; duplo abre; paginação 20/40/60 com total e página; filtros num cartão compacto com "Limpar filtros"; servidor pagina listas grandes.
- `UNRESOLVED DESIGN CONFLICT`: sorting contract (headers sortable/direção/persistência) por fechar.

### Canonical Shell visual contract

- Header global 76px com logo (44px), identidade módulo/página, nome + título/função à direita (título não concede permissões).
- Tabs operacionais à esquerda; Definições/Admin à direita; sidebar contextual não substitui navegação global.
- `UNRESOLVED DESIGN CONFLICT`: module switcher/launcher global, account/logout interaction, page width/gutters, layers, sticky/sidebar responsive (Shell visual PARTIAL).

### Canonical form behavior

- Label sempre visível; required indicado; helper explica formato; erro under field + summary em forms longos; Save/Cancel à direita (Cancel antes de Save); dirty-state usa Confirmation Dialog.

### Canonical feedback behavior

- Field error (até corrigir), inline info/warning, toast para sucesso breve, Error State com Retry para load falhado, loading preserva dimensões, Confirmation/Dialog para destrutivas; nunca toast como única via para erro que exige correção.

### Module-specific layout boundaries

- Módulos usam os componentes globais; apenas a composição é específica do módulo. Exceções justificadas: Login (sem tabs/header autenticado), Job On (folha técnica consulta), side panel Boquilhas, Responsável Peso master-detail, Armazém/Reparação interna registo rápido.

### Responsive expectations

- Desktop usa largura disponível (sem coluna estreita central); sidebar fixo só quando a largura permite (mobile → drawer); tabelas com scroll horizontal só em ecrãs pequenos; alvos móveis min 44px; WCAG AA; reduced motion.
- `UNRESOLVED DESIGN CONFLICT`: breakpoints/gutters/tablet-mobile exatos por fechar (P0).

# EXTRACTION COVERAGE

- Total files discovered: 41
- Markdown: 20
- HTML: 18
- CSS: 1
- JS: 1
- JSON: 0
- TXT: 0
- XML: 0
- SVG: 0
- Binary assets: 1
- Text files included: 40
- Files skipped: 0
- Files failed: 0
- All relevant files represented: YES (40 text + 1 asset = 41)

