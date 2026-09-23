# Aula 1
## Ciência de Dados
- A ciência de dados pode ser definida como a **disciplina que fornece princípios, metodologias e orientações para transformação, validação, análise e criação de significado a partir de dados**.
- O **objetivo** é **extrair conhecimento de conjuntos de dados usando as análises estatísticas tradicionais, algoritmos e ferramentas**. As mesmas técnicas podem ser usadas em pequenos e grandes volumes de dados (Big Data).
## Big Data
- Refere-se à enorme quantidade de dados gerados a partir de várias fontes, como transações comerciais, mídias sociais, sensores, dispositivos móveis, entre outros.
- Podemos classificar uma fonte de dados como Big Data quando utilizamos os 3 Vs:
	- Volume (grande quantidade de dados).
	- Velocidade (gerados em alta velocidade).
	- Variedade (diversidade de tipos e formatos de dados).
## Outliers
- São valores que se diferenciam significativamente do restante dos dados em um conjunto.
- Esses valores extremos estão longe da média ou dos demais valores do conjunto e podem ser causados por erros de medição, comportamentos anômalos ou eventos raros.
- Outliers podem distorcer a análise de dados e afetar negativamente a precisão de modelos e estatísticas descritivas.
- A detecção e o tratamento de outliers são importantes em várias aplicações, pois podem indicar erros de coleta de dados, indicar a presença de eventos incomuns ou fornecer insights valiosos sobre comportamentos excepcionais no conjunto de dados.
![[Pasted image 20260922185916.png]]
## Insights
- São percepções, entendimentos e conclusões significativas e valiosas obtidas a partir da análise de dados ou informações.
- São descobertas que vão além dos dados brutos, revelando padrões, tendências ou relações ocultas que podem levar a novas ideias, melhorias em processos, estratégias de negócios mais eficientes e tomadas de decisões informadas.
- Podem ser alcançados por meio de diversas técnicas de análise de dados, como estatísticas descritivas, mineração de dados, aprendizado de máquina e visualização de dados.
- Os insights são valiosos para orientar ações e estratégias de negócios, identificar oportunidades e desafios, prever tendências futuras e entender melhor o comportamento dos clientes e usuários.
## Tomada de Decisão Orientada por Dados
#DOD
- A **Tomada de Decisão Orientada por Dados (DOD)** é uma prática na qual as **decisões são embasadas na análise de dados, em vez de dependerem apenas da intuição dos executivos da alta direção**.
- A DOD não é uma prática de "tudo ou nada", e muitas empresas a adotam em diferentes graus, dependendo das suas necessidades e recursos.
- **A DOD se baseia nos dados**, em **análises e técnicas de estatística e probabilidade para indicar tendências**, insights e outliers que somados ao know-how dos executivos da alta direção podem apoiar a tomada de decisão estratégica.
## Soluções baseadas em dados
### Soluções de dados
- Limpeza e pré-processamento de dados
- Análise exploratória de dados
- Modelagem preditiva
- Segmentação e personalização
- Detecção de anomalias e fraudes
- Otimização de processos
- Visualização de dados
### Abordagens que também são consideradas soluções baseadas em dados
- Text mining
- Processamento de linguagem natural
- Aprendizado de máquina interpretável
- Aprendizado de reforço
- Dados em tempo real
- Automação de processos
## KDD
#KDD
- **Descoberta de Conhecimento em Bancos de Dados** ou **KDD (Knowledge Discovery in Databases)** refere-se ao processo de identificar padrões, conhecimentos úteis e informações ocultas em grandes volumes de dados.
- Esse processo abrange **várias etapas**, incluindo seleção e pré-processamento de dados, transformação, mineração de dados para descoberta de padrões, avaliação dos resultados e interpretação dos achados.
- O **objetivo** é **transformar dados brutos em informações significativas** e conhecimento acionável.
- O termo "mineração de dados" (Data Mining) refere-se ao estágio de descoberta do processo de KDD.
### Etapas do KDD
#### Seleção de dados
- Nesta etapa, os **dados relevantes** são **identificados** e **selecionados** para a análise.
- Isso envolve a **definição** de **critérios de inclusão** e **exclusão** e a obtenção dos conjuntos de dados adequados para o problema em questão.
#### Pré-processamento de dados
- Os **dados brutos** podem ser complexos, inconsistentes ou conter **ruído**.
- Nesta etapa, ocorre **a limpeza** e a **transformação dos dados**, incluindo a remoção de dados ausentes ou duplicados, normalização, discretização e outras técnicas de preparação dos dados para análise.
#### Transformação de dados
- A transformação de dados é feita para representar os dados em uma forma mais adequada para análise.
- Isso pode envolver a agregação de dados, a criação de novos atributos ou a redução da dimensionalidade por meio de técnicas como análise de componentes principais (PCA) ou seleção de recursos.
#### Mineração de dados
- A mineração de dados é a etapa central do processo de descoberta de conhecimento.
- Nessa etapa, são aplicadas técnicas e algoritmos de aprendizado de máquina, estatística e visualização de dados para identificar padrões, tendências, associações ou relações interessantes nos dados.
- Isso pode incluir técnicas como classificação, regressão, clusterização, regras de associação, redes neurais, entre outras.
#### Avaliação e interpretação dos resultados
- Após a aplicação das técnicas de mineração de dados, os resultados obtidos são avaliados e interpretados.
- Isso envolve a análise dos padrões descobertos, a validação dos modelos construídos e a interpretação dos insights obtidos em termos do problema ou domínio específico em questão.
#### Utilização e aplicação dos conhecimentos
- Os conhecimentos e insights descobertos durante o processo são utilizados para tomar decisões informadas, desenvolver estratégias, resolver problemas e promover melhorias nos negócios ou em outras áreas de aplicação.
## Exercício
![[Pasted image 20260922190917.png]]

