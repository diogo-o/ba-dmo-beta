# MODULES 01 — BOQUILHAS SPEC

Autoridade funcional do módulo Boquilhas. Fontes: `01_BQ_COMPLETE_SPEC.md`, `05-repairs.md`
(via merged §7), `BOQUILHAS_INTERFACE_BEHAVIOR.md`, decisões UD-07/UD-08/UD-09, TD-14/TD-15, C27.

## 1. Scope e boundary (GLM-BQ-01)

**Pertence:** gestão de lotes de boquilhas, traces de produção/reparação, movimentos diários,
cálculo de inventário, discrepances de retorno, utilização manual, reparadores, lifecycle, histórico.
**Não pertence:** autenticação/Shell; ferramentas CM/MF (módulo Ferramentas); posição física
(Armazém consulta/apresenta contexto); listas programadas de reparação externa (criadas em
Reparação Externa, refletidas como movimentos BQ quando aplicável via contrato).
Nota: o módulo `reparacao_externa` inclui uma tab `Boquilhas` que reutiliza este fluxo (UD-13);
o domínio BQ permanece único e partilhado, sem duplicação de entidades. Todas as ações relevantes
emitem eventos de auditoria global (UD-17/TD-19; módulos/11 §10).

Verificação de segurança desta passagem (legacy vs regra atual): a implementação legacy contém o
hard block `BQ.RETURN_UNMATCHED_NOT_ALLOWED` (`MovementValidator.cs`) que exigia a flag
`AllowUnmatched` para retornos acima do saldo em reparação — classificado **D. CONTRADICTS CURRENT
USER/DESIGN DECISION** (UD-08/UD-09) e NÃO transportado. O cálculo legacy
(`InventoryCalculator.cs`) já é conforme o caso 20→25 (matched/min(qty,repair), unmatched →
exceptionalReceived, prod += matched) e mantém-se como conhecimento confirmado (02_DEC §3.34).

## 2. Atores e permissões (GLM-BQ-02)

| Ator | Permissão | Notas |
|---|---|---|
| Utilizador com módulo `boquilhas` | todas as operações do módulo | sem distinção funcional operador/responsável (01_BQ §15) |
| Sem módulo | nada | guard server-side (gap legacy corrigido) |

## 3. Tabs e rotas (GLM-BQ-03)

Rotas `/boquilhas`; tabs: `Registo`, `Boquilhas`, `Histórico`; `Definições` isolada à direita.
**Sem tab Fabrico** (NC-04). Painel lateral de linhas presente em todas as páginas.

## 4. Entidades e invariantes (GLM-BQ-04)

Conforme 06_DATA §3.2. Invariantes: UNIQUE(reference, batch_code); referência `^[A-Z][0-9]{3}$`;
movimentos append-only; estado do lote derivado (active/preparing) vs `lifecycle_state` persistido
(available/archived/scrapped); `start_line` obrigatório em produção (TD-14).

## 5. Workflows e lifecycle (GLM-BQ-05)

1. **Criar lote + trace inicial** (transação única: lote + trace + movimento START + leitura inicial de utilização);
2. **Adicionar movimento**: Saída (DISPATCH), Entrada (RETURN), Não reparadas, Corrigir contagem, Linha, Fim;
3. **Fechar trace**: data fim, contagem física, motivo; se rawDiff ≠ 0 → resolução com motivo obrigatório; snapshot final imutável;
4. **Reabrir**: apenas último trace fechado e sem outro ativo; evento de reabertura registado;
5. **Arquivar/Sucata/Restaurar**: apenas sem trace ativo; `bq_lifecycle_history` com motivo/ator;
6. **Editar lote**: reference/batch_code/allowed_lines/nominal_qty; linhas em uso não removíveis; nota de alteração;
7. **Reparadores**: CRUD ativo/inativo + defaults por linha (TD-15); desativar preserva histórico.

## 6. Caso obrigatório 20→25 (GLM-BQ-06 — UD-08)

Saem **20** boquilhas para reparação; regressam **25**:

| Passo | Comportamento |
|---|---|
| 1 | O movimento de retorno é aceite com quantidade real **25** e persistido integralmente. Sem bloqueio, sem limite ao saldo esperado, sem autorização especial (`AllowUnmatched` removido — UD-09) |
| 2 | Cálculo: `matched = min(25, 20) = 20`; `unmatched = 5` |
| 3 | As 20 reconciliadas: saldo `repair` passa a **0**; `prod += 20` |
| 4 | As 5 excedentes: registadas como **entrada excecional/discrepância** (`exceptional_received` + `bq_discrepancies` com excess=5, status `open`) |
| 5 | As 5 **não aumentam automaticamente a produção** |
| 6 | UI: aviso claro (“retorno excede o enviado em 5”) + campo de observação/motivo; gravação nunca impedida |
| 7 | Resolução posterior do excesso = outro evento auditável (`resolve_discrepancy`: nota, autor, data); nunca altera o retorno original de 25 |
| 8 | Histórico preserva: movimento original 25, excesso, atores, timestamps |

