# MODULES 08 — REPARACAO INTERNA SPEC

Autoridade funcional do módulo Reparação Interna. Fontes: `REPARACAO_INTERNA_DESIGN_BRIEF.md`,
AB-03/BT-07 (02_DEC §3.7, §4), MODELO_LOCKED §9.

## 1. Scope e boundary (GLM-RI-01)

Registo rápido de reparações de CM e MF **durante a produção** (intervenção de turno; a ferramenta
continua na linha). Exige apenas Linha + Tipo + número individual; o resto vem do contexto ativo.
**Não pertence:** reparação externa programada (módulo próprio); posição (Armazém); dados mestres.
O registo **não altera** posição, vida útil, estado técnico, Job On ou dados mestres.

## 2. Atores e permissões (GLM-RI-02)

| Operação | Quem |
|---|---|
| Registar | Reparador de turno com módulo `reparacao_interna` |
| Consultar histórico | segundo o template de acesso |
| Corrigir | capability `reparacao_interna.corrigir` |

## 3. Tabs e rotas (GLM-RI-03)

`/reparacao-interna`; tabs `Registo` e `Histórico`.

## 4. Workflow de registo (GLM-RI-04)

1. **Seletor de Linha**: cartão horizontal de largura total no topo com B1–C3 (desktop 6 colunas;
   3 em larguras intermédias; 2 em mobile) — layout antigo de seis botões numa fila está rejeitado.
   Cada cartão mostra Linha + Referência completa do Job On ativo (read-only, obtida por consulta)
   ou `Sem Job On ativo`.
2. **Contexto automático**: Production/Referência/Job On/data para a data-hora do registo;
   usar relações e IDs registados; não deduzir produção pelo código; sem auto-seleção em ambiguidade;
   não criar Job On ausente; consultas não alteram dados.
3. **Registar**: escolher CM/MF + número individual; número validado no domínio do tipo e lote
   associados à produção; operador e data-hora automáticos; não pedir nada mais.
4. **Guardar**: resumo antes de confirmar; validar contexto+ferramenta; criar registo com IDs
   estáveis (ferramenta/lote, Job On, produção); sucesso só após persistência; limpar apenas o
   número e devolver foco; Linha/Tipo podem manter-se para registos consecutivos; mudar de Linha
   recarrega o contexto.

## 5. Estados excecionais (GLM-RI-05)

Sem produção ativa → impedir guardar com mensagem; mais de um contexto → escolha explícita;
ferramenta não encontrada → mensagem própria, **sem criação automática nem lote alternativo**;
tipo errado (número existe noutro tipo) → informar sem mudar automaticamente; falha ao guardar →
preservar Linha/Tipo/número, sem sucesso falso.

## 6. Histórico e correção (GLM-RI-06)

Filtros: datas, Linha, Produção/Job On, Referência/lote, Tipo, número, operador, apenas corrigidos.
Lista com colunas do brief §7; `Corrigir registo` fora da tabela (botão standard 36px), antes das
setas de paginação. Correção (capability): Linha/contexto, Tipo, número; data/hora e operador
originais read-only; ao mudar Linha, re-resolver contexto para a data-hora original; sem resultado
inequívoco → não guardar por aproximação. Auditoria: registo afetado, valores anteriores/novos,
autor, data/hora; original nunca desaparece; lista mostra versão válida, detalhe a sequência completa.

## 7. Dados (GLM-RI-07)

06_DATA §3.7 `internal_repair_records` (+ `repair_events` quando aplicável). Cada registo: linha,
jobon_id/production_id resolvidos, tool referência/lote + peça/número, tipo, operador, occurred_at,
correções como eventos before/after.

## 8. Hard blocks vs avisos (GLM-RI-08)

Hard blocks (TECHNICAL INTEGRITY): sem contexto ativo não há registo; número inexistente não é
registado. Tudo o resto é registo de facto: uma intervenção invulgar é registável com observação
(GLM-CORE-01). Nenhum diagnóstico automático da ferramenta.

## 9. Integração do histórico (GLM-RI-09)

O mesmo registo é consultável no Histórico da Reparação interna, na ficha da ferramenta e no Job On
histórico (mesmo ID; sem cópias divergentes) — contrato de leitura com o módulo História.

## 10. Testes e acceptance (GLM-RI-10)

Unit: resolução de contexto (único/ambíguo/ausente). Integration: registo + correção auditável.
E2E: fluxo completo com Job On ativo e sem Job On. Acceptance: critérios do brief §14.

## 11. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-RI-11)

**MUST PRESERVE:** minimalidade do registo (3 inputs); contexto read-only; correção auditável; IDs estáveis.
**DO NOT CARRY FORWARD:** layout de seis botões em fila; criação automática de ferramentas; alteração
de outros domínios pelo registo.

## 12. Open questions (GLM-RI-12)

Formato do número individual CM/MF; observação/motivo obrigatórios no futuro?; limites temporais da
correção; vários lotes do mesmo tipo ativos na linha? — `UNRESOLVED — NO AUTHORITATIVE SOURCE FOUND`
(pesquisado: REPARACAO_INTERNA brief §13, v2 004_repair.sql, verified knowledge — sem resposta).
O “momento exato de Job On ativo” ficou **RESOLVED** nesta passagem: definição canónica
`Resolve(line, at)` em TD-27 (02_DEC §3.27; modules/05 §5).
