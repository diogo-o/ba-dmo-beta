# Portal DMO — handoff técnico completo para implementação

Estado: especificação funcional e visual V1  
Destinatário: programador responsável pela implementação real  
Regra: os HTML deste pacote demonstram a interface e as interações; não substituem domínio, autorização, persistência, auditoria, cálculo, PDF ou email.

## 1. Começar aqui

O Portal DMO é uma aplicação única com módulos separados. A shell, a identidade, os componentes e as regras de interação são partilhados, mas os domínios não devem ser fundidos.

Ordem mínima de leitura:

1. este documento;
2. `DMO_DESIGN_SYSTEM.md`;
3. o brief específico do módulo;
4. o HTML correspondente;
5. `dmo-design-system.css` e `dmo-interactions.js`.

Quando o mockup, um texto auxiliar e o brief parecerem contraditórios, prevalece:

1. regra funcional explícita mais recente no brief;
2. regra transversal do Design System;
3. mockup visual;
4. dados fictícios presentes no HTML.

Não inferir regras de negócio a partir de valores de demonstração.

## 2. Modelo mental da aplicação

### Shell única

- Login único.
- Header global com logótipo, nome do módulo, descrição curta, nome do utilizador e título/função.
- Navegação e módulos disponíveis dependem do template de acesso do utilizador; a landing page após autenticação é sempre Job On.
- `Definições` fica no extremo direito das tabs quando existir.
- O título/função visível no header é texto livre gerido pelo Administrador. Não concede permissões.
- A autorização deve ser validada no servidor em cada comando; esconder botões não é segurança.

### Módulos e responsabilidade

| Módulo | Responsabilidade principal | Não deve fazer |
|---|---|---|
| Login | autenticar e encaminhar | decidir permissões no cliente |
| Administração | utilizadores, título, templates e aplicações | executar fluxos operacionais |
| Boquilhas | lotes BQ, movimentos, reparação e tracking | gerir CM/MF |
| Peso — Operador | criar controlos, calcular e enviar para aprovação | aprovar o próprio controlo |
| Peso — Responsável | analisar, aprovar/rejeitar e decidir comparações por CM | registar leituras operacionais |
| Job On | folha com a informação necessária para produzir | ser catálogo mestre de ferramentas |
| Armazém | localização, entradas, saídas e recolhas programadas | guardar estado técnico que pertence à ferramenta |
| Reparação interna | intervenções CM/MF durante produção | substituir a reparação externa programada |
| Reparação externa | ciclo de envio e retorno de BQ/CM/MF | fundir os três domínios |
| Moldes | registo de CM e MF em áreas separadas | combinar CM e MF num único tipo |
| Tampões | quantidades por configuração e planeamento | identificar peças individualmente |
| Pegamentos | controlo dimensional contextualizado | criar os catálogos de CM/BQ/MF |

## 3. Regras visuais globais obrigatórias

### Tokens

Usar apenas variáveis de `dmo-design-system.css`. Não introduzir cores, alturas, raios, sombras ou espaçamentos isolados na página sem primeiro os transformar num token reutilizável.

Base visual:

| Uso | Token/valor canónico |
|---|---|
| Marca principal | `--dmo-brand-600: #3c73a8` |
| Texto principal | `--dmo-text: #172d42` |
| Texto secundário | `--dmo-muted: #64778a` |
| Página | `--dmo-page: #f6f9fc` |
| Cartão | `--dmo-card: #fff` |
| Superfície suave | `--dmo-subtle: #f1f6fa` |
| Contorno | `--dmo-border: #d9e6f2` |
| Sucesso | `--dmo-success: #527c72` |
| Aviso | `--dmo-warning: #a97943` |
| Perigo | `--dmo-danger: #9a625d` |
| Altura de campo/filtro | `40px` |
| Botão normal | mínimo `36px` |
| Botão em linha de filtros | `40px` |
| Seta de paginação | `36 × 36px` |
| Ação mobile | mínimo `44px` |
| Raio de campo | `8px` |
| Raio de cartão | `12px` |

### Botões

- Repouso: fundo preenchido com a cor semântica, contorno igual e texto branco.
- Hover e foco: fundo branco, contorno e texto com a cor que preenchia o botão.
- Não aplicar apenas brilho/brightness.
- Disabled: visual neutro e sem ação.
- Perigo usa o vermelho moderado do sistema, nunca vermelho vivo.
- Botões contextuais ficam fora da tabela/lista.
- A largura acompanha o texto; evitar botões largos sem necessidade.

### Campos

- Altura canónica de 40px.
- Largura proporcional ao conteúdo esperado: máquina 2 caracteres, lote normalmente curto, percentagem 3 algarismos, data com largura própria.
- Textarea só quando o conteúdo é realmente longo.
- Placeholder deve exemplificar o formato ou a relação esperada.
- Foco: contorno de marca mais halo discreto.
- Datas aparecem no fim da linha quando essa ordem foi definida no fluxo.

