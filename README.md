# Ciclo de vida de dados: Modelo de Estimativas dos Preços do Airbnb

O objetivo aqui é descobrir quais fatores são mais relevantes para se estimar os preços de locação dos imóveis oferecidos pelo sistema de serviço do airbnb. 
Como sabemos o airbnb é um aplicativo de serviço para localização e locação de imóveis em diversos lugares espalhados pelo mundo. O modelo de negócio do airbnb é 
classificado como um tipo de múltipla plataforma onde os clientes se conectam aos fornecedores do serviço. Em 2020 a empresa foi avalida em quase U$$ 100 bilhões.
O aplicativo acumula milhões de usuários atualmente. Naturalmente o aplicativo produz, devido a interação dos usuários, vastas quantidades 
de dados. Os hosts geralmente fornecem informações sobre suas locações, como número de quartos, banheiros, preços, localização etc. Enquanto que os hospedes 
geralmente produzem rewiers sobre os locais locados, como satisfação com a hospitalidade.

Montaremos um base de dados que possui todas as informações listas acima. Essa base de dados servirá para determinarmos quais dos fatores encontrados na base 
de dados serem mais correlacionados ao preço dos imóveis. Além disso iremos construir um modelo de previsão dos preços. Reuniremos algumas tabelas em formato .xlsx 
e tabelas em um banco de dados MySQL para montarmos nosso dataset usando data lake minio e airflow para extriar e manejar os dados. Podemos resumir todas as etapas 
que que seguiremos:
