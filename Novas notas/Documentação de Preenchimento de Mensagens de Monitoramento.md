Esta documentação descreve semanticamente como os campos da mensagem de retorno (`LoadTicketReturn`) são preenchidos por cada processador de monitoramento. O objetivo é esclarecer o significado de cada dado extraído das fontes externas (arquivos de texto, XML ou API) e transformado para o padrão da automação.

---

## 1. Command Alkon (`CommandAlkonMonitoringProcessor`)
**Fonte de Dados:** Arquivos XML (`*.xml`).

### Cabeçalho da Carga (`LoadTicketReturn`)
| Campo                       | Significado Semântico                                                                                                                                            | Obrigatório?                  |
| :-------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------- |
| **IdAutomationIntegration** | Identificador único da transação gerado pelo sistema externo (Command Alkon).                                                                                    | **Sim**                       |
| **LoadTicketCode**          | Código do ticket ou número do pedido de produção.                                                                                                                | **Sim**                       |
| **LoadingPointCode**        | Código do ponto de carregamento onde a operação ocorreu.                                                                                                         | **Sim**                       |
| **WeighingStartDatetime**   | Momento exato em que a pesagem/dosagem começou. <br>*Nota:* Se ausente no XML, assume a data/hora atual.                                                         | **Sim** (Valor Padrão: Agora) |
| **WeighingFinishDatetime**  | Momento exato em que a pesagem/dosagem terminou. <br>*Nota:* Se ausente no XML, assume a data/hora atual.                                                        | **Sim** (Valor Padrão: Agora) |
| **StatusProductionOrder**   | Estado final da carga. Determinado pelo nome do arquivo (Completed, Aborted) e validado internamente (se houve razão de cancelamento no XML, torna-se CANCELED). | **Sim**                       |
| **WaterInConcreteMixer**    | Volume de água presente no caminhão betoneira antes do carregamento.                                                                                             | Não (Padrão: 0)               |
| **RealWaterCut**            | Quantidade de água retirada/compensada da receita (corte de água).                                                                                               | Não (Padrão: 0)               |
| **ReturnType**              | Define se o processo foi automático ou manual (baseado no nome do arquivo, ex: "NotUsedInLoad" gera um ticket manual).                                           | **Sim**                       |
| **Observation**             | Campo de observações gerais. (Fixo como vazio nesta integração).                                                                                                 | Não                           |

### Itens da Carga (`LoadTicketReturnItem`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **SupplyCodeinAutomation** | Código identificador do insumo/material no sistema de automação. | **Sim** |
| **DosedQuantity** | Quantidade efetiva que foi dosada para este material. | **Sim** |
| **RealHumidityPercentage** | Percentual de umidade medido para o material durante a dosagem. | Não (Padrão: 0) |

---

## 2. SHM Text File (`ShmTextFileMonitoringProcessor`)
**Fonte de Dados:** Arquivos de Texto Posicional/Delimitado (`*.txt`).
**Modos:** Automático (Padrão) ou Manual (Identificado por "Manual" no nome do arquivo).

### Cabeçalho da Carga (`LoadTicketReturn`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **LoadTicketCode** | **Automático:** Extraído do cabeçalho do arquivo.<br>**Manual:** Não preenchido/Vazio. | **Sim** (Apenas no Automático) |
| **LoadingPointCode** | Código do ponto de carregamento configurado. | **Sim** |
| **WeighingStartDatetime** | **Automático:** Data extraída do cabeçalho do arquivo.<br>**Manual:** Data da primeira pesagem de insumo encontrada no arquivo. | **Sim** |
| **WeighingFinishDatetime** | **Automático:** Data extraída do cabeçalho do arquivo.<br>**Manual:** Data da última pesagem de insumo encontrada no arquivo. | **Sim** |
| **StatusProductionOrder** | **Automático:** Status lido do arquivo.<br>**Manual:** Fixo como `COMPLETED`. | **Sim** |
| **Observation** | Observações presentes no cabeçalho do arquivo (apenas modo Automático). | Não |
| **ReturnType** | Classificação do arquivo (AUTOMATIC ou MANUAL). | **Sim** |

### Itens da Carga (`LoadTicketReturnItem`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **LocalCodeinAutomation** | Código do silo/caixa ou local físico de onde saiu o material. | Não |
| **SupplyCodeinAutomation** | Código do insumo no sistema de automação. | **Sim** |
| **DosedQuantity** | Quantidade dosada do material. | **Sim** |

---

## 3. Topcon Standard (`TopconStandardMonitoringProcessor`)
**Fonte de Dados:** Arquivos de Texto (`*.txt`) com separador `;`.
**Modos:** Retorno/Self (Automático) ou Manual.