### Cartões e densidade

- Um cartão representa um contexto ou tarefa, não cada valor isolado.
- Evitar espaços vazios e cartões-resumo sem ação ou significado.
- Informação crítica tem maior contraste/tamanho; metadados têm contraste secundário.
- Não esconder informação funcional necessária apenas para tornar o layout menor.

## 4. Contrato universal de listas

Todas as listas operacionais seguem o mesmo comportamento:

1. um clique seleciona uma linha;
2. a seleção fica visualmente mais escura;
3. ações externas passam a atuar sobre a linha selecionada;
4. duplo clique abre o detalhe/folha/registo associado;
5. teclado: linha focável, `Enter` abre e `Espaço` seleciona;
6. seleção não executa uma mutação;
7. mudar filtros limpa uma seleção que deixou de estar visível;
8. após corrigir/eliminar, atualizar a lista e a paginação;
9. estado vazio explica por que não existem resultados;
10. estado de erro preserva filtros e permite repetir.

Paginação:

- limites disponíveis: 20, 40 e 60;
- mostrar total, página atual e total de páginas;
- setas junto às restantes ações da lista, alinhadas;
- anterior/seguinte ficam disabled nos limites;
- o servidor deve paginar listas grandes; o cliente não deve carregar tudo por defeito.

## 5. Contrato universal de filtros

- Pesquisa textual e filtros específicos vivem num cartão compacto.
- Botão `Limpar filtros` repõe o estado inicial e volta à primeira página.
- A lista, contadores e cartões-resumo respondem aos mesmos filtros quando pertencem ao mesmo contexto.
- Filtros devem poder ser representados na URL quando o utilizador é encaminhado de outra página.
- Datas usam intervalo `Desde`/`Até`; validar `Desde <= Até`.
- Dropdowns usam a aparência e hover do sistema; não usar menus brancos enormes para três opções.
- Quando uma Referência encaminha para Histórico, abrir com a referência já aplicada.

## 6. Contrato universal de calendários

- Reutilizar exatamente o componente canónico, não recriar por módulo.
- Cabeçalho: mês/ano e setas anterior/seguinte.
- Semana com sete colunas estáveis.
- Dia com registos: indicador discreto.
- Dia selecionado: fundo de marca e texto branco.
- Um clique filtra/seleciona o dia.
- Ação `Mostrar todas as datas/dias` remove o filtro diário.
- O calendário não pode dominar a página: largura aproximada de 300–340px em desktop quando está ao lado da lista.
- Em mobile passa para cima da lista.
- No Peso, substituir qualquer variante restante pelo mesmo calendário de Boquilhas.

## 7. Estados, feedback e auditoria

### Registo global de ações

- Aplicar `AUDITORIA_GLOBAL_HANDOFF.md` a todos os módulos e a todos os utilizadores autenticados.
- Cada comando relevante cria um evento append-only associado ao utilizador, módulo, ação, entidade, data/hora e resultado.
- A auditoria é criada pelo backend e acompanha a transação do domínio; não depende de JavaScript no browser.
- O Admin disponibiliza consulta anual, filtros, detalhe e exportação autorizada.
- O sistema regista ações factuais; não calcula pontuações, rankings ou avaliações automáticas.

### Operações assíncronas

Todo comando deve ter:

1. estado inicial;
2. loading e proteção contra duplo envio;
3. sucesso com mensagem curta;
4. erro com causa utilizável;
5. atualização dos dados afetados;
6. auditoria com utilizador e data/hora.

### Correções

- Corrigir não apaga silenciosamente a realidade anterior.
- Guardar valor anterior, valor novo, justificação, operador e timestamp.
- Eliminar movimentos exige confirmação e deve respeitar a política de auditoria do domínio.
- Um registo histórico é snapshot e não deve mudar quando o estado mestre atual muda.

### Estado visual

- `Pendente`, `Aprovado`, `Não aprovado`, `Por decidir`, `Manter` e `Colocar de parte` usam a mesma paleta moderada em todas as páginas.
- Cor nunca é o único indicador; manter texto explícito.

## 8. Login e Administração

### Login

- Email e palavra-passe.
- Erro genérico para credenciais inválidas.
- Após autenticação, encaminhar todos os utilizadores para Job On.
- Administrador também entra em Job On e abre Administração através da navegação quando necessário.
- Não persistir palavra-passe no browser.

### Administração

- Gerir utilizadores, título/função visual, templates de acesso e aplicações.
- A tab `Auditoria` consulta o histórico anual de ações de todos os módulos.
- Filtros mínimos: ano, utilizador, módulo, ação, resultado e período.
- Lista canónica: um clique seleciona e duplo clique abre o detalhe do evento.
- Paginação: 20, 40 ou 60 linhas.
- Apenas capacidades `audit.view` e `audit.export` autorizam consulta e exportação.

