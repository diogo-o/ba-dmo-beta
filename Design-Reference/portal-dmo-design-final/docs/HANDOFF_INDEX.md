# Handoff consolidado — índice de implementação

## Ordem recomendada

1. Ler `DESIGN_IMPLEMENTATION_CONTRACT.md`; é a auditoria final que separa o que está pronto, parcial, em falta ou dependente de validação funcional.
2. Ler `CODER_IMPLEMENTATION_HANDOFF.md`; é o ponto de entrada que consolida funcionamento, dados, permissões, interações, design e aceitação.
3. Resolver os gaps P0/P1 do contrato antes de integrar tokens e componentes de `DMO_DESIGN_SYSTEM.md`/`dmo-design-system.css`.
   Antes de criar um módulo, preencher `MODULE_UI_HANDOFF_TEMPLATE.md` com os fluxos e fontes de dados confirmados.
   Consultar `DESIGN_INPUT_EXTRACTION.md` para padrões derivados das discussões funcionais e respetivo estado de confirmação.
   Para Job On, usar `JOB_ON_DESIGN_BRIEF.md` como base de evidência antes do mockup e `JOB_ON_DATA_MODEL.md` como contrato de persistência do snapshot editável.
   Para observações e verificações do Job On, aplicar também `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`.
   Para Armazém, usar `ARMAZEM_DESIGN_BRIEF.md` antes de desenhar ou implementar movimentos/localizações.
   Para reparadores de turno, usar `REPARACAO_INTERNA_DESIGN_BRIEF.md`; este fluxo é separado da Reparação externa programada.
   Para Reparação externa, usar `REPARACAO_EXTERNA_DESIGN_BRIEF.md`; BQ, CM e MF partilham o ciclo externo mas permanecem domínios separados.
   Para Tampões, usar `TAMPOES_DESIGN_BRIEF.md`; a V1 controla quantidades agregadas, campos comparáveis e transformações atómicas entre configurações.
   Para criação de ferramentas e lotes, usar `FERRAMENTAS_REGISTO_DESIGN_BRIEF.md` antes dos handoffs específicos de CM, MF ou BQ.
   Para o registo transversal de ações, usar `AUDITORIA_GLOBAL_HANDOFF.md`; aplica-se a todos os utilizadores e módulos e é consultado no Admin por ano.
4. Aplicar a shell, Login e perfil do utilizador.
5. Integrar Administração e templates de acesso.
6. Ligar Peso Operador ao motor de cálculo e persistência existentes.
7. Ligar Peso Responsável ao fluxo de aprovação e decisões por CM.
8. Aplicar os componentes comuns a Boquilhas sem alterar as regras de domínio documentadas.
9. Integrar os restantes módulos apenas depois de confirmar os respetivos contratos de dados.

## Cobertura entregue

| Área | Design | Comportamento | Documento principal |
|---|---|---|---|
| Login | `login.html` | autenticação e encaminhamento | `PORTAL_LOGIN_ADMIN_HANDOFF.md` |
| Administração | `admin.html` | utilizadores, título/função, reset, aplicações e auditoria anual de ações | `PORTAL_LOGIN_ADMIN_HANDOFF.md` + `AUDITORIA_GLOBAL_HANDOFF.md` |
| Boquilhas | `boquilhas.html` | lotes, movimentos, histórico e painel lateral | `BOQUILHAS_INTERFACE_BEHAVIOR.md` |
| Peso Operador | `peso-operador.html` | controlo, referências, comparação e histórico | `PESO_INTERFACE_HANDOFF.md` |
| Peso Responsável | `peso-responsavel.html` | aprovação normal e decisão por CM | `PESO_INTERFACE_HANDOFF.md` |
| Pegamentos | `pegamentos.html` | contexto obrigatório do Job On, medições, histórico estruturado e PDF local | `PEGAMENTOS_INTERFACE_HANDOFF.md` |
| Tampões | `tampoes.html` | consulta, quantidades, transformação técnica, planeamento, opções e histórico | `TAMPOES_DESIGN_BRIEF.md` |
| Armazém | `armazem.html` | registo, consulta, saídas programadas e histórico | `ARMAZEM_DESIGN_BRIEF.md` |
| Job On | `job-on.html` → `job-on-v48-folha-producao.html` | folha necessária para produzir; consulta não editável; edição integral do snapshot; seleção live de ferramentas/localização; Data/Máquina e CM/MP/MF/BQ prioritários | `JOB_ON_DESIGN_BRIEF.md` + `JOB_ON_DATA_MODEL.md` + `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md` |
| Reparação interna | `reparacao-interna.html` | Linha, contexto de produção, CM/MF, Histórico e Correção | `REPARACAO_INTERNA_DESIGN_BRIEF.md` |
| Moldes | `moldes.html` | módulo principal semelhante a Boquilhas; CM e MF permanecem separados | `REPARACAO_EXTERNA_DESIGN_BRIEF.md` |
| Componentes | CSS + JavaScript partilhados | botões, listas, calendário e estados | `DMO_DESIGN_SYSTEM.md` |

## Regras transversais verificadas

- Botões: preenchidos em repouso; brancos com contorno e texto da cor no hover/foco.
- Listas: um clique seleciona; duplo clique abre.
- Ações ficam fora das listas.
- Tabelas usam densidade compacta e scroll apenas quando inevitável em ecrãs pequenos.
- Registo de peso e Comparação usam estados e filtros consistentes.
- Valores decimais apresentam no máximo duas casas.
- Título/função do cabeçalho vem do perfil gerido pelo Administrador e não concede permissões.
- Cada ação de negócio relevante gera um evento factual append-only associado ao utilizador e ao módulo; não existe pontuação automática.
- Job On é a landing page de todos os utilizadores autenticados; apenas o papel/template técnico Responsável possui edição e configuração do Job On.
- Processo NNPB/PS é definido ao criar o lote no módulo Peso e é herdado pelo Job On, Novo controlo e Comparação.
- Máquinas permitidas associam funcionalmente o lote às máquinas/linhas já suportadas pelo programa.
- O Job On guarda CM/MP, MF e BQ concretos com os respetivos lotes; Peso e Pegamentos consomem essas escolhas sem nova seleção.
- No Job On, localização e disponibilidade live aparecem apenas durante a edição/substituição; o modo consulta permanece uma folha não editável.
- Cada revisão do Job On guarda contexto, componentes, campos tipados e linhas CAL num snapshot completo; duplicar copia tudo e a nova folha pode ser alterada sem modificar a origem nem as bases mestre.
- Dados numéricos/históricos de Peso e Pegamentos ficam no servidor; PDFs de Produção ficam no diretório local configurado e na subpasta definida ao criar o lote.
- Comparações são registos adicionais; não alteram o controlo aprovado usado como base.
- Decisão de Comparação é individual por CM e inclui peso/capacidade atuais e aprovados.

## Integrações que o mockup não substitui

- Autenticação e reset real de palavra-passe.
- Autorização de comandos e templates de acesso.
- Persistência e sincronização com o servidor.
- Motor de cálculo de peso, capacidade e diferenças.
- Geração de documentos e envio de email.
- Auditoria de ações e snapshots históricos.

## Ponto visual adiado

O calendário do Peso está funcionalmente normalizado, mas a passagem visual final deve reutilizar exatamente o calendário de Boquilhas para eliminar a diferença ainda visível entre módulos.
