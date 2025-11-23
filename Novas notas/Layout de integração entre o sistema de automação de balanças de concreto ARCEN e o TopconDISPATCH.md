# Introdução
Este documento estabelece o padrão de integração via WebHook para envio de dados de LoadTicket (Ticket de pesagem) e o padrão de integração para retorno dos dados de pesagem após o processamento desse ticket.
Neste documento constam informações necessárias para o devido recebimento e utilização dos dados do ticket de pesagem e o posterior retorno para o DISPATCH com os dados obtidos pela automação da ARCEN, destacando os campos necessários para tal.

# Fluxo Completo de Integração

1. **Envio para automação na expedição do DISPATCH:** Na expedição de uma entrega, na etapa de "Produção", após preencher os campos obrigatório e clicar no botão "Enviar para automação" é feito o envio do evento para o webhook com o id de "LOAD_TICKET_SEND_TO_AUTOMATION"
2. **Recebimento do Webhook:** Sistema de automação recebe ordem de produção via webhook
3. **Processamento:** Operador inicia pesagem baseado nos dados recebidos
4. **Dosagem:** Sistema de automação realiza a dosagem dos materiais
5. **Coleta de Dados:** Sistema registra quantidades dosadas, umidades e tempos
6. **Montagem do Payload:** Sistema monta o JSON de resposta conforme especificação
7. **Envio da Resposta:** POST para /loadTicketResponse com os dados da pesagem
8. **Confirmação:** Sistema aguarda resposta HTTP 200 OK do servidor

# Envio dos dados do ticket de pesagem
## Estrutura do Payload
### CABEÇALHO/TICKET DE PESAGEM (LoadTicketAutomationDto)

| CAMPO                        | TIPO     | TAMANHO MAX. | DESCRIÇÃO                                                                                 |
| ---------------------------- | -------- | ------------ | ----------------------------------------------------------------------------------------- |
| id                           | GUID     | -            | Identificador único do LoadTicket                                                         |
| code                         | INTEGER  | -            | Número de controle Topcon (Loadticket)                                                    |
| createdAt                    | DATETIME | -            | Data/hora de criação do LoadTicket. Formato ISO 8601: "yyyy-MM-ddTHH:mm:ss"               |
| sentToAutomationAt           | DATETIME | -            | Data/hora de envio para automação. Formato ISO 8601: "yyyy-MM-ddTHH:mm:ss". Pode ser nulo |
| volume                       | DECIMAL  | -            | Volume em metros cúbicos (máximo duas casas decimais). Pode ser nulo                      |
| reusedVolume                 | DECIMAL  | -            | Volume reaproveitado em metros cúbicos (máximo duas casas decimais). Pode ser nulo        |
| loadLabel                    | STRING   | -            | Número do lacre da betoneira                                                              |
| releaseObservation           | STRING   | 255          | Observações gerais da liberação. Pode ser nulo                                            |
| waterCut                     | INTEGER  | 4            | Quantidade de água a ser cortada na pesagem. Pode ser nulo                                |
| operatorName                 | STRING   | 50           | Nome do operador responsável                                                              |
| concreteBatchingPlant        | OBJETO   | -            | Informações da central de concreto                                                        |
| loadingPoint                 | OBJETO   | -            | Informações do ponto de carregamento                                                      |
| vehicle                      | OBJETO   | -            | Informações do veículo                                                                    |
| concreteRecipe               | OBJETO   | -            | Informações do traço de concreto                                                          |
| concreteRecipeFamilyVersion  | OBJETO   | -            | Informações da versão da família do traço                                                 |
| employee                     | OBJETO   | -            | Informações do motorista/funcionário                                                      |
| loadTicketDeliveryRelational | ARRAY    | -            | Lista de entregas relacionadas ao LoadTicket                                              |
| loadTicketItems              | ARRAY    | -            | Lista de itens (insumos) a serem dosados                                                  |

### CENTRAL (ConcreteBatchingPlant)

| CAMPO        | TIPO   | TAMANHO MAX. | DESCRIÇÃO                                      |
| ------------ | ------ | ------------ | ---------------------------------------------- |
| id           | GUID   | -            | Identificador único da central                 |
| abbreviation | STRING | 20           | Abreviação da central                          |
| name         | STRING | 100          | Nome completo da central                       |
| code         | STRING | 20           | Código identificador da central. Pode ser nulo |