### Administração

Lista de utilizadores com pesquisa, template/perfil, estado e título/função.

Ações:

- criar/ativar/desativar utilizador;
- alterar template de acesso;
- editar título/função livre mostrado no header;
- reset de palavra-passe;
- gerir módulos/capacidades do template.

Separar:

- identidade de autenticação;
- perfil interno;
- título visual;
- template e capacidades.

O título `Responsável de qualidade`, `Chefe`, `Engenheiro`, etc. é uma variável do perfil e não uma role técnica.

## 9. Boquilhas

Documento de autoridade: `BOQUILHAS_INTERFACE_BEHAVIOR.md`.

### Estrutura

- Tabs: `Registo`, `Boquilhas`, `Histórico`, `Definições`.
- Remover tab `Fabrico`.
- Side panel fixo em todas as páginas do módulo.
- Botões antigos do header desaparecem; o side panel oferece o contexto de linhas.

### Side panel

Cada linha B1–C3 mostra:

- linha;
- referência completa ativa;
- lote(s) dessa referência;
- quantidade de BQ;
- estado temporal quando necessário.

Clicar na referência abre o respetivo lote/Job On conforme o contexto definido. Menu `…`:

- com referência: `Substituir` e `Remover`;
- sem referência: `Adicionar`;
- menu compacto, integrado no fundo escuro, sem branco excessivo.

Regra crítica: uma linha não pode ter duas referências diferentes. Pode ter dois lotes da mesma referência. Se dados existentes violarem a regra, mostrar alerta no cartão e pedir para remover/substituir uma referência; nunca abrir um prompt nativo do browser.

### Novo lote

- Abre inline na mesma página e pode ser fechado/cancelado.
- Não apresenta `Fabricar/Reparar`.
- Campos compactos: Boquilha, Lote, Total, Utilização inicial e Data no fim.
- Não existe dropdown `Linha associada`; máquinas/linhas são escolhidas nos cartões `Linhas permitidas`.
- Utilização é tempo de vida, não quantidade.
- Botões de linha são selecionáveis e mostram check legível.

### Lote ativo

- Resumo do lote e estado atual permanecem disponíveis.
- Mostrar movimentos apenas desse lote; o Histórico serve visão geral e comparação.
- Retirar `Contagem reconciliada`.
- Entrada/Saída usa cabeçalho grande com Referência, Lote e Linha.
- Formulário compacto: Data, Quantidade e Motivo na primeira área; Detalhe abaixo; Observações apenas quando necessário; retirar Material/Trabalho.
- Entrada e Saída na lista podem ter fundos subtilmente diferentes.

### Histórico

Campos mínimos: Referência, Lote, Movimento (`Entrada`/`Saída`), Quantidade, Saldo, Reparador, Linha, Data/hora e Operador.

Filtros: pesquisa, data/período, movimento, reparador, ficheiro/estado quando aplicável e limite 20/40/60.

## 10. Peso e Volume

Documento de autoridade: `PESO_INTERFACE_HANDOFF.md`.

### Origem dos dados

- Processo `NNPB/PS` é escolhido na criação do lote no módulo Peso. Não é pedido novamente no Novo controlo nem na Comparação.
- Máquinas permitidas e Processo pertencem ao contexto do lote do Peso usado nessa produção.
- O Job On fornece a ligação operacional à Referência, Produção e Máquina e identifica a instância concreta de CM usada nessa produção, incluindo o lote. Exemplo: `Produção 202601 · CM 5447 · Lote 4`.
- O lote do Peso referido pelo Job On fornece Processo e restantes dados técnicos; o controlo não volta a selecionar CM ou lote.
- Todo Novo controlo e toda Comparação guardam e mostram a referência ao Job On daquela produção.
- Valores calculados vêm do motor de cálculo do domínio.
- Todas as casas decimais apresentadas são limitadas a duas.

### Operador

- Existem dois tipos de registo: `Novo controlo` e `Comparação`.
- `Novo controlo` inicia no contexto do Job On da produção e controla os valores previstos/introduzidos para essa produção.
- `Comparação` é posterior: mede CM que já estão em produção e compara-os com o Novo controlo aprovado associado ao mesmo Job On.
- Cria e edita controlos.
- Leituras CM podem ser adicionadas/removidas realmente.
- Em cada leitura mostrar CM, peso em água e peso estimado em vidro em tempo real.
- `Calcular` recalcula os campos atuais; não é apenas uma decoração.
- Resultados em cartões compactos e tabela por CM.
- Botões `Adicionar leitura`, `Remover leitura`, `Calcular` e `Enviar para aprovação` são consistentes entre Novo controlo e Comparação.
- Retirar progress cards de processo e ações duplicadas de produção.

Campos compactos: máquina, lote e temperatura. Aumentar Notas; reduzir `Fim da produção anterior (SAP)` e `Peso médio anterior (SAP)`.

