# MODULES 00 — MODULE CATALOG (canónico)

Fonte única do catálogo de módulos e capabilities (TD-10). Implementação: `ModuleCatalog` em código
(fonte de verdade) + espelho DB (`module_catalog_mirror`) para ordem/visualização na Administração.
A Administração atribui apenas entradas deste catálogo; identificadores fora dele são rejeitados.

## 1. Taxonomia (GLM-CAT-01)

- **Módulo**: unidade funcional com domínio, dados, rotas e grants próprios.
- **Área/domínio funcional**: agregação com significado de negócio — `Controlo` (UD-14). Não tem
  grants próprios; existe quando pelo menos um filho está autorizado; filhos atribuíveis separadamente.
- **Experiência**: variante de interface de um módulo determinada por capability — Peso Operador/
  Responsável (UD-15). Não é submódulo nem módulo separado; as experiências são mutuamente exclusivas.
- **Capability**: `{moduleId}.{ação}`; concede operações específicas além da entrada.
- **Tab**: entrada de navegação derivada de grants (módulo, área ou experiência).

## 2. Catálogo canónico (GLM-CAT-02)

| moduleId | Nome | Tipo | Ordem | Rota inicial | Capabilities |
|---|---|---|---|---|---|
| `jobon` | Job On | módulo (contexto central de produção; **landing global** — UD-16) | 05 | `/jobon` | `jobon.view` (todos os utilizadores ativos), `jobon.edit`, `jobon.configure`, `jobon.confirmar` (TD-20) |
| `boquilhas` | Boquilhas | módulo | 10 | `/boquilhas` | — |
| `controlo` | Controlo | **área/domínio funcional** (sem grant próprio — UD-14) | 20 | primeira entrada filha autorizada | — |
| `peso` | Peso | módulo (filho de Controlo; experiências Operador/Responsável — UD-15) | 21 | `/peso` ou `/peso/responsavel` por `peso.aprovar` | `peso.aprovar` |
| `pegamentos` | Pegamentos | módulo (filho de Controlo) | 22 | `/pegamentos` | — |
| `ferramentas` | Ferramentas (CM/MF) | módulo | 40 | `/ferramentas` | — |
| `armazem` | Armazém | módulo | 50 | `/armazem` | — |
| `reparacao_interna` | Reparação Interna | módulo | 60 | `/reparacao-interna` | `reparacao_interna.corrigir` |
| `reparacao_externa` | Reparação Externa | módulo **único atribuível** (UD-13) com Boquilhas, Contra Moldes, Moldes Finais, Envios, Histórico e Definições | 70 | `/reparacao-externa` | — |
| `tampoes` | Tampões | módulo | 80 | `/tampoes` | — |
| `historia` | História | módulo (leitura transversal; limitada aos módulos autorizados — TD-24) | 90 | `/historia` | — |
| `admin` | Administração | módulo (alinhado à direita; inclui tab Auditoria — UD-17) | 99 | `/admin` | `admin.gerir`, `audit.view`, `audit.export` |

Regras:
1. Regra da área: `controlo` visível quando pelo menos um filho (`peso`, `pegamentos`) está
   autorizado; filhos não autorizados não aparecem; área sem filhos não aparece (UD-05/UD-14).
2. `peso.aprovar` exige o módulo `peso`; a capability sozinha não concede entrada.
3. Ordem do espelho DB pode ser ajustada pela Administração dentro dos módulos ativos; não altera
   autorização. Job On permanece primeiro por ser a landing global.
4. Landing: sempre Job On (UD-16); Boquilhas deixou de ser landing (substituição registada em 02_DEC).
5. `reparacao_externa` atribui-se como um único módulo; as tabs internas Boquilhas/Contra Moldes/
   Moldes Finais/Envios/Histórico/Definições não são módulos separados (UD-13).

## 3. Capabilities — catálogo (GLM-CAT-03)

| capabilityId | Módulo | Concede | Fonte |
|---|---|---|---|
| `jobon.view` | jobon | entrada/consulta do Job On (todos os utilizadores ativos) | CODER_HANDOFF §11 (design atual) |
| `jobon.edit` | jobon | criar, duplicar, Modo edição, substituir ferramentas, alterar campos/datas, guardar revisão (papel/template técnico Responsável) | CODER_HANDOFF §11 |
| `jobon.configure` | jobon | gerir Definições do Job On (catálogos de opções) | CODER_HANDOFF §11 |
| `jobon.confirmar` | jobon | confirmar ocorrências de verificações; não concede edição estrutural | TD-20 (nomenclatura técnica; o brief declara capability “própria”) |
| `peso.aprovar` | peso | aprovar/rejeitar/reabrir controlos; experiência Responsável; delete elegível | UD-06; 02_PESO; 08_SUPABASE §9 |
| `admin.gerir` | admin | todas as operações administrativas | 04_ADMIN |
| `audit.view` | admin | consultar a tab Auditoria (registo anual de eventos) | AUDITORIA_GLOBAL §8 |
| `audit.export` | admin | exportar o registo anual autorizado | AUDITORIA_GLOBAL §8 |
| `ferramentas.configure` | ferramentas | gerir regras de verificação na ficha da ferramenta/lote (Chefe/Responsável autorizado); server-side | TD-33 |
| `reparacao_interna.corrigir` | reparacao_interna | corrigir registos de reparação interna | REPARACAO_INTERNA brief §12 |

Validação server-side: prefixo da capability = moduleId; capability pertence ao módulo; entradas
desconhecidas descartadas na normalização e rejeitadas em escritas de templates. Verificação de
colisão semântica executada nesta passagem (TD-33): nenhuma colisão entre as capabilities listadas.

## 4. Divergências reconciliadas (GLM-CAT-04)

- 3 módulos (família A) vs 7 módulos (v2) → catálogo novo de 12 entradas incluindo `admin` (C19/C25);
- `entry_capability` v2 não transportado como regra geral: entrada = presença do módulo no template
  (modelo família A); o Job On é a exceção atual com capabilities explícitas (TD-20);
- `revision` envelopes v2 não transportados (BT-06);
- `firebase_uid` rejeitado (TD-09);
- módulo único `repair` v2 → dois módulos distintos (reparacao_interna/reparacao_externa);
- landing Boquilhas/Admin → landing Job On universal (UD-16; DS-01);
- auditoria Admin isolada → auditoria global única `audit_events` + tab Auditoria (UD-17/TD-19).

## 5. Extensão futura (GLM-CAT-05)

Novo módulo = alteração de código aprovada (catálogo + spec + migrations + testes, conforme
03_ARCH §8). A UI Admin passa a poder atribuí-lo sem alterações adicionais.
