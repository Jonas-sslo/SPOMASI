# Relatório Técnico — EcoCidade Bio

**Plataforma digital para monitoramento ambiental e análise biotecnológica do Córrego das Palmeiras, município fictício de Santa Aurora**

## Resumo

Este relatório apresenta a EcoCidade Bio, plataforma digital proposta para integrar medições de sensores, resultados de DNA ambiental (eDNA) e registros de cidadãos sobre o Córrego das Palmeiras. O trabalho aplicou Interação Humano-Computador, arquitetura da informação, acessibilidade (WCAG 2.2, nível AA), Internet das Coisas, sistemas de informação e biotecnologia a um problema ambiental urbano fictício.

Foi construída uma base de dados SQLite com as 9 medições do enunciado, temperatura simulada pelo grupo, 171 leituras IoT sintéticas, 6 resultados de eDNA e 8 ocorrências fictícias. As regras didáticas de alerta classificam o **P3 (trecho industrial) como prioridade de investigação em todas as coletas**: em 16/10/2026 ele ultrapassou os três limiares (pH 5,8; turbidez 62 NTU; oxigênio dissolvido 3,4 mg/L) e foi o único ponto com cianobactéria detectada e peixe nativo não detectado. O P2 (urbano) passou de “Sem alerta” a “Prioridade” em duas semanas. O P1 (nascente) ficou sem alerta.

**Palavras-chave:** monitoramento ambiental; cidades inteligentes; Internet das Coisas; acessibilidade digital; DNA ambiental.

## 1. Introdução

A informação ambiental só ajuda uma cidade quando chega a quem decide e a quem convive com o problema. Este projeto aplica tecnologias da informação a um problema ambiental urbano: a poluição de um córrego cujos dados existem, mas estão espalhados em arquivos sem integração e sem interface para o cidadão.

O estudo de caso integra os conteúdos da disciplina em uma única solução. A Interação Humano-Computador (IHC) define para quem o sistema existe. A usabilidade, a acessibilidade e a arquitetura da informação definem como ele é usado. Cidades inteligentes, Internet das Coisas (IoT) e sistemas de informação definem de onde vêm e como circulam os dados. Biotecnologia e bioinformática acrescentam evidências biológicas. Ética, LGPD e sustentabilidade definem os limites do que o sistema pode coletar, afirmar e consumir.

O resultado esperado do projeto é um protótipo de sistema de informação acompanhado de análise técnica, ambiental, social e ética. Este relatório documenta a concepção da plataforma **EcoCidade Bio**, a base de dados simulada e a análise dos dados fornecidos.

**Estrutura do relatório.** As seções seguem o roteiro do item 12 do enunciado: introdução (1), problema (2), objetivos (3), fundamentação (4), metodologia (5), resultados (6), discussão (7), resultado esperado (8), conclusão (9) e referências. Os resultados trazem as 7 etapas (6.1 a 6.7), o estudo biotecnológico (6.8) e o protótipo (6.9). A discussão traz a avaliação de sustentabilidade e ética (7.2) e as respostas às 10 questões (7.3). A tabela “Mapa do enunciado”, antes desta seção, liga cada item do enunciado ao lugar onde ele está respondido.

> **Aviso.** Todos os dados deste trabalho são fictícios e didáticos. Nenhuma informação aqui apresentada pode ser usada como diagnóstico ambiental oficial ou recomendação de consumo de água.

## 2. Problema: o município de Santa Aurora

Santa Aurora tem dados sobre o Córrego das Palmeiras, mas não tem um sistema que os transforme em decisão e participação. A prefeitura opera três pontos de monitoramento e recebe dados de sensores e de um laboratório parceiro, armazenados em arquivos diferentes, sem integração e sem interface adequada para o cidadão.

Os três pontos seguem o curso d’água, da nascente para jusante:

- **P1 — Nascente:** área próxima à nascente.
- **P2 — Urbano:** trecho com ocupação residencial.
- **P3 — Industrial:** trecho a jusante de uma área industrial fictícia.

A proximidade de atividades urbanas ou industriais não comprova, por si só, a origem de eventual poluição. Essa ressalva orienta toda a forma como o sistema apresenta os dados.

A tabela abaixo relaciona cada problema identificado pela prefeitura à resposta que a plataforma propõe.

| Problema identificado pela prefeitura | Resposta proposta pela EcoCidade Bio |
|---|---|
| Descarte irregular de resíduos sólidos | Formulário de ocorrência com categoria, ponto aproximado e foto opcional; triagem no painel administrativo |
| Alterações na qualidade da água | Regras de alerta automáticas para pH, turbidez e oxigênio dissolvido, com histórico e tendência |
| Dificuldade de monitoramento contínuo | Sensores IoT com leitura a cada 6 horas e registro de falhas de comunicação |
| Falta de integração entre medições e análises biotecnológicas | Banco único que relaciona medições físico-químicas e resultados de eDNA pelo mesmo ponto |
| Ausência de plataforma acessível para consulta pública | Interface web seguindo WCAG 2.2 nível AA, com linguagem simples e alternativas textuais aos gráficos |
| Dificuldade dos cidadãos para registrar problemas | Registro sem cadastro, em poucos passos, com número de protocolo para acompanhamento |
| Pouco uso de dados para orientar decisões | Classificação operacional por ponto, painel consolidado e exportação de dados para relatórios |

**Desafio.** Projetar uma solução digital capaz de transformar dados ambientais e biotecnológicos em informações acessíveis, compreensíveis e úteis para a sociedade, sem exagerar o que os dados permitem afirmar.

## 3. Objetivos

### 3.1 Objetivo geral

Projetar uma plataforma digital sustentável e acessível que integre o monitoramento ambiental do Córrego das Palmeiras, dados de sensores IoT, indicadores biológicos e análises de bioinformática, aplicando conceitos de IHC, sustentabilidade, acessibilidade digital e biotecnologia moderna.

### 3.2 Objetivos específicos

1. Identificar os três perfis de usuários (cidadão, agente ambiental e pesquisador) e descrevê-los como personas.
2. Definir requisitos funcionais e requisitos de usabilidade e acessibilidade rastreáveis às personas.
3. Organizar a arquitetura da informação: mapa de navegação, rótulos de menu e fluxos das tarefas principais.
4. Especificar a interface segundo a WCAG 2.2, nível AA, incluindo navegação por teclado, contraste e alternativas textuais aos gráficos.
5. Construir uma base de dados simulada com temperatura, pH, turbidez e oxigênio dissolvido, com consultas, filtros, histórico, gráficos e alertas operacionais.
6. Modelar a recepção, o armazenamento e a apresentação de resultados de DNA ambiental, distinguindo hipótese, resultado simulado e evidência científica.
7. Estimar o consumo de energia, o ciclo de vida dos equipamentos e os resíduos eletrônicos da própria solução.
8. Analisar os dados fornecidos e indicar, com justificativa, qual ponto merece prioridade de investigação.

## 4. Fundamentação teórica

Cada área obrigatória da disciplina responde a uma pergunta diferente do projeto. Esta seção resume o conceito e indica onde ele foi aplicado.

### 4.1 Interação Humano-Computador (IHC)

IHC estuda o projeto, a avaliação e a implementação de sistemas interativos para uso humano (BARBOSA; SILVA, 2010). Seu ponto de partida é conhecer usuários, necessidades e tarefas antes de desenhar telas. As personas são arquétipos de usuários construídos a partir desse conhecimento, que mantêm a equipe focada em pessoas concretas (COOPER et al., 2014). **Aplicação:** três personas e requisitos derivados delas (Etapa 1).

### 4.2 Usabilidade e experiência do usuário (UX)

A ISO 9241-11:2018 define usabilidade como o grau em que um produto permite atingir objetivos com eficácia, eficiência e satisfação em um contexto de uso. Nielsen (1993) acrescenta aprendizado, memorização e prevenção de erros. UX amplia o foco para a experiência como um todo, incluindo confiança e compreensão (ROGERS; SHARP; PREECE, 2013). **Aplicação:** metas mensuráveis por tarefa e plano de testes com usuários (Etapa 7).

### 4.3 Arquitetura da informação (AI)

AI é o projeto estrutural de ambientes de informação: organização, rotulagem, navegação e busca (ROSENFELD; MORVILLE; ARANGO, 2015). Em dados ambientais, a AI decide se o cidadão vê primeiro um número técnico ou uma resposta à pergunta “o córrego está pior?”. **Aplicação:** mapa de navegação, rótulos em linguagem do usuário e camadas de detalhe (Etapa 2).

### 4.4 Acessibilidade digital