### Referências

Lista compacta sem scroll horizontal em desktop normal. Um clique seleciona; duplo clique encaminha para Histórico com a referência aplicada. Ações `Editar`, `Novo controlo` e `Comparar` ficam fora da lista.

Editar uma referência aprovada:

- exige justificação;
- retira o estado aprovado;
- cria nova revisão;
- volta a pedir aprovação.

Criar a Referência define a identidade mestre. Criar o lote no Peso inclui NNPB/PS e máquinas permitidas; estes valores são herdados pelos controlos através do contexto do Job On.

Na criação do lote existe também `Subpasta dos relatórios`. É um nome relativo, por exemplo `5447T173`, resolvido por baixo do diretório principal configurado, por exemplo `Capacidades / 5447T173`.

### Comparação

Compara CM que já estão em produção com a média do Novo controlo aprovado ligado ao mesmo Job On/produção.

- Não altera o controlo aprovado base.
- Mantém Job On, Produção e contexto da Referência ativa no topo.
- Mantém Data do registo da comparação, leituras, resultados e botão Calcular.
- Retirar `Procurar comparação` do rodapé depois da base já estar escolhida.
- Enviar para aprovação cria registo adicional de comparação.

Responsável decide cada CM individualmente. Tabela mínima:

- CM;
- Peso atual;
- Capacidade atual;
- Média aprovada;
- Capacidade aprovada;
- Diferença;
- Decisão `Manter` ou `Colocar de parte`.

### Responsável

- Não regista pesos.
- Vê calendário, lista pendente, detalhe completo e histórico.
- Aprova/rejeita controlos normais.
- Em comparações decide CM a CM.
- Rejeição exige nota.

Depois de aprovado, pode gerar folha ou enviar email para produção. Destinatários são escolhidos automaticamente pela linha/máquina e configurados em Definições; assunto/mensagem aceitam variáveis documentadas.

## 11. Job On

Documentos de autoridade: `JOB_ON_DESIGN_BRIEF.md` e `JOB_ON_VERIFICACOES_DESIGN_BRIEF.md`.

Definição: **Job On é a folha onde a equipa obtém toda a informação necessária para produzir**.

Job On é também a landing page global do Portal. Todos os utilizadores autenticados podem consultar; apenas o papel/template técnico `Responsável` pode editar. Validar no backend:

- `jobon.view`: todos os utilizadores ativos;
- `jobon.edit`: apenas Responsável;
- `jobon.configure`: apenas Responsável;
- criação, duplicação, substituição de ferramenta, alteração de datas/campos e gravação de revisão exigem `jobon.edit`;
- confirmação operacional de verificações usa capability própria e não concede edição estrutural da folha.

Ocultar `Editar folha`, `Criar Job On` e `Definições` aos restantes perfis, mas nunca usar a ocultação como controlo de autorização.

### Hierarquia

Informação instantânea:

1. Data início/fim;
2. Máquina/Linha;
3. Referência e Produção;
4. MP/CM, MF e BQ, incluindo referência e lote;
5. imagem do artigo.

Informação secundária permanece presente: PU, CAL, AN, ARR, PI, CS, TP, FO, parâmetros, notas e verificações.

### Comportamento

- Abre por defeito em `Modo consulta`, com aparência de folha técnica.
- `Editar folha` ativa campos apenas para utilizadores autorizados.
- Guardar fecha edição e volta a consulta.
- Não cria ferramentas mestre; escolhe lotes existentes dos módulos de domínio.
- Upload de imagem permite pré-visualização e armazenamento associado à folha/Referência.
- Planeamento é tab separada com calendário compacto.
- Um clique numa produção seleciona; duplo clique abre a folha.

### Criar

- `Novo em branco` cria template vazio para Referência sem Job On anterior.
- `Duplicar anterior` procura o Job On anterior da mesma Referência, copia conteúdo e limpa/atualiza a Data.
- `Duplicar histórico selecionado` usa explicitamente a linha escolhida.
- Criar Job On fica fora dos cartões/listas.
- A data planeada pode ser alterada; atualizar também a marcação no calendário e preservar auditoria.
- Data início e Data fim do Job On são a única fonte da marcação/range no calendário. Guardar uma alteração cria nova revisão, atualiza a projeção do calendário e preserva as datas antigas no histórico.
- O Histórico permite primeiro filtrar uma Referência e ver todas as suas Produções. Um clique seleciona e duplo clique abre; dentro da Produção, o utilizador pode consultar as revisões imutáveis.

### Ferramentas

Ao editar MP/CM, MF ou BQ:

- pesquisa por Referência/número;
- opções de lote são filtradas por Referência e Máquina;
- a lista mostra apenas lotes compatíveis existentes;
- selecionar lote atualiza os dados apresentados na folha;
- não existir resultado significa que não existe ferramenta compatível registada; não inventar.