### PONTO DE CARGA (LoadingPoint)

| CAMPO | TIPO    | TAMANHO MAX. | DESCRIÇÃO                                     |
| ----- | ------- | ------------ | --------------------------------------------- |
| name  | STRING  | 100          | Nome do ponto de carregamento                 |
| code  | INTEGER | -            | Código identificador do ponto de carregamento |

### VEÍCULO (Vehicle)

| CAMPO             | TIPO   | TAMANHO MAX. | DESCRIÇÃO                                      |
| ----------------- | ------ | ------------ | ---------------------------------------------- |
| code              | STRING | 20           | Código identificador do veículo                |
| licensePlate      | STRING | 20           | Placa do veículo                               |
| licensePlateState | STRING | 2            | Estado (UF) da placa do veículo. Pode ser nulo |

### TRAÇO (ConcreteRecipe)

| CAMPO                             | TIPO    | TAMANHO MAX. | DESCRIÇÃO                                                           |
| --------------------------------- | ------- | ------------ | ------------------------------------------------------------------- |
| numeration                        | INTEGER | -            | Numeração do traço                                                  |
| description                       | STRING  | 200          | Descrição completa do traço                                         |
| externalId                        | STRING  | 50           | Código identificador externo do produto. Pode ser nulo              |
| specification                     | STRING  | 200          | Especificação técnica do traço. Pode ser nulo                       |
| concreteCharacteristicsType       | OBJETO  | -            | Tipo de característica do concreto (ex: resistência). Pode ser nulo |
| concreteRecipeCharacteristicValue | DECIMAL | -            | Valor da característica (ex: valor MPa). Máximo duas casas decimais |
| productGroup                      | OBJETO  | -            | Grupo do produto. Pode ser nulo                                     |
| gravel                            | OBJETO  | -            | Informações sobre a brita. Pode ser nulo                            |
| slump                             | OBJETO  | -            | Slump final do concreto. Pode ser nulo                              |
| slumpInitial                      | OBJETO  | -            | Slump inicial do concreto. Pode ser nulo                            |

### TIPO DE CARACTERÍSTICA DO CONCRETO (ConcreteCharacteristicsType)

| CAMPO        | TIPO   | TAMANHO MAX. | DESCRIÇÃO                                               |
| ------------ | ------ | ------------ | ------------------------------------------------------- |
| abbreviation | STRING | 20           | Abreviação do tipo de característica (ex: "MPa", "PSI") |

### GRUPO DE PRODUTO (ProductGroup)

| CAMPO | TIPO   | TAMANHO MAX. | DESCRIÇÃO                |
| ----- | ------ | ------------ | ------------------------ |
| name  | STRING | 100          | Nome do grupo de produto |

### BRITA (Gravel)

| CAMPO | TIPO   | TAMANHO MAX. | DESCRIÇÃO               |
| ----- | ------ | ------------ | ----------------------- |
| name  | STRING | 100          | Nome/descrição da brita |

### SLUMP (Slump)

| CAMPO         | TIPO    | TAMANHO MAX. | DESCRIÇÃO                      |
| ------------- | ------- | ------------ | ------------------------------ |
| value         | INTEGER | -            | Valor do slump                 |
| amplitude     | INTEGER | -            | Amplitude de variação do slump |
| slumpUnitType | OBJETO  | -            | Unidade de medida do slump     |

### TIPO DE UNIDADE DO SLUMP (SlumpUnitType)

| CAMPO        | TIPO   | TAMANHO MAX. | DESCRIÇÃO                              |
| ------------ | ------ | ------------ | -------------------------------------- |
| abbreviation | STRING | 10           | Abreviação da unidade (ex: "cm", "mm") |

### VERSÃO DA FAMÍLIA DO TRAÇO (ConcreteRecipeFamilyVersion)

| CAMPO                | TIPO    | TAMANHO MAX. | DESCRIÇÃO                       |
| -------------------- | ------- | ------------ | ------------------------------- |
| version              | INTEGER | -            | Número da versão do traço       |
| concreteRecipeFamily | OBJETO  | -            | Informações da família do traço |

