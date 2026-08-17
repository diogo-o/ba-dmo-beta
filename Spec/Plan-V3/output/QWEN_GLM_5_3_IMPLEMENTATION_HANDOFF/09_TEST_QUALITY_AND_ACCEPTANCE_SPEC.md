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

## 9. Testes documentais determinísticos — legacy recovery (GLM-TST-09)

Formato Given/When/Then (não implementar aqui; contrato para o coding agent):

**Densidade (TD-25):**
1. Given T=20.4 °C When lookupDensity Then 0.99717 (arredonda ao inteiro mais próximo).
2. Given T=4.50 When lookupDensity Then 0.99888 (valor de 5 °C; AwayFromZero).
3. Given T=4.49 When lookupDensity Then erro de domínio (abaixo de 5).
4. Given T=35.49 When lookupDensity Then 0.99305. Given T=35.50 Then erro (acima de 35).
5. Given T inteira 5..35 When lookupDensity Then exatamente os 31 valores da tabela TD-25.

**Cálculo Peso + volumes (TD-28):**
6. Given capacity C, volumeNeck=marisa M, volumePu=punção P, tipo NNPB When glass Then
   (C + M − P) × 2.4027; para PS × 2.4231.
7. Given peso em água W e densidade ρ When volume Then W/ρ; peso nulo/zero → volume null.
8. Given volume_tampao When cálculo da calote Then π·s²·(3r−s)/3 e não entra na fórmula do vidro.

**Comparação anterior (TD-30):**
9. Given controlos aprovados do mesmo molde+neckring em linhas diferentes com produção/data
   anteriores When resolve anterior Then o mais recente independentemente da linha (cross-line).
10. Given nenhum aprovado anterior When resolve Then null; deltas [null, null].

**Job On ativo/transições (TD-27):**
11. Given Job On planeado em B1 com [start,end] cobrindo `at` When Resolve(B1, at) Then devolvido.
12. Given dois Job Ons ativos sobrepostos na mesma linha When Resolve Then escolha explícita (sem auto-selection).
13. Given Job On fechado/cancelado/rascunho When Resolve Then excluído.
14. Given `planned_end_at` nulo When Resolve Then limite superior = próximo `planned_start_at` da mesma linha.
15. Given estado When transição Then apenas rascunho→planeado→em fabrico→fechado e cancelado de
    rascunho/planeado, com ator/timestamp; edições em fechado exigem change_reason.

**Lote CM (TD-26):**
16. Given criação de lote do Peso When processo/máquinas/subpasta Then guardados no lote do Peso
    (não na referência mestre); referência UNIQUE(mold+neckring) preservada.
17. Given Job On When guarda CM Then guarda IDs Ferramentas + referência/lote do Peso (mesmo CM,
    sem identidades paralelas).

**BQ 20→25 (obrigatório; ver §2.1):** manter os três níveis (unit/integration/E2E).

**Pegamentos (TD-32):**
18. Given Costura c e Contra costura n When medição Then ovalização = c − n; média = (c+n)/2.
19. Given nominal N When média fora de N±0.20 Then aviso (sem bloqueio do registo).
20. Given componentes CM/BQ/MF When gaps Then CM→BQ e BQ→MF verificados com tolerância default 0.05 (aviso).

**Access/capabilities clarificadas:**
21. Given utilizador sem `ferramentas.configure` When tenta gerir regras de verificação Then 403.
22. Given utilizador com `jobon.confirmar` When confirma ocorrência Then persistido; sem acesso a
    `jobon.edit`/`jobon.configure`.
23. Given interação de listas When teclado/Enter/Espaço Then nenhum comportamento funcional exigido
    (1 clique seleciona, 2 cliques abrem; sem shortcuts funcionais).

## 10. Testes do contrato técnico Plano-V3 (sem aumentar scope desnecessariamente)

Cobrem apenas o novo technical contract (03_ARCH §12–18; 06_DATA §12–16), sem duplicar a pirâmide
funcional:

1. **Migration checksum/idempotência:** uma migration aplicada não volta a executar (comparação
   `schema_migrations.sha256`); reaplicar o runner sobre estado já aplicado não produz duplicados;
2. **Migration failure:** falha de execução não regista a migration e propaga erro/falha (exit code
   != 0), mantendo o estado anterior aplicável;
3. **CLI routing:** `dotnet BA.Dmo.Web.dll migrate`, `bootstrap-admin` e web startup normal
   distinguem-se por argumentos, sem novo projeto CLI;
4. **No production debug bypass:** nenhum debug auth bypass / anonymous admin / debug claims no
   código de produção; doubles de teste confinados aos projetos `tests/*`;
5. **PDF abstraction:** código de negócio depende de `IPdfRenderer` (não de library concreta);
   renderer concreto testável sem dependência comercial fixa;
6. **Directory handle exception boundary:** IndexedDB persiste apenas o `FileSystemDirectoryHandle`;
   nenhum dado de domínio entra em IndexedDB; fallback de download quando API/permissão indisponível
   (06_DATA §16; 03_ARCH §17);
7. **Job On image directory (TD-23 clarificado):** (a) o diretório da imagem liga-se via File System
   Access API e o `FileSystemDirectoryHandle` recupera-se de IndexedDB com permissão válida; (b) a
   imagem da revisão apresenta-se a partir do diretório autorizado; (c) permissão perdida → UI de
   religar/reautorizar, registo do Job On intacto; (d) ficheiro em falta → estado "imagem em falta"
   recuperável; (e) attach/replace/remove geram eventos de auditoria; (f) utilizador sem `jobon.edit`
   não altera a associação da imagem; (g) nenhum binário de imagem no PostgreSQL; (h) nenhuma imagem
   do Job On no filesystem do Render; (i) nenhum dado de domínio em IndexedDB
   (06_DATA §9; modules/05 §13).

Incluídas nas unidades U-01 (CLI routing, no debug bypass), U-02 (migration checksum/idempotency/
failure), U-10/U-11 (PDF abstraction, directory handle boundary) e U-13 (Job On image directory) do
roadmap.
