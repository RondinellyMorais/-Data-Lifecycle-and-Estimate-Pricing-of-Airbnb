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