### FAMÍLIA DO TRAÇO (ConcreteRecipeFamily)

| CAMPO      | TIPO    | TAMANHO MAX. | DESCRIÇÃO                     |
| ---------- | ------- | ------------ | ----------------------------- |
| numeration | INTEGER | -            | Numeração da família do traço |

### MOTORISTA/FUNCIONÁRIO (Employee)

| CAMPO                  | TIPO   | TAMANHO MAX. | DESCRIÇÃO                                                  |
| ---------------------- | ------ | ------------ | ---------------------------------------------------------- |
| name                   | STRING | 100          | Nome completo do motorista                                 |
| registrationCode       | STRING | 20           | Código de registro/matrícula do funcionário. Pode ser nulo |
| identificationDocument | STRING | 20           | Documento de identificação (CPF/RG)                        |

### ENTREGAS RELACIONADAS (LoadTicketDeliveryRelational)

| CAMPO    | TIPO   | TAMANHO MAX. | DESCRIÇÃO                          |
| -------- | ------ | ------------ | ---------------------------------- |
| delivery | OBJETO | -            | Informações da entrega/agendamento |

### ENTREGA (Delivery)

| CAMPO      | TIPO   | TAMANHO MAX. | DESCRIÇÃO                  |
| ---------- | ------ | ------------ | -------------------------- |
| scheduling | OBJETO | -            | Informações do agendamento |

### PROGRAMAÇÃO (Scheduling)

| CAMPO             | TIPO     | TAMANHO MAX. | DESCRIÇÃO                                                                |
| ----------------- | -------- | ------------ | ------------------------------------------------------------------------ |
| scheduledDate     | DATETIME | -            | Data/hora agendada para entrega. Formato ISO 8601: "yyyy-MM-ddTHH:mm:ss" |
| constructionFront | OBJETO   | -            | Informações da frente de obra. Pode ser nulo                             |
| contract          | OBJETO   | -            | Informações do contrato                                                  |
| code              | INTEGER  | -            | Código do agendamento                                                    |

### FRENTE DE OBRA (ConstructionFront)

| CAMPO        | TIPO   | TAMANHO MAX. | DESCRIÇÃO                                 |
| ------------ | ------ | ------------ | ----------------------------------------- |
| name         | STRING | 120          | Nome da frente de obra. Pode ser nulo     |
| address      | STRING | 200          | Endereço da frente de obra. Pode ser nulo |
| number       | STRING | 20           | Número do endereço. Pode ser nulo         |
| district     | STRING | 100          | Bairro. Pode ser nulo                     |
| municipality | OBJETO | -            | Informações do município. Pode ser nulo   |

### CONTRATO (Contract)

| CAMPO          | TIPO   | TAMANHO MAX. | DESCRIÇÃO                     |
| -------------- | ------ | ------------ | ----------------------------- |
| contractNumber | STRING | 50           | Número do contrato            |
| seller         | OBJETO | -            | Informações do vendedor       |
| construction   | OBJETO | -            | Informações da obra principal |
| client         | OBJETO | -            | Informações do cliente        |

### VENDEDOR (Seller)

| CAMPO    | TIPO    | TAMANHO MAX. | DESCRIÇÃO                                |
| -------- | ------- | ------------ | ---------------------------------------- |
| name     | STRING  | 100          | Nome do vendedor. Pode ser nulo          |
| fullName | STRING  | 200          | Nome completo do vendedor. Pode ser nulo |
| code     | INTEGER | -            | Código do vendedor. Pode ser nulo        |

### OBRA (Construction)

| CAMPO        | TIPO   | TAMANHO MAX. | DESCRIÇÃO                               |
| ------------ | ------ | ------------ | --------------------------------------- |
| name         | STRING | 120          | Nome da obra                            |
| address      | STRING | 200          | Endereço da obra. Pode ser nulo         |
| number       | STRING | 20           | Número do endereço. Pode ser nulo       |
| district     | STRING | 100          | Bairro. Pode ser nulo                   |
| municipality | OBJETO | -            | Informações do município. Pode ser nulo |

### CLIENTE (Client)

