# dio-az-automated-ml
Módulo: Trabalhando com Machine Learning na Prática no Azure ML

# Previsão de Aluguel de Bicicletas com Azure AutoML

Este projeto utiliza o Azure Automated Machine Learning (AutoML) para prever o número de aluguéis de bicicletas com base em dados históricos. O modelo foi treinado e implantado no Azure ML sem a necessidade de código customizado.

## Tecnologias e Ferramentas

*   **Azure Machine Learning**
*   **Azure Automated Machine Learning (AutoML)**
*   **Azure Blob Storage**

## Passo a Passo

1.  **Workspace:** Criado um workspace no Azure ML na região `East US`.
2.  **Dataset:** Dataset `bike-rentals` criado a partir de arquivos CSV fornecidos (https://aka.ms/bike-rentals).
3.  **AutoML Job:** Criado um job de AutoML conforme tutorial.
4.  **Melhor Modelo:** O melhor modelo foi VotingEnsemble, por seu menor erro médio quadrático normalizado de 0,08898 em comparação com o valor de 0,09358 do "MaxAbsScaler, LightGBM", mas ainda acima do configurado no tutorial (0,085)
5.  **Implantação:** O modelo foi implantado como um endpoint em tempo real em https://wsai900-lwcwh.eastus.inference.ml.azure.com/swagger.json
.
6.  **Teste:** O endpoint foi testado com um JSON de exemplo e retornou uma previsão.

## Resultados

O modelo retorna dados interessantes com mudanças, por exemplo no windspeed de 0,3 para 0,4 a queda em aluguéis é de 351 para 287.
## Arquivos

*   O arquivo `.json` do endpoint está incluído neste repositório.
*   Os arquivos de dados CSV podem ser baixados em (https://aka.ms/bike-rentals).