Depois de o Job On ser guardado, as escolhas de CM/MP, MF e BQ são instâncias concretas, com IDs e lotes. Peso e Pegamentos consomem essas escolhas como dados não editáveis. Não voltam a filtrar para escolher uma alternativa. Se uma ferramenta obrigatória estiver ausente ou inválida, o utilizador corrige o Job On.

O Job On tem dois modos:

- `Consulta`: folha estritamente não editável; não adiciona, remove, substitui nem duplica.
- `Edição`: todos os campos da folha ficam editáveis, incluindo PU, CAL, AN, ARR, PI, CS, TP, FO, quantidades e notas; ativa também duplicação e `Alterar` em CM/MP, MF e BQ.

Ao alterar uma ferramenta, abrir lista canónica filtrada pela Referência da ferramenta e Máquina do Job On. Permitir refinar por lote, localização/contexto e estado. A linha agrega Referência, Lote, Nome técnico, compatibilidade, posição do Armazém, contexto atual, estado técnico e `% de uso`. Posição vem do Armazém; estado/% vêm do domínio da ferramenta. Selecionar não cria Saída nem altera o Armazém; apenas associa o ID/lote ao rascunho do Job On.

### Persistência do Job On

- Implementar o esquema e os limites de ownership descritos em `JOB_ON_DATA_MODEL.md`.
- Guardar uma fotografia completa por Job On/revisão, não apenas referências a entidades mestre.
- Revisões guardadas são imutáveis. Guardar uma edição cria uma revisão e respetivos filhos novos, atualizando apenas o apontador de revisão corrente; nunca altera os valores da revisão anterior.
- O snapshot inclui o contexto e todos os grupos visíveis. CAL guarda cada linha (`Elemento`, `Valor`, `Qtd. máquina`); PI guarda Pinças, Diâmetro e Notas; os restantes grupos guardam todos os seus campos, quantidades e notas.
- CM/MP, MF e BQ guardam `toolId`/`lotId` e também snapshot legível dos valores usados na produção.
- Estado técnico, `% de uso`, posição e movimentos permanecem nas bases das ferramentas/Armazém; são consultas live durante a seleção e não são copiados como estado atual do Job On.
- Editar o Job On altera apenas o snapshot/revisão do Job On. Nunca altera a ficha mestre nem a localização da ferramenta.
- Nenhum estado, localização, percentagem ou incompatibilidade bloqueia a escolha/gravação; apresentar aviso e deixar a decisão ao utilizador autorizado.
- Duplicar copia o snapshot completo. Apenas o novo ID, Produção e datas são preparados para o novo fabrico; todos os campos copiados podem ser alterados antes de guardar.
- Exemplo de aceitação: duplicar `202601 · 5447T173` para `202602` copia a configuração de PI/CAL/pinças usada em 202601 e permite substituí-la livremente em 202602, sem alterar 202601.
- Aprovações, históricos e documentos guardam o `job_on_revision_id` utilizado, para que uma correção posterior não altere retroativamente o que foi visto ou emitido.
- A interface não tem de imitar fotograficamente a folha histórica. Deve apresentar os campos pela forma mais clara para consulta e edição, mantendo todos os valores acessíveis através das tabelas normalizadas do snapshot.
- Dropdowns de negócio evolutivos são data-driven e administrados em Definições por Família/Campo. Permitir adicionar, editar, ordenar e desativar; nunca apagar retroativamente valores guardados nos snapshots. Ver `job_on_field_option` em `JOB_ON_DATA_MODEL.md`.

### Verificações

Regras configuradas na ficha da ferramenta/lote geram ocorrências no Job On:

- `Uma vez neste lote`: aparece até ao primeiro check desse lote;
- `Por fabrico`: cria ocorrência em cada Job On/fabrico.

Guardar quem confirmou e quando. O responsável pode desativar, reativar, apagar ou fazer reset conforme permissões e regras do brief.

A confirmação de uma ocorrência é manual e só existe depois de persistida. Não inferir a partir de outros módulos; guardar `completion_source=manual_job_on`, utilizador, data/hora e revisão.

## 12. Armazém

Documento de autoridade: `ARMAZEM_DESIGN_BRIEF.md`.

### Limite

Armazém regista onde a ferramenta está e os movimentos. `% vida`, sucata, arquivo e estado técnico pertencem ao domínio da ferramenta; no Armazém existe apenas Observações quando necessário.

### Entrada

- Escolher tipo CM/MF/BQ.
- Encontrar a ferramenta nos catálogos existentes.
- Registar posição e observações.
- Uma entrada numa posição ocupada é bloqueada pelo estado registado do sistema.
- A aplicação não consegue detetar fisicamente uma ferramenta não registada naquela posição; não apresentar essa inferência como facto.

### Saída

- Ao confirmar a saída, libertar imediatamente a posição registada.
- Destino: Produção ou Reparação.
- Para Reparação, reparador vem das associações do domínio da ferramenta.

