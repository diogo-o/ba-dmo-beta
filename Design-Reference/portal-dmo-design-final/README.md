# Portal DMO — pacote de design e handoff

## Páginas

- `login.html`: entrada única do Portal DMO.
- `admin.html`: gestão de utilizadores, aplicações e auditoria anual de ações por utilizador/módulo.
- `boquilhas.html`: mockup de Boquilhas.
- `peso-operador.html`: Novo controlo, Referências, Comparação, Histórico e configuração da pasta local de PDFs.
- `peso-responsavel.html`: aprovações de controlos e decisões individuais por CM.
- `pegamentos.html`: contexto obrigatório do Job On, ferramentas/lotes herdados, medições, histórico estruturado e PDF local.
- `armazem.html`: registo, consulta, saídas programadas e histórico do Armazém.
- `job-on.html`: entrada canónica que abre `job-on-v48-folha-producao.html`.
- `job-on-v48-folha-producao.html`: folha operacional com toda a informação necessária para produzir; abre em consulta não editável e, em edição, permite substituir ferramentas através de uma lista live que combina posição do Armazém com estado/% do domínio.
- `reparacao-interna.html`: registo rápido por Linha/CM/MF, últimos registos, Histórico e Correção.

## Base partilhada

- `dmo-design-system.css`: tokens e componentes visuais.
- `dmo-interactions.js`: comportamento canónico de listas e calendário.
- `logo_recolored(1).png`: identidade visual utilizada.

## Documentação

- `docs/DESIGN_IMPLEMENTATION_CONTRACT.md`: auditoria final do design para uma fresh build; avalia foundation, CSS architecture, componentes, shell, cobertura, contradições, gaps e critérios de aceitação sem criar regras de negócio.
- `docs/CODER_IMPLEMENTATION_HANDOFF.md`: ponto de entrada completo para o programador; consolida arquitetura funcional, módulos, fontes de dados, regras visuais, interações, auditoria e critérios de aceitação.
- `docs/DMO_DESIGN_SYSTEM.md`: especificação técnica de cores, componentes, estados, grelhas, interações e acessibilidade.
- `docs/MODULE_UI_HANDOFF_TEMPLATE.md`: template obrigatório para explicar o funcionamento da UI de cada módulo novo.
- `docs/DESIGN_INPUT_EXTRACTION.md`: padrões de UI extraídos das ideias funcionais, separados entre regras globais e candidatos por módulo.
- `docs/JOB_ON_DESIGN_BRIEF.md`: leitura dos exemplos Job On, reorganização proposta e questões ainda por confirmar.
- `docs/JOB_ON_DATA_MODEL.md`: contrato de persistência do snapshot integral e editável; separa BD Job On, ferramentas e Armazém e define a duplicação por revisão.
- `docs/JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`: configuração de verificações na ficha da ferramenta/lote, frequência Por fabrico, checks, reset e histórico no Job On.
- `docs/ARMAZEM_DESIGN_BRIEF.md`: contrato de UI de posições/movimentos, Consulta, alertas e Saídas programadas; regras de domínio e persistência ficam para os serviços responsáveis.
- `docs/REPARACAO_INTERNA_DESIGN_BRIEF.md`: registo rápido de CM/MF por Linha e produção ativa, Histórico e correções auditáveis.
- `docs/TAMPOES_DESIGN_BRIEF.md`: consulta mobile, campos técnicos comparáveis, transformação de quantidades entre configurações, planeamento e histórico.
- `docs/FERRAMENTAS_REGISTO_DESIGN_BRIEF.md`: criação da Referência mestre e primeiro lote, Nome técnico, desenho e duplicação controlada de novos lotes.
- `docs/PESO_INTERFACE_HANDOFF.md`: regras funcionais e técnicas de Peso/Controlo.
- `docs/BOQUILHAS_INTERFACE_BEHAVIOR.md`: regras funcionais e técnicas de Boquilhas.
- `docs/PORTAL_LOGIN_ADMIN_HANDOFF.md`: autenticação, perfil, título/função e gestão administrativa.
- `docs/AUDITORIA_GLOBAL_HANDOFF.md`: evento append-only por ação, associação a utilizador/módulo, consulta anual no Admin, filtros, permissões e critérios de aceitação.
- `docs/HANDOFF_INDEX.md`: índice de cobertura, ordem de integração e pontos ainda dependentes do programa real.
- `docs/PEGAMENTOS_INTERFACE_HANDOFF.md`: Job On obrigatório, ferramentas concretas herdadas, persistência no servidor e PDF local.

## Nota de implementação

Os HTML são mockups funcionais. Dados, fórmulas e atrasos simulados não são implementação de produção. Devem ser ligados aos serviços, permissões, perfis e motores de cálculo existentes.

Peso e Pegamentos guardam o histórico estruturado no servidor. Os PDFs enviados à Produção são gerados localmente no diretório principal configurado e na subpasta relativa definida ao criar o lote no Peso. O Job On é a fonte da Produção, Referência, Máquina e ferramentas/lotes concretos usados por ambos os módulos.

O Login encaminha todos os utilizadores autenticados para Job On. Todos podem consultar; apenas o papel/template técnico Responsável pode criar, duplicar, editar, guardar revisões e gerir Definições. Administração continua acessível ao Administrador através da navegação.

Todas as ações de negócio relevantes criam um evento de auditoria associado ao utilizador e ao módulo. O Admin pode consultar e exportar o registo anual autorizado; o sistema não atribui pontos nem produz rankings automáticos.
