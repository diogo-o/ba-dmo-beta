# MODULES 10 — TAMPOES SPEC

Autoridade funcional do módulo Tampões. Fontes: `TAMPOES_DESIGN_BRIEF.md`, MODELO_LOCKED §11,
v2 `006_tampoes.sql` (evidência), merged §5.11 (USER: quantidade apenas, sem associação a ferramentas).

## 1. Scope e boundary (GLM-TP-01)

Controlo agregado de quantidades de tampões por **configuração técnica**, mobile-first. V1 sem
números individuais. Tampões **não estão associados** a ferramentas/referências (USER CONFIRMED).
O módulo é autoridade dos seus saldos, movimentos e configurações.

## 2. Atores e permissões (GLM-TP-02)

Módulo `tampoes`. O **Operador tem acesso total**: quantidades, transformações, planeamento e
gestão de campos/configurações em `Opções` (não reservado ao Administrador).

## 3. Tabs e rotas (GLM-TP-03)

`/tampoes`; tabs `Registo`, `Consulta`, `Planeamento`, `Histórico`; `Opções` à direita.

## 4. Modelo funcional (GLM-TP-04)

- Campos comparáveis configuráveis (inicialmente Diâmetro mm e Profundidade/Calote mm), definidos
  separados dos valores (nome, unidade, precisão, ordem, ativo).
- Valores disponíveis em tabela (normalizados — nunca variantes `4/4.0/4,00`); dropdowns em todas
  as páginas operacionais; só valores ativos, ordenados numericamente.
- Configuração = combinação de valores com ID próprio (UNIQUE values).
- Dois saldos por configuração: `Enchidos` e `Por encher` (≥ 0). `Maquinado` **não existe** como
  terceiro estado sem decisão funcional explícita.

## 5. Operações (GLM-TP-05)

1. **Adicionar/Remover quantidade**: saldo escolhido + inteiro positivo; incrementa/decrementa apenas
   esse saldo; nunca saldo negativo; novo saldo só após confirmação persistida; falha preserva inputs.
2. **Alterar estado**: transferência entre `Enchidos`↔`Por encher` como **um único movimento**
   atómico (origem, destino, quantidade, saldos antes/depois, operador, data/hora); origem insuficiente → impedida.
3. **Alterar configuração**: transforma quantidade de configuração origem em destino; ≥1 característica
   diferente; destino existente reutiliza ID, senão cria após validação+confirmação; origem+destino na
   mesma transação; nunca edição retroativa (`4 mm`→`7 mm` direto proibido); movimento guarda tudo
   (origem, destino, qty, valores anteriores/novos, saldos antes/depois, operador, data/hora).
4. **Planear**: necessidade prevista (configuração, quantidade, data prevista, Job On/produção apenas
   com relação inequívoca); **não adiciona, remove nem reserva stock**; diferença vs `Enchidos` é
   informativa; cancelar/alterar plano não altera saldos.
5. **Opções**: CRUD de campos e valores; desativar remove dos dropdowns novos sem apagar
   configurações/histórico; alteração de nome/unidade/precisão não reinterpreta valores guardados
   (mudança incompatível exigiria migração explícita).

## 6. Dados (GLM-TP-06)

06_DATA §3.9: `tampao_field_defs`, `tampao_field_values`, `tampao_configurations`, `tampao_saldos`,
`tampao_movements` (imutáveis), `tampao_planos` (separado dos movimentos físicos).

## 7. Histórico e correções (GLM-TP-07)

Movimentos imutáveis com colunas do brief §9; filtros (datas, diâmetro, calote, movimento, saldo,
valores anterior/novo, operador); `Corrigir movimento` fora da lista; correção preserva original +
valores anteriores/novos + autor + data/hora + justificação; sem edição silenciosa de saldos.

## 8. Hard blocks vs avisos (GLM-TP-08)

Hard blocks (TECHNICAL INTEGRITY): saldos não negativos; atomicidade das transferências; destino
igual à origem impedido. Avisos: transformação concorrente (recarregar saldos e nova confirmação).
Nenhuma dedução automática (planear não converte `Por encher` em `Enchidos`).

## 9. Estados e mensagens (GLM-TP-09)

Conforme brief §11: sem configuração, sem resultado, ambiguidade (lista + escolha explícita),
quantidade insuficiente, destino igual, concorrência, sem movimentos (vazio, não erro), falha com Retry.

## 10. Regras visuais/mobile (GLM-TP-10)

Mobile-first: saldos em blocos lado a lado com algarismos dominantes; alvos ≥44px; teclado numérico;
≤2 casas em diâmetro/calote; quantidades inteiras; configuração ativa visível durante operações;
transformação apresenta Atual/Novo comparável (empilhado no mobile). Sem scroll horizontal.

## 11. Testes e acceptance (GLM-TP-11)

Unit: transferências atómicas (estado e configuração), normalização de valores. Integration:
concorrência em saldos; criação de configuração destino. E2E: fluxo mobile consulta→transformação→
planeamento. Acceptance: critérios do brief §15.

## 12. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-TP-12)

**MUST PRESERVE:** quantidade apenas; normalização de valores; movimento único atómico; planear ≠ reservar.
**DO NOT CARRY FORWARD:** associação de tampões a ferramentas/referências; estado `Maquinado` sem decisão; edição retroativa.

## 13. Open questions (GLM-TP-13)

Significado exato de `Enchido`; saldo próprio para maquinação?; reserva futura?; limites/incrementos;
onde nasce configuração nova; estados do plano; campos comparáveis adicionais — `UNRESOLVED — NO
AUTHORITATIVE SOURCE FOUND` (pesquisado: TAMPOES brief §14, v2 006_tampoes.sql, verified knowledge
§5.11 — o brief mantém as questões; nenhuma fonte as fecha). SAFE TO DEFER: defaults do brief
(dois saldos `Enchidos`/`Por encher`; planear ≠ reservar; sem estado `Maquinado`).
