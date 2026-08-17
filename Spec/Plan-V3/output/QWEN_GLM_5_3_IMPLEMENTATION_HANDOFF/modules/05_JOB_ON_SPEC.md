# MODULES 05 — JOB ON SPEC

Autoridade funcional do módulo Job On. Fontes: `JOB_ON_DESIGN_BRIEF.md` (design atual),
`JOB_ON_DATA_MODEL.md` (contrato técnico de persistência — TD-18), `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`,
CODER_HANDOFF §11, UD-16, TD-20, TD-23.

## 1. Scope e boundary (GLM-JOB-01)

O Job On é a **folha onde a equipa consulta toda a informação necessária para executar uma produção**
(USER: “informação de todo lado”) e o **contexto central de produção** consumido por Peso e
Pegamentos. Agrega contexto e ferramentas escolhidas nos módulos de domínio; **não cria nem altera
registos mestres** de CM/MP, MF, BQ ou restantes ferramentas; não é formulário permanentemente
editável. Depois de guardado, identifica a Referência, Produção, Máquina e os CM/MP, MF e BQ exatos
(com lotes) usados pela produção.

## 2. Atores e permissões (GLM-JOB-02; UD-16; TD-20)

| Capability | Quem | Operações |
|---|---|---|
| `jobon.view` | todos os utilizadores ativos | Modo consulta: folha, planeamento, histórico |
| `jobon.edit` | papel/template técnico Responsável | criar, duplicar, entrar em Modo edição, substituir ferramentas, alterar campos/datas, guardar revisão |
| `jobon.configure` | papel/template técnico Responsável | Definições (catálogos de opções por Família/Campo) |
| `jobon.confirmar` | operador autorizado do Job On | confirmar ocorrências de verificações (não concede edição estrutural) |

Ocultar `Editar folha`/`Criar Job On`/`Definições` aos restantes perfis nunca substitui a validação
server-side. O título livre do perfil não concede estas capabilities.

## 3. Tabs e rotas (GLM-JOB-03)

`/jobon` (landing global): tabs `Planeamento`, `Job On` (folha), `Histórico`; `Definições` à direita
(apenas `jobon.configure`). A entrada direta abre a tab `Job On`. Indicador de modo distinguível sem
cores agressivas (azul-cinza suave em consulta; âmbar/castanho suave em edição).

## 4. Modos (GLM-JOB-04; DS-06)

| Modo | Objetivo | Comportamento |
|---|---|---|
| `Modo consulta` | ler a folha necessária à produção | estritamente não editável; não adiciona/remove/substitui/duplica; informação live de disponibilidade não ocupa a folha |
| `Modo edição` | preparar/corrigir o Job On | todos os campos da folha editáveis (contexto, quantidades, notas e todas as famílias: PU, CAL, AN, ARR, PI, CS, TP, FO); ativa duplicação e `Alterar` em CM/MP, MF e BQ |

`Guardar alterações` fecha a edição e devolve a folha ao modo consulta; cria **nova revisão** — nunca
UPDATE destrutivo da revisão anterior (TD-18).

## 5. Entidades e dados (GLM-JOB-05; TD-18)

Família canónica `job_on*` de `JOB_ON_DATA_MODEL.md` (detalhe em 06_DATA §3.6): `job_on`,
`job_on_revision` (imutáveis), `job_on_component`, `job_on_component_field` (tipados),
`job_on_component_row` (CAL), `job_on_verification_occurrence`, `job_on_audit_event`,
`job_on_field_option`. Regras:
1. Cada gravação cria revisão com snapshot completo; a folha em consulta lê a revisão corrente.
2. Consumidores (Peso, Pegamentos, PDFs, aprovações) guardam o `job_on_revision_id` exato.
3. `planned_start_at/planned_end_at` são a fonte única do calendário; alterar datas atualiza o Job On
   e a projeção na mesma transação/evento, preservando as datas antigas na revisão anterior e na auditoria.
4. **Lifecycle (TD-27; GAP-003 RESOLVED)**: estados `rascunho`, `planeado`, `em fabrico`, `fechado`,
   `cancelado`. Transições: `rascunho → planeado` (Gravar do Responsável com datas planeadas);
   `planeado → em fabrico` (início confirmado/edição autorizada, com timestamp); `em fabrico →
   fechado` (fecho autorizado; regista `fechado_em`); `rascunho/planeado → cancelado` (motivo +
   ator). Efeitos: `rascunho` nunca é resolvido como ativo; `fechado`/`cancelado` excluídos da
   resolução de atividade e de novos consumos; edições sobre `fechado` exigem `change_reason` e
   criam nova revisão. Estados readiness do v2 (attention_required/blocked/…) não transitam como
   estados de domínio (apenas avisos).
