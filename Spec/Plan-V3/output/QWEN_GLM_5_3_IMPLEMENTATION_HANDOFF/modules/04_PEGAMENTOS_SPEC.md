# MODULES 04 — PEGAMENTOS SPEC

Autoridade funcional do módulo Pegamentos. Fontes: `PEGAMENTOS_INTERFACE_HANDOFF.md` (design atual —
Job On obrigatório), merged §5.10, v2 `003_controlo.sql` (evidência), MODELO_LOCKED §8. Módulo filho
da área Controlo (02_CONTROLO; UD-14).

## 1. Scope e boundary (GLM-PEG-01)

**Pertence:** folha de controlo dimensional de pegamentos com contexto **Job On obrigatório**,
medições, tolerâncias, verificação dimensional, histórico estruturado, PDF local derivado, lista de
registos.
**Não pertence:** Peso (não fusão); Boquilhas; criação de catálogos de CM/BQ/MF; seleção de
ferramentas — as instâncias vêm do Job On.

## 2. Atores e permissões (GLM-PEG-02)

Módulo `pegamentos` (entrada). Sem capabilities próprias na V1. Autorização server-side em todos os
comandos.

## 3. Fluxo aprovado (GLM-PEG-03)

1. Abrir tab Pegamentos; 2. **selecionar/receber o Job On** da produção; 3. a aplicação carrega do
   Job On a **Referência, Produção, Máquina** e as **instâncias exatas de CM, BQ e MF**, incluindo os
   respetivos lotes; 4. `Abrir folha` só prossegue quando o Job On contém todo o contexto
   obrigatório; 5. contexto e ferramentas herdadas permanecem visíveis no topo como dados não
   editáveis; 6. folha mantém medições, limites, validações, mapa dimensional e `Imprimir/Guardar PDF`.

## 4. Contrato de dados (GLM-PEG-04)

Não criar catálogos paralelos; a resolução vem do Job On e é validada contra os catálogos do backend:

| Campo | Origem | Regra |
|---|---|---|
| Job On | Job On/planeamento | identificador estável obrigatório (`job_on_id` + `job_on_revision_id`) |
| Referência | Job On | herdada; não voltar a escolher |
| Produção | Job On | herdada; seis dígitos (AAAANN) |
| Máquina | Job On | herdada; B1–C3 |
| CM | instância/lote selecionado no Job On (proveniente do Peso) | usar exatamente o CM e lote do Job On |
| BQ | instância/lote selecionado no Job On (proveniente de Boquilhas/Reparação) | usar exatamente a BQ e lote do Job On |
| MF | instância/lote selecionado no Job On (proveniente do respetivo domínio) | usar exatamente o MF e lote do Job On |

O `COMPONENT_CATALOG` do protótipo é apenas demonstrativo: na implementação não é um seletor
alternativo dentro de Pegamentos.

## 5. Entidades, dados e regras de cálculo (GLM-PEG-05; TD-32)

06_DATA §3.4: `pegamento_controlos` + `pegamento_medicoes`. Verificação de provenance desta
passagem: **não existe implementação funcional legacy dos Pegamentos no workspace** (apenas hooks de
schema v2 `control_type='pegamento'`); as regras abaixo provêm do design baseline congelado
(`pegamentos.html` v1.9 + brief) e mantêm-se até surgir fonte funcional comprovada:

- Medições por componente (CM/BQ/MF): `Costura` e `Contra costura` (o nome interno legado `noventa`
  corresponde à Contra costura e pode ser migrado sem alterar o cálculo).
- **Ovalização = Costura − Contra costura** (por medição; nula quando falta um dos valores).
- **Média = (Costura + Contra costura) / 2**; com um só valor, a média é esse valor.
- **Tolerância nominal: nominal ± 0,20 mm** por componente (default `ovalMax` configurável por
  registo); a verificação aplica-se sobre a média de cada medição (`fora do intervalo` = aviso —
  nunca bloqueia o registo, GLM-CORE-01).
- **Boundary map (gaps entre componentes)**: CM → Boquilha e Boquilha → Molde final, tolerância
  default `gapTol` 0,05 (configurável); avisos, não bloqueios.
- Contagens de secções por omissão (demonstrativo v1.9): CM 2, Boquilha 8, Molde final 14.
- AVG por componente: médias de Costura/Contra costura/Ovalização/Média sobre as medições com dados.
- Filename do relatório: `Pegamentos_{produção6}_{referência}_{jobOnId}_relatorio.pdf` (TD-31);
  caminho resolvido `diretório principal / subpasta do lote` (§14).