> [!Success] Resposta B
> 

# Aula 2

> [!QUOTE] Nessa aula
> Neste vídeo, exploramos os principais conceitos e técnicas da **Ciência de Dados**, com foco no **modelo de processo CRISP-DM** e no **aprendizado de máquina**. Na introdução ao CRISP-DM, o modelo é apresentado como uma estrutura flexível para guiar a análise de dados, abrangendo etapas como **entendimento do negócio**, **preparação dos dados**, **modelagem**, **avaliação** e **implementação**. Ao abordar a extração de conhecimento a partir de dados, destacam-se técnicas como **mineração de dados**, **aprendizado de máquina**, **processamento de linguagem natural** e **visualização de dados**.

## CRISP-DM
- O CRISP-DM é, segundo Chapman (2000), um modelo de processo muito utilizado na área de mineração de dados para guiar projetos de análise de dados.
- Ele fornece uma estrutura flexível e abrangente para a condução de projetos de mineração de dados, permitindo que as equipes enfrentem desafios complexos e tomem decisões informadas ao longo de todo o ciclo de vida do projeto.
- O CRISP-DM é composto por seis etapas interconectadas.
![[Pasted image 20260922192118.png]]
### Etapas CRISP
#### Entendimento do Negócio
- Nesta fase inicial, a equipe trabalha para compreender os objetivos e requisitos do projeto, identificando como a mineração de dados pode contribuir para as metas de negócios.
#### Entendimento dos Dados
- Nesta fase, os dados disponíveis são explorados e analisados para identificar sua qualidade, relevância e potencial para atender aos objetivos do projeto. Isso envolve a realização de análises exploratórias e a compreensão das características dos dados.
#### Preparação dos Dados
- Aqui os dados são limpos, transformados e preparados para análise. Isso inclui lidar com valores ausentes, normalização, seleção de atributos relevantes e outras tarefas de preparação.
#### Modelagem
- Nesta fase, são desenvolvidos modelos de mineração de dados, como algoritmos de aprendizado de máquina, para explorar os padrões e relacionamentos nos dados. Diferentes abordagens são testadas e avaliadas para encontrar a mais adequada.
#### Avaliação
- Os modelos construídos na fase de modelagem são avaliados para garantir que eles atendam aos critérios de sucesso do projeto. Isso pode envolver testes de desempenho, validação cruzada e outros métodos para garantir que os modelos sejam robustos e generalizáveis.
#### Implantação
- Nesta última fase, os resultados da análise são apresentados aos stakeholders e são tomadas medidas para implementar os insights obtidos no ambiente de negócios. Isso pode envolver a criação de relatórios, integração com sistemas existentes ou outras formas de utilização prática.
## Extração de conhecimento
- A extração de conhecimento envolve a aplicação de algoritmos e técnicas de descoberta de padrões, associações e tendências nos dados para identificar informações valiosas e conhecimento útil.
- Destacam-se:
	- Mineração de Dados.
	- Aprendizado de Máquina.
	- Processamento de Linguagem Natural.
	- Visualização de Dados.