| CAMPO                  | TIPO    | TAMANHO MAX. | DESCRIÇÃO                               |
| ---------------------- | ------- | ------------ | --------------------------------------- |
| code                   | INTEGER | -            | Código do cliente. Pode ser nulo        |
| name                   | STRING  | 200          | Nome do cliente. Pode ser nulo          |
| address                | STRING  | 200          | Endereço do cliente. Pode ser nulo      |
| number                 | STRING  | 20           | Número do endereço. Pode ser nulo       |
| complement             | STRING  | 100          | Complemento do endereço. Pode ser nulo  |
| district               | STRING  | 100          | Bairro. Pode ser nulo                   |
| municipality           | OBJETO  | -            | Informações do município. Pode ser nulo |
| identificationDocument | STRING  | 20           | CNPJ/CPF do cliente. Pode ser nulo      |

### MUNICÍPIO (Municipality)

| CAMPO                    | TIPO    | TAMANHO MAX. | DESCRIÇÃO                                  |
| ------------------------ | ------- | ------------ | ------------------------------------------ |
| code                     | INTEGER | -            | Código interno do município. Pode ser nulo |
| name                     | STRING  | 100          | Nome do município                          |
| state                    | STRING  | 2            | Sigla do estado (UF)                       |
| countryCode              | INTEGER | -            | Código do país. Pode ser nulo              |
| officialMunicipalityCode | INTEGER | -            | Código IBGE do município. Pode ser nulo    |
| zipCode                  | STRING  | 10           | CEP do município. Pode ser nulo            |

### ITENS DO TICKET DE PESAGEM (LoadTicketItem)

| CAMPO                         | TIPO    | TAMANHO MAX. | DESCRIÇÃO                                                                                  |
| ----------------------------- | ------- | ------------ | ------------------------------------------------------------------------------------------ |
| id                            | GUID    | -            | Identificador único do item                                                                |
| loadTicketId                  | GUID    | -            | Identificador do LoadTicket pai. Pode ser nulo                                             |
| supplyId                      | GUID    | -            | Identificador do insumo. Pode ser nulo                                                     |
| quantityTheoreticalDryPerM3   | DECIMAL | -            | Quantidade teórica seca por metro cúbico em KG (máximo duas casas decimais). Pode ser nulo |
| theoreticalHumidityPercentage | DECIMAL | -            | Percentual de umidade teórica (máximo duas casas decimais). Pode ser nulo                  |
| contractedHumidityPercentage  | DECIMAL | -            | Percentual de umidade contratada (máximo duas casas decimais). Pode ser nulo               |
| quantityTheoreticalDryTotal   | DECIMAL | -            | Quantidade teórica seca total em KG (máximo duas casas decimais). Pode ser nulo            |
| supplyCodeinAutomation        | STRING  | 20           | Código identificador do material no sistema de automação. Pode ser nulo                    |
| localCodeinAutomation         | STRING  | 32           | Número identificador do local de insumo na automação. Pode ser nulo                        |
| supply                        | OBJETO  | -            | Informações do insumo. Pode ser nulo                                                       |
| supplier                      | OBJETO  | -            | Informações do fornecedor. Pode ser nulo                                                   |
| dosageType                    | ENUM    | -            | Tipo de dosagem (enum EDosageType). Pode ser nulo                                          |

### INSUMO (Supply)

| CAMPO                      | TIPO    | TAMANHO MAX. | DESCRIÇÃO                                             |
| -------------------------- | ------- | ------------ | ----------------------------------------------------- |
| code                       | INTEGER | -            | Código do insumo. Pode ser nulo                       |
| name                       | STRING  | 100          | Nome completo do insumo                               |
| abbreviation               | STRING  | 20           | Abreviação do insumo                                  |
| active                     | BOOLEAN | -            | Indica se o insumo está ativo                         |
| group                      | ENUM    | -            | Grupo do insumo (enum ESupplyGroup)                   |
| composeMortar              | BOOLEAN | -            | Indica se compõe argamassa                            |
| externalId                 | STRING  | 50           | Identificador externo. Pode ser nulo                  |
| classificationSupplyTypeId | GUID    | -            | Identificador do tipo de classificação. Pode ser nulo |

### FORNECEDOR (Supplier)

