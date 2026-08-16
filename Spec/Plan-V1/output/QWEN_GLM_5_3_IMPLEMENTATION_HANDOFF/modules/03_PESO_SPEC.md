# MODULES 03 — PESO SPEC

Autoridade funcional do módulo Peso. Fontes: `02_PESO_COMPLETE_SPEC.md`, `08_SUPABASE §8–9`,
`99_CURRENT_STATE.md` (segunda passagem), `PESO_INTERFACE_HANDOFF.md` (design atual), UD-06/UD-15,
TD-12, TD-13, TD-17, TD-21, DS-04/DS-08.

## 1. Scope e boundary (GLM-PESO-01)

**Pertence:** referências mestre, **lotes do Peso** (processo, máquinas permitidas, subpasta dos
relatórios), controlos de peso/volume (Novo controlo) e Comparação, cálculos, workflow de aprovação,
calendário de aprovações diárias, documento de produção derivado, definições Peso.
**Não pertence:** Shell/auth; Pegamentos (módulo separado, mesma área Controlo); criação de CM/lotes
de ferramenta (módulo Ferramentas/Boquilhas); seleção de ferramentas — o contexto vem do Job On.

## 2. Atores e permissões (GLM-PESO-02)

| Experiência | Regra (04_ACC §5; UD-15) | Operações |
|---|---|---|
| Operador | módulo `peso`, sem `peso.aprovar` | criar/editar/submeter Novo controlo e Comparação; gerir referências e lotes do Peso; histórico; gerar documento; preparar email |
| Responsável | `peso.aprovar` | aprovar/rejeitar/reabrir; decisões por CM em comparações; eliminar controlos elegíveis; day approvals |

Exclusividade total de páginas/comandos; sem selector manual; sem acesso cruzado (UD-06/UD-15).

## 3. Rotas e ecrãs (GLM-PESO-03)

- `/peso` (Operador): Novo controlo, Referências (+ controlos da referência e Comparação via botão
  externo `Comparar`), Histórico, Configurações.
- `/peso/responsavel` (Responsável): página única de aprovações (calendário + lista do dia + detalhe)
  e Configurações limitadas; não existe segunda vista de Comparações.
- `Comparações` não é tab principal: a Comparação cria-se a partir do Novo controlo aprovado do Job On.

## 4. Entidades e invariantes (GLM-PESO-04)

Tabelas 06_DATA §3.3. Invariantes: referência mestre UNIQUE(mold_number, neckring_number); lote do
Peso UNIQUE(referência, lote) com processo NNPB/PS obrigatório e mínimo uma máquina permitida;
unicidade do controlo (mold+neckring+produção+linha+lote+date); submissão exige ≥1 leitura; rejeição
exige nota; edição apenas em `rascunho`/`nao_aprovado`; linha do controlo pertence às allowed lines
do lote; `job_on_id`/`job_on_revision_id` obrigatórios em Novo controlo e Comparação.

## 5. Cálculos (fonte única C# — TD-12, UD-11) (GLM-PESO-05)

`lookupDensity(T)` tabela 5–35 °C (fora → erro); `volume = weight/density`;
`glass = (capacity + volumeNeck − volumePu) × constante[tipo]` (NNPB 2.4027 / PS 2.4231, editáveis em
`peso_settings`); média de glass = `diferenca_peso`; deltas vs anterior/nominal `[delta, pct]` com
`null/null` se anterior nulo/zero; calote `π·s²·(3r−s)/3`. Proibido duplicar fórmulas em JS; o live
preview chama o motor existente (nunca reimplementação). Todos os decimais apresentados com ≤2 casas.

## 6. Lotes do Peso, contexto Job On e comparações (GLM-PESO-06)

1. **Lote do Peso** (TD-17): criado/duplicado no módulo Peso com `Processo` (NNPB/PS), `Máquinas
   permitidas` (cartões B1–C3, mínimo uma) e `Subpasta dos relatórios` (nome relativo). O processo
   pertence ao lote — não à referência mestre nem ao formulário do controlo — e é herdado pelo
   Job On, Novo controlo e Comparação.
