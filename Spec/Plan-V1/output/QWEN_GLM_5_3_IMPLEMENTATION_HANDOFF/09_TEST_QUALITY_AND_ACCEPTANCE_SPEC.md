# 09 — TEST, QUALITY AND ACCEPTANCE SPEC

Autoridade para qualidade. Aplica-se a todas as unidades do roadmap (10) e a todos os módulos.

## 1. Pirâmide de testes (GLM-TST-01)

| Camada | Scope | Ambiente |
|---|---|---|
| Unit | Domain (entidades, cálculos puros) + Application handlers com fakes | Sem I/O |
| Integration | Repositórios Dapper, transações, RLS, autorização server-side | Supabase de teste remoto (sem Docker/Testcontainers) |
| Contract | Contratos lookup inter-módulos; catálogo vs espelho DB | Unit/integration |
| E2E (fluxo) | Fluxos críticos via páginas Razor (login → operação → histórico) | App de teste + BD de teste |
| Accessibilidade | Teclado, foco, contraste, ARIA | Por componente e por página |
| Visual regression | Capturas desktop/tablet/mobile (07_DESIGN §8) | Por componente canónico |

Sem percentagens arbitrárias de cobertura; cobertura = regras e cenários definidos nas specs.

## 2. Testes de registo sem bloqueio heurístico (GLM-TST-02)

Para cada módulo, testes que provam que **situações anómalas continuam registáveis** quando não
existe proibição autoritativa (GLM-CORE-01/02):

1. **Boquilhas 20→25 (obrigatório, UD-08):** unit (cálculo matched=20/unmatched=5, saldo repair→0,
   prod +20, exceptional=5), integration (movimento persistido completo + discrepancy criada,
   ausência de erro), E2E (UI apresenta aviso, permite observação, grava sem autorização especial;
   resolução posterior auditável; retorno original 25 nunca alterado);
2. retorno exatamente igual ao saldo (sem aviso);
3. retorno menor (parcial) — saldo residual preservado;
4. correção de contagem com motivo obrigatório;
5. Peso: controlo com valores fora do nominal → registável; deltas apenas apresentados;
6. Tampões: transformação com destino inexistente → criação apenas após confirmação;
7. Armazém: diferença física → correção auditável, sem previsão de ocupante;
8. Reparação externa: retorno sem saída → aviso + registo, não invenção.

## 3. Testes de histórico imutável e auditoria (GLM-TST-03)

1. Correções produzem before/after + autor + motivo; original intacto;
2. void/anulação preserva o registo; estado efetivo recalculado;
3. snapshots (revisões Job On, comparações, saídas programadas) nunca reescritos por alterações
   posteriores; duplicar Job On copia tudo sem alterar a origem; consumidores mantêm o
   `job_on_revision_id` que usaram (TD-18);
4. ausência de overwrite silencioso: concorrência rejeitada com mensagem;
5. avisos distinguem facto / cálculo determinístico / possível inconsistência;
6. **Auditoria global (TD-19):** cada comando relevante cria exatamente um evento principal em
   `audit_events` (com correlação para auxiliares); evento na mesma transação do domínio; correção
   gera novo evento sem apagar o anterior; Admin filtra por ano/utilizador/módulo/ação/resultado;
   nenhum cálculo de pontuação/ranking; `audit.view`/`audit.export` exigidos;
7. **Contexto Job On (DS-04/DS-05):** Novo controlo/Comparação herdam CM/lote do Job On sem segunda
   seleção; contexto em falta/inválido bloqueia a abertura com `Corrigir ferramentas no Job On`
   (bloqueio confirmado pelo design, testado como tal); Pegamentos idem para CM/BQ/MF.

## 4. Matriz positiva e negativa de acesso (GLM-TST-04)

Os 18 cenários de `04_IDENTITY_ACCESS_AND_ADMIN_SPEC.md` §7 são implementados como pares
(positivo + negativo), incluindo obrigatoriamente:

- Operador sem `peso.aprovar` bloqueado em `/peso/responsavel` (FE + handler + comando);
- Responsável bloqueado na página/comandos do Operador;
- utilizador BQ bloqueado em Peso/Admin/Armazém… (uma rota por módulo); Job On em Modo consulta
  permanece acessível a todos os utilizadores ativos (UD-16);
- não-Responsável bloqueado em criar/editar/duplicar Job On e Definições do Job On (cenário 16);
- utilizador sem `audit.view`/`audit.export` bloqueado na tab Auditoria (cenário 17);
- História transversal limitada aos módulos autorizados do utilizador (cenário 18);
- deep link não autorizado redireciona com segurança;
- capability removida durante sessão → 403 no próximo comando;
- self-lockout recusa a última conta admin;
- título/função sem capability não concede nada;
- template desativado não concede acesso.

## 5. Fixtures e dados de teste (GLM-TST-05)

- Fixtures próprios por teste; nenhum seed operacional partilhado permanente.
- Utilizadores de teste com templates cobrindo cada linha da matriz de acesso.
- Datas fixas via `IClock` fake; cálculos do Peso validados com casos confirmados
  (ex.: densidade 5–35 °C, filename `9262T288__202604__C3__L16.pdf`).

## 6. Quality gates por unidade (GLM-TST-06)

Cada unidade do roadmap termina com:
1. build sem erros; testes dedicados: relatório `total/passed/failed/duration`;
2. testes negativos de acesso do módulo (quando aplicável);
3. verificação de arquitetura: nenhum using cruzado entre módulos; nenhum CSS local de design;
4. revisão contra acceptance criteria da unidade; evidência objetiva no gate.

## 7. Definition of Done global (GLM-TST-07)

1. Todas as regras das specs com ID na traceability matrix têm teste associado (unit/integration/E2E);
2. Matriz de acesso 18 cenários verde;
3. Fluxo Boquilhas 20→25 verde nas três camadas;
4. Calendário único e componentes canónicos aprovados no laboratório (gate E);
5. Acessibilidade: teclado + foco + AA verificados;
6. Visual regression baseline criado;
7. Nenhum hard block não classificado; nenhuma heurística de previsão/bloqueio em código;
8. Auditoria global a emitir eventos para todos os módulos, sem pontuações;
9. Landing Job On para todos os utilizadores (UD-16) verificada em E2E.

## 8. Definition of Done por módulo (GLM-TST-08)

Um módulo está done quando: workflows da sua spec executáveis; invariantes testadas; histórico/
auditoria capturados; UI conforme design system; guards FE+BE+RLS; testes negativos; acceptance
criteria da spec todos verificados; cutover readiness reportada (08_MIG §5).
