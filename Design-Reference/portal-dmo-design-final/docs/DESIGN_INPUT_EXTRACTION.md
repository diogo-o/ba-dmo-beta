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