2. **Novo controlo**: inicia no contexto do Job On selecionado/ativo para a produção e referência.
   Referência, Produção, Máquina, CM e lote são herdados do Job On como dados não editáveis; o
   Operador introduz estado do molde, temperatura, leituras e notas. Sem segunda seleção de CM/lote.
3. **Job On em falta/inválido**: bloquear a abertura do controlo com mensagem acionável
   `Corrigir ferramentas no Job On`; nunca escolher outro CM automaticamente (DS-04; bloqueio
   confirmado pelo design — 02_DEC §5).
4. **Comparação**: segundo tipo de registo; criada no contexto de um Job On que já tem Novo controlo
   aprovado. Base = esse Novo controlo aprovado (imutável). O Operador introduz apenas CM atualmente
   em produção (elementos do CM/lote associado ao Job On — não permitem mudar de lote), peso e notas.
   `Data do registo da comparação` substitui `Fim da produção anterior (SAP)`.
5. **Decisão da Comparação pelo Responsável**: por CM, `Manter`/`Colocar de parte`; confirmação
   bloqueada enquanto algum CM sem decisão; justificação obrigatória se algum colocado de parte;
   decisões com operador/responsável/data e referência à revisão aprovada; o Novo controlo aprovado
   permanece imutável.
6. **Workflow**: rascunho → (submeter) pendente → aprovar/nao_aprovado; nao_aprovado → editar+submeter
   (revisão+1); aprovado/nao_aprovado → reabrir(motivo) → rascunho (revisão+1, reopened_from_status).
   Guardar pode ser automático (`A guardar`/`Guardado`/erro); Enviar para aprovação nunca é automático.
7. **Eliminar**: apenas `rascunho`/`nao_aprovado`; autor OU `peso.aprovar`; `pendente`/`aprovado`
   nunca (08_SUPABASE §9 CONFIRMED); após aprovação, correção = reabertura/revista, nunca delete.
8. **Editar referência aprovada**: exige justificação, retira a aprovação, cria nova revisão e volta
   a pedir aprovação; a revisão anterior permanece imutável.
9. Aprovação regista decisão + `day_approval`; `Enviar para produção` disponível imediatamente após
   aprovar na própria folha (não obriga navegar ao Histórico).
10. Email: destinatários por grupo de linha (B1–B3→Linha B, C1–C3→Linha C); pré-visualização +
    confirmação explícita; configuração em falta bloqueia **apenas o envio** (aprovação válida).

## 7. Histórico (GLM-PESO-07)

Contém apenas controlos enviados para aprovação; filtros mínimos: Job On, Referência, Produção, Tipo
(`Novo controlo`/`Comparação`), Estado, intervalo de datas. Um clique seleciona; duplo clique abre a
folha; sem botão `Abrir folha`. `Gerar folha de produção`/`Enviar email para produção` fora da
tabela, ativos apenas para a revisão aprovada selecionada; documento/email usam o snapshot aprovado.
Lista de referências: duplo clique encaminha para Histórico com referência (e lote, se aplicável)
pré-aplicados, indicando os filtros pré-aplicados.

## 8. Dados e APIs (GLM-PESO-08)

Comandos: `SaveReference`, `CreatePesoLote`, `DuplicatePesoLote`, `CreateControl` (Novo controlo,
contexto Job On), `SaveControl`, `SubmitControl`, `ApproveControl`, `RejectControl`, `ReopenControl`,
`DeleteControl` (policy §6.7), `CreateComparison`, `DecideComparisonPerCm`, `SaveSettings`,
`SaveDayApproval`, `GenerateDocument`, `PrepareEmail`.
Queries: referências, lotes do Peso, controlos com filtros (incl. tipo), day approvals por intervalo,
settings. `peso_comparacao_anterior` com **read path completo** (TD-13). Todo o registo guarda
`job_on_id` + `job_on_revision_id` (TD-18).