As Diretrizes de Acessibilidade para Conteúdo Web (WCAG) organizam critérios em quatro princípios: perceptível, operável, compreensível e robusto. A [WCAG 2.2](https://w3.org/WAI/standards-guidelines/wcag/new-in-22/) tornou-se Recomendação do W3C em 5 de outubro de 2023 e acrescentou 9 critérios à versão 2.1, entre eles foco não obscurecido, tamanho mínimo de alvo (24 × 24 px) e autenticação acessível. No Brasil, a Lei Brasileira de Inclusão (Lei 13.146/2015, art. 63) obriga a acessibilidade em sítios mantidos por órgãos de governo. **Aplicação:** checklist WCAG 2.2 AA e plano de navegação por teclado (Etapa 3).

### 4.5 Sustentabilidade

Sustentabilidade, aqui, é avaliar o sistema pelo seu ciclo de vida completo: fabricação, energia em uso e destinação final. A infraestrutura digital tem pegada real: os data centers consumiram cerca de 415 TWh em 2024, perto de 1,5% da eletricidade mundial ([IEA, 2025](https://www.iea.org/reports/energy-and-ai/executive-summary)). Em 2022 o mundo gerou 62 bilhões de kg de resíduos eletroeletrônicos, e só 22,3% tiveram coleta e reciclagem formal documentada ([BALDÉ et al., 2024](https://www.itu.int/en/ITU-D/Environment/Pages/Publications/The-Global-E-waste-Monitor-2024.aspx)). **Aplicação:** estimativa energética e plano de fim de vida dos equipamentos (Etapa 4).

### 4.6 Cidades inteligentes

Cidades inteligentes usam dados e tecnologias conectadas para melhorar serviços urbanos, sustentabilidade e participação. A ISO 37122:2019 propõe indicadores para essas cidades, inclusive de monitoramento ambiental e de participação cidadã digital. A tecnologia só é “inteligente” quando integra serviços e devolve informação útil à população. **Aplicação:** integração entre sensores, laboratório, fiscalização e cidadão em um mesmo fluxo (Etapas 4 e 5).

### 4.7 Internet das Coisas (IoT)

IoT é a conexão de objetos físicos com sensores e atuadores a redes de dados. No monitoramento hídrico, sondas multiparâmetro medem pH, turbidez, oxigênio dissolvido e temperatura e enviam leituras por redes de baixo consumo, como LoRaWAN, a um gateway conectado à internet. Sensores reduzem visitas de campo, mas exigem calibração, manutenção e tratamento de falhas. **Aplicação:** arquitetura conceitual e série IoT sintética com falhas propositais (Etapa 5).

### 4.8 Sistemas de informação

Um sistema de informação coleta, armazena, processa e apresenta dados para apoiar decisões. Bancos relacionais garantem integridade com chaves e restrições de validação. Visões (views) permitem que a regra de negócio fique em um só lugar e seja reutilizada por todas as telas. **Aplicação:** banco SQLite com restrições, visões de alerta e consultas de demonstração (Etapa 5).

### 4.9 Biotecnologia moderna

Biotecnologia moderna usa organismos, células e moléculas, sobretudo DNA, para produzir bens e informação. No ambiente, três aplicações se destacam. O **DNA ambiental (eDNA)** detecta espécies a partir do material genético que deixam na água, sem capturar organismos (TABERLET et al., 2012; DEINER et al., 2017). **Biossensores** acoplam um componente biológico, como uma enzima ou anticorpo, a um transdutor que gera sinal medido. A **biorremediação** usa organismos para degradar ou reter poluentes, como em wetlands construídos com plantas aquáticas (VYMAZAL, 2011). **Aplicação:** integração dos resultados de eDNA e discussão de biorremediação (Etapa 6).

### 4.10 Engenharia genética e organismos geneticamente modificados (OGMs)

Engenharia genética é a modificação direcionada do material genético de um organismo, gerando um OGM. Organismos modificados poderiam, em tese, degradar poluentes com mais eficiência ou funcionar como biossensores. Os riscos incluem dispersão no ambiente, transferência horizontal de genes e efeitos sobre espécies nativas. No Brasil, a Lei 11.105/2005 (Lei de Biossegurança) atribui à CTNBio a avaliação de atividades com OGMs. **Aplicação:** análise de possibilidades, riscos e regulamentação (Etapa 6).

### 4.11 Bioinformática

Bioinformática aplica computação à organização e interpretação de dados biológicos. Em eDNA, ela filtra a qualidade das sequências, agrupa variantes, compara-as com bancos de referência e atribui táxons. Ferramentas usuais incluem DADA2 (CALLAHAN et al., 2016), QIIME 2 (BOLYEN et al., 2019) e BLAST (ALTSCHUL et al., 1990). O resultado depende da qualidade dos bancos de referência: uma espécie sem sequência cadastrada não é identificada. **Aplicação:** modelo de dados dos resultados e textos de limitação na interface (Etapa 6).

### 4.12 Ética e LGPD

A Lei Geral de Proteção de Dados Pessoais ([Lei 13.709/2018](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)) exige finalidade, necessidade, transparência e segurança no tratamento de dados pessoais (art. 6º). Fotos e localização enviadas por cidadãos podem identificar pessoas. A ética do projeto também inclui transparência dos algoritmos de alerta e responsabilidade na divulgação de resultados científicos. **Aplicação:** registro de ocorrências sem dados pessoais, regra de alerta publicada em linguagem simples e avisos sobre limites dos dados (Etapas 5 a 7).

## 5. Metodologia

O projeto seguiu as sete etapas obrigatórias do enunciado, em ciclo de design centrado no usuário: entender usuários, organizar a informação, especificar a interface, modelar os dados e avaliar. A pesquisa foi aplicada e exploratória, com dados simulados e análise documental de normas e literatura.

| Etapa | O que foi feito | Técnicas e ferramentas | Produto |
|---|---|---|---|
| 1. IHC: usuários | Identificação dos três perfis do enunciado e construção de uma persona por perfil; derivação de requisitos a partir das dificuldades de cada persona | Proto-personas (Cooper et al., 2014), baseadas no enunciado e em literatura; matriz persona × requisito | 3 personas, 12 requisitos funcionais, 11 de usabilidade e acessibilidade |
| 2. AI e UX | Definição das telas, do menu, dos rótulos e dos fluxos das quatro tarefas de teste | Mapa de navegação, fluxos de tarefa, especificação de telas | Mapa de navegação com 7 telas e fluxos de tarefa |
| 3. Acessibilidade | Seleção dos critérios WCAG 2.2 A e AA aplicáveis e definição de como cada um é atendido e verificado | Checklist WCAG 2.2; cálculo de contraste pela fórmula de luminância relativa do W3C | Checklist com 25 critérios e paleta verificada |
| 4. Sustentabilidade | Estimativa do consumo elétrico anual e das emissões; análise do ciclo de vida e dos resíduos eletrônicos | Estimativa por premissas declaradas; fator de emissão do Sistema Interligado Nacional (MCTI) | Tabela de consumo e plano de fim de vida |
| 5. IoT e dados | Modelagem do banco, carga dos dados do enunciado, criação de temperatura e de série IoT sintética, implementação das regras de alerta e de 10 consultas | SQLite 3, SQL (restrições CHECK, chaves, visões, funções de janela), Python 3 e matplotlib | Banco `ecocidade_bio.db`, CSVs, 4 gráficos e testes de validação |
| 6. Biotecnologia | Modelagem do fluxo laboratório → sistema para eDNA; textos de interpretação; discussão de engenharia genética, OGMs e limitações | Revisão de literatura; vocabulário controlado de resultados | Tabelas de eDNA, leitura integrada por ponto |
| 7. Validação | Plano de testes de usabilidade com 4 tarefas, métricas e roteiro; análise de impactos | Teste de usabilidade moderado; teste de teclado; matriz de impactos | Protocolo e formulário de registro; matriz de impactos |

**Critério de análise dos dados.** As medições foram classificadas pelas regras didáticas do enunciado: pH abaixo de 6,0 ou acima de 9,0; turbidez acima de 25 NTU; oxigênio dissolvido abaixo de 5,0 mg/L. Nenhum limiar ultrapassado resulta em “Sem alerta”; um, em “Atenção”; dois ou três, em “Prioridade de investigação”. O grupo acrescentou uma regra para dados ausentes, descrita na seção 6.5.

**Distinção entre tipos de informação.** Em todo o relatório, marca-se o que é dado do enunciado, o que é dado criado pelo grupo e o que é hipótese. A temperatura, a série IoT, os sensores e as ocorrências foram criados pelo grupo; pH, turbidez, oxigênio dissolvido e eDNA são do enunciado.

## 6. Resultados

Os resultados estão organizados pelas sete etapas. O principal achado dos dados é que o P3 deve ser o primeiro ponto a ser investigado, seguido de perto pelo P2, cuja piora foi a mais rápida.

### 6.1 Etapa 1 — IHC: identificação dos usuários

Cada perfil tem uma pergunta diferente para o sistema, e a interface precisa responder às três sem misturar níveis de detalhe. As personas completas, com cenários e tarefas, estão no documento de personas, requisitos e arquitetura da informação.

| Persona | Perfil | Objetivo principal | Principais dificuldades | Necessidades-chave |
|---|---|---|---|---|
| Shirley Silva, 67 anos | Cidadã, aposentada, mora há 30 anos perto do P2 | Saber se o córrego está piorando e avisar sobre lixo e esgoto | Pouca experiência digital, visão reduzida para letras pequenas, termos técnicos, medo de errar | Linguagem simples, letras e botões grandes, formulário curto sem cadastro, protocolo de retorno |
| Jason Duval, 34 anos | Agente ambiental da Secretaria de Meio Ambiente | Saber onde agir primeiro e dar retorno às ocorrências | Dados espalhados em planilhas, pouco tempo, tablet sob sol no campo, sinal de internet instável | Painel de prioridades, filtros, histórico, triagem de ocorrências, páginas leves, exportação |
| Dra. Andy Duarte, 45 anos | Bióloga e bioinformata do laboratório parceiro; baixa visão, usa leitor de tela e ampliação | Publicar resultados de eDNA com metadados e cruzá-los com os dados físico-químicos | Gráficos sem tabela, formulários sem rótulo, interpretação exagerada dos resultados pelo público | Tabelas de dados acessíveis, navegação por teclado, campos de metadados, avisos sobre limites do método |

**Requisitos funcionais (RF).** Cada requisito indica a persona que o originou: C (cidadã), A (agente) e P (pesquisadora).

| Código | Requisito | Origem |
|---|---|---|
| RF01 | Consultar os pontos de monitoramento em lista e mapa, com a situação atual de cada um | C, A |
| RF02 | Exibir a última medição de cada ponto (temperatura, pH, turbidez, OD), com data e classificação | C, A |
| RF03 | Filtrar o histórico por ponto, parâmetro e período, com gráfico e tabela equivalente | A, P |
| RF04 | Calcular e exibir alertas operacionais pelas regras didáticas, tratando dados ausentes | A |
| RF05 | Registrar ocorrência sem cadastro (categoria, ponto aproximado, descrição, foto opcional) e gerar protocolo | C |
| RF06 | Acompanhar a ocorrência pelo número de protocolo | C |
| RF07 | Exibir resultados de eDNA por ponto com “o que indica” e “o que não indica” | C, P |
| RF08 | Importar resultados laboratoriais (CSV) com validação de vocabulário e metadados obrigatórios | P |
| RF09 | Painel administrativo para triagem de ocorrências (status e resposta pública) e indicadores consolidados | A |
| RF10 | Exportar dados filtrados em CSV, com aviso de uso didático | A, P |
| RF11 | Oferecer conteúdo educativo sobre cada indicador e glossário de termos | C |
| RF12 | Receber leituras de sensores IoT, rejeitar valores fisicamente impossíveis e sinalizar falhas de comunicação | A |

**Requisitos de usabilidade e acessibilidade (RU).**

| Código | Requisito | Referência |
|---|---|---|
| RU01 | Todas as funções operáveis por teclado, com foco visível e nunca encoberto | WCAG 2.1.1, 2.4.7, 2.4.11 |
| RU02 | Contraste mínimo de 4,5:1 para texto e 3:1 para elementos gráficos e componentes | WCAG 1.4.3, 1.4.11 |
| RU03 | Situação de alerta nunca indicada só por cor: sempre texto e ícone | WCAG 1.4.1 |
| RU04 | Todo gráfico com resumo textual e tabela de dados equivalente | WCAG 1.1.1 |
| RU05 | Formulários com rótulos visíveis, instruções e mensagens de erro que digam como corrigir | WCAG 3.3.1, 3.3.2, 3.3.3 |
| RU06 | Linguagem simples, com termos técnicos explicados na primeira ocorrência | Boa prática; WCAG 3.1.5 (AAA) como inspiração |
| RU07 | Alvos de toque com no mínimo 24 × 24 px; o projeto adota 44 × 44 px | WCAG 2.5.8 |
| RU08 | Layout legível em 320 px de largura e com zoom de 200% sem perda de conteúdo | WCAG 1.4.4, 1.4.10 |
| RU09 | Registrar uma ocorrência em até 3 passos, com meta de até 2 minutos | Meta de eficiência a validar nos testes |
| RU10 | Link de ajuda sempre no mesmo lugar em todas as telas | WCAG 3.2.6 |
| RU11 | Páginas leves (meta de até 1 MB) para funcionar em conexões móveis lentas | Inclusão digital e sobriedade digital |

### 6.2 Etapa 2 — Arquitetura da informação e UX

A plataforma tem 7 telas em dois níveis: seis no menu público e a área da equipe, acessada por um link no rodapé. Os rótulos usam verbos e palavras do cidadão (“Participe”, “Aprenda”) em vez de jargão (“Ocorrências”, “Base de conhecimento”). O mapa de navegação desenhado está no documento de personas e arquitetura da informação.

A informação é apresentada em três camadas, para que cada persona pare no nível de que precisa:

1. **Resposta:** um selo com a situação do ponto (“Prioridade de investigação”), com texto e ícone.
2. **Explicação:** por que o ponto recebeu esse selo, em uma frase (“turbidez e oxigênio fora do limite”).
3. **Dado:** valores, unidades, histórico, tabela e exportação.

| Tela | Objetivo | Conteúdo principal | Requisitos atendidos |
|---|---|---|---|
| 1. Início | Mostrar a situação geral em segundos | Três cartões (P1, P2, P3) com selo de situação, data da última coleta e atalho “Registrar um problema” | RF01, RF02, RU03 |
| 2. Monitoramento | Consultar pontos, medições e alertas | Seletor de ponto; cartões de pH, turbidez, OD e temperatura com valor, unidade, limiar e explicação curta | RF01, RF02, RF04 |
| 3. Histórico e gráficos | Ver a evolução dos indicadores | Filtros (ponto, parâmetro, período), gráfico de linhas com limiar, resumo em texto, tabela de dados, botão Exportar CSV | RF03, RF10, RU04 |
| 4. Participe | Registrar e acompanhar ocorrências | Formulário em 3 passos; campo “Acompanhar protocolo” | RF05, RF06, RU05, RU09 |
| 5. Biotecnologia | Apresentar resultados de eDNA com responsabilidade | Tabela de resultados por ponto, cartões “o que indica / o que não indica”, data e método da amostra | RF07 |
| 6. Aprenda | Explicar indicadores e conceitos | Textos curtos sobre pH, turbidez, OD, temperatura, eDNA e biorremediação; glossário | RF11, RU06 |
| 7. Administração (equipe) | Triar ocorrências e ver indicadores consolidados | Lista de ocorrências com filtros e status; importação de resultados de laboratório; contagem de alertas por ponto | RF08, RF09, RF12 |

**Fluxos das tarefas de teste.** As quatro tarefas mínimas do enunciado foram desenhadas para ser concluídas no menor número de passos.

- **T1 — Localizar o P3:** Início → cartão “P3 — Industrial” (1 passo) ou menu Monitoramento → P3 (2 passos).
- **T2 — Consultar a última turbidez do P3:** Monitoramento → P3 → cartão Turbidez, que mostra “62 NTU · acima do limite de 25 NTU · coleta de 16/10/2026” (2 passos).
- **T3 — Interpretar o eDNA do P3:** Biotecnologia → P3 → “Cianobactéria: detectada”, seguido de “Isso indica que havia DNA de cianobactérias na água. Não indica, sozinho, floração nem toxinas” (2 passos).
- **T4 — Registrar uma ocorrência:**
  1. Participe → Registrar um problema.
  2. “O que você viu?”: escolher uma das 6 categorias.
  3. “Onde?”: escolher P1, P2, P3 ou “Não sei”.
  4. “Conte o que aconteceu”: descrição (10 a 500 caracteres) e foto opcional.
  5. Revisar e enviar; a tela mostra o protocolo (ex.: OC-2026-0009) e o anuncia ao leitor de tela.
  6. Erro: descrição curta mostra “Escreva pelo menos 10 letras para a equipe entender o problema”, e o foco volta ao campo.

**Informação indisponível.** Quando falta um dado, a tela diz o que falta e por quê, em vez de mostrar zero ou deixar em branco. Exemplos: “Sensor sem comunicação desde 10/10, 18h” e “Data de coleta não informada pelo laboratório”.

### 6.3 Etapa 3 — Acessibilidade digital

A interface foi especificada para atender aos 25 critérios WCAG 2.2 de nível A e AA mais relevantes para uma plataforma de dados públicos com formulário. Cinco deles são novos na versão 2.2 e estão marcados com asterisco.

| Critério WCAG 2.2 | Nível | Como a EcoCidade Bio atende | Como verificar |
|---|---|---|---|
| 1.1.1 Conteúdo não textual | A | Ícones com texto alternativo; cada gráfico tem resumo em texto e tabela de dados | Leitor de tela (NVDA) lê o resumo do gráfico |
| 1.3.1 Informações e relações | A | Títulos h1–h3 em ordem; tabelas com cabeçalho `th`; campos ligados ao rótulo | Inspeção do código e lista de títulos do leitor de tela |
| 1.4.1 Uso de cor | A | Selos de situação com texto e ícone; linhas do gráfico com marcadores diferentes e rótulo direto | Ver a tela em escala de cinza |
| 1.4.3 Contraste mínimo | AA | Texto com no mínimo 4,5:1 (paleta abaixo) | Cálculo de contraste |
| 1.4.4 Redimensionar texto | AA | Unidades relativas (rem); nada se perde com 200% de zoom | Zoom do navegador em 200% |
| 1.4.10 Refluxo | AA | Layout em coluna única abaixo de 600 px; sem rolagem horizontal em 320 px | Janela com 320 px de largura |
| 1.4.11 Contraste não textual | AA | Bordas de campos, foco e linhas de gráfico com no mínimo 3:1 | Cálculo de contraste |
| 2.1.1 Teclado | A | Todas as funções por teclado, inclusive filtros e envio do formulário | Teste só com teclado |
| 2.1.2 Sem bloqueio do teclado | A | Diálogos fecham com Esc e devolvem o foco ao botão de origem | Teste só com teclado |
| 2.4.1 Ignorar blocos | A | Link “Pular para o conteúdo” é o primeiro item focável | Primeiro Tab da página |
| 2.4.2 Página com título | A | Título único por tela (ex.: “Monitoramento — P3 — EcoCidade Bio”) | Aba do navegador |
| 2.4.3 Ordem do foco | A | Ordem do foco segue a ordem visual | Teste só com teclado |
| 2.4.6 Cabeçalhos e rótulos | AA | Rótulos descrevem a finalidade (“Descreva o que você viu”) | Revisão de textos |
| 2.4.7 Foco visível | AA | Contorno de foco azul de 3 px (#1A4FD6, 6,7:1 sobre branco) | Teste só com teclado |
| 2.4.11 Foco não obscurecido (mínimo)\* | AA | Cabeçalho não fixo em telas pequenas; rolagem compensa a altura do cabeçalho | Tab até o fim de páginas longas |
| 2.5.8 Tamanho do alvo (mínimo)\* | AA | Botões e links de menu com 44 × 44 px (o mínimo é 24 × 24 px) | Medição nas ferramentas do navegador |
| 3.1.1 Idioma da página | A | `lang="pt-BR"` em todas as páginas | Inspeção do código |
| 3.2.6 Ajuda consistente\* | A | Link “Ajuda” sempre no canto superior direito e no rodapé | Navegar pelas 7 telas |
| 3.3.1 Identificação do erro | A | Erro descrito em texto junto ao campo, não só com borda vermelha | Enviar formulário vazio |
| 3.3.2 Rótulos ou instruções | A | Rótulos sempre visíveis; formato indicado antes do campo | Revisão do formulário |
| 3.3.3 Sugestão de erro | AA | Mensagem diz como corrigir (“Escreva pelo menos 10 letras…”) | Enviar descrição curta |
| 3.3.7 Entrada redundante\* | A | Tela de revisão mostra o que já foi digitado; nada é pedido duas vezes | Percorrer o formulário |
| 3.3.8 Autenticação acessível (mínimo)\* | AA | Login da equipe aceita gerenciador de senhas e colar; sem CAPTCHA de imagem | Tentar colar a senha |
| 4.1.2 Nome, função, valor | A | Componentes HTML nativos; seletor de ponto como grupo de botões de opção | Leitor de tela anuncia “P3, selecionado, 3 de 3” |
| 4.1.3 Mensagens de status | AA | Confirmação do protocolo em região `aria-live` | Leitor de tela anuncia o protocolo sem mover o foco |

**Paleta verificada.** O grupo calculou o contraste de cada par de cores pela fórmula de luminância relativa da WCAG.

| Uso | Cores | Contraste | Mínimo exigido |
|---|---|---|---|
| Texto principal | #1F2328 sobre #FFFFFF | 15,8:1 | 4,5:1 |
| Texto secundário | #57606A sobre #FFFFFF | 6,4:1 | 4,5:1 |
| Botão primário (texto branco) | #FFFFFF sobre #0B5D3B | 7,9:1 | 4,5:1 |
| Selo “Sem alerta” | #0B5D3B sobre #E6F4EA | 7,0:1 | 4,5:1 |
| Selo “Atenção” | #6B4E00 sobre #FFF4CE | 7,0:1 | 4,5:1 |
| Selo “Prioridade” | #8A1C1C sobre #FDE7E9 | 7,9:1 | 4,5:1 |
| Contorno de foco | #1A4FD6 sobre #FFFFFF | 6,7:1 | 3:1 |
| Linha P1 no gráfico | #0072B2 sobre #FFFFFF | 5,2:1 | 3:1 |
| Linha P2 no gráfico | #9A6700 sobre #FFFFFF | 4,9:1 | 3:1 |
| Linha P3 no gráfico | #D55E00 sobre #FFFFFF | 3,9:1 | 3:1 |

A verificação encontrou um problema real: o laranja original da paleta Okabe-Ito (#E69F00), escolhido por ser seguro para daltonismo, tinha só 2,3:1 sobre branco e reprovava no critério 1.4.11. Ele foi escurecido para #9A6700. Segurança para daltonismo e contraste são requisitos diferentes e precisam ser verificados separadamente.

**Plano de navegação por teclado (tela Monitoramento).**

1. Tab 1: “Pular para o conteúdo”.
2. Tabs seguintes: logotipo (Início), seis itens do menu, “Ajuda”.
3. Seletor de ponto: um único Tab entra no grupo; as setas trocam entre P1, P2 e P3.
4. Cartões de indicadores: cada um com link “Ver histórico”.
5. Botão “Registrar um problema neste ponto”.
6. Rodapé: “Acesso da equipe”, “Sobre os dados”, “Privacidade”.

Teclas: Tab e Shift+Tab movem o foco; Enter ativa links e botões; Espaço marca opções; setas navegam em grupos; Esc fecha diálogos. **Linguagem simples** não é exigência do nível AA, mas foi adotada como requisito (RU06): frases curtas, voz ativa e termos técnicos explicados na primeira vez, como em “turbidez (o quanto a água está turva)”.

### 6.4 Etapa 4 — Sustentabilidade e cidades inteligentes

A solução consome pouca energia em operação (cerca de 307 kWh por ano da rede, pela estimativa abaixo); seu maior impacto ambiental está na fabricação e no descarte dos equipamentos. Por isso, o plano de fim de vida pesa mais do que a economia de energia.

**Contribuição para a cidade inteligente.**

- **Qualidade da água:** leituras a cada 6 horas mostram tendências que coletas semanais não captam, como quedas noturnas de oxigênio (seção 6.5).
- **Planejamento urbano:** a comparação entre trechos indica onde priorizar fiscalização, ligações de esgoto, limpeza de margens e drenagem.
- **Participação cidadã:** o cidadão deixa de ser só consumidor de informação e passa a produzir dados (ocorrências) com retorno público pelo protocolo.
- **Integração de serviços:** uma ocorrência pode ser encaminhada à limpeza urbana, à vigilância sanitária ou à fiscalização ambiental a partir do mesmo painel.

**Estimativa de consumo energético anual.** Os valores são estimativas por premissas declaradas, não medições de equipamentos reais.

| Componente | Premissa | Fonte de energia | Energia anual |
|---|---|---|---|
| 3 nós sensores (sonda + datalogger + rádio LoRa) | 1,0 W médio cada, 24 h × 365 dias | Painel solar de 20 W + bateria (fora da rede) | 26,3 kWh (não consome da rede) |
| Gateway LoRaWAN | 5 W contínuos | Rede elétrica | 43,8 kWh |
| Servidor (fração de máquina virtual) | 20 W atribuídos ao sistema × PUE 1,5 (refrigeração e perdas do data center) | Rede elétrica | 262,8 kWh |
| **Total da rede** | — | — | **306,6 kWh** |

Com o fator médio de emissão do Sistema Interligado Nacional em 2023, de [38,5 kg de CO₂ por MWh](https://cenarioenergia.com.br/2024/02/15/emissoes-de-co2-na-geracao-de-energia-atingem-minima-em-onze-anos-no-brasil/) (MCTI), a operação emitiria cerca de **11,8 kg de CO₂ por ano**. A conta não inclui os aparelhos dos usuários nem as análises de laboratório, que usam reagentes, plásticos descartáveis e refrigeração.

**Ciclo de vida dos equipamentos.** As vidas úteis abaixo são faixas típicas de referência, a confirmar com o fabricante escolhido.

| Componente | Vida útil típica (estimativa) | Principal impacto | Medida proposta |
|---|---|---|---|
| Eletrodo de pH | 1 a 2 anos | Troca frequente de peça com vidro e eletrólito | Comprar sondas com eletrodo substituível, sem trocar a sonda inteira |
| Sensores ópticos de turbidez e OD | vários anos, com limpeza e recalibração | Incrustação biológica reduz a precisão | Escova automática de limpeza; calendário de calibração no sistema |
| Baterias | 3 a 5 anos | Metais e eletrólitos perigosos | Recolhimento pelo fornecedor (Resolução CONAMA 401/2008) |
| Painéis solares | décadas | Vidro, silício e metais no fim da vida | Reaproveitar em outros pontos antes do descarte |
| Gateway e servidor | 5 a 7 anos | Placas eletrônicas e metais | Hospedagem compartilhada; reuso interno; reciclagem certificada |

**Resíduos eletrônicos.** O problema é global: em 2022 o mundo gerou 62 bilhões de kg de resíduos eletroeletrônicos, e só 22,3% tiveram coleta e reciclagem formal documentada ([Global E-waste Monitor 2024](https://www.itu.int/en/ITU-D/Environment/Pages/Publications/The-Global-E-waste-Monitor-2024.aspx)). No Brasil, a Política Nacional de Resíduos Sólidos (Lei 12.305/2010) prevê a logística reversa de eletroeletrônicos. O [Decreto 10.240/2020](https://planalto.gov.br/ccivil_03/_ato2019-2022/2020/decreto/d10240.htm) regulamenta o sistema para produtos de **uso doméstico**; equipamentos de uso governamental ficam fora desse sistema. Por isso, a prefeitura, como geradora, deve prever no contrato de compra o recolhimento e a destinação certificada pelo fornecedor.

**Sobriedade digital.** Medidas que reduzem consumo e também ajudam a inclusão:

1. Leituras a cada 6 horas, e não a cada minuto: o intervalo basta para tendências e poupa bateria e armazenamento.
2. Guardar leituras brutas por 2 anos e depois mantê-las apenas como médias diárias.
3. Fotos de ocorrências comprimidas e sem metadados de localização.
4. Páginas leves (meta de até 1 MB), que carregam rápido em celulares antigos e redes lentas (RU11).

### 6.5 Etapa 5 — IoT e tratamento de dados

O P3 é o ponto de maior prioridade de investigação: ultrapassou dois limiares em todas as coletas e os três em 16/10/2026. O P2 piorou mais rápido, passando de “Sem alerta” a “Prioridade” em duas semanas. O P1 permaneceu sem alerta e funciona como referência do trecho menos alterado.

**Arquitetura de dados.** Três fontes alimentam um único banco, e as regras de alerta ficam no banco, não nas telas. Assim, todas as telas mostram a mesma classificação para o mesmo dado.

![Arquitetura de dados da EcoCidade Bio · 3 fontes, 3 validações, 1 banco, 3 personas](imagens/figura-1-arquitetura-de-dados.png)

*Arquitetura de dados da EcoCidade Bio · 3 fontes, 3 validações, 1 banco, 3 personas*

A validação acontece na entrada: um dado impossível é recusado antes de chegar às telas, e um dado ausente chega marcado como ausente.

**Modelo de dados.** O banco SQLite tem 9 tabelas e 4 visões; o dicionário completo está no README do pacote da base de dados.

| Grupo | Tabelas e visões | Registros |
|---|---|---|
| Cadastros | `pontos_monitoramento`, `parametros`, `sensores` | 3 pontos, 4 parâmetros, 4 equipamentos |
| Medições | `medicoes` (consolidadas), `leituras_iot` (série de 6 em 6 horas) | 9 medições; 171 leituras |
| Biologia | `marcadores_edna`, `amostras_edna`, `resultados_edna` | 2 marcadores, 3 amostras, 6 resultados |
| Participação | `ocorrencias` | 8 ocorrências fictícias, sem dados pessoais |
| Regras | `vw_medicoes_classificadas`, `vw_leituras_iot_classificadas`, `vw_ultima_medicao`, `vw_painel_integrado` | calculadas a cada consulta |

A temperatura não consta da base do enunciado e foi criada pelo grupo (20,8 a 25,1 °C, mais quente a jusante). Ela não tem limiar de alerta; serve para interpretar o oxigênio, porque água mais quente retém menos oxigênio dissolvido.

![Medições do enunciado (seção 6) · 9 coletas, 3 pontos, 02 a 16/10/2026 · limiares didáticos da seção 6](imagens/figura-2-medicoes-do-enunciado.png)

*Medições do enunciado (seção 6) · 9 coletas, 3 pontos, 02 a 16/10/2026 · limiares didáticos da seção 6*

Turbidez e OD do P3 já estavam na zona de alerta na primeira coleta; o pH só cruzou o limite de 6,0 na última. A tabela de dados de cada indicador fica disponível na tela Histórico como alternativa textual ao gráfico.

**Classificação operacional por coleta.**

| Ponto | 02/10 | 09/10 | 16/10 |
|---|---|---|---|
| P1 — Nascente | Sem alerta (0 de 3) | Sem alerta (0 de 3) | Sem alerta (0 de 3) |
| P2 — Urbano | Sem alerta (0 de 3) | Atenção (1 de 3: turbidez) | Prioridade (2 de 3: turbidez e OD) |
| P3 — Industrial | Prioridade (2 de 3: turbidez e OD) | Prioridade (2 de 3: turbidez e OD) | Prioridade (3 de 3: pH, turbidez e OD) |

**Por que o P3 tem maior prioridade.** Quatro evidências independentes apontam na mesma direção:

1. **Persistência:** é o único ponto em “Prioridade” nas três coletas.
2. **Gravidade:** em 16/10 tem os piores valores dos três indicadores (pH 5,8; turbidez 62 NTU, 2,5 vezes o limiar; OD 3,4 mg/L).
3. **Tendência:** a turbidez subiu 13 e depois 14 NTU por semana (+77% no período) e o OD caiu 0,5 e depois 0,7 mg/L, ou seja, a piora acelerou.
4. **Convergência biológica:** é o único ponto com cianobactéria detectada e sem detecção de peixe nativo (seção 6.6).

O P2 vem em seguida: a turbidez subiu 52% (25 → 38 NTU) e o OD cruzou o limiar em 16/10 (4,8 mg/L). O P1 variou pouco (turbidez de 9 a 12 NTU; OD de 7,5 a 7,1 mg/L), mas a leve piora também merece acompanhamento.

**Tratamento de dados ausentes e inválidos.** O enunciado não diz o que fazer quando falta um parâmetro. O grupo decidiu que o sistema nunca presume “Sem alerta” na falta de dado:

| Situação | Classificação exibida |
|---|---|
| Faltam dados e 2 ou mais limiares já foram ultrapassados | Prioridade de investigação (o dado ausente não mudaria o resultado) |
| Faltam dados e 1 limiar foi ultrapassado | Atenção (dados incompletos) |
| Faltam dados e nenhum limiar foi ultrapassado | Inconclusivo (dados incompletos) |
| Faltam os três parâmetros | Sem dados |

O banco recusa entradas impossíveis por restrições de validação. No teste automatizado, os 7 casos inválidos foram rejeitados: pH 15,2; turbidez negativa; ponto inexistente; medição duplicada; categoria de ocorrência fora da lista; descrição com menos de 10 caracteres; resultado de eDNA fora do vocabulário. Um oitavo teste confirmou que uma medição sem turbidez, com pH e OD normais, é classificada como “Inconclusivo”.

**Série IoT sintética.** Para demonstrar o monitoramento contínuo, o grupo gerou 171 leituras (57 por ponto, de 6 em 6 horas, de 02/10 12h a 16/10 12h). Os valores foram interpolados entre as coletas semanais com ruído controlado e um ciclo diário hipotético de temperatura e oxigênio. Foram inseridas de propósito duas falhas de comunicação (P2 em 07/10 6h; P1 em 10/10 18h) e uma leitura de pH impossível (15,2 no P3 em 12/10 0h), que o sistema descartou mantendo os demais parâmetros. Dois achados só aparecem nessa série e são consequência das hipóteses do grupo, não dados do enunciado:

- **Quedas noturnas de oxigênio:** no P2, o OD fica abaixo de 5,0 mg/L de madrugada a partir de 06/10, dez dias antes de a coleta diurna de 16/10 registrar o problema. Isso ilustra por que coletas pontuais durante o dia podem subestimar a falta de oxigênio.
- **Alerta oscilante:** como seus valores ficam perto dos limiares, o P2 trocou de classificação 20 vezes em 14 dias. O grupo testou duas regras de estabilidade [Verificação técnica]: exigir duas leituras seguidas reduz as trocas para 12, porque a queda de oxigênio dura as duas leituras da madrugada; usar a pior classificação das últimas 24 horas reduz para 4. A regra de 24 horas é a recomendada para o alerta operacional, sem mudar a classificação de cada leitura, e está calculada ao vivo na tela da equipe do protótipo.

**Consultas implementadas.** O arquivo `sql/03_consultas.sql` demonstra os recursos pedidos na Etapa 5:

| Consulta | Recurso demonstrado |
|---|---|
| Q1 Painel integrado | Última medição, classificação e eDNA por ponto |
| Q2 Histórico do P3 | Filtro por ponto e histórico |
| Q3 Período de 09 a 16/10 | Filtro por período |
| Q4 Alertas operacionais | Medições com limiares ultrapassados e quais indicadores |
| Q5 Comparação entre pontos | Médias e variação percentual da turbidez |
| Q6 Tendência semanal | Diferença semana a semana (função de janela) |
| Q7 Leitura integrada | Cruzamento entre classificação físico-química e eDNA |
| Q8 Entrada em cada classe (IoT) | Primeiro horário em que cada ponto entrou em cada classificação |
| Q9 Qualidade dos dados | Falhas de comunicação e leituras descartadas |
| Q10 Ocorrências | Contagem por ponto e status para o painel administrativo |

### 6.6 Etapa 6 — Biotecnologia e bioinformática

No P3, o resultado biológico converge com o físico-químico: cianobactéria detectada, peixe nativo não detectado e três limiares ultrapassados. A convergência reforça a prioridade de investigação, mas não prova causa, floração ou toxicidade. A técnica escolhida foi o DNA ambiental (eDNA), porque é a que o laboratório parceiro já usa.

**Como o eDNA chega ao sistema.**

1. **Coleta:** a equipe coleta água no ponto, registrando data, hora e volume.
2. **Filtração e extração:** a água passa por filtro que retém células e fragmentos; o DNA é extraído no laboratório.
3. **Amplificação:** a PCR copia uma região marcadora do DNA. Como hipótese didática, o grupo adotou o gene 12S rRNA para peixes e o 16S rRNA para cianobactérias.
4. **Sequenciamento e bioinformática:** programas filtram a qualidade das leituras, agrupam sequências e as comparam com bancos de referência para atribuir o grupo biológico.
5. **Laudo:** o laboratório emite, por ponto e marcador, “Detectado”, “Não detectado” ou “Indeterminado”.
6. **Importação:** o laboratório envia um CSV padronizado (amostra, ponto, data de coleta, método, lote, marcador, resultado, observação). O sistema recusa resultados fora do vocabulário e aceita data ausente, mas a exibe como “não informada pelo laboratório”.
7. **Armazenamento e apresentação:** os resultados ficam nas tabelas `amostras_edna` e `resultados_edna`, ligados ao mesmo ponto das medições. A tela Biotecnologia mostra cada resultado com dois textos fixos: “o que indica” e “o que não indica”.

**Três tipos de indicador.** O sistema distingue a natureza de cada indicador para não somar coisas diferentes.

| Tipo | Indicadores no sistema | O que revela | Limite |
|---|---|---|---|
| Físico | Temperatura, turbidez | Estado do meio: calor, partículas em suspensão | Não identifica a origem das partículas |
| Químico | pH, oxigênio dissolvido | Condições para a vida aquática e sinais de matéria orgânica | Medição pontual; varia ao longo do dia |
| Biológico | eDNA de peixe nativo e de cianobactéria | Presença recente de material genético de grupos de organismos | Não mede abundância, vitalidade nem toxinas |

**Leitura integrada por ponto.**

| Ponto | Físico-químico (16/10) | eDNA | Leitura responsável | Próximo passo sugerido |
|---|---|---|---|---|
| P1 | Sem alerta | Peixe nativo detectado; cianobactéria não detectada | Sinais compatíveis com o trecho menos alterado | Manter monitoramento como referência |
| P2 | Prioridade (turbidez e OD) | Peixe nativo detectado; cianobactéria indeterminada | Piora físico-química rápida; biologia ainda inconclusiva | Repetir a amostra de eDNA; vistoriar ligações de esgoto relatadas |
| P3 | Prioridade (pH, turbidez e OD) | Peixe nativo não detectado; cianobactéria detectada | Sinais físico-químicos e biológicos convergem para degradação | Investigar a origem; analisar genes de toxina (ex.: mcyE) e cianotoxinas na água |

**Biorremediação.** Depois de identificar e interromper as fontes de poluição, o trecho do P3 poderia receber técnicas de biorremediação, como wetlands construídos (“jardins filtrantes”) com plantas aquáticas. As raízes retêm sólidos e abrigam microrganismos que degradam matéria orgânica e retêm nutrientes (VYMAZAL, 2011). A biorremediação não substitui o controle das fontes e exige manejo da biomassa. O sistema pode acompanhar seu efeito comparando turbidez, OD e eDNA antes e depois da intervenção.

**Engenharia genética e OGMs.** Bactérias modificadas podem funcionar como biossensores, emitindo fluorescência na presença de um poluente, ou ser projetadas para degradar compostos específicos. Os riscos de liberar esses organismos no ambiente incluem dispersão descontrolada, transferência de genes para microrganismos nativos e efeitos sobre a cadeia alimentar. No Brasil, qualquer atividade com OGM depende de avaliação da CTNBio, conforme a Lei 11.105/2005. Por isso, este projeto não propõe liberar OGMs no córrego; admite o uso de biossensores modificados apenas em ambiente laboratorial contido.

**Limitações científicas.**

- Detectar DNA não comprova organismos vivos no momento da coleta, nem sua abundância, nem produção de toxinas.
- Não detectar não prova ausência: o DNA pode estar abaixo do limite de detecção ou degradado.
- Em água corrente, DNA de trechos acima pode ser detectado mais abaixo; a localização do sinal é aproximada.
- Temperatura, luz solar e pH aceleram a degradação do DNA e alteram a chance de detecção.
- Bancos de referência têm lacunas para espécies nativas brasileiras; sem sequência de referência, não há identificação.
- O enunciado não informa a data de coleta do eDNA, o que impede parear o resultado biológico com uma medição físico-química específica.
- O resultado “Indeterminado” do P2 exige nova amostra, e não deve ser lido como positivo nem como negativo.

### 6.7 Etapa 7 — Validação e avaliação final

O protocolo e os formulários do teste com participantes estão prontos, mas o teste ainda não foi feito: **[A FAZER PELO GRUPO]**. O enunciado proíbe resultados inventados, por isso os campos estão em branco. A verificação técnica automatizada do protótipo, que não envolve pessoas, foi feita e está em 6.7.2.

#### 6.7.1 Teste de usabilidade com participantes — [A FAZER PELO GRUPO]

**Protocolo do teste de usabilidade.**

- **Participantes:** no mínimo 5 pessoas que não participaram do desenvolvimento, buscando ao menos uma pessoa com 60 anos ou mais e uma com pouca experiência digital.
- **Consentimento:** explicação oral do objetivo; registro apenas de faixa etária e experiência digital, sem nome (LGPD, princípio da necessidade).
- **Método:** teste moderado, com o participante pensando em voz alta; um integrante conduz e outro cronometra e anota.
- **Regra do moderador:** não ensinar o caminho; só ajudar após 2 minutos de bloqueio, registrando a ajuda.

| Tarefa | Enunciado lido ao participante | Critério de sucesso | Meta de tempo |
|---|---|---|---|
| T1 | “Encontre o ponto de monitoramento do trecho industrial.” | Abrir a página do P3 | até 30 s |
| T2 | “Qual foi a última medição de turbidez nesse ponto?” | Dizer “62 NTU” e a data 16/10 | até 60 s |
| T3 | “O que o resultado de DNA ambiental desse ponto quer dizer?” | Explicar que houve DNA de cianobactéria e que isso não prova toxina ou floração | até 90 s |
| T4 | “Avise a prefeitura sobre lixo jogado na margem.” | Enviar o formulário e informar o protocolo | até 120 s |

**Formulário de registro (um por participante).**

| Tarefa | Concluiu? (sim / parcial / não) | Tempo (s) | Erros e dificuldades observados | Precisou de ajuda? | Sugestão do participante |
|---|---|---|---|---|---|
| T1 |  |  |  |  |  |
| T2 |  |  |  |  |  |
| T3 |  |  |  |  |  |
| T4 |  |  |  |  |  |

**Teste de navegação por teclado** (sem mouse, nas 7 telas):

- [ ] O primeiro Tab mostra “Pular para o conteúdo”.
- [ ] Todo item focável mostra contorno visível e não fica escondido.
- [ ] A ordem do foco segue a ordem visual.
- [ ] O seletor de ponto funciona com as setas.
- [ ] O formulário pode ser preenchido e enviado só com teclado.
- [ ] Erros de formulário levam o foco ao campo com problema.
- [ ] Diálogos fecham com Esc e devolvem o foco.
- [ ] A tabela do gráfico pode ser lida pelo leitor de tela.

**Análise dos dados do teste.** Para cada tarefa, calcular a taxa de conclusão, o tempo mediano e o número de pedidos de ajuda. Problemas observados em dois ou mais participantes viram melhorias prioritárias. Cada problema deve ser associado ao requisito ou critério WCAG afetado.

**Problemas encontrados e melhorias propostas — [A FAZER PELO GRUPO].** Preencher depois dos testes, uma linha por problema observado.

| Problema observado | Participantes afetados (de 5) | Tarefa | Requisito ou critério WCAG | Melhoria proposta |
|---|---|---|---|---|

#### 6.7.2 Verificação técnica do protótipo — [Verificação técnica]

O protótipo passou em todas as 97 verificações automatizadas depois de 5 correções. Esta verificação atende à parte do item 10 que pede conferir os critérios de acessibilidade adotados, mas não substitui o teste manual de teclado nem o teste com pessoas.

**Método.** Um script em Node.js abriu o protótipo no Google Chrome pela biblioteca Playwright e executou o axe-core 4.10.2 com as regras WCAG 2.0, 2.1 e 2.2 de nível A e AA. Foram analisadas 8 telas, em 2 larguras (1280 px e 390 px, tamanho de celular) e 2 temas (claro e escuro): 32 análises. O mesmo script simulou o uso só com teclado e preencheu formulários com entradas válidas e inválidas.

**Problemas encontrados e corrigidos.**

| Problema | Onde | Critério | Correção |
|---|---|---|---|
| Etiqueta “Simulado pelo grupo” com contraste de 4,49:1 (mínimo 4,5:1) | Início e Histórico, tema claro | 1.4.3 Contraste | Cor escurecida para #6B4E00 (7,0:1) |
| Links do rodapé com menos de 24 px de altura | Todas as telas | 2.5.8 Tamanho do alvo | Altura mínima de 44 px |
| Tabela com rolagem lateral que o teclado não alcançava | Biotecnologia, no celular | 2.1.1 Teclado | Região focável e nomeada |
| Botões “Voltar” e “Enviar” visíveis no passo 1 do formulário | Participe | 3.2 Previsível | Regra que esconde de fato elementos ocultos |
| Rótulos do gráfico cortados na borda direita | Histórico, no celular | 1.4.10 Refluxo | Margem direita maior |

Os dois últimos problemas foram vistos nas capturas de tela, não pelo axe. Por precaução, os rótulos de valor do gráfico passaram a usar a cor do texto, porque o laranja do P3 (3,9:1) serve para linhas, mas não para texto pequeno.

**Resultado final.**

- **Acessibilidade automática:** 32 de 32 análises sem nenhuma violação WCAG A/AA detectada; nenhuma tela com rolagem lateral em 390 px.
- **Teclado:** o primeiro Tab mostra “Pular para o conteúdo”; a ordem do foco segue a ordem visual (logotipo, 6 itens do menu, Ajuda, conteúdo, rodapé); todo item focado tem contorno de pelo menos 2 px e fica visível; as setas trocam o ponto selecionado.
- **Entradas inválidas:** foram recusadas com mensagem clara a categoria vazia, o ponto vazio, a descrição com 4 letras, a descrição com telefone, o protocolo em formato errado, o protocolo inexistente, a data inicial depois da final, o filtro sem ponto, o login vazio, a senha errada e 2 linhas inválidas de laudo.
- **Informações indisponíveis:** período sem coleta mostra “Nenhuma medição neste período”; leitura ausente aparece no resumo e no gráfico; sem a turbidez, o P1 fica “Inconclusivo” e o P3 continua “Prioridade”.
- **Erros de JavaScript:** nenhum.

**Limites desta verificação.** Ferramentas automáticas detectam só parte dos problemas de acessibilidade. Elas não avaliam se os textos são compreensíveis, se a ordem faz sentido para quem usa leitor de tela ou se uma pessoa idosa consegue concluir a tarefa. Por isso o teste de teclado manual (lista acima) e o teste com 5 participantes continuam **[A FAZER PELO GRUPO]**.

#### 6.7.3 Matriz de impactos da plataforma

| Dimensão | Impactos positivos | Impactos negativos e riscos | Mitigação |
|---|---|---|---|
| Ambiental | Detecção precoce de piora; menos deslocamentos de campo; base para avaliar intervenções | Fabricação e descarte de sondas, baterias e servidores; reagentes de laboratório | Equipamentos reparáveis; recolhimento contratual; sobriedade digital |
| Social | Participação cidadã com retorno; acesso público à informação; educação ambiental | Exclusão de quem não tem internet; estigma de bairros próximos ao P2 e P3 | Registro também por telefone 156 e presencial; linguagem que não culpa moradores |
| Tecnológico | Dados integrados e rastreáveis; regras de alerta únicas | Falhas de sensores; leituras erradas; dependência de conectividade | Validação de faixa física; aviso de dado ausente; calibração periódica |
| Ético | Transparência: a regra de alerta é publicada em linguagem simples | Leitura de alertas como laudo; acusação de empresas sem prova; fotos que identificam pessoas | Avisos fixos sobre limites dos dados; nenhuma atribuição de origem; ocorrências sem dados pessoais |

### 6.8 Estudo biotecnológico obrigatório

O DNA ambiental mostra quem passou pela água, mas não quantos, quando exatamente nem se estavam vivos; por isso ele complementa, e não substitui, os indicadores físico-químicos.

#### 6.8.1 Funcionamento geral do DNA ambiental (ponto 1)

Peixes, algas e microrganismos liberam DNA na água o tempo todo, por muco, pele, fezes e células mortas. Esse material dura de horas a semanas, conforme temperatura, luz solar e atividade de microrganismos. A análise filtra alguns litros de água, extrai o DNA retido e usa a PCR para copiar um trecho marcador. Pode-se buscar uma única espécie (PCR quantitativa com iniciadores específicos) ou muitos grupos de uma vez (metabarcoding, com iniciadores amplos e sequenciamento). A detecção de espécies a partir de amostras de água foi demonstrada por Ficetola et al. (2008), e o método se consolidou no monitoramento de biodiversidade (TABERLET et al., 2012; DEINER et al., 2017).

#### 6.8.2 Integração dos resultados moleculares à base de dados (ponto 2)

O resultado molecular entra no mesmo banco das medições, ligado pelo ponto de coleta. A integração foi implementada e demonstrada:

| Etapa | Na base de dados | No protótipo |
|---|---|---|
| Receber | CSV padronizado: amostra, ponto, data de coleta, marcador, resultado, observação | Tela da equipe → “Importar laudos de DNA ambiental” |
| Validar | Restrições: ponto existente, marcador cadastrado, resultado em vocabulário fechado | Linhas inválidas recusadas com número e motivo |
| Armazenar | Tabelas `amostras_edna`, `resultados_edna` e `marcadores_edna`; data ausente gravada como nula | Importação atualiza o resultado e marca “importado nesta sessão” |
| Apresentar | Visão `vw_painel_integrado`: última medição e eDNA lado a lado | Tela Biotecnologia, com “o que indica” e “o que não indica” |

O texto de interpretação fica guardado na tabela `marcadores_edna`, e não na tela. Assim, todo resultado aparece sempre com o mesmo limite científico, seja qual for a tela que o exibe.

#### 6.8.3 Indicadores químicos, físicos e biológicos (ponto 3)

| Tipo | O que mede | Exemplos no projeto | Escala de tempo que representa |
|---|---|---|---|
| Físico | Propriedades do meio, sem mudar sua composição | Temperatura, turbidez | O instante da medição |
| Químico | Composição da água e reações nela | pH, oxigênio dissolvido | O instante da medição; varia ao longo do dia |
| Biológico | Presença ou resposta de seres vivos | Marcadores de eDNA de peixe nativo e de cianobactéria | Horas a semanas antes da coleta, porque o DNA persiste e a biota reage a condições acumuladas |

A diferença de escala de tempo explica por que os tipos se complementam: um pico de turbidez pode passar entre duas coletas, mas a ausência prolongada de peixes deixa marca no eDNA.

#### 6.8.4 Bioinformática e análise de sequências (ponto 4)

O sequenciador produz milhões de leituras curtas, e só a bioinformática as transforma em “Detectado” ou “Não detectado”. As etapas usuais são:

1. **Controle de qualidade:** descartar leituras com bases de baixa confiança e remover os iniciadores.
2. **Variantes de sequência:** agrupar leituras idênticas e corrigir erros de sequenciamento, como faz o DADA2 (CALLAHAN et al., 2016).
3. **Remoção de quimeras:** eliminar sequências artificiais formadas na PCR.
4. **Atribuição taxonômica:** comparar cada variante com bancos de referência, por BLAST (ALTSCHUL et al., 1990) contra o GenBank ou com bancos curados como o SILVA para 16S rRNA (QUAST et al., 2013).
5. **Tabela de ocorrência:** contar leituras por grupo e por amostra; o grupo é “Detectado” acima de um mínimo de leituras e se não aparecer nos controles negativos.

Todo o fluxo pode ser reproduzido em plataformas como o QIIME 2 (BOLYEN et al., 2019). A principal limitação está no passo 4: uma espécie nativa sem sequência de referência cadastrada não é identificada, ou é confundida com uma parente.

#### 6.8.5 Técnica de biorremediação: jardins filtrantes (ponto 5)

Jardins filtrantes, ou wetlands construídos, são canteiros com plantas aquáticas e substrato por onde a água passa devagar. As raízes retêm sólidos e abrigam microrganismos que degradam matéria orgânica e consomem nutrientes; a técnica tem cinco décadas de uso documentado no tratamento de efluentes (VYMAZAL, 2011). No Brasil, há aplicação urbana como o sistema de [jardins filtrantes do Parque Orla Piratininga](https://www.maxwell.vrac.puc-rio.br/71688/71688.PDF), em Niterói (RJ).

| Aspecto | Para o Córrego das Palmeiras |
|---|---|
| Possibilidade | Tratar a água de galerias pluviais e lançamentos difusos antes de chegarem ao P2 e ao P3, reduzindo turbidez e matéria orgânica |
| Benefícios extras | Habitat para fauna, paisagem e espaço de educação ambiental |
| Limites | Exige área, manutenção e destino da biomassa; reter metais concentra-os nas plantas; não substitui coleta de esgoto nem fiscalização |
| Como o sistema acompanharia | Comparar turbidez, OD e eDNA antes e depois, com o P1 como referência |

#### 6.8.6 Benefícios e riscos da engenharia genética e dos OGMs (ponto 6)

| Benefícios possíveis | Riscos |
|---|---|
| Biossensores: bactérias com um gene repórter que emitem luz ou cor na presença de um poluente, com teste barato e rápido | Dispersão descontrolada se o organismo escapar do laboratório |
| Microrganismos projetados para degradar compostos específicos | Transferência horizontal de genes para microrganismos nativos |
| Plantas modificadas para acumular ou tolerar contaminantes | Efeitos imprevistos sobre a cadeia alimentar e a biodiversidade local |
| Produção de enzimas e reagentes usados no próprio diagnóstico molecular | Baixa aceitação social e conflitos sobre quem decide o uso |

No Brasil, a Lei 11.105/2005 exige que instituições que trabalham com OGMs tenham Comissão Interna de Biossegurança e atribui à CTNBio a avaliação de risco, inclusive para liberação no meio ambiente. A posição do grupo **[Hipótese de projeto]**: biossensores modificados só em uso laboratorial contido; nenhuma liberação de OGM no córrego.

#### 6.8.7 Diagnóstico molecular no ambiente e na saúde (ponto 7)

As ferramentas são as mesmas (PCR, PCR quantitativa, sequenciamento); mudam a amostra, a pergunta e as consequências.

| Aspecto | Saúde | Meio ambiente |
|---|---|---|
| Amostra | De uma pessoa (sangue, secreção) | Mistura de água, solo ou esgoto, com DNA degradado e substâncias que inibem a PCR |
| Pergunta | Esta pessoa tem o agente? | Este grupo de organismos passou por este lugar? |
| Padronização | Protocolos clínicos validados e registro sanitário | Protocolos ainda em padronização; resultados dependem do laboratório |
| Consequência de um erro | Tratamento errado de um paciente | Investigação desnecessária ou problema ambiental ignorado |

Os dois campos se encontram na vigilância do esgoto. Durante a pandemia, um projeto piloto da Agência Nacional de Águas com o INCT ETEs Sustentáveis da UFMG [quantificou o material genético do SARS-CoV-2 no esgoto de Belo Horizonte e Contagem](https://www.gov.br/ana/pt-br/assuntos/acontece-na-ana/monitoramento-covid-esgotos/boletins-monitoramento-covid-esgotos/nota-tecnica-no-012020.pdf) como apoio à vigilância epidemiológica: uma amostra ambiental respondendo a uma pergunta de saúde pública.

#### 6.8.8 Aplicação farmacêutica da biotecnologia moderna (ponto 8)

A insulina humana recombinante foi o primeiro medicamento produzido por DNA recombinante. Genes sintéticos das cadeias A e B da insulina foram inseridos na bactéria *Escherichia coli*, cultivada em grandes fermentadores, e o produto da Eli Lilly (Humulin) foi [aprovado pela FDA em outubro de 1982](https://pmc.ncbi.nlm.nih.gov/articles/PMC8152450). Antes disso, os pacientes usavam insulina extraída de pâncreas de bois e porcos. O exemplo mostra o lado da engenharia genética em sistema fechado e controlado, oposto ao da liberação no ambiente discutida em 6.8.6.

#### 6.8.9 Decisões que não podem ser tomadas apenas com os dados disponíveis (ponto 9)

| Decisão tentadora | Por que os dados não bastam | O que seria necessário |
|---|---|---|
| “Os peixes nativos sumiram do P3” | Não detectar não prova ausência; há uma única amostra, sem réplicas | Réplicas, PCR quantitativa e levantamento de fauna |
| “Há risco de toxina no P3” | DNA de cianobactéria não indica produção de toxina | Genes de toxina (como mcyE) e análise química de cianotoxinas |
| “A indústria é a culpada” | Proximidade não é causa; o DNA pode vir de trechos acima | Vistoria, rastreamento de lançamentos, análise de efluentes |
| “A biodiversidade está caindo” | Uma campanha só, sem data de coleta | Série de coletas datadas ao longo de meses |
| “O P2 tem cianobactéria” | O resultado é indeterminado | Nova amostra |
| “A água do P1 é boa para beber ou nadar” | Faltam parâmetros microbiológicos e regulamentares | Ensaios do enquadramento legal e de potabilidade |

### 6.9 Protótipo navegável

O protótipo tem 7 telas e uma página de ajuda, funciona no navegador e usa os mesmos dados da base.

**Tecnologia.** HTML, CSS e JavaScript puros, em um único arquivo, sem servidor; uma das opções aceitas no item 8. Os dados embutidos são os do banco SQLite: 9 medições, 171 leituras de sensor, 6 resultados de eDNA e 8 ocorrências. A fonte Atkinson Hyperlegible foi escolhida por ter sido desenhada para leitores com baixa visão.

**Sinalização para avaliação.** Cada tela abre com uma caixa tracejada “Anotação do projeto”, que diz qual tela mínima do enunciado ela cumpre, quais requisitos atende e quais dados são do enunciado ou simulados. As anotações podem ser escondidas pelo rodapé, para mostrar a tela como o cidadão a veria. A página Ajuda traz o mapa completo do protótipo.

| Tela mínima do enunciado | Tela do protótipo | O que mostrar na avaliação |
|---|---|---|
| 1. Página inicial | Início | Esquema do córrego com P1, P2 e P3 coloridos pela situação; cartões com valores e eDNA |
| 2. Monitoramento | Monitoramento | Seletor de ponto; 4 indicadores com limite; situação do sensor; simulação de falha |
| 3. Histórico e gráficos | Histórico | Filtros por ponto, indicador, fonte e período; gráfico com título-achado; resumo em texto; tabela; cópia em CSV |
| 4. Participação cidadã | Participe | Formulário em 3 passos com revisão e protocolo; consulta do protocolo |
| 5. Biotecnologia | Biotecnologia | Resultados de eDNA, “o que indica / o que não indica”, leitura integrada, limites do método |
| 6. Administração | Acesso da equipe (usuário `equipe`, senha `aurora2026`) | Indicadores consolidados; estabilidade dos alertas; triagem de ocorrências; importação de laudos |
| Extra (Etapa 2) | Aprenda | Explicações educativas e glossário |

![Telas 1, 2, 3 e 5 do protótipo: Início com o esquema do córrego, Monitoramento do P3, Histórico do oxigênio pelos sensores e Biotecnologia](imagens/figura-3-telas-1-2-3-5.png)

*Figura — Telas 1, 2, 3 e 5 do protótipo, capturadas em 1280 px. A caixa tracejada no topo de cada tela é a anotação do projeto para avaliação.*

![Tela 4 com erro de descrição curta e com o protocolo OC-2026-0009 gerado, Tela 7 da equipe e a página inicial em um celular](imagens/figura-4-telas-4-7-e-celular.png)

*Figura — Tratamento de entrada inválida e envio com protocolo (Tela 4), área da equipe (Tela 7) e página inicial em 390 px de largura.*

**Tratamento de entradas inválidas.**

- Formulário de ocorrência: não avança sem categoria ou ponto; recusa descrição com menos de 10 letras; recusa telefone, e-mail ou CPF no texto (minimização de dados da LGPD); recusa foto que não seja JPG ou PNG ou que passe de 5 MB. Os erros aparecem em uma caixa-resumo com links e junto de cada campo, e o foco vai para o resumo.
- Consulta de protocolo: recusa formato errado e informa quando o número não existe; aceita letras minúsculas.
- Histórico: recusa data inicial depois da final, período fora de 02/10 a 16/10 e filtro sem ponto.
- Acesso da equipe: recusa campos vazios e senha errada, sem revelar qual dos dois está errado.
- Importação de laudos: recusa linha a linha ponto inexistente, data fora do formato, marcador desconhecido e resultado fora do vocabulário; importa só as linhas válidas e permite desfazer.

**Tratamento de informações indisponíveis.**

- Data de coleta do eDNA ausente aparece como “não informada pelo laboratório”.
- Leitura de sensor ausente aparece como lacuna no gráfico, linha pontilhada, “sem leitura” na tabela e contagem no resumo.
- A leitura impossível de pH 15,2 aparece como descartada, com explicação.
- Período sem coleta semanal mostra “Nenhuma medição neste período” e sugere a série dos sensores.
- A caixa “simular sensor de turbidez sem comunicação”, na tela Monitoramento, mostra a regra de dado ausente funcionando: o P1 vira “Inconclusivo” e o P3 continua “Prioridade”.

**Roteiro de demonstração (cerca de 3 minutos na apresentação).**

1. Início: mostrar o esquema do córrego e ler o selo do P3.
2. Monitoramento: escolher o P3 com as setas do teclado; ler “62 NTU, fora do limite”.
3. Histórico: trocar para “Sensores, a cada 6 horas” e “Oxigênio dissolvido”; mostrar as quedas de madrugada no P2.
4. Biotecnologia: ler o cartão do P3, incluindo “o que não indica”.
5. Participe: tentar enviar com 4 letras, corrigir e receber o protocolo.
6. Acesso da equipe: mudar a situação de uma ocorrência e mostrar a tabela de estabilidade dos alertas.

## 7. Discussão

Os dados permitem ordenar onde investigar, mas não permitem diagnosticar. Essa distinção guiou tanto a análise quanto o desenho da interface.

### 7.1 Interpretação responsável dos dados

**O que os dados sustentam.** Há um gradiente claro da nascente para jusante: em 16/10 a turbidez vai de 12 (P1) a 38 (P2) e 62 NTU (P3), e o OD cai de 7,1 a 4,8 e 3,4 mg/L. A piora no tempo aparece nos dois pontos mais abaixo, e no P3 ela acelerou. Quando indicadores independentes (físico, químico e biológico) apontam na mesma direção, como no P3, a confiança na prioridade aumenta. Quando divergem ou são inconclusivos, como o eDNA indeterminado do P2, o sistema deve pedir mais dados, e não escolher a leitura mais alarmante.

**Decisões que não podem ser tomadas com os dados disponíveis.**

- **Declarar a água potável, imprópria ou própria para banho:** isso exige parâmetros microbiológicos e procedimentos de amostragem que a base não tem.
- **Afirmar conformidade ou infração legal:** depende do enquadramento do corpo hídrico. Os limiares didáticos coincidem com a Classe 2 da Resolução CONAMA 357/2005 para pH (6 a 9) e OD (mínimo de 5 mg/L), mas a Classe 2 admite turbidez até 100 UNT ([ANA](https://www.ana.gov.br/portalpnqa/Publicacao/CursoEnquadramentoPortal2010.pdf)), bem acima dos 25 NTU do exercício. Um trecho pode estar em “Atenção” aqui e ainda atender a um limite legal.
- **Atribuir a origem da poluição à área industrial:** a proximidade não é prova. Seria preciso investigar lançamentos, galerias pluviais e a contribuição do trecho urbano que chega ao P3.
- **Afirmar risco de cianotoxinas:** detectar DNA de cianobactéria não mostra produção de toxina; faltam genes de toxina e análise química.
- **Concluir que os peixes nativos desapareceram do P3:** não detectar DNA não prova ausência.
- **Aplicar sanções ou multas:** exige fiscalização, laudo oficial e devido processo.

**Limitações da base.** São só 3 pontos e 3 datas, sem dados de chuva ou vazão; a turbidez costuma subir após chuvas, e sem esse dado parte da variação pode ser climática. O horário das coletas não é informado, e o OD varia ao longo do dia. A temperatura e a série IoT foram criadas pelo grupo, e o eDNA não tem data. Por isso, os achados da série IoT (quedas noturnas de OD e alerta oscilante) são ilustrações de comportamento do sistema, não fatos sobre o córrego.

**Ocorrências cidadãs como pista, não como prova.** As ocorrências fictícias registram um cano de esgoto no P2 e espuma, água escura e peixes mortos no P3. Elas orientam a vistoria, mas são relatos não verificados e não devem aparecer na interface pública como confirmação de causa.

**Implicações para o desenho do sistema.** Três escolhas de IHC reduzem o risco de má interpretação: o selo de situação usa “Prioridade de investigação”, e não “Poluído”; cada resultado de eDNA vem com “o que não indica”; e dado ausente é dito como ausente. A janela de 24 horas no alerta operacional evita que oscilações próximas ao limiar gerem alarmes que desgastam a confiança da equipe e do público.

### 7.2 Sustentabilidade, ética e responsabilidade

> **No enunciado:** item 11 — avaliação crítica dos impactos positivos e negativos, com os seis temas obrigatórios, LGPD e distinção entre hipóteses, resultados simulados e evidências.

O saldo da plataforma é positivo se ela for usada para priorizar investigação e dar voz ao morador, e negativo se for lida como laudo ou se gerar equipamentos descartados sem destino. Os seis temas obrigatórios estão abaixo, cada um com o ponto forte, o risco e a medida adotada.

| Tema obrigatório | Ponto forte | Risco ou impacto negativo | Medida adotada no projeto |
|---|---|---|---|
| Consumo energético da infraestrutura digital | Operação estimada em 306,6 kWh por ano da rede e 11,8 kg de CO₂ **[Estimativa]**; o protótipo inteiro pesa 83 KB (27 KB comprimido) **[Verificação técnica]** | Data centers usam cerca de 1,5% da eletricidade mundial; sistemas pequenos somados também pesam | Leituras a cada 6 h, retenção de 2 anos dos dados brutos, páginas leves, sensores solares |
| Destinação de equipamentos eletrônicos | Sondas modulares permitem trocar só o eletrodo de pH | Baterias, eletrodos e placas viram resíduo perigoso; equipamento público fica fora da logística reversa doméstica | Cláusula contratual de recolhimento; cadastro de equipamentos no banco (`sensores`) com data de instalação |
| Inclusão digital | WCAG 2.2 AA verificado, linguagem simples, fonte para baixa visão, funciona em celular simples | Quem não tem internet ou celular fica de fora da participação digital | Canais não digitais previstos no banco (telefone 156 e presencial); textos sem culpar moradores de bairros próximos |
| Transparência dos algoritmos | Regra de alerta simples e publicada na tela Ajuda; nenhum modelo “caixa-preta” | Uma regra simples pode parecer mais exata do que é | Regra escrita em linguagem simples; dado ausente nunca vira “Sem alerta”; regra de estabilidade testada e documentada (seção 6.5) |
| Privacidade dos usuários | Aviso anônimo, sem nome, CPF, e-mail ou telefone | Texto livre e fotos podem identificar pessoas | Bloqueio de telefone, e-mail e CPF na descrição; metadados de localização das fotos apagados; login só para a equipe |
| Responsabilidade na divulgação científica | Cada resultado de eDNA mostra “o que não indica” | Manchetes como “indústria contamina o córrego” a partir de dados fictícios ou incompletos | Selo “Prioridade de investigação” em vez de “Poluído”; aviso fixo de dados fictícios; nenhuma atribuição de origem |

**LGPD.** Os registros de ocorrência não guardam dados de pessoas identificadas ou identificáveis, aplicando o princípio da necessidade (art. 6º, III, da Lei 13.709/2018). Os únicos dados pessoais do sistema são os de login da equipe; em um sistema real, o tratamento teria como base a execução de políticas públicas pela administração (art. 7º, III) e precisaria de controle de acesso e registro de quem alterou cada ocorrência. Como recomenda o enunciado, todas as ocorrências deste trabalho são fictícias.

**Hipóteses, resultados simulados e evidências científicas.**

| Categoria | O que, neste projeto, pertence a ela |
|---|---|
| Resultados simulados | pH, turbidez, OD e eDNA (fornecidos pelo enunciado); temperatura, série de sensores e ocorrências (criadas pelo grupo); classificações calculadas sobre esses dados |
| Hipóteses | Personas; genes marcadores 12S e 16S; ciclo diário de oxigênio na série de sensores; premissas de consumo de energia; recomendação da regra de 24 horas |
| Evidências científicas | Funcionamento e limites do eDNA; papel da bioinformática; eficácia de wetlands construídos; dados globais de resíduos eletrônicos e de energia de data centers; critérios WCAG; legislação citada |
| Verificações técnicas | Contrastes calculados; 97 verificações automatizadas do protótipo; testes de validação do banco |

Nenhuma informação deste trabalho pode ser usada como diagnóstico ambiental oficial ou recomendação de consumo de água. Esse aviso aparece no topo de todas as telas do protótipo e no início deste relatório.

### 7.3 Respostas às questões do estudo de caso

**1. De que maneira a aplicação de princípios de IHC melhora a participação dos cidadãos no monitoramento ambiental?**

A IHC mostrou que a barreira de participação não é falta de interesse, mas atrito: cadastro, jargão e medo de errar (persona Marlene, seção 6.1). Por isso o registro não pede cadastro, tem 3 passos curtos, aceita “Não sei” como local e mostra uma revisão antes de enviar. O protocolo e a resposta pública fecham o ciclo: quem vê que seu aviso foi atendido tende a avisar de novo. As categorias fechadas também melhoram a qualidade do dado que chega à equipe, porque a triagem fica mais rápida.

**2. Quais problemas de usabilidade podem prejudicar idosos, pessoas com deficiência visual e usuários com pouca experiência digital? Como esses problemas podem ser reduzidos?**

| Grupo | Problemas típicos | Como o projeto reduz |
|---|---|---|
| Idosos | Letra pequena, contraste baixo, botões pequenos e próximos, tempo limite em formulários | Texto de 17 px ampliável a 200%, contraste mínimo de 4,5:1, alvos de 44 px, nenhum tempo limite |
| Pessoas com deficiência visual | Gráfico sem alternativa, campos sem rótulo, situação indicada só por cor, mensagens que o leitor de tela não anuncia | Resumo em texto e tabela em todo gráfico; rótulos visíveis; selos com texto e ícone; protocolo anunciado em região `aria-live` |
| Pouca experiência digital | Termos técnicos, cadastro obrigatório, menus confusos, erros sem explicação | Linguagem simples, registro anônimo, 6 itens de menu com palavras do cotidiano, erros que dizem como corrigir |

As medidas foram verificadas automaticamente (seção 6.7.2), mas só o teste com pessoas desses grupos confirma se funcionam **[A FAZER PELO GRUPO]**.

**3. Como a arquitetura da informação influencia a compreensão dos dados ambientais apresentados na plataforma?**

A mesma medição pode informar ou confundir conforme a ordem em que aparece. A plataforma usa três camadas: primeiro a resposta (selo “Prioridade de investigação”), depois a explicação (“turbidez e oxigênio fora do limite”) e por último o dado (62 NTU, histórico, tabela). A organização por ponto, seguindo o curso do córrego, deixa visível o gradiente da nascente para jusante sem que o leitor precise comparar tabelas. Os rótulos na linguagem do usuário e a regra de alerta guardada em um só lugar fazem com que todas as telas digam a mesma coisa sobre o mesmo dado.

**4. Qual é a contribuição dos sensores IoT e da integração de dados para uma cidade inteligente e sustentável?**

Sensores trocam fotografias semanais por um filme contínuo. Na série de sensores criada pelo grupo, o oxigênio do P2 fica abaixo de 5,0 mg/L de madrugada desde 06/10, dez dias antes de a coleta diurna registrar o problema **[Simulado pelo grupo]**; o exemplo ilustra por que coletas pontuais podem subestimar a falta de oxigênio. A integração permite ler juntos sensores, laudos e relatos de moradores e acionar a fiscalização, a limpeza urbana ou a vigilância sanitária a partir do mesmo painel. Os custos são calibração, manutenção, falhas de comunicação e resíduos eletrônicos, que o sistema precisa registrar em vez de esconder.

**5. Considerando a base de dados fornecida, qual ponto de monitoramento apresenta maior prioridade de investigação? Justifique com os indicadores.**

O **P3**. Em 16/10/2026 ele ultrapassou os três limiares: pH 5,8 (abaixo de 6,0), turbidez 62 NTU (mais que o dobro de 25) e oxigênio 3,4 mg/L (abaixo de 5,0). Foi o único ponto em “Prioridade de investigação” nas três coletas, e sua piora acelerou: a turbidez subiu 13 e depois 14 NTU por semana (+77% no período) e o OD caiu 0,5 e depois 0,7 mg/L. O eDNA converge: cianobactéria detectada e peixe nativo não detectado. O P2 vem em segundo, porque passou de “Sem alerta” a “Prioridade” em duas semanas. A prioridade indica onde investigar primeiro, não a causa (seções 6.5 e 7.1).

**6. Quais impactos ambientais podem ser provocados pelo próprio sistema tecnológico, considerando consumo de energia, equipamentos e resíduos eletrônicos?**

A operação consumiria cerca de 306,6 kWh por ano da rede e emitiria cerca de 11,8 kg de CO₂ **[Estimativa]**, um impacto pequeno graças à matriz elétrica brasileira e aos sensores solares. O impacto maior está nos objetos: eletrodos de pH trocados a cada 1 a 2 anos, baterias a cada 3 a 5 anos, placas eletrônicas e painéis no fim da vida, além de reagentes e plásticos das análises de laboratório. Em escala global, só 22,3% do lixo eletrônico de 2022 teve reciclagem formal documentada. As medidas propostas são equipamentos reparáveis, recolhimento previsto em contrato e coleta de dados só na frequência necessária (seção 6.4).

**7. Como a biotecnologia pode contribuir para identificar alterações na biodiversidade ou apoiar a recuperação de ambientes degradados?**

Para identificar alterações, o eDNA detecta grupos de organismos sem capturá-los, a partir de alguns litros de água, e permite comparar pontos e datas: no estudo, o contraste entre o P1 (peixe nativo detectado) e o P3 (não detectado, com cianobactéria) é um sinal a investigar. Repetido ao longo do tempo, o eDNA mostra tendências e a chegada de espécies invasoras ou de grupos potencialmente tóxicos. Para recuperar, a biorremediação com jardins filtrantes pode tratar a água antes que ela chegue ao córrego, e o próprio eDNA serve para medir se a vida aquática volta depois da intervenção (seções 6.8.1 e 6.8.5).

**8. Qual é o papel da bioinformática na organização e interpretação de dados genéticos ambientais? Quais são suas limitações?**

A bioinformática transforma milhões de leituras de sequenciamento em uma tabela de quais grupos estão em cada amostra: filtra a qualidade, corrige erros, remove artefatos, compara as sequências com bancos de referência e aplica limiares e controles negativos (seção 6.8.4). Ela também organiza os metadados (ponto, data, método, lote) que permitem cruzar biologia e química, como faz a base do projeto. As limitações são bancos de referência incompletos para espécies nativas brasileiras, viés dos iniciadores, risco de contaminação, número de leituras que não equivale à abundância e a necessidade de pessoal especializado e capacidade de computação.

**9. Quais benefícios, riscos e questões éticas devem ser considerados ao empregar engenharia genética, organismos geneticamente modificados e diagnósticos moleculares?**

Os benefícios incluem biossensores baratos, microrganismos e plantas que retêm ou degradam poluentes e medicamentos como a insulina recombinante (seções 6.8.6 e 6.8.8). Os riscos vêm sobretudo da liberação no ambiente: dispersão, transferência de genes e efeitos sobre espécies nativas, por isso a Lei 11.105/2005 exige avaliação pela CTNBio. As questões éticas são o princípio da precaução diante de efeitos irreversíveis, a justiça sobre quem recebe os riscos e quem recebe os benefícios, a transparência com a população e a privacidade genética: amostras de água podem conter DNA humano, que o laboratório deve descartar. No diagnóstico molecular, a ética exige comunicar os limites do resultado, para que “Detectado” não vire “Contaminado”.

**10. Como integrar IHC, acessibilidade, sustentabilidade, cidades inteligentes e biotecnologia em uma solução viável, inclusiva e cientificamente responsável?**

A integração proposta tem cinco decisões que se apoiam umas nas outras:

1. **Um banco, uma regra:** sensores, laboratório e cidadãos gravam no mesmo banco, e a regra de alerta fica nele, o que mantém coerência entre telas (cidade inteligente).
2. **Interface para quem tem mais dificuldade:** se funciona para a Shirley e para a Andy, funciona para todos (IHC e acessibilidade).
3. **Ciência com seus limites à vista:** cada resultado biológico vem com o que não prova (biotecnologia responsável).
4. **Tecnologia proporcional:** 3 sensores solares, leituras a cada 6 horas, páginas leves e fim de vida previsto em contrato (sustentabilidade).
5. **Viabilidade por etapas:** começar com os 3 pontos e o laboratório parceiro, usar software aberto e expandir só depois de validar com moradores e equipe.

O protótipo mostra que essas decisões cabem em um sistema simples de 83 KB, sem servidor; a viabilidade em escala real depende de orçamento para sensores, análises e manutenção, que este trabalho não estimou.

## 8. Resultado esperado e questão central

Sete dos nove critérios de conclusão estão atendidos e dois estão parcialmente atendidos. Acessibilidade e IHC só se completam com os testes com pessoas do item 10, que ficam a cargo do grupo.

| Critério do enunciado | Como este trabalho responde | Situação |
|---|---|---|
| Solução digital coerente | Cada problema da prefeitura leva a requisitos, telas e dados rastreáveis (seções 2 e 6.1) | Atendido |
| Solução navegável | Protótipo com 7 telas publicado, com endereço e arquivo (seções 6.2 e 6.9) | Atendido |
| Solução acessível | Checklist WCAG 2.2 AA, paleta verificada e 32 análises automáticas sem violações (seções 6.3 e 6.7.2); falta o teste com pessoas | Parcial |
| Uso de dados ambientais simulados | Banco SQLite com regras de alerta, 10 consultas, gráficos e testes de validação (seção 6.5) | Atendido |
| Uso de informações biotecnológicas | eDNA integrado ao banco e lido em conjunto com os indicadores físico-químicos (seção 6.6) | Atendido |
| Fundamentação científica | 12 áreas fundamentadas, com normas e literatura (seção 4 e referências) | Atendido |
| Aplicação dos princípios de IHC | Personas, requisitos e arquitetura da informação prontos; falta ouvir usuários reais | Parcial |
| Interpretação responsável dos dados | Lista do que os dados não permitem decidir e textos de limitação na interface (seção 7) | Atendido |
| Relação entre tecnologia, sociedade e meio ambiente | Consumo, ciclo de vida, resíduos e matriz de impactos (seções 6.4 e 6.7) | Atendido |

### Resposta à questão central

*Como a informática pode transformar informações ambientais e biotecnológicas em conhecimento útil para a construção de cidades mais sustentáveis, inclusivas e ambientalmente responsáveis?*

A informática transforma dado em conhecimento quando faz quatro coisas ao mesmo tempo: **integra**, **traduz**, **envolve** e **delimita**.

**Integra** porque coloca medições de sensores, laudos de laboratório e relatos de cidadãos no mesmo banco, ligados ao mesmo ponto. Separados, os arquivos de Santa Aurora mostravam números; juntos, mostram que no P3 três indicadores físico-químicos e um biológico apontam na mesma direção. **Traduz** porque a IHC e a acessibilidade convertem “turbidez 62 NTU, OD 3,4 mg/L” em “Prioridade de investigação: água turva e com pouco oxigênio”, compreensível para a Marlene, operável pela Andy com leitor de tela e acionável pelo Jason no campo.

**Envolve** porque transforma o morador em fonte de informação, com protocolo e resposta pública, o que torna a cidade inteligente também participativa. **Delimita** porque um sistema responsável diz o que não sabe: não atesta potabilidade, não acusa sem prova, não lê DNA como toxina e não esconde dado ausente.

Por fim, a própria tecnologia precisa ser sustentável. Uma plataforma que consome cerca de 307 kWh por ano da rede, mas gera sondas e baterias descartadas sem destino, apenas desloca o problema ambiental. Cidades mais sustentáveis exigem sistemas que medem o ambiente e também medem a si mesmos.

## 9. Conclusão

O projeto mostrou que os dados dispersos de Santa Aurora, quando integrados e apresentados com cuidado, apontam o P3 como prioridade de investigação e o P2 como o trecho de piora mais rápida. Esse resultado nasce da combinação entre regras de alerta transparentes, indicadores físico-químicos e DNA ambiental, e não de um único número.

Os objetivos específicos foram cumpridos: personas e requisitos rastreáveis, arquitetura da informação em camadas, protótipo navegável com 7 telas e acessibilidade verificada por ferramenta, base de dados funcional com consultas e alertas, integração do eDNA ao sistema e avaliação do impacto ambiental da própria solução. Duas decisões do grupo foram além do enunciado: tratar dado ausente sem presumir normalidade e usar a pior classificação das últimas 24 horas no alerta operacional, depois de testar e descartar a regra de duas leituras seguidas.

A principal lição é que responsabilidade científica também é requisito de interface. Um selo que diz “Prioridade de investigação” em vez de “Poluído”, um resultado de DNA acompanhado do que ele não prova e um aviso de sensor sem comunicação protegem a confiança pública tanto quanto a precisão dos dados.

**Trabalhos futuros.** Aplicar os testes de usabilidade e de teclado com pelo menos 5 participantes e registrar as melhorias (seção 6.7, **[A FAZER PELO GRUPO]**); incluir dados de chuva e vazão para separar efeitos climáticos de lançamentos; acrescentar ao laudo de eDNA a data de coleta e marcadores de genes de toxina; e estimar o custo de implantação e manutenção dos sensores.

## Referências

AGÊNCIA NACIONAL DE ÁGUAS (ANA). **Curso de enquadramento de corpos d’água** [material didático]. Brasília: ANA, 2010. Disponível em: [ana.gov.br](https://www.ana.gov.br/portalpnqa/Publicacao/CursoEnquadramentoPortal2010.pdf).

AGÊNCIA NACIONAL DE ÁGUAS E SANEAMENTO BÁSICO (ANA); INCT ETEs SUSTENTÁVEIS. **Nota Técnica nº 01/2020**: monitoramento da ocorrência do novo coronavírus no esgoto. Brasília: ANA, 2020. Disponível em: [gov.br/ana](https://www.gov.br/ana/pt-br/assuntos/acontece-na-ana/monitoramento-covid-esgotos/boletins-monitoramento-covid-esgotos/nota-tecnica-no-012020.pdf).

ALTSCHUL, S. F. et al. Basic local alignment search tool. **Journal of Molecular Biology**, v. 215, n. 3, p. 403-410, 1990.

BALDÉ, C. P. et al. **The Global E-waste Monitor 2024**. Genebra; Bonn: ITU; UNITAR, 2024. Disponível em: [itu.int](https://www.itu.int/en/ITU-D/Environment/Pages/Publications/The-Global-E-waste-Monitor-2024.aspx).

BARBOSA, S. D. J.; SILVA, B. S. **Interação humano-computador**. Rio de Janeiro: Elsevier, 2010.

BARBOZA, A. C. D. **Avaliação do sistema de jardins filtrantes do Parque Orla Piratininga diante de cenários de precipitações intensas e mudanças climáticas**. Trabalho de Conclusão de Curso — PUC-Rio, Rio de Janeiro, [s.d.]. Disponível em: [puc-rio.br](https://www.maxwell.vrac.puc-rio.br/71688/71688.PDF).

BOLYEN, E. et al. Reproducible, interactive, scalable and extensible microbiome data science using QIIME 2. **Nature Biotechnology**, v. 37, p. 852-857, 2019.

BRASIL. Conselho Nacional do Meio Ambiente. **Resolução nº 357, de 17 de março de 2005**. Dispõe sobre a classificação dos corpos de água e diretrizes ambientais para o seu enquadramento. Brasília, 2005.

BRASIL. Conselho Nacional do Meio Ambiente. **Resolução nº 401, de 4 de novembro de 2008**. Estabelece critérios para pilhas e baterias. Brasília, 2008.

BRASIL. **Decreto nº 10.240, de 12 de fevereiro de 2020**. Sistema de logística reversa de produtos eletroeletrônicos de uso doméstico. Disponível em: [planalto.gov.br](https://planalto.gov.br/ccivil_03/_ato2019-2022/2020/decreto/d10240.htm).

BRASIL. **Lei nº 11.105, de 24 de março de 2005**. Lei de Biossegurança. Brasília, 2005.

BRASIL. **Lei nº 12.305, de 2 de agosto de 2010**. Política Nacional de Resíduos Sólidos. Brasília, 2010.

BRASIL. **Lei nº 13.146, de 6 de julho de 2015**. Lei Brasileira de Inclusão da Pessoa com Deficiência. Brasília, 2015.

BRASIL. **Lei nº 13.709, de 14 de agosto de 2018**. Lei Geral de Proteção de Dados Pessoais (LGPD). Disponível em: [planalto.gov.br](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm).

CALLAHAN, B. J. et al. DADA2: high-resolution sample inference from Illumina amplicon data. **Nature Methods**, v. 13, p. 581-583, 2016.

CENÁRIO ENERGIA. Emissões de CO₂ na geração de energia atingem mínima em onze anos no Brasil. 15 fev. 2024. Disponível em: [cenarioenergia.com.br](https://cenarioenergia.com.br/2024/02/15/emissoes-de-co2-na-geracao-de-energia-atingem-minima-em-onze-anos-no-brasil/). (Dados do MCTI.)

COOPER, A. et al. **About Face**: the essentials of interaction design. 4. ed. Indianapolis: Wiley, 2014.

DEINER, K. et al. Environmental DNA metabarcoding: transforming how we survey animal and plant communities. **Molecular Ecology**, v. 26, n. 21, p. 5872-5895, 2017.

DEQUE SYSTEMS. **axe-core**: accessibility engine for automated web UI testing. Versão 4.10.2. Software livre. Disponível em: [github.com/dequelabs/axe-core](https://github.com/dequelabs/axe-core).

FICETOLA, G. F. et al. Species detection using environmental DNA from water samples. **Biology Letters**, v. 4, n. 4, p. 423-425, 2008.

INSTITUTO FEDERAL DE SÃO PAULO (IFSP). **Projeto de Conclusão da Disciplina Meio Ambiente, Sustentabilidade e Informática**: estudo de caso EcoCidade Bio. 2026. Enunciado da atividade.

INTERNATIONAL ENERGY AGENCY (IEA). **Energy and AI**. Paris: IEA, 2025. Disponível em: [iea.org](https://www.iea.org/reports/energy-and-ai/executive-summary).

INTERNATIONAL ORGANIZATION FOR STANDARDIZATION. **ISO 9241-11:2018**: Ergonomics of human-system interaction — Part 11: Usability: definitions and concepts. Genebra: ISO, 2018.

INTERNATIONAL ORGANIZATION FOR STANDARDIZATION. **ISO 37122:2019**: Sustainable cities and communities — Indicators for smart cities. Genebra: ISO, 2019.

MICROSOFT. **Playwright**: framework for web testing and automation. Software livre. Disponível em: [playwright.dev](https://playwright.dev/).

NIELSEN, J. **Usability engineering**. San Diego: Academic Press, 1993.

QUAST, C. et al. The SILVA ribosomal RNA gene database project: improved data processing and web-based tools. **Nucleic Acids Research**, v. 41, n. D1, p. D590-D596, 2013.

RIGGS, A. D. Making, cloning, and the expression of human insulin genes in bacteria: the path to Humulin. **Endocrine Reviews**, v. 42, n. 3, p. 374-380, 2021. Disponível em: [PMC8152450](https://pmc.ncbi.nlm.nih.gov/articles/PMC8152450).

ROGERS, Y.; SHARP, H.; PREECE, J. **Design de interação**: além da interação humano-computador. 3. ed. Porto Alegre: Bookman, 2013.

ROSENFELD, L.; MORVILLE, P.; ARANGO, J. **Information architecture**: for the web and beyond. 4. ed. Sebastopol: O’Reilly, 2015.

TABERLET, P. et al. Environmental DNA. **Molecular Ecology**, v. 21, n. 8, p. 1789-1793, 2012.

VYMAZAL, J. Constructed wetlands for wastewater treatment: five decades of experience. **Environmental Science & Technology**, v. 45, n. 1, p. 61-69, 2011.

W3C. **Web Content Accessibility Guidelines (WCAG) 2.2**. W3C Recommendation, 5 out. 2023. Disponível em: [w3.org/TR/WCAG22](https://www.w3.org/TR/WCAG22/). Resumo das novidades em: [What’s New in WCAG 2.2](https://w3.org/WAI/standards-guidelines/wcag/new-in-22/).