| CAMPO | TIPO   | TAMANHO MAX. | DESCRIÇÃO          |
| ----- | ------ | ------------ | ------------------ |
| name  | STRING | 200          | Nome do fornecedor |

## Observações Importantes

* **Formato de Data/Hora:** Todas as datas seguem o padrão ISO 8601: "yyyy-MM-ddTHH:mm:ss"
* **Separador Decimal:** Valores decimais utilizam ponto (.) como separador. Exemplo: 7.5
* **Encoding:** UTF-8
* **Content-Type:** application/json
* **Valores Nulos:** Campos que podem ser nulos serão omitidos ou enviados como `null` no JSON
* **Arrays:** As listas (loadTicketDeliveryRelational e loadTicketItems) podem conter zero ou múltiplos elementos
* **GUIDs:** Identificadores únicos no formato padrão: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

## Exemplo de Payload Simplificado

```json
{
  "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "code": 12345,
  "createdAt": "2024-01-15T08:30:00",
  "sentToAutomationAt": "2024-01-15T08:35:00",
  "volume": 8.5,
  "reusedVolume": 0.5,
  "loadLabel": "LAC123456",
  "releaseObservation": "Entrega urgente",
  "waterCut": 15,
  "operatorName": "João da Silva",
  "concreteBatchingPlant": {
    "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
    "abbreviation": "CP01",
    "name": "Central Principal",
    "code": "001"
  },
  "vehicle": {
    "code": "BET001",
    "licensePlate": "ABC1234",
    "licensePlateState": "SP"
  },
  "employee": {
    "name": "José Motorista",
    "registrationCode": "M001",
    "identificationDocument": "12345678900"
  },
  "loadTicketItems": [
    {
      "id": "4fa85f64-5717-4562-b3fc-2c963f66afa6",
      "quantityTheoreticalDryPerM3": 350.00,
      "supply": {
        "code": 101,
        "name": "Cimento CP II",
        "abbreviation": "CIM",
        "active": true,
        "group": 1
      }
    }
  ]
}
```

# Montagem e envio da resposta da pesagem
## Estrutura do Payload de Resposta

### CABEÇALHO DA CARGA (LoadTicketReturn)

| CAMPO                   | TIPO     | TAMANHO MAX. | DESCRIÇÃO                                                                                                                                              |
| ----------------------- | -------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| idAutomationIntegration | STRING   | 50           | Identificador único da transação gerado pelo sistema externo. Em retornos manuais, preencher com "MANUAL" ou "NotUsedInLoad"                           |
| loadTicketCode          | STRING   | 20           | Código do ticket ou número do pedido de produção recebido no webhook. Em modo manual pode ficar vazio                                                  |
| loadingPointCode        | STRING   | 20           | Código do ponto de carregamento onde a operação ocorreu                                                                                                |
| weighingStartDatetime   | DATETIME | -            | Momento exato em que a pesagem/dosagem começou. Formato ISO 8601: "yyyy-MM-ddTHH:mm:ss"                                                                |
| weighingFinishDatetime  | DATETIME | -            | Momento exato em que a pesagem/dosagem terminou. Formato ISO 8601: "yyyy-MM-ddTHH:mm:ss"                                                               |
| statusProductionOrder   | STRING   | 20           | Estado final da carga. Valores aceitos: "COMPLETED" (concluída com sucesso), "CANCELED" (cancelada), "ERROR" (erro/interrompida), "ABORTED" (abortada) |
| waterInConcreteMixer    | DECIMAL  | -            | Volume de água presente no caminhão betoneira antes do carregamento (lastro). Máximo duas casas decimais. Valor padrão: 0                              |
| realWaterCut            | DECIMAL  | -            | Quantidade de água retirada/compensada da receita (corte de água real executado). Máximo duas casas decimais. Valor padrão: 0                          |
| volume                  | DECIMAL  | -            | Volume real produzido em metros cúbicos. Máximo uma casa decimal. Campo opcional                                                                       |
| observation             | STRING   | 255          | Campo de observações gerais sobre a pesagem. Campo opcional                                                                                            |
| returnType              | STRING   | 20           | Classificação do retorno. Valores aceitos: "AUTOMATIC" (pesagem automática baseada em ordem recebida), "MANUAL" (pesagem manual sem ordem prévia)      |
| loadTicketReturnItems   | ARRAY    | -            | Lista de itens (insumos) efetivamente dosados na pesagem                                                                                               |