### Saída programada

Manager/Reparação cria lista de lotes a recolher. Armazém vê e pode imprimir a lista. Operador confirma cada ferramenta com check. Quando todas estiverem confirmadas:

- finalizar saída;
- libertar posições;
- guardar operador e data/hora;
- criar registo histórico.

Quando regressam e recebem entrada no Armazém, fechar o ciclo e guardar dados de entrada. Linha concluída usa estado visual discreto verde/cinza.

### Consulta

Filtros, paginação 20/40/60 e últimos movimentos. Alertas apenas com base nos dados registados: duplicação lógica de posição ou ferramenta sem contexto operacional registado.

## 13. Reparação interna

Documento de autoridade: `REPARACAO_INTERNA_DESIGN_BRIEF.md`.

- Operador escolhe Linha B1–C3 em cartões no topo, ocupando toda a largura disponível sem overflow.
- O sistema carrega automaticamente Referência, Produção e Job On ativos da linha naquele dia.
- Depois escolhe `CM` ou `MF` e introduz o número individual.
- Guardar cria intervenção com linha, produção, referência, lote, tipo, número, operador e timestamp.
- Últimos registos ficam abaixo, nunca ao lado do seletor principal.
- Histórico permite filtros, seleção, duplo clique e correção auditada.
- Sem produção ativa, mostrar estado explícito e impedir associação automática falsa.

## 14. Reparação externa e Moldes

Documento de autoridade: `REPARACAO_EXTERNA_DESIGN_BRIEF.md`.

- Reparação externa acontece antes/fora da produção e é diferente da reparação interna.
- Boquilhas, CM e MF podem partilhar a shell e o ciclo de saída/retorno.
- CM e MF são domínios separados, mesmo quando partilham Referência.
- `Contra moldes` e `Moldes finais` são tabs do módulo, não dois botões de estado dentro da página.
- A tab ativa é preenchida; a inativa começa branca/outline.
- Não mostrar cartões `Produções ativas` se não forem necessários ao fluxo externo.
- Listas usam a regra universal; ações ficam fora.

## 15. Tampões

Documento de autoridade: `TAMPOES_DESIGN_BRIEF.md`.

- Unidade de consulta é a configuração técnica, por exemplo diâmetro + calote.
- Diâmetro e calote são escolhidos de opções configuradas, não texto livre repetido.
- Operador tem liberdade para adicionar/remover quantidades e alterar configuração.
- Selector de estado: `Enchidos` / `Por encher`.
- Alterar de calote 4mm para 7mm é transformação atómica: subtrair da origem e adicionar ao destino no mesmo comando/auditoria.
- Planeamento permite consultar quantidades disponíveis antes da produção.
- Histórico mostra configuração anterior/nova, movimento, quantidade, saldo antes/depois, operador e data.
- Interface mobile-first para consulta no telemóvel.

## 16. Pegamentos

Documento de autoridade: `PEGAMENTOS_INTERFACE_HANDOFF.md`.

- Ao abrir a tab, selecionar/receber o Job On da produção.
- A folha só abre depois de o Job On fornecer Referência, Produção, Máquina e as instâncias/lotes obrigatórios de CM, BQ e MF.
- CM vem do lote escolhido no Job On a partir do Peso; BQ do lote escolhido no Job On a partir de Boquilhas; MF do lote escolhido no Job On a partir do respetivo domínio.
- Pegamentos não apresenta uma segunda escolha de ferramentas. Se faltar uma ferramenta, bloquear e encaminhar para correção do Job On.
- Campo `Contra costura` substitui a designação antiga incorreta.
- Lista Referências: clique seleciona, duplo clique abre; sem botão `Abrir folha selecionada`.
- Remover base de dados/importar/apagar local e ações de impressão duplicadas.
- Manter apenas `Imprimir / Guardar PDF` quando aplicável.

## 17. Origem dos dados e ownership

| Dado | Fonte de verdade esperada | Consumidores |
|---|---|---|
| Utilizador/template/capacidades | Administração/identidade interna | shell e comandos |
| Título/função | perfil interno | header |
| Referência Peso | catálogo mestre do Peso | Peso, Job On, Pegamentos |
| Lote do Peso, processo NNPB/PS e máquinas permitidas | criação/gestão de lotes no Peso | Peso, Job On, Pegamentos |
| Lote CM | módulo Peso/CM definido no domínio | Job On, Pegamentos, Armazém |
| Lote BQ | Boquilhas | Job On, Pegamentos, Armazém |
| Lote MF | módulo MF; manual apenas onde explicitamente temporário | Job On, Pegamentos, Armazém |
| Produção ativa por linha | Job On/planeamento de produção | side panel, reparação interna |
| Localização atual | Armazém | pesquisa e recolhas |
| Reparadores permitidos | definição do domínio da ferramenta | reparação e armazém |
| Job On da produção | Job On/planeamento | Novo controlo, Comparação e restantes consumidores do contexto de produção |
| CM/BQ/MF concretos e respetivos lotes da produção | escolhas guardadas no Job On | Peso, Pegamentos e folha Job On |
| Novo controlo aprovado do Job On | Peso | Comparação e produção |
| Destinatários de email | Definições do Peso por grupo de linhas | envio de controlos aprovados |