## 9. Documentos — fronteira servidor/local (GLM-PESO-09; DS-08/DS-10)

- Servidor: registo estruturado (números, resultados, estado, revisão, auditoria, Job On).
- PDF: derivado do snapshot aprovado; escrito em `diretório principal / subpasta do lote`
  (ex.: `Capacidades / 5447T173`); diretório principal em Definições; subpasta definida na criação
  do lote (nunca caminho absoluto livre); Peso e Pegamentos do mesmo lote resolvem a mesma subpasta.
- Nome do ficheiro derivado do snapshot (Produção, Referência, tipo `Peso`, revisão/data — TD-21);
  convenção legada `{mold}{neckring}__{periodo}__{line}__L{lote}.pdf` permanece instanciação válida.
- Estados distintos `Dados guardados no servidor` / `PDF guardado localmente`; falha local não desfaz
  aprovação; repetir apenas a geração/gravação é possível.
- Permissão do diretório local (File System Access) com estados próprios e recuperação; nunca
  apresentar a pasta como disponível antes de confirmar a permissão (07_DESIGN §4.17–18).

## 10. Hard blocks vs avisos (GLM-PESO-10)

Hard blocks autorizados: unicidade do controlo (TECHNICAL INTEGRITY); ≥1 leitura para submeter;
nota na rejeição; estados de edição; delete policy; linha fora das allowed lines; contexto Job On em
falta/inválido (CONFIRMED pelo design atual — encaminha correção ao Job On). Avisos apenas: diferenças
vs nominal/anterior; sem controlo anterior; alterações por guardar. Nenhum resultado de cálculo
bloqueia, recomenda ou decide.

## 11. Casos limite (GLM-PESO-11)

Temperatura fora 5–35 °C (erro de cálculo; rascunho guardável sem calcular); anterior nulo/zero →
deltas null; duplicado de identidade de controlo; alteração de processo do lote (aplica-se apenas a
novos registos; snapshots preservados); Job On sem CM válido; comparação criada antes de Novo controlo
aprovado → não permitida (mensagem); destinatários em falta → envio bloqueado, aprovação válida.

## 12. Testes e acceptance (GLM-PESO-12)

Unit: WeightCalculator completo + validadores + delete policy + herança de processo do lote.
Integration: workflow de estados + contexto Job On obrigatório + comparação (base imutável, decisões
por CM) + concorrência + day approvals. E2E: fluxo Operador→Responsável, segregação (Operador sem
aprovar; Responsável sem página Operador), geração de PDF no caminho resolvido com falha local sem
desfazer aprovação. Acceptance: exclusividade de experiências provada; cálculos idênticos para inputs
confirmados; contexto Job On presente em todos os registos.

## 13. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-PESO-13)

**MUST PRESERVE:** cálculos C# autoritativos; separação Operador/Responsável; `peso_comparacao_anterior`;
comparação imutável da base; decisões por CM; live preview; snapshot aprovado nos documentos;
fronteira servidor/local dos relatórios.
**DO NOT CARRY FORWARD:** duplicação `peso.js`; workflow cards Preencher/Guardar/Aprovação/Produção;
botão Atualizar manual; botão eliminar fora da policy; `lote padrão` (substituído pelos lotes do Peso);
processo guardado na referência mestre (TD-17); segunda seleção de CM/lote no controlo; `Procurar
comparação` no rodapé após base escolhida; caminho absoluto livre para relatórios.

## 14. Open questions (GLM-PESO-14)

- Entrega do PDF via HTTP vs pasta local (V1: pasta local configurável).

## 15. Contexto herdado do Job On (GLM-PESO-15)

Todo o Novo controlo/Comparação apresenta e guarda o identificador do Job On/Produção; o vínculo usa
o ID estável devolvido pela aplicação (texto de Referência/Produção/Máquina é apenas apresentação e
filtro). Alterar posteriormente o Job On não reescreve controlos históricos: cada registo mantém o
`job_on_revision_id` que consumiu (TD-18).