### Cabeçalho da Carga (`LoadTicketReturn`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **IdAutomationIntegration** | Identificador da integração. Em modo manual é preenchido com o texto fixo "MANUAL". | **Sim** |
| **LoadTicketCode** | Número do ticket. Em modo manual ou "Self", este campo pode ficar vazio. | Depende do Modo |
| **LoadingPointCode** | Código do ponto de carregamento. | **Sim** |
| **WeighingStartDatetime** | Data de início lida do cabeçalho. Se modo manual, usa data atual. | **Sim** |
| **WeighingFinishDatetime** | Data de fim lida do cabeçalho. Se modo manual, usa data atual. | **Sim** |
| **StatusProductionOrder** | Status da carga. No modo manual é fixo como `COMPLETED`. | **Sim** |
| **WaterInConcreteMixer** | Água na betoneira (Apenas modo Automático). | Não (Padrão: 0) |
| **RealWaterCut** | Corte de água (Apenas modo Automático). | Não (Padrão: 0) |

### Itens da Carga (`LoadTicketReturnItem`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **SupplyCode** | Código do material no padrão Topcon (Apenas modo Automático). | Não |
| **LocalCodeinAutomation** | Código do local de armazenagem/silo. | Não |
| **DosedQuantity** | Quantidade dosada. | **Sim** |

---

## 4. Kartrak (`KartrakMonitoringProcessor`)
**Fonte de Dados:** API REST (Consulta periódica/Polling).
**Modos:** Cargas em Progresso (Loop) e Cargas Manuais.

### Cabeçalho da Carga (`LoadTicketReturn`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **IdAutomationIntegration** | Código da resposta vinda da API (igual ao LoadTicketCode). | **Sim** |
| **LoadTicketCode** | Código do ticket retornado pela API. | **Sim** |
| **LoadingPointCode** | Código do ponto de carregamento. | **Sim** |
| **WeighingStartDatetime** | Horário de início registrado na API. Se nulo, usa data atual. | **Sim** |
| **WeighingFinishDatetime** | Horário de fim registrado na API. Se nulo, usa data atual. | **Sim** |
| **StatusProductionOrder** | Mapeado do status da API (`Finished` vira `COMPLETED`, outros viram `CANCELED`). | **Sim** |
| **ReturnType** | Fixo como `AUTOMATIC`. | **Sim** |

### Itens da Carga (`LoadTicketReturnItem`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **SupplyCodeinAutomation** | Código do material retornado pela API (`ShipmentMaterials`). | **Sim** |
| **DosedQuantity** | Quantidade efetiva (`EffectiveQuantity`) retornada pela API. | **Sim** |

---

## 5. Install (`InstallMonitoringProcessor`)
**Fonte de Dados:** Arquivos de Texto (`*.txt`).
**Modos:** Automático (Posicional fixo) vs Manual (CSV com separador `;`).

### Cabeçalho da Carga (`LoadTicketReturn`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **LoadTicketCode** | **Automático:** Extraído de posição fixa no cabeçalho.<br>**Manual:** Vazio. | Depende do Modo |
| **LoadingPointCode** | Código do ponto de carregamento. | **Sim** |
| **WeighingStartDatetime** | **Automático:** Posição fixa no cabeçalho (formato `yyyyMMddHH:mm:ss`).<br>**Manual:** Extraído da primeira linha de item (formato `dd/MM/yyyy HH:mm:ss`). | **Sim** |
| **WeighingFinishDatetime** | **Automático:** Posição fixa no cabeçalho.<br>**Manual:** Extraído da última linha de item. | **Sim** |
| **StatusProductionOrder** | **Automático:** Fixo como "C".<br>**Manual:** Fixo como `COMPLETED`. | **Sim** |
| **ReturnType** | AUTOMATIC ou MANUAL dependendo do formato do arquivo. | **Sim** |

### Itens da Carga (`LoadTicketReturnItem`)
| Campo | Significado Semântico | Obrigatório? |
| :--- | :--- | :--- |
| **SupplyCodeinAutomation** | Código do insumo. (Apenas modo Automático, extraído por posição fixa). | Depende do Modo |
| **LocalCodeinAutomation** | Código do local/silo. (Apenas modo Manual, extraído por coluna CSV). | Depende do Modo |
| **DosedQuantity** | Quantidade dosada. <br>*Nota Importante:* No modo Automático, o sistema insere um ponto decimal antes dos últimos 3 dígitos (ex: `12345` vira `12.345`). | **Sim** |