5. **Atividade — lookup canónico `Resolve(line, at)` (TD-27)**: candidatos = Job Ons com
   `machine = line`, estado ∈ {`planeado`, `em fabrico`} e `at` dentro de `[planned_start_at,
   planned_end_at]`; se `planned_end_at` for nulo, o limite superior é o próximo `planned_start_at`
   do mesmo `machine` (provenance legacy: v2 derivava `data_saida` como a próxima `data_entrada` da
   mesma linha). 1 candidato → devolvido; múltiplos no mesmo intervalo → **escolha explícita**
   (nunca auto-selection); 0 → “Sem Job On ativo para esta Linha/data” e os consumidores bloqueiam
   de forma acionável. Consumidores: Reparação Interna, Boquilhas (painel lateral), Peso, Pegamentos.
6. Dropdowns de negócio evolutivos (materiais, tipos, versões…) são data-driven via `job_on_field_option`,
   geridos em Definições por Família/Campo; desativar preserva valores em revisões antigas.
7. Imagem do artigo guardada por revisão (`image_asset_id` — TD-23); substituir/remover com
   confirmação e auditoria; sem cópia entre produções sem regra explícita.

## 6. Workflows (GLM-JOB-06)

1. **Criar Job On** (no cartão do dia selecionado, dentro da lista): `Novo em branco` (sem cópias por
   aproximação), `Duplicar anterior` (mesma Referência; origem mostrada; indisponível com explicação
   se não existir), `Duplicar histórico selecionado` (linha escolhida explicitamente). Rascunho abre
   imediatamente a vista Folha; só persiste com `Guardar`.
2. **Duplicação**: copia o snapshot completo (componentes, campos, linhas CAL, ocorrências aplicáveis);
   atribui novo ID, nova Produção/datas, `copied_from_job_on_id`; a data de origem é o único valor não
   reutilizado (dia futuro selecionado aplica-se; sem dia, fica por preencher); todos os valores
   copiados podem ser alterados antes de guardar; origem imutável; nenhum valor copiado é atualizado
   silenciosamente com dados live (a interface pode propor/avisar; o utilizador decide).
3. **Alterar datas**: cartão inline; apenas a data escolhida; auditoria mínima (anterior, nova, autor,
   data/hora); não cria novo Job On; falha mantém valores; após fecho, correções seguem fluxo auditável.
4. **Substituir ferramenta (CM/MP, MF, BQ)** em Modo edição: `Alterar` abre lista filtrada pela
   Referência da ferramenta e Máquina do Job On (Tool Availability Picker); refinar por lote,
   localização/contexto, estado técnico e disponibilidade; cada resultado mostra Referência, Lote,
   Nome técnico, Máquina compatível, Posição atual, Localização/contexto (`Armazém`, `Produção`,
   `Reparação` ou não registada), Estado técnico (`Novo`, `Reparado`, `Por reparar`), `% de uso` e
   disponibilidade (ex.: `CM 5447 · Lote 3 · Posição 2421 · Por reparar · 38% uso`); posição vem do
   Armazém; estado/% vêm do domínio da ferramenta; composição read-only; 1 clique seleciona, duplo
   clique abre a ficha no módulo autoritativo; `Associar lote selecionado` confirma no rascunho.
   Selecionar/pesquisar **não cria movimento de Armazém nem reserva** a ferramenta. Localização,
   estado, compatibilidade ou % de uso geram aviso — nunca bloqueiam a associação/gravação (a decisão
   é do utilizador autorizado). Falha parcial por fonte é distinguida (`Localização indisponível`).
5. **Comparar com produção anterior**: separa snapshot anterior (ferramentas/lotes/valores/notas da
   altura) e estado atual (disponibilidade, reparações posteriores, utilização); valores anteriores
   são candidatos; copiar não transforma histórico em verdade atual.

## 7. Verificações (GLM-JOB-07)

Regras configuradas na ficha da ferramenta/lote (módulo Ferramentas): `Uma vez neste lote` /
`Por fabrico`. O Job On materializa ocorrências e recebe checks (`jobon.confirmar`): contador de
pendentes por família; check persiste antes de remover das pendentes; guarda operador/data/hora e
`completion_source=manual_job_on`; falha mantém pendente; abrir/ler não conta como confirmação;
confirmadas ocultas por defeito (`Mostrar confirmadas`); duplicar Job On gera ocorrências do novo
Job On, não copia checks antigos; substituir o lote preserva o snapshot das ocorrências anteriores e
carrega a configuração do novo lote sem reutilizar checks de outro lote. Não existe `Adicionar
verificação` dentro do Job On; `Gerir na ficha da ferramenta` para o Chefe autorizado.

## 8. Planeamento e calendário (GLM-JOB-08)

