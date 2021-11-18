# Ciclo de vida de dados: Modelo de Estimativas dos Preços do Airbnb

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

Os dados que queremos passar pelo processo de ETL estão armazenados no zona de landing no data lake do minio.
![hallo](https://github.com/RondinellyMorais/Ciclo-de-vida-de-dados-Modelo-de-estimativas-de-pre-os-do-Airbnb/blob/master/imagens/minio01.png)

Em seguida rodamos as DEGs do airflow. Escrevi 4 DEGs, duas para extrair as tabelas xlsx da landing do minio e salvar os dados em tabelas .parquet na zona de 
processing. Uma DEG para extrair uma tabela de uma base de dados MySQL, transformar os dados e carregar na zona de processing. Criei uma última DEG para unir todos 
os arquivos .parquert e armazenar na zona de processing.

Uma vez que tenhamos nossa base de dados construída podemos carrega-la em qualquer notebook através da conecção ao data lake usando a função de client.fget_object().
Ao fazer uma análise exploratória de dados preliminar, preenchemos as linhas identificadas com dados ausentes e removemos features desnecessárias. Ao testarmos as 
correlações das features numéricas, encontrei que as maiores correlações entre o preço e as features ocorre para os valores de longitude e latitude dos imóveis. 
Em outras palavras, a localização dos imóveis oferecidos pelo airbnb é fator mais importante para determinar seu preço de locação. No final da análise de dados criei 
um novo dataset .csv e salvei no data lake na zona de cureted
