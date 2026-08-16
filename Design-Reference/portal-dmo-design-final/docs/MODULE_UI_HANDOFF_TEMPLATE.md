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
