# MODULES 06 — FERRAMENTAS CM/MF SPEC

Autoridade funcional do módulo Ferramentas (CM/MF). Fontes: `FERRAMENTAS_REGISTO_DESIGN_BRIEF.md`,
`REPARACAO_EXTERNA_DESIGN_BRIEF.md` §4–5 (unidades CM/MF), MODELO_LOCKED §1–3, merged §5.4/5.5,
resoluções AB-01/AB-02 (02_DEC §4).

## 1. Scope e boundary (GLM-FERR-01)

**Pertence:** registo mestre de ferramentas CM e MF (referência + lotes + peças físicas), criação
de novos registos, duplicação de lotes, ficha da referência, verificações por lote, estados de
condição (new/repaired/not_repaired/sucatado) como **factos registados**.
**Não pertence:** Boquilhas (módulo próprio); posição física (Armazém); reparações (Repair);
Job On (associação apenas). CM e MF **nunca fundidos**: tipos, identidades e históricos separados.

## 2. Identidade (camadas — AB-01) (GLM-FERR-02)

1. Registo mestre: `tool_reference` (tool_type CM/MF, ref_code, technical_name, owner_plant —
   **sem processo**; o processo pertence ao lote no fluxo Peso — TD-17);
2. Lote: ocorrência física/operacional da referência (lote, quantidade, allowed_lines, desenho+revisão;
   processo NNPB/PS quando criado no fluxo Peso);
3. Peça física (CM/MF): número individual com ID imutável (`physical_pieces`);
4. Contexto operacional por linha: factos (produção, utilização, reparações) ligam-se ao registo
   estável com contexto de linha; mudança de linha = evento, não nova identidade (AB-02).

## 3. Atores e permissões (GLM-FERR-03)

Módulo `ferramentas`. Criação/edição/duplicação para utilizadores com o módulo; edições auditáveis.

## 4. Tabs e rotas (GLM-FERR-04)

`/ferramentas`; áreas `Contra moldes` e `Moldes finais` com o mesmo padrão visual (padrão Boquilhas)
e dados separados; lista de referências; ficha da referência com lotes e verificações; página
`Criar novo registo` própria (não modal).

## 5. Workflows (GLM-FERR-05)

1. **Criar novo registo**: identificação (Tipo, Referência, Nome técnico largo, Owner plant com
   default `MG — Marinha Grande`) + compatibilidade (cartões B1–C3 multi-seleção, guardada
   explicitamente) + primeiro lote (lote, **Processo do lote** NNPB/PS no fluxo Peso, quantidade,
   desenho, revisão) + campos específicos do tipo; referência+lote persistidos como **operação
   consistente**; falha do lote não deixa referência órfã sem recuperação prevista.
2. **Novo lote a partir deste**: identidade mestre read-only (Tipo, Referência, Nome técnico,
   Processo, Owner plant); dados do lote editáveis (novo número obrigatório, quantidade, linhas,
   desenho/revisão, características); regras de verificação copiadas como configuração
   (ocorrências/checks/histórico nunca copiados; `Uma vez no lote` começa sem confirmação no novo
   lote); ficha identifica origem.
3. **Editar referência mestre**: fluxo auditável próprio (alterações não retroativas a lotes).
4. **Nome técnico**: junto da Referência em listas/seletores/detalhes; participa na pesquisa; não
   assumido único sem regra confirmada.
5. **Ficha da Referência**: topo mostra Tipo, Referência, Nome técnico em destaque e Owner plant; o
   Processo é apresentado no respetivo lote quando pertence ao fluxo Peso; lista de lotes com lote,
   processo (quando aplicável), quantidade, linhas permitidas, desenho+revisão e estado atual; cada
   lote inclui a tab `Verificações` (JOB_ON_VERIFICACOES brief).

## 6. Campos e desenho (GLM-FERR-06)

Desenho: guardar nome/número explicitamente + revisão separada; permitir abrir fonte oficial quando
houver integração; **não gerar/decompor/validar automaticamente** códigos de desenho (OP 99 PMD 02/d
é referência documental). Correspondência `NNPB/PS` ↔ códigos documentais não é inferida.

## 7. Estados e utilização (GLM-FERR-07)

Condição (`state`: new/repaired/not_repaired/sucatado) muda por registo explícito com motivo/autor;
status operacional (disponivel/em_producao/em_reparacao/…) deriva de factos; utilização % é manual,
informa e nunca bloqueia (UD-12); os dois podem coexistir (em_producao E in_repair — MODELO_LOCKED).

## 8. Dados e APIs (GLM-FERR-08)

Tabelas 06_DATA §3.5. Comandos: `CreateToolReferenceWithFirstLote`, `CreateLoteFromBase`,
`EditReference`, `EditLote`, `RegisterPiece`, `SetCondition`, `RecordCheckRule`, `ConfirmCheckOccurrence`.
Queries: lista de referências (pesquisa por referência, nome técnico, lote, desenho, linha, processo
do lote, owner plant), ficha, lotes por referência, peças por lote, lookups para Job On/Armazém/Repair.

**Verificações (regras por lote):** criar/editar/desativar/reativar/resetar/apagar regras na tab
`Verificações` da ficha do lote (Chefe/Responsável autorizado); edições aplicam-se ao futuro e não
reescrevem ocorrências/histórico; reset preserva confirmações anteriores e cria nova pendência;
apagar retira a regra da configuração sem destruir ocorrências históricas (JOB_ON_VERIFICACOES brief).

## 9. Hard blocks vs avisos (GLM-FERR-09)

Hard blocks: lote duplicado na mesma referência (duplicação); campos obrigatórios; consistência
referência+lote (TECHNICAL INTEGRITY). Avisos: referência existente com mesmo código (mostrar
resultados antes de criar); nenhuma criação automática por pesquisa sem resultado.

## 10. Casos limite (GLM-FERR-10)

Referência não encontrada → oferecer `Criar novo registo` (sem criação automática); lote existente
na mesma referência → conflito explícito; nenhuma linha selecionada → validar obrigatoriedade;
desenho não definido → `Não definido`.

## 11. Testes e acceptance (GLM-FERR-11)

Unit: criação consistente (atomicidade), duplicação de lote (origem imutável), identidade por camadas.
Integration: lookups por linha/referência; verificações. E2E: criar referência+lote, duplicar lote,
ficha. Acceptance: critérios do brief §13; IDs estáveis usados por Job On/Armazém/Reparação.

## 12. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-FERR-12)

**MUST PRESERVE:** separação CM/MF; nome técnico visível; desenho+revisão explícitos; owner plant;
verificações por lote; IDs estáveis.
**DO NOT CARRY FORWARD:** identidades paralelas em Armazém/Job On/Reparação; parsing de códigos de
desenho; fusão CM+MF num único tipo; criação automática por pesquisa.

## 13. Open questions (GLM-FERR-13)

Tipos além de CM/MF/BQ; unicidade do nome técnico; campos obrigatórios por tipo; formato/unicidade
do lote por referência (brief §12 — SAFE TO DEFER com defaults visuais).

## 14. Processo no lote (GLM-FERR-14; TD-17)

O processo `NNPB/PS` é definido no lote (fluxo do módulo Peso) e herdado pelo Job On, Novo controlo
e Comparação. A referência mestre não guarda processo. A apresentação do processo acontece no lote e
nos contextos que o consomem; alterações ao processo de um lote aplicam-se apenas a registos futuros,
com snapshots preservados nos registos existentes.