### ITENS DOSADOS (LoadTicketReturnItem)

| CAMPO                  | TIPO    | TAMANHO MAX. | DESCRIÇÃO                                                                                                   |
| ---------------------- | ------- | ------------ | ----------------------------------------------------------------------------------------------------------- |
| supplyCodeinAutomation | STRING  | 20           | Código identificador do insumo/material no sistema de automação                                             |
| localCodeinAutomation  | STRING  | 32           | Código do silo/caixa ou local físico de onde saiu o material. Campo opcional                                |
| dosedQuantity          | DECIMAL | -            | Quantidade efetiva que foi dosada para este material em KG (quilos). Máximo duas casas decimais             |
| realHumidityPercentage | DECIMAL | -            | Percentual de umidade medido para o material durante a dosagem. Máximo duas casas decimais. Valor padrão: 0 |
| supplyCode             | STRING  | 20           | Código do material no padrão Topcon (usado apenas em algumas integrações específicas). Campo opcional       |

## Tipos de Retorno

### Retorno Automático (AUTOMATIC)

Utilizado quando a pesagem foi realizada baseada em uma ordem de produção recebida via webhook.

**Características:**
* Campo `loadTicketCode` deve conter o código recebido no webhook original
* Campo `idAutomationIntegration` deve conter um identificador único gerado pelo sistema de automação
* Campo `returnType` deve ser "AUTOMATIC"
* Todos os itens da composição enviada na ordem devem ter um item correspondente no retorno

### Retorno Manual (MANUAL)

Utilizado quando a pesagem foi executada manualmente no sistema de automação, sem uma ordem prévia do Topcon.

**Características:**
* Campo `loadTicketCode` pode ficar vazio ou conter código interno do sistema de automação
* Campo `idAutomationIntegration` deve ser "MANUAL" ou "NotUsedInLoad"
* Campo `returnType` deve ser "MANUAL"
* Campo `statusProductionOrder` geralmente é "COMPLETED"
* Datas de início e fim podem ser a mesma (momento da pesagem)

## Regras de Validação

1. **Obrigatoriedade de Campos:**
   * Campos obrigatórios: `loadingPointCode`, `weighingStartDatetime`, `weighingFinishDatetime`, `statusProductionOrder`, `returnType`
   * Para retorno automático: `loadTicketCode` e `idAutomationIntegration` são obrigatórios
   * Para cada item: `supplyCodeinAutomation` e `dosedQuantity` são obrigatórios

2. **Formato de Dados:**
   * Datas devem seguir ISO 8601: "yyyy-MM-ddTHH:mm:ss"
   * Valores decimais utilizam ponto (.) como separador
   * Quantidades de material sempre em KG (quilogramas)
   * Volume sempre em m³ (metros cúbicos)

3. **Status da Ordem:**
   * "COMPLETED": Pesagem concluída com sucesso
   * "CANCELED": Ordem cancelada antes ou durante a pesagem
   * "ERROR": Erro durante o processo de pesagem
   * "ABORTED": Pesagem abortada por intervenção do operador

4. **Correlação de Itens:**
   * Em retornos automáticos, cada material enviado na ordem de produção deve ter um item correspondente no retorno
   * A quantidade dosada pode diferir da quantidade teórica solicitada
   * Materiais não dosados devem ser retornados com quantidade zero

## Observações Importantes

* **Encoding:** UTF-8
* **Content-Type:** application/json
* **Método HTTP:** POST
* **Endpoint:** /loadTicketResponse
* **Timeout:** Máximo 30 segundos para processamento
* **Retry:** Em caso de falha, recomenda-se retry exponencial com até 3 tentativas
* **Idempotência:** O sistema deve ser capaz de processar o mesmo retorno múltiplas vezes sem duplicação

## Exemplos de Payload

### Exemplo 1: Retorno Automático Completo

