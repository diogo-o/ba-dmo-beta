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