Tab Planeamento: calendário canónico compacto (~300px) + lista do dia. Dia passado: registos de
entrada/saída desse dia (factos registados; nada deduzido pela ausência de Job On); dia presente:
registos do dia; dia futuro: lista vazia + `Criar Job On para este dia`. Mudar de mês não seleciona
dia. Linha da lista: data, Referência, Produção, Máquina, resumo de atenção quando existir facto.
1 clique seleciona; duplo clique abre a folha; filtros por período, Referência e Máquina; sem seleção
automática em ambiguidade.

## 9. Histórico em dois níveis (GLM-JOB-09)

1. **Produções da Referência**: selecionar uma Referência lista os Job Ons por Produção (ex.: 202601,
   202602) com datas, máquina, revisão corrente, total de revisões e estado; 1 clique seleciona;
   duplo clique abre a produção.
2. **Revisões da Produção**: dentro da produção, revisões imutáveis; abrir revisão antiga mostra
   exatamente o snapshot então guardado.
Filtros por Referência, Produção e Máquina; intervalo de datas; sem inferir equivalência entre
máquinas. A lista de Produções não substitui o histórico de revisões.

## 10. Hierarquia visual (GLM-JOB-10)

Ordem confirmada: 1) Data início/fim; 2) Máquina/Linha; 3) Referência e Produção; 4) MP/CM, MF e BQ
com referência/lote; 5) imagem do artigo; restante (PU, CAL, AN, ARR, PI, CS, TP, FO, parâmetros,
notas, verificações) com contraste secundário. Contexto fixo: Referência larga; Produção, Máquina,
Secções, Gota, Tipo, Processo (herdado do lote do Peso — mostrado, não redefinido), Peso e Paragem
compactos; datas no fim. `Processo` vem do lote do Peso; o operador não o redefine na folha.

## 11. Hard blocks vs avisos (GLM-JOB-11)

Hard blocks: autorização (edição/configuração sem `jobon.edit`/`jobon.configure` — SECURITY);
integridade do snapshot (revisão imutável — TECHNICAL INTEGRITY). Avisos (WARNING ONLY): estado
técnico/posição/% de uso na substituição; valor copiado incompatível (visível, guardável se o
utilizador autorizado decidir); faltas de informação (`Não definido`). Readiness “attention/blocked”
do MODELO_LOCKED 6b são avisos descritivos — nunca impedem o registo (GLM-CORE-01).

## 12. Casos limite (GLM-JOB-12)

Sem Job On anterior para `Duplicar anterior`; dia futuro sem planeamento; várias produções candidatas
(escolha explícita); valor copiado incompatível mantido com atenção; erro de carregamento das fontes
live (nunca lista vazia como válida); lote substituído com verificações pendentes; revisão fechada
alterada (exige `change_reason`); falha ao guardar (rascunho preservado).

## 13. Testes e acceptance (GLM-JOB-13)

Unit: criação de revisões (imutabilidade), duplicação (tudo copiado; origem imutável; datas novas),
geração de ocorrências por frequência. Integration: calendário lê `planned_*` (sem segunda cópia de
datas); alteração de datas auditada; consumidores guardam `job_on_revision_id`; permissões
jobon.view/edit/configure/confirmar; catálogos de opções preservam revisões antigas. E2E: landing
Job On para todos os perfis; consulta vs edição; substituição de ferramenta com lista live sem criar
movimentos; verificações confirmadas manualmente. Acceptance: critérios do brief §14 + JOB_ON_DATA_MODEL §6.

## 14. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-JOB-14)

**MUST PRESERVE:** folha completa com todas as famílias; snapshot ≠ live; origem das duplicações;
auditoria de alterações; verificações regra≠ocorrência; modo consulta não editável; IDs estáveis nas relações.
**DO NOT CARRY FORWARD:** criação implícita de ferramentas por pesquisa; cópia silenciosa de valores
live sobre snapshots; inferência de compatibilidade por desenho; disponibilidade live em modo consulta;
edição por utilizador sem `jobon.edit`; calendário com fonte de datas própria.

## 15. Open questions (GLM-JOB-15)

Itens remanescentes do brief §13, todos `UNRESOLVED — NO AUTHORITATIVE SOURCE FOUND` (pesquisado
nesta passagem: JOB_ON_DESIGN_BRIEF §13, JOB_ON_DATA_MODEL, legacy v2 jobon, verified knowledge,
mockup job-on-v48 — nenhuma fonte autoritativa os resolve): significado oficial das siglas das
famílias; campos obrigatórios por família; diferença exata entre stock e quantidade em máquina;
relação de `Tipo` com processo; ferramentas obrigatórias por produção; regras oficiais de
compatibilidade/apertos; prioridade/comentário/anulação nas verificações. A ordenação canónica de
“anterior” para `Duplicar anterior` usa, por default técnico, produção/data descendente com desempate
estável (sem fonte que defina outra ordem). Não bloqueiam a estrutura V1: estrutura visual sem
regras automáticas até decisão do owner.
