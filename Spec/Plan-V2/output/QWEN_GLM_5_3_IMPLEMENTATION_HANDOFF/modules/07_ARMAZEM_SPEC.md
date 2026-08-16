# MODULES 07 — ARMAZEM SPEC

Autoridade funcional do módulo Armazém. Fontes: `ARMAZEM_DESIGN_BRIEF.md`, MODELO_LOCKED §10,
v2 `005_warehouse.sql` (evidência; VIEW partida C21 não transportada), BT-05.

## 1. Scope e boundary (GLM-ARM-01)

O Armazém é responsável **apenas** por: ferramenta que entrou/saiu; posição física; destino de saída;
observações livres; operador/data; diferenças físicas e correções. Tudo o resto pertence ao domínio
da ferramenta. O Armazém **não cria/altera/recalcula**: utilização, estados técnicos, linha,
reparador, dados técnicos, referência/lote. Esses valores aparecem como contexto read-only da fonte autoritativa.

## 2. Atores e permissões (GLM-ARM-02)

Módulo `armazem`. Operador de armazém executa movimentos; correções de localização auditáveis.

**Relação com Job On** (ARMAZEM brief §10): o Job On consulta o Armazém (read-only) para apresentar
posição atual exata, localização/contexto e último movimento; nunca altera posições nem cria Saídas;
associa o ID estável da ferramenta/lote; snapshot histórico separado da localização atual. Em **Modo
consulta** do Job On, a informação live do Armazém não ocupa a folha; em **Modo edição**, a lista de
substituição combina posição (Armazém) + estado técnico/`% de uso` (domínio da ferramenta) em
composição read-only (Tool Availability Picker — 07_DESIGN §4.16).

## 3. Tabs e rotas (GLM-ARM-03)

`/armazem`; tabs `Registo` e `Consulta`. Sem Definições de reparadores/estados/vida neste módulo.

## 4. Modelo de dados (GLM-ARM-04)

06_DATA §3.8 + BT-05: ocupação materializada 1:1 (`warehouse_stock` UNIQUE), posição = fonte de
verdade, `fora` = calculado (sem posição). Posição+movimento escritos **atomicamente**; conflitos
nunca sobrescrevem. Movimentos guardam ID estável da ferramenta/lote (CM/MF/BQ dos módulos donos).

## 5. Workflows (GLM-ARM-05)

1. **Entrada**: tipo (CM/MF/BQ) → pesquisa no domínio do tipo (sem auto-escolha ambígua) → Posição +
   Observações → resumo antes de guardar → persistência → posição torna-se localização atual.
   Falhar/cancelar não altera localização anterior.
2. **Saída imediata**: posição atual read-only; Destino (Produção/Reparação) + Observações; posição
   libertada **apenas após persistência**; formulário sem linha/vida/estado/reparador/campos técnicos.
3. **Saídas programadas** (fluxo partilhado com Reparação Externa):
   - origem: lista criada no módulo de Reparação (itens + posições na criação); Armazém recebe pendente;
   - imprimir é opcional e nunca condição; receber/abrir/imprimir não cria saídas nem liberta posições;
   - checkboxes = confirmação de recolha; checks persistidos para retomar;
   - posição na criação ≠ posição atual → mostrar ambas + alerta (snapshot preservado);
   - **último check conclui o conjunto atomicamente**: cria Saída por item (dia/hora + operador),
     liberta todas as posições; falha → nenhuma posição libertada, lista pendente com checks preservados;
   - **retorno**: Entrada por item com posição; linha conclui por item; lista `Concluída` quando todos
     os itens têm Entrada; retorno parcial mantém restantes abertas.
4. **Corrigir localização**: para diferenças físicas; separada de Entrada normal; apresenta valores
   registados vs encontrados; auditável. Sem alertas preditivos nem ocupante inventado.

## 6. Estados da lista programada (GLM-ARM-06)

`Pendente de saída` → `Em reparação` → `Retorno parcial` → `Concluída`. Estados operacionais da
lista; não alteram estados técnicos das ferramentas. Concluídas: read-only, pesquisáveis, visual secundário.

## 7. Consulta e histórico (GLM-ARM-07)

Pesquisa por referência/lote/posição/tipo; filtros (tipo, localização, posição, intervalo, alertas);
lista canónica (1 clique seleciona, duplo clique abre histórico de localização); ferramenta fora →
posição `—` (anterior permanece no histórico). Histórico de localização contém apenas movimentos/
posições — nunca reparações, vida útil, estados técnicos ou produção.

## 8. Hard blocks vs avisos (GLM-ARM-08)

Hard blocks (TECHNICAL INTEGRITY): atomicidade do fecho do conjunto; ocupação 1:1; saldo de posição.
Avisos: posição na criação diferente da atual; `Localização operacional não registada`; conflito de
contextos incompatíveis (encaminhar para correção humana, sem prioridade automática).
Nenhum estado/localização é inventado (GLM-CORE-01).

## 9. Casos limite (GLM-ARM-09)

Ferramenta não encontrada; sem lotes; posição sem ocupação; erro de carregamento (Retry, nunca lista
vazia como válida); falha de entrada de retorno (linha aberta, estado válido preservado).

## 10. Testes e acceptance (GLM-ARM-10)

Unit: regras de ocupação/`fora` calculado. Integration: atomicidade do último check (incl. falha
simulada), entrada/saída com posições. E2E: ciclo completo saída programada → retorno. Acceptance:
critérios do brief §13; Job On não altera posições; nenhum falso sucesso.

## 11. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-ARM-11)

**MUST PRESERVE:** posição fonte de verdade; atomicidade; pares saída/entrada ligados à mesma lista;
histórico só de localização; IDs estáveis.
**DO NOT CARRY FORWARD:** VIEW `tool_status` partida (C21); edição de dados técnicos no Armazém;
substituição de posição por heurística.

## 12. Open questions (GLM-ARM-12)

Formato do código de posição; Destino obrigatório em todas as saídas?; quem cancela listas e em que
estados; Reparação pode alterar listas após visíveis no Armazém? — `UNRESOLVED — NO AUTHORITATIVE
SOURCE FOUND` (pesquisado nesta passagem: ARMAZEM brief §12, REPARACAO_EXTERNA brief, v2
005_warehouse.sql/004_repair.sql, verified knowledge — nenhuma resposta autoritativa encontrada).
SAFE TO DEFER: decidir durante U-14/U-15 com evidência/owner; defaults seguros: destino opcional com
observação, cancelamento apenas pelo criador/`jobon.edit`-equivalente com motivo.