- Registos antigos preservam valores e tolerâncias da altura (snapshot por registo).

## 6. Integração obrigatória com Job On (GLM-PEG-06)

Payload mínimo: `{jobOnId, reference, production, machine, cm{id,reference,lot}, bq{...}, mf{...}}`.
Ao receber o Job On: preencher contexto como não editável; carregar IDs/referências/lotes concretos;
validar que as instâncias ainda existem e são compatíveis com a máquina; manter tudo visível para
confirmação. **Não existe fallback** que permita escolher silenciosamente outra ferramenta: se faltar
CM, BQ ou MF obrigatório, ou se um lote estiver inválido, bloquear a folha com a mensagem acionável
`Corrigir ferramentas no Job On` (DS-05). Alterar o Job On substitui todo o contexto como uma
unidade; não mistura ferramentas de produções diferentes.

## 7. Lista de registos (GLM-PEG-07)

Filtros: Job On, referência/produção, máquina, data inicial e data final. Um clique seleciona
visualmente a linha; duplo clique abre a folha associada; sem botão adicional de abertura; ações fora
da lista.

## 8. Elementos removidos (GLM-PEG-08)

`+ Nova referência` acima dos tabs; cartão Base de dados em Configurações; `Guardar ficheiro para
imprimir`; `Enviar resumo`. Base de dados local do protótipo não existe na app nova (resíduos do HTML
listados no contrato §23).

## 9. Hard blocks vs avisos (GLM-PEG-09)

Hard blocks: contexto Job On completo e válido para abrir a folha (CONFIRMED pelo design atual —
encaminha correção ao Job On); integridade de registos. Avisos: medições fora de tolerância
(informativo). Nenhuma medição bloqueia o registo; registos antigos preservam valores.

## 10. Casos limite (GLM-PEG-10)

Instância do Job On entretanto inválida → bloqueio acionável; Job On alterado → contexto substituído
como unidade; adicionar/remover medições mantém cálculos originais; erro de carregamento do contexto →
mensagem com Retry, nunca folha parcial.

## 11. Testes e acceptance (GLM-PEG-11)

Unit: cálculo ovalização/tolerâncias como dados. Integration: contexto Job On obrigatório (bloqueio
sem fallback); instâncias validadas contra os domínios; histórico estruturado. E2E: receber Job On →
abrir folha → medir → guardar no servidor → PDF local no caminho resolvido; falha local sem perder o
registo. Acceptance: números com ≤2 casas; relatório identifica referência/produção/máquina/data; sem
catálogo paralelo; sem seleção alternativa de ferramentas.

## 12. MUST PRESERVE / DO NOT CARRY FORWARD (GLM-PEG-12)

**MUST PRESERVE:** cálculos/medições/tolerâncias do original; contexto obrigatório do Job On;
impressão/PDF; histórico estruturado no servidor.
**DO NOT CARRY FORWARD:** base local; ações removidas §8; catálogo demo `COMPONENT_CATALOG`; fallback
manual silencioso; seleção alternativa de CM/BQ/MF.

## 13. Open questions (GLM-PEG-13)

- Conjunto completo de campos de medição além de Costura/Contra costura: `UNRESOLVED — NO
  AUTHORITATIVE SOURCE FOUND` (pesquisado: pegamentos.html v1.9, brief, verified knowledge, v2
  003_controlo.sql — apenas Costura/Contra costura documentados). Estrutura de medições extensível;
  confirmar com owner durante U-11.

## 14. Persistência e pasta do relatório (GLM-PEG-14; DS-08)

- Servidor: registo estruturado (Job On, Produção, Referência, Máquina, IDs/lotes CM-BQ-MF, medições,
  resultados, estado, revisão, auditoria).
- PDF enviado/impresso para Produção: gerado a partir do snapshot da folha e guardado no
  computador/local configurado; diretório principal em Definições; subpasta = definida na criação do
  lote no Peso; caminho resolvido apresentado `Capacidades / 5447T173` (componente Resolved Report
  Path); Pegamentos não permite escolher outra pasta para o mesmo Job On/lote nem cria pasta concorrente.
- Nome do ficheiro com dados do Job On (pelo menos Produção, Referência, tipo `Pegamentos`,
  revisão/data — TD-21).
- Mostrar separadamente `Dados guardados no servidor` e `PDF guardado localmente`; falha local não
  apaga o registo numérico.
