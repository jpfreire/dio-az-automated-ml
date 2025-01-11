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

Para obter o arquivo swagger.json foi preciso utilizar no console do chrome um request 

```javascript
const primaryKey = "chave conforme Primary key na aba Consume";
const swaggerUrl = "https://wsai900-lwcwh.eastus.inference.ml.azure.com/swagger.json";


const requestHeaders = new Headers({
    "authorization":
"Bearer "+primaryKey,
    "accept": "*/*",
    "accept-language": "en-US,en;q=0.9,pt-BR;q=0.8,pt;q=0.7",
     "azureml-model-deployment": "eagerspinachrp02-1",
    "cache-control": "no-cache",
    "pragma": "no-cache",
     "priority": "u=1, i",
    "sec-ch-ua": "\"Chromium\";v=\"130\", \"Opera\";v=\"115\", \"Not?A_Brand\";v=\"99\"",
    "sec-ch-ua-mobile": "?0",
    "sec-ch-ua-platform": "\"Windows\"",
    "sec-fetch-dest": "empty",
    "sec-fetch-mode": "cors",
    "sec-fetch-site": "same-site",
    "x-azure-debuginfo": "1",
    "x-ms-client-request-id": crypto.randomUUID(),
    "x-ms-client-session-id": "9DXQ6",
    "x-ms-client-user-type": "Workspace 2.0",
    "x-ms-page-name": "OnlineEndpointDetailRoute",
     "x-ms-pageview-id": crypto.randomUUID(),
    "x-ms-user-agent": "AzureMachineLearningWorkspacePortal/2.0",
    "x-ms-useragent": "AzureMachineLearningWorkspacePortal/2.0"
});



fetch(swaggerUrl, {
    method: "GET",
    headers: requestHeaders,
     mode: "cors" //credentials removido
})
.then(response => {
    if (!response.ok) {
         console.debug("Headers da resposta:", response.headers);
        return response.text().then(text => {
            throw new Error(`Falha na requisição com status ${response.status}: ${text}`);
         });
    }
    return response.json();
})
.then(data => {
    console.log("Swagger JSON recebido:", data);
    downloadJson(data, "swagger.json");
})
.catch(error => {
    console.error("Erro ao buscar swagger.json:", error);
});

function downloadJson(data, filename) {
  const json = JSON.stringify(data, null, 2);
  const blob = new Blob([json], { type: 'application/json' });
  const url = URL.createObjectURL(blob);

  const a = document.createElement('a');
  a.href = url;
  a.download = filename;
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
}

```