**Tabela de cálculo do caso:**

| Evento | qty | prod | repair | irreparable | exceptional |
|---|---:|---:|---:|---:|---:|
| START (initial 60) | — | 60 | 0 | 0 | 0 |
| Saída reparação | 20 | 40 | 20 | 0 | 0 |
| Retorno | 25 | 60 | 0 | 0 | 5 |

## 7. Cálculos (GLM-BQ-07)

- `calculateTrace`: DISPATCH `prod-=qty, repair+=qty` (erro se qty>prod — BQ-RULE-003); RETURN
  matched/unmatched conforme §6; NON_REPAIRABLE `repair-=qty, irreparable+=qty` (erro se qty>repair);
  LINE_CHANGE; COUNT_CORRECTION com deltas (não negativos); outputs + `rawDiff = accounted − initial`.
- **Saldo transacional (UI)** ≠ **inventário físico** (C10): delta = RETURN +qty / DISPATCH −qty;
  balance acumula −unmatched. Apresentados separadamente; nunca colapsar.
- Estado efetivo com correções/voids: aplicar última correção não anulada; movimento anulado ignorado.

## 8. Campos e ecrãs (GLM-BQ-08)

Conforme `BOQUILHAS_INTERFACE_BEHAVIOR.md`: criar lote inline (Boquilha, Lote, Total, Utilização
inicial, Data abertura, Linhas permitidas em cartões, Observações); formulário de movimento modal
(Data, Quantidade, Motivo, Detalhe, Observações); resumo do lote ativo em três blocos; painel
lateral com cartões de linha (referência completa abre Job On via IDs, não por texto); alerta de
referências diferentes na mesma linha (aviso, não erro); múltiplos lotes da mesma referência na
mesma linha = válido. Histórico: calendário + resumos + filtros + tabela canónica (sem colunas
Detalhe/Ficheiro); `Corrigir movimento`/`Eliminar movimento` fora da lista com confirmação.

## 9. Dados, tabelas e APIs (GLM-BQ-09)

Tabelas 06_DATA §3.2. Comandos: `CreateLoteWithTrace`, `RegisterMovement`, `CloseTrace`,
`ReopenTrace`, `EditLote`, `Archive/Scrap/Restore`, `ReviseMovement`, `VoidMovement`,
`ResolveDiscrepancy`, `CreateRepairer`, `SetLineRepairerDefault`, `UpdateUtilisation`.
Queries: dataset por lote, movimentos com filtros, histórico agregado, painel de linhas.
Todas as escritas transacionais; autoria server-side (BQ-RULE-009).

## 10. Erros, concorrência, avisos (GLM-BQ-10)

- Erros de validação → Result com mensagem; UI mantém formulário;
- Concorrência em edições (BT-06); append-only isento;
- Avisos (WARNING ONLY): retorno excedente; referências diferentes na linha; utilização próxima do limite (contexto).

## 11. Hard blocks autorizados vs heurísticas proibidas (GLM-BQ-11)

Autorizados: BQ-RULE-001/003/005/006/007/008 (CONFIRMED BUSINESS RULES) + integridade técnica
(transações, UNIQUE). Proibidos: qualquer limite de retorno ao saldo esperado; exigir autorização
para unmatched; bloquear registo por sequência presumida.

## 12. Casos limite (GLM-BQ-12)

Retorno sem saldo em reparação (tudo excecional); qty de linha NULL; fecho com rawDiff≠0; reopen em
cadeia (só último); restaurar lote sucata; correção que zera saldo; dois lotes mesma ref mesma linha.

## 13. Testes e acceptance (GLM-BQ-13)

Unit: calculateTrace (todos os tipos), saldo transacional, estado efetivo com correções/voids.
Integration: fluxos transacionais; 20→25 persistência completa. E2E: 20→25 UI com aviso + observação
+ resolução auditável. Acceptance: secções 6 e 12 verdes; gap BQ module guard inexistente; sem tab Fabrico.

## 14. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-BQ-14)

**MUST PRESERVE:** calculateTrace; imutabilidade + correções/voids; distinção saldos; reopen último;
lifecycle history; reparadores por linha; alerta de conflito de referências; autoria server-side.
**DO NOT CARRY FORWARD:** `allow_unmatched`/`RETURN_UNMATCHED_NOT_ALLOWED`; tab Fabrico; localStorage/
pasta partilhada; entrada excecional exibida em tipos errados; `start_line` NULL em produção.

## 15. Open questions (GLM-BQ-15)

- Offline é requisito? — **RESOLVED: NÃO** (V1 online; BACKEND-DECISION-14; legacy offline não transportado).
- Formato do código de referência além de `[A-Z][0-9]{3}`: `UNRESOLVED — NO AUTHORITATIVE SOURCE
  FOUND` (pesquisado: 01_BQ spec, runtime legacy, migrations 002, verified knowledge, design brief —
  nenhum formato adicional documentado). Mantém-se o formato legado verificado; extensão futura
  exige decisão do owner. SAFE TO DEFER para a fresh build.
