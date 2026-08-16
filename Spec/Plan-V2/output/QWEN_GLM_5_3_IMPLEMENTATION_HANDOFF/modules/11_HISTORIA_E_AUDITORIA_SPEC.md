# MODULES 11 — HISTORIA E AUDITORIA SPEC

Autoridade do módulo transversal História/Auditoria. Fontes: merged §5.15, BT-03 (02_DEC §3.3),
UD-17/TD-19/TD-24 (02_DEC), `AUDITORIA_GLOBAL_HANDOFF.md` (design), 08_SUPABASE §12, design contract
§13 (History Entry).

## 1. Natureza (GLM-HIST-01)

Duas camadas complementares:
1. **História (leitura transversal)**: apresenta o histórico dos domínios por ferramenta, produção,
   linha, período e ator. **Não escreve** em dados de outros módulos; não existe tabela universal de
   histórico de negócio (BT-03): cada domínio mantém o seu histórico próprio e o módulo consome
   projeções/contratos de leitura (03_ARCH §6).
2. **Auditoria global de ações (UD-17/TD-19)**: todos os utilizadores autenticados geram eventos
   factuais append-only em `audit_events` (tabela única canónica) para cada ação de negócio relevante;
   consulta anual no Admin (tab Auditoria). Sem pontuações, rankings ou avaliação automática.

## 2. Atores e permissões (GLM-HIST-02; TD-24)

Módulo `historia` (entrada). A vista transversal aplica a autorização dos **módulos de origem**: o
utilizador apenas vê eventos/registos de módulos que o seu template autoriza. A tab Auditoria do
Admin exige `audit.view` (consulta) e `audit.export` (exportação anual). Sem capabilities próprias
do módulo `historia` na V1.

## 3. Rotas e ecrãs (GLM-HIST-03)

`/historia`: pesquisa transversal com filtros (entidade: ferramenta/lote/referência; produção; linha;
período; tipo de evento; ator); resultados agrupados por entidade com timeline/tabela; detalhe
expansível usando o componente **History Entry** canónico (07_DESIGN §4.14).
Admin→tab `Auditoria`: registo anual com filtros ano/utilizador/módulo/ação/resultado/período;
paginação 20/40/60; 1 clique seleciona, duplo clique abre o detalhe factual; exportação anual
autorizada; detalhe sem colunas de pontos/nota/ranking.

## 4. Contrato de leitura por domínio (GLM-HIST-04)

| Domínio | Fonte de histórico | Eventos incluídos |
|---|---|---|
| Boquilhas | movimentos, correções/voids, lifecycle, discrepances, utilização | criação, movimentos, correções, fechos, reaberturas, resolução de excesso |
| Peso | approval_log, change_log, comparações, decisões por CM | controlos, decisões, reaberturas |
| Ferramentas | criação/edição de referências/lotes, condição, verificações | registos mestres e alterações |
| Job On | job_on_audit_event, revisões, ocorrências de verificações | preparação, alterações, checks |
| Repair | repair_events, listas programadas, internal_repair_records | intervenções e ciclos |
| Armazém | warehouse_movements, correções de localização | entradas/saídas |
| Tampões | tampao_movements, planos | quantidades e transformações |
| Administração | `audit_events` com `moduleId=admin` | alterações administrativas (visível com `audit.view`) |

Regras: mesmo ID referenciado em todas as vistas (sem cópias); snapshot histórico separado de estado
live; datas/hora locais com timezone no detalhe; ator por nome legível; correção ≠ eliminação;
mesmo formato de ator/data/status em todos os domínios.

## 5. Comportamento de registo transversal (GLM-HIST-05)

1. Todo o evento apresentado traz: quem, quando, o quê, sobre o quê, motivo quando crítico, before/after;
2. Avisos/inconsistências aparecem como factos registados, nunca como conclusões do sistema;
3. A vista não permite edição; correções fazem-se nos módulos donos e aparecem aqui como eventos;
4. A auditoria administrativa/global segue GLM-ACC-11 (visibilidade restrita por capabilities).

## 6. Hard blocks vs avisos (GLM-HIST-06)

Hard block: autorização de módulo + restrição de dados por grants de origem (SECURITY — TD-24);
`audit.view`/`audit.export` na tab Auditoria. Não existem regras de bloqueio funcional num módulo de
leitura. Aviso: eventos de domínios ainda sem dados (estado vazio).

## 7. Testes e acceptance (GLM-HIST-07)

Unit: montagem de projeções a partir dos contratos; ordenação cronológica estável. Integration:
cada domínio expõe o contrato de leitura; eventos de auditoria emitidos por módulo. E2E: pesquisar
uma entidade e ver eventos apenas dos módulos autorizados; utilizador sem módulo não vê nada; tab
Auditoria filtra por ano/utilizador/módulo/ação/resultado. Acceptance: nenhum write; formato
consistente; correções visíveis como eventos; sem pontuações/rankings.

## 8. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-HIST-08)

**MUST PRESERVE:** histórico por domínio; quem/quando/o quê/motivo; correções rastreáveis; separação
snapshot/live; auditoria global factual anual.
**DO NOT CARRY FORWARD:** tabela universal de histórico de negócio; tabelas de auditoria separadas
por módulo/ano; edição de histórico a partir desta vista; timeline que misture estado live com
snapshots; pontuações/rankings/avaliações automáticas; eventos criados apenas no browser.

## 9. Open questions (GLM-HIST-09)

Composição visual canónica final (contrato P2.6) — gap de design do lado do design system (marcado
no contrato §19 P2.6); resolver com o design system durante U-18. Não é questão de domínio: o
contrato funcional (grants de origem + History Entry) está fechado.

## 10. Contrato técnico da auditoria global (GLM-HIST-10; UD-17/TD-19)

Unidade de registo: uma ação de negócio concluída = um evento principal (navegação/cliques sem
consequência não são registados). Ações auditáveis: criar, duplicar, guardar, corrigir, confirmar,
aprovar, rejeitar, enviar, fechar, reabrir; alterar data/ferramenta/lote/quantidade/localização/
configuração; confirmar/repor verificação; iniciar/concluir saída programada; criar/editar/desativar
utilizador ou opção administrável; tentativa de operação protegida terminada em falha/acesso negado
quando relevante para segurança/rastreabilidade.

Campos mínimos do evento: `id` imutável; `occurredAtUtc` (servidor); `year` (derivado, filtro/índice
anual); `actorUserId`; `actorNameSnapshot`; `moduleId` (ex.: jobon, peso, boquilhas, armazem, admin);
`actionCode` estável e pesquisável (catálogo versionado; ex.: `jobon.revision.saved`,
`weight.control.approved`, `bq.movement.corrected`, `warehouse.scheduled_exit.completed`,
`admin.access_template.updated`; novas ações usam códigos novos — nunca reutilizar código com
significado diferente); `entityType`/`entityId`/`entityLabelSnapshot`; `result`
(succeeded/failed/denied/corrected); `reason` quando obrigatória; `correlationId`; `jobOnId` quando
associado a produção/Job On; `revisionId` quando aplicável; `beforeSummary`/`afterSummary` apenas com
os campos necessários.

Implementação: backend é a fonte autoritativa (registo criado apenas no browser não é auditoria);
evento na mesma transação da alteração de negócio sempre que possível (outbox/correlação se
assíncrono); data/hora e utilizador obtidos da sessão no servidor; tabela única pode ser
particionada/indexada por ano mas não partida em tabelas incompatíveis; retenção e acesso segundo a
política interna; não guardar palavras-passe, tokens, cookies, credenciais, conteúdo integral de
emails, PDFs, imagens ou cargas arbitrárias.
