# Ciclo de Vida de Dados: Modelo de Estimativas dos Preços do Airbnb

![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)

Temos dois objetivos principais para demostrar nesse trabalho: O primeiro é descobrir quais fatores são mais relevantes para se estimar os preços de locação dos imóveis oferecidos pelo sistema de serviço do airbnb. O segundo objetivo é mostrar como podemos encontar a solução do priveiro objetivo passando por todas as etapas do ciclo de vida de dados.
Para realizar esse objetivo, vamos usar poderosas ferramentas de ciência de dados, como docker, airflow, banco de dados MySQL e data lake minio. 

Como sabemos o airbnb é um aplicativo de serviço para localização e locação de imóveis em diversos lugares espalhados pelo mundo. O modelo de negócio do airbnb é 
classificado como um tipo de múltipla plataforma onde os clientes se conectam aos fornecedores do serviço. Em 2020 a empresa foi avalida em quase U$$ 100 bilhões.
O aplicativo acumula milhões de usuários atualmente. Naturalmente o aplicativo produz, devido a interação dos usuários, vastas quantidades 
de dados. Os hosts geralmente fornecem informações sobre suas locações, como número de quartos, banheiros, preços, localização etc. Enquanto que os hospedes 
geralmente produzem rewiers sobre os locais locados, como satisfação com a hospitalidade.

Montaremos uma base de dados que possui todas as informações listas acima. Essa base de dados servirá para determinarmos quais dos fatores são mais correlacionados ao preço dos imóveis. Além disso, iremos construir um modelo de previsão dos preços com machine learning. Reuniremos algumas tabelas em formato .xlsx 
e tabelas em um banco de dados MySQL para montarmos nosso dataset usando data lake minio e airflow para extriar e manejar os dados. Podemos resumir todas as etapas 
que que seguiremos:

* Usando as DAGs do airflow para extrair os dados do data lake, transformar e carregar os dados de volta no data lake.

* Importar dataset do data lake.

* Fazer análise exploratória de dados e carregar resultados obtidos para o data lake.

* Criar modelo de predição usando a biblioteca de auto machine learning com o pycaret.

O primeiro passo é rodar os containers do data lake, airflow e do MySQL no docker.

![oi](https://github.com/RondinellyMorais/Ciclo-de-vida-de-dados-Modelo-de-estimativas-de-pre-os-do-Airbnb/blob/master/imagens/docker.png)

Os dados que queremos passar pelo processo de ETL estão armazenados no zona de landing no data lake do minio e no banco de dados MySQL

![hallo](https://github.com/RondinellyMorais/Ciclo-de-vida-de-dados-Modelo-de-estimativas-de-pre-os-do-Airbnb/blob/master/imagens/minio01.png)

![my](https://github.com/RondinellyMorais/Ciclo-de-vida-de-dados-Modelo-de-estimativas-de-pre-os-do-Airbnb/blob/master/imagens/my.png)

Em seguida rodamos as DEGs do airflow. Escrevi 4 DEGs, duas para extrair as tabelas xlsx da landing do minio e salvar os dados em tabelas .parquet na zona de 
processing. Uma DEG para extrair uma tabela de uma base de dados MySQL, transformar os dados e carregar na zona de processing. Criei uma última DEG para unir todos 
os arquivos .parquert e armazenar na zona de processing.

![hello](https://github.com/RondinellyMorais/Ciclo-de-vida-de-dados-Modelo-de-estimativas-de-pre-os-do-Airbnb/blob/master/imagens/airflow.png)

![kunichua](https://github.com/RondinellyMorais/Ciclo-de-vida-de-dados-Modelo-de-estimativas-de-pre-os-do-Airbnb/blob/master/imagens/minio02.png)

Uma vez que tenhamos nossa base de dados construída podemos carrega-la em qualquer notebook através da conecção ao data lake usando a função de client.fget_object().
Ao fazer uma análise exploratória de dados preliminar, preenchemos as linhas identificadas com dados ausentes e removemos features desnecessárias. Ao testarmos as 
correlações das features numéricas, encontrei que as maiores correlações entre o preço e as features ocorre para os valores de longitude e latitude dos imóveis. 
Em outras palavras, a localização dos imóveis oferecidos pelo airbnb é fator mais importante para determinar seu preço de locação. O notebook desta análise exploratória de dados pode ser encontrado no seguinte link: [airbnb_analysis](https://github.com/RondinellyMorais/-Data-Lifecycle-and-Estimate-Pricing-of-Airbnb/blob/master/notebooks/airbnb_analysis.ipynb). No final da análise de dados criei um novo dataset .csv e salvei no data lake na zona de cureted

![kunichua](https://github.com/RondinellyMorais/Ciclo-de-vida-de-dados-Modelo-de-estimativas-de-pre-os-do-Airbnb/blob/master/imagens/minio03.png)

Na etapa final carreguei o dataset da zona de cureted do data lake para um notebook a fim de criar um modelo preditivo. Fiz mais uma tratamento das features 
categóricas e carreguei a biblioteca do pycaret.regression. Como queremos prever os preços dos imóveis devemos construir um modelo de regressão. O pycaret 
vai criar e comparar as métricas de validação em diversos modelos de regressão automaticamente. O melhor modelo obtido foi o Light Gradient Boosting Machine. Contudo 
quando comparamos os valore previstos com dados ainda não vistos pelo modelo, observei consideráveis discrepâncias para preços maiores do que 500. Como mostrado pela 
métrica de avaliação R<sup>2</sup> e pelo gráfico dos resíduos. 

![graf](https://github.com/RondinellyMorais/-Data-Lifecycle-and-Estimate-Pricing-of-Airbnb/blob/master/imagens/graf.png)

O notebook do modelo de previsão pode ser encontradono no seguinte link: [airbnb_ml](https://github.com/RondinellyMorais/-Data-Lifecycle-and-Estimate-Pricing-of-Airbnb/blob/master/notebooks/airbnb_ml.ipynb). A justificativa para essas discrepâncias deve estar na baixa correlação das features com os valores preços. 
Sendo a localização, representada pelas latitude e longitude, bem mais relevantes para se estimar o preço dos imóveis. A primeira abordagem que podemos buscar para construir um modelo de previsão de preços melhor e testar com modelos de machine learining não cobertos pelo pycaret. Possivelmente uma rede neural robusta utilizando uma outra base de dados com correlações mais fortes com preço possa performar bem melhor a previsão dos preços. No mais, podemos agrupar os preços em níveis e plotar em um mapa sua localização. Desse modo teremos um modelo visual dos preços de locação dos imóveis do airbnb.
