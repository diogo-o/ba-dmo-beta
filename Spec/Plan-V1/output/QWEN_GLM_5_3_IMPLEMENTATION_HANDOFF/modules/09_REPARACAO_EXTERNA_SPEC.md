# MODULES 09 — REPARACAO EXTERNA SPEC

Autoridade funcional do módulo Reparação Externa. Fontes: `REPARACAO_EXTERNA_DESIGN_BRIEF.md`,
AB-03/BT-07/TD-22 (02_DEC), C26/C27 reconciliados (02_DEC §6), UD-13, MODELO_LOCKED §9.

## 1. Scope e boundary (GLM-RE-01)

`reparacao_externa` é **um único módulo atribuível** (UD-13). Preparação, envio, acompanhamento e
retorno de BQ, CM e MF enviados para reparadores externos (ciclo de dias, pré-produção).
**BQ por referência/lote/quantidade; CM e MF por referência/lote/número individual** (AB-03). CM e MF
seguem **o mesmo fluxo de preparação e envio antes da produção** (UD-13; confirmados os dois no
brief §4–5); permanecem separados internamente devido ao tipo de entidade e apresentação, não por
serem módulos diferentes.
O ciclo externo **não parte da ferramenta em produção** (isso é Reparação interna): o responsável
seleciona uma produção futura planeada e prepara a lista dias antes.
Ownership: Reparação = plano/reparador/acompanhamento/ciclo; Armazém = posição/confirmação física;
domínios BQ/CM/MF = identidade/características; Job On = contexto apenas quando relação explícita.

## 2. Estrutura e navegação (GLM-RE-02)

Módulo `reparacao_externa`, rota `/reparacao-externa`; tabs: **`Boquilhas`**, `Contra moldes`,
`Moldes finais` (padrão visual Boquilhas, dados separados), `Envios`, `Histórico`, `Definições`
(UD-13 — a tab Boquilhas fazia falta na versão anterior do pacote). Sem página intermédia
obrigatória `Reparação`; o módulo Boquilhas permanece igualmente acessível como módulo próprio sem
migração/cópia/redesenho — a tab `Boquilhas` da Reparação externa reutiliza o fluxo e detalhe de
`BOQUILHAS_INTERFACE_BEHAVIOR.md` (unidade referência+lote, movimentos por quantidade, saldos,
reparador associado, registo do lote acessível; 1 clique seleciona, duplo clique abre o lote). CM e MF
partilham o ciclo externo e componentes visuais mas **nunca** entidades/movimentos.

## 3. Workflow do ciclo externo (GLM-RE-03)

1. Responsável cria **saída programada**: seleção de ferramentas/lotes + reparador permitido;
   associação a produção prevista como campo compacto no formulário (sem cartões de produções);
2. Lista fica disponível no Armazém (execução física — 07_ARM §5.3) e pode ser impressa;
3. Reparação acompanha o envio **sem duplicar** os movimentos do Armazém;
4. Retorno: Armazém confirma entradas/posições; ciclo fecha item a item; lista concluída quando
   todos os itens regressam; datas e operadores de saída/entrada preservados por item;
5. Listas concluídas não desaparecem: passam para Histórico.

## 4. Lista programada (GLM-RE-04)

Cabeçalho: código, tipo BQ/CM/MF, reparador, data prevista, criado por/data, estado.
Itens BQ: Referência, lote, quantidade. Itens CM/MF: Referência, lote, número individual,
máquina/linha, posição atual quando conhecida. Persistência: `repair_exits`/`repair_exit_items` com
`repair_type BQ|CM|MF` (TD-22; qty para BQ, número para CM/MF). Estados V1: `Preparação`,
`A retirar`, `Enviado`, `Retorno parcial`, `Concluído`, `Cancelado` — cada transição corresponde a
confirmações persistidas; nunca inferir transições pela abertura da página.

## 5. Reparadores (GLM-RE-05)

`repairers` + `line_repairer_defaults` por tipo e linha (TD-15). Definições gerem reparadores e
associações (tipo BQ/CM/MF, linha permitida, ativo/inativo). Alterar associação não reescreve
listas/movimentos antigos: cada envio guarda **snapshot** do reparador usado. Desativar, nunca apagar.

## 6. Alertas e integridade (GLM-RE-06)

- Item sem localização conhecida: **aviso**, não localização inventada;
- Item já incluído noutra saída aberta: bloquear duplicação (TECHNICAL INTEGRITY);
- Confirmação parcial: progresso explícito;
- Retorno sem saída correspondente: encaminhar para correção com registo do facto (WARNING + ação
  humana; não apagar nem inventar saída — GLM-CORE-01);
- Falha de persistência: manter seleção, sem sucesso.
- BQ: excesso de retorno segue o modelo de discrepances de Boquilhas (modules/01 §6, C27).

## 7. Histórico (GLM-RE-07)

Colunas mínimas: Lista, Tipo, Referência, Lote, Qtd./N.º, Reparador, Saída, Operador saída, Entrada,
Operador entrada, Estado. Filtros: período, tipo, Referência, lote, reparador, estado, Linha/máquina, operador.

## 8. Listas e ações (GLM-RE-08)

1 clique seleciona; duplo clique abre detalhe; ações fora da tabela; botões de ação 36px junto da
paginação quando pertencem à seleção; paginação 20/40/60; filtros não selecionam automaticamente.

## 9. Hard blocks vs avisos (GLM-RE-09)

Hard blocks autorizados: duplicação de item em saída aberta; integridade do ciclo (saída antes de
retorno fechada por item); snapshot de reparador. Aviso apenas: localização desconhecida. Proibido:
bloquear criação de lista por heurística de produção; inferir estado por tempo decorrido.

## 10. Testes e acceptance (GLM-RE-10)

Unit: máquina de estados da lista (transições apenas por confirmações). Integration: criação de
lista → execução no Armazém → retorno parcial → conclusão; snapshot de reparador. E2E: ciclo BQ por
quantidade e CM/MF por número. Acceptance: critérios do brief §12; CM e MF nunca combinados no domínio.

## 11. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-RE-11)

**MUST PRESERVE:** separação BQ/CM/MF; ciclo com Armazém sem duplicar movimentos; histórico com
operadores de saída/entrada; reparadores por tipo/linha com snapshot.
**DO NOT CARRY FORWARD:** cartões de produções ativas na página; página intermédia obrigatória;
transições inferidas; reparação externa a carregar produção ativa atual.

## 12. Open questions (GLM-RE-12)

Regras de cancelamento de listas pelo Manager; adição/remoção de itens após lista visível no Armazém
(partilhado com 07_ARM §12); motivo obrigatório em encerramento de linha que não regressa.

## 13. Módulo único atribuível (GLM-RE-13; UD-13)

A atribuição nos templates de acesso faz-se ao módulo `reparacao_externa` (entrada única). As tabs
internas Boquilhas/Contra Moldes/Moldes Finais/Envios/Histórico/Definições fazem parte do módulo e
não são atribuíveis separadamente. Testes: utilizador com `reparacao_externa` acede às seis tabs;
sem o módulo, nenhuma rota/tab é acessível; CM e MF com o mesmo fluxo de preparação/envio pré-produção
e domínios separados.