Não copiar estes dados para campos paralelos sem necessidade. Guardar IDs e snapshots históricos apropriados.

### Fronteira servidor/local dos relatórios

- O servidor guarda os dados estruturados de Peso e Pegamentos, incluindo números, resultados, estado, revisão, auditoria e `jobOnId`.
- Os PDFs aprovados/enviados para Produção são guardados no computador/local configurado; não são a fonte primária do histórico.
- `Definições` fornece o diretório principal, por exemplo `Capacidades`.
- A criação do lote no Peso fornece a subpasta relativa, por exemplo `5447T173`.
- O caminho resolvido é `diretório principal / subpasta do lote` e é partilhado por Peso e Pegamentos associados ao mesmo Job On/lote.
- Os nomes dos ficheiros são derivados do snapshot do Job On/controlo, nunca introduzidos como identificação paralela pelo operador.
- A interface distingue sucesso de persistência no servidor de sucesso de escrita local. Uma falha local não desfaz uma aprovação nem apaga dados numéricos.
- O histórico do servidor continua disponível noutro computador; o PDF só abre onde a pasta local/partilhada estiver acessível e autorizada.

### Fluxo ponta a ponta: Job On → Peso/Pegamentos → Produção

1. No Peso, criar o lote e definir Processo NNPB/PS, máquinas permitidas e `Subpasta dos relatórios`.
2. No Job On, identificar Referência, Produção e Máquina; em `Modo edição`, escolher CM/MP, MF e BQ concretos com os respetivos lotes.
3. A lista de escolha do Job On consulta disponibilidade live: posição/contexto pelo Armazém e estado técnico/% de uso pelo domínio da ferramenta.
4. Guardar o Job On persiste IDs/lotes associados. Não cria um movimento de Armazém.
5. `Novo controlo` do Peso recebe desse Job On a Produção e o CM/lote exatos. Não apresenta outro seletor de CM.
6. `Pegamentos` recebe do mesmo Job On a Produção e os CM, BQ e MF/lotes exatos. Não apresenta seletores alternativos.
7. Peso e Pegamentos guardam os valores estruturados no servidor com `jobOnId` e snapshot das ferramentas usadas.
8. Depois do estado/aprovação aplicável, gerar o PDF de Produção a partir do snapshot, resolver `diretório principal / subpasta do lote` e escrever localmente.
9. O nome do ficheiro é derivado do Job On/snapshot. O servidor regista o evento de geração/envio e respetivo resultado, sem tratar o PDF como substituto dos dados numéricos.
10. Se uma ferramenta estiver em falta/inválida, corrigir o Job On. Se a pasta local falhar, preservar o registo aprovado e permitir repetir apenas a geração/gravação do PDF.

Não implementar:

- associação apenas por texto de Referência/Produção sem `jobOnId` e IDs das ferramentas;
- escolha automática de outro lote dentro de Peso ou Pegamentos;
- cópia da posição, estado técnico ou `% de uso` para propriedades live do Job On;
- gravação de PDF no servidor como única fonte do histórico;
- caminho absoluto introduzido livremente na criação do lote.

## 18. Contratos técnicos recomendados

### Comandos

Cada mutação recebe:

- ID do agregado/registo;
- versão/concurrency token quando aplicável;
- dados explícitos da ação;
- justificação em correções/reaberturas;
- identidade obtida da sessão, nunca enviada como verdade pelo cliente.

### Respostas

Devolver:

- estado atualizado;
- versão nova;
- mensagens de validação por campo;
- evento/audit ID quando útil;
- dados necessários para atualizar cartões, lista e paginação sem recarregar informação não relacionada.

### Histórico

Para movimentos e aprovações guardar pelo menos:

- tipo de evento;
- agregado e registo afetado;
- antes/depois ou delta suficiente;
- operador;
- timestamp UTC e apresentação no fuso local;
- justificação;
- origem da ação.

## 19. Responsividade e acessibilidade

- Desktop: conteúdo usa a largura disponível; evitar uma coluna estreita perdida no centro de ecrãs grandes.
- Side panel fixo só quando a largura o permite; mobile usa drawer/área recolhível.
- Tabelas podem ter scroll horizontal apenas em ecrãs pequenos e só depois de compactar colunas.
- Campos e botões têm labels acessíveis.
- Não usar `prompt`, `alert` ou `confirm` nativos para fluxos operacionais; usar modal do sistema.
- Foco visível, ordem de tab lógica e ações possíveis por teclado.
- Alvos mobile mínimos de 44px.
- Respeitar contraste AA.