### Fontes de Dados
![[Pasted image 20260922192304.png]]
## Visão Geral sobre Aprendizado de Máquina
### Definição
- Aprendizado de Máquina, também conhecido como Machine Learning, é uma subárea da inteligência artificial que se concentra no desenvolvimento de algoritmos e modelos capazes de aprender e tomar decisões a partir dos dados, sem serem explicitamente programados.
- O objetivo principal do Aprendizado de Máquina é permitir que os sistemas "aprendam" automaticamente a partir dos dados e melhorem seu desempenho ao longo do tempo, sem a necessidade de regras ou instruções específicas.
### Categorias do Machine Learning
#### Aprendizado Supervisionado
- Os algoritmos são **treinados** utilizando um conjunto de dados de entrada pré-rotulados (dados de treinamento).
- O objetivo é aprender a **relação entre as entradas e as saídas** correspondentes, para que o modelo seja capaz de fazer **previsões** ou **tomar decisões** em novos dados não vistos anteriormente.
#### Aprendizado Não Supervisionado
- Os algoritmos são aplicados a conjuntos de dados **sem informações prévias sobre as saídas** desejadas.
- O objetivo é descobrir estruturas, padrões ou grupos intrínsecos nos dados, fornecendo uma visão mais profunda dos dados e insights sobre seu comportamento.
#### Aprendizado Semissupervisionado
- É usado para as mesmas finalidades que o aprendizado supervisionado, porém **envolve** tanto **dados com rótulos como sem rótulos** para treinamento.
- Ele pode ser aplicado a tarefas como classificação, regressão e previsão. É vantajoso quando rotular todos os dados é caro demais.
#### Aprendizado por Reforço
- Envolve o treinamento de algoritmos por meio de interações com um ambiente. O **agente de aprendizado** toma ações em um ambiente e recebe recompensas ou punições com base no desempenho de suas ações.
- O objetivo é **maximizar as recompensas ao longo do tempo**, aprendendo a melhor política de ação.
- Isso é frequentemente aplicado em jogos, robótica e otimização de processos.
![[Pasted image 20260922192559.png]]
## Overfiting e Underfitting
### Overfitting (sobreajuste)
- Ocorre quando o modelo **se torna excessivamente complexo** e se **ajusta** perfeitamente aos **dados de treinamento**, capturando até mesmo o ruído presente nesses dados.
- Como resultado, o **modelo memoriza** os exemplos de treinamento **em vez de aprender os padrões** subjacentes que permitem generalizar para novos dados.
- Isso pode levar a um desempenho pobre na etapa de teste, em que o modelo falha em fazer previsões precisas em dados não vistos.
### Sinais de Overfitting incluem:
- O desempenho do modelo é excelente nos dados de treinamento, mas ruim nos dados de teste.
- O modelo possui uma complexidade excessiva em relação ao tamanho dos dados disponíveis.
- O modelo captura o ruído presente nos dados de treinamento, resultando em uma precisão excessivamente alta nesses dados, mas não em novos dados.
## Underfitting (subajuste)
- Ocorre quando o modelo é muito simples ou não é capaz de capturar os padrões presentes nos dados de treinamento.
- Nesse caso, o modelo não consegue se ajustar adequadamente aos dados e acaba subestimando a complexidade do problema.
- O resultado é um desempenho insatisfatório tanto nos dados de treinamento quanto nos dados de teste.
### Sinais de Underfitting incluem:
- O desempenho do modelo é ruim tanto nos dados de treinamento quanto nos dados de teste.
- O modelo não consegue capturar os padrões e relações importantes presentes nos dados.
- O modelo é muito simples em relação à complexidade do problema.
![[Pasted image 20260922192724.png]]
## Espaço de Hipóteses
- O espaço de hipóteses se refere ao conjunto de todas as possíveis funções ou modelos que um algoritmo de aprendizado pode escolher como solução para um determinado problema.
- Essas hipóteses são expressas por meio de parâmetros, pesos ou estruturas específicas, dependendo do algoritmo e do tipo de aprendizado utilizado.
- O espaço de hipóteses define as restrições sobre o conjunto de soluções possíveis que o algoritmo de aprendizado pode explorar durante o processo de treinamento.
## Exercício:
![[Pasted image 20260922193017.png]]

> [!success] Resposta B