```json
{
  "idAutomationIntegration": "AUTO-2024-001234",
  "loadTicketCode": "12345",
  "loadingPointCode": "001",
  "weighingStartDatetime": "2024-01-15T08:35:00",
  "weighingFinishDatetime": "2024-01-15T08:42:00",
  "statusProductionOrder": "COMPLETED",
  "waterInConcreteMixer": 50.00,
  "realWaterCut": 15.00,
  "volume": 8.5,
  "observation": "Pesagem realizada com sucesso",
  "returnType": "AUTOMATIC",
  "loadTicketReturnItems": [
    {
      "supplyCodeinAutomation": "CIM001",
      "localCodeinAutomation": "SILO-01",
      "dosedQuantity": 2975.50,
      "realHumidityPercentage": 0.00
    },
    {
      "supplyCodeinAutomation": "AREIA001",
      "localCodeinAutomation": "SILO-03",
      "dosedQuantity": 5950.00,
      "realHumidityPercentage": 4.50
    },
    {
      "supplyCodeinAutomation": "BRITA001",
      "localCodeinAutomation": "SILO-05",
      "dosedQuantity": 8415.75,
      "realHumidityPercentage": 1.20
    },
    {
      "supplyCodeinAutomation": "AGUA001",
      "localCodeinAutomation": "TANQUE-01",
      "dosedQuantity": 1487.25,
      "realHumidityPercentage": 0.00
    },
    {
      "supplyCodeinAutomation": "ADIT001",
      "localCodeinAutomation": "TANQUE-ADT",
      "dosedQuantity": 42.50,
      "realHumidityPercentage": 0.00
    }
  ]
}
```

### Exemplo 2: Retorno Manual

```json

{
  "idAutomationIntegration": "MANUAL",
  "loadTicketCode": "",
  "loadingPointCode": "001",
  "weighingStartDatetime": "2024-01-15T10:15:00",
  "weighingFinishDatetime": "2024-01-15T10:22:00",
  "statusProductionOrder": "COMPLETED",
  "waterInConcreteMixer": 0.00,
  "realWaterCut": 0.00,
  "volume": 6.0,
  "observation": "Carga manual - Cliente balcão",
  "returnType": "MANUAL",
  "loadTicketReturnItems": [
    {
      "supplyCodeinAutomation": "CIM001",
      "localCodeinAutomation": "SILO-01",
      "dosedQuantity": 2100.00,
      "realHumidityPercentage": 0.00
    },
    {
      "supplyCodeinAutomation": "AREIA001",
      "localCodeinAutomation": "SILO-03",
      "dosedQuantity": 4200.00,
      "realHumidityPercentage": 5.00
    },
    {
      "supplyCodeinAutomation": "BRITA001",
      "localCodeinAutomation": "SILO-05",
      "dosedQuantity": 5940.00,
      "realHumidityPercentage": 0.80
    },
    {
      "supplyCodeinAutomation": "AGUA001",
      "localCodeinAutomation": "TANQUE-01",
      "dosedQuantity": 1050.00,
      "realHumidityPercentage": 0.00
    }
  ]
}
```

### Exemplo 3: Retorno com erro/cancelamento

```json
{
  "idAutomationIntegration": "AUTO-2024-001235",
  "loadTicketCode": "12346",
  "loadingPointCode": "001",
  "weighingStartDatetime": "2024-01-15T11:00:00",
  "weighingFinishDatetime": "2024-01-15T11:03:00",
  "statusProductionOrder": "CANCELED",
  "waterInConcreteMixer": 0.00,
  "realWaterCut": 0.00,
  "volume": 0.0,
  "observation": "Pesagem cancelada - Falta de material no silo 03",
  "returnType": "AUTOMATIC",
  "loadTicketReturnItems": [
    {
      "supplyCodeinAutomation": "CIM001",
      "localCodeinAutomation": "SILO-01",
      "dosedQuantity": 2975.50,
      "realHumidityPercentage": 0.00
    },
    {
      "supplyCodeinAutomation": "AREIA001",
      "localCodeinAutomation": "SILO-03",
      "dosedQuantity": 0.00,
      "realHumidityPercentage": 0.00
    }
  ]
}
```

## Considerações de Segurança

- Utilizar HTTPS para todas as comunicações
- Validar origem das requisições
- Implementar rate limiting para prevenir abuso
- Registrar logs de todas as transações para auditoria