## 20. Critérios de aceitação da implementação

### Shell

- [ ] Header igual em todos os módulos.
- [ ] Título do perfil vem do Administrador.
- [ ] Template controla módulos e comandos no servidor.
- [ ] Definições alinhadas à direita.

### Componentes

- [ ] Botões seguem filled → inverted hover.
- [ ] Campos/filtros têm 40px.
- [ ] Listas seguem clique/duplo clique/teclado.
- [ ] Paginação oferece 20/40/60.
- [ ] Calendário é o mesmo componente em todos os módulos.
- [ ] Modais substituem prompts nativos.

### Dados

- [ ] UI não inventa entidades ou estados.
- [ ] Snapshots históricos permanecem imutáveis.
- [ ] Correções têm justificação e auditoria.
- [ ] Identidade vem da sessão autenticada.
- [ ] Datas e números respeitam formatação PT-PT e duas casas quando aplicável.

### Fluxos críticos

- [ ] Admin entra em Administração.
- [ ] Operador Peso não aprova; Responsável não regista leituras.
- [ ] Comparação não altera controlo aprovado base.
- [ ] Uma linha BQ não aceita referências diferentes.
- [ ] Dois lotes da mesma Referência BQ são permitidos.
- [ ] Job On abre em consulta e contém toda a informação necessária para produzir.
- [ ] Saída do Armazém liberta posição ao confirmar.
- [ ] Reparação interna usa produção ativa da linha.
- [ ] CM e MF permanecem separados.

## 21. Não implementar a partir dos mockups

Os seguintes elementos são demonstração e precisam de serviço real:

- utilizadores, emails e palavras-passe fictícios;
- números, datas, saldos e contagens de exemplo;
- timeouts e mensagens simuladas;
- fórmulas JavaScript simplificadas;
- geração de PDF demonstrativa;
- envio por `mailto:`;
- persistência local;
- permissões apenas por ocultação de componentes.

## 22. Pontos ainda por confirmar

Não bloquear a estrutura V1, mas marcar como configuração/decisão pendente:

- destino definitivo de imagens do artigo e documentos;
- política exata de eliminação versus anulação por módulo;
- fonte final de produção ativa enquanto a integração não estiver concluída;
- catálogo definitivo de MF para Pegamentos;
- notificações reais e respetivos canais;
- regras finais de arquivo/retenção;
- passagem visual final do calendário do Peso para o componente canónico.

Se uma regra de domínio não estiver explicitamente documentada, não adivinhar: implementar o estado `não disponível`, registar a dependência e pedir confirmação.

## 23. Desvios conhecidos nos mockups — não copiar para produção

Esta passagem encontrou resíduos demonstrativos que não alteram a especificação:

| Ficheiro | Resíduo | Implementação correta |
|---|---|---|
| `boquilhas.html` | usa `confirm()` em fecho/cancelamento | modal canónico com ação primária e cancelar |
| `admin.html` | reset usa `confirm()` | modal canónico com utilizador, consequência e confirmação |
| `peso-operador.html` | reabertura usa `prompt()` | modal com campo obrigatório de justificação |
| `armazem.html` | fecho de registo usa `confirm()` | modal canónico apenas quando existem dados por perder |
| `reparacao-v2.html` | ação demonstrativa usa `alert()` | navegação/estado real; nunca alerta nativo |
| `pegamentos.html` | conserva código legado de importação/base local e confirmações | não expor Base de dados local, Importar/Apagar, `Enviar resumo` ou impressão duplicada |
| alguns HTML | tokens e CSS repetidos inline | na aplicação real importar a folha central e usar componentes partilhados |
| Peso | calendário visual ainda não é cópia exata do de Boquilhas | usar o componente canónico único |

Os HTML são úteis para composição, prioridade e fluxo. O coder deve usar os documentos para decidir comportamento final quando encontrar estes resíduos.

## 24. Definition of Done por página

Uma página não está terminada apenas porque se parece com o mockup. Só fica concluída quando:

1. usa a shell e tokens globais sem duplicar valores crus;
2. respeita capabilities e autorização no servidor;
3. carrega dados reais e apresenta loading/empty/error;
4. formulários têm validação cliente e servidor coerente;
5. comandos são idempotentes ou protegidos contra duplo envio;
6. listas, filtros, seleção, duplo clique, teclado e paginação funcionam;
7. ações críticas têm modal, justificação e auditoria quando aplicável;
8. estado atualizado aparece sem exigir refresh manual;
9. desktop e mobile foram testados;
10. testes cobrem caminho feliz, validação, permissão, concorrência e falha do serviço;
11. não existem `alert()`, `prompt()` ou `confirm()` nativos;
12. o conteúdo e a ordem visual permitem executar a tarefa sem consultar outra página desnecessariamente.
