---
Ticket: 537858
Status: Em análise
Data de início: 2025-11-25
Data de fim:
---
# Problemas
## Problema 1
Não integrando na topnfe
### Causa
Uma aspa simples ao tentar inserir no sql na mensagem de DeliveryTicket.
### Solução 1
Adicionar tratativa para limpar a aspa

## Problema 1
Não integrando na topnfe
### Causa
Fluxo de integração força integração da remessa em caso de ordem invertida mas perde a mensagem de autorização
### Solução 1
Corrigir fluxo para o novo.

#### Fluxo atual

```mermaid
graph TD
    subgraph "Sistema de Origem"
        A["Remessa Criada"] --> B("Gera DeliveryTicket");
        A --> C("Gera DeliveryTaxDocumentAuthorization");
    end

    subgraph "Mensageria"
        B --> D{"Azure Service Bus"};
        C --> D;
    end

    subgraph "Processamento"
        D --> E["FUNCTION le a fila e encaminha"];
        E --> F["INTEGRATOR recebe as mensagens"];
        F --> G{"Qual mensagem e processada primeiro?"};
    end

    subgraph "Fluxo de Erro: Ordem Incorreta"
        G -- "DeliveryTaxDocumentAuthorization" --> H["Erro: Ordem Incorreta"];
        H --> I["Nao altera o banco do DESKTOP"];
        H --> J["Gera mensagem para forcar integracao da Remessa"];
        J --> K["Remessa DeliveryTicket e integrada com sucesso"];
        K --> L("FIM: Falha na integracao do DeliveryTaxDocumentAuthorization");
    end

    subgraph "Fluxo Ideal: Ordem Correta"
        G -- "DeliveryTicket" --> M["Sucesso: Processa DeliveryTicket"];
        M --> N["Atualiza banco do DESKTOP"];
        N --> O["Sucesso: Processa DeliveryTaxDocumentAuthorization"];
        O --> P["Atualiza banco do DESKTOP"];
        P --> Q("FIM: Sucesso");
    end

```


#### Fluxo novo
```mermaid
graph TD

subgraph "INTEGRADOR"
A["INTEGRATOR recebe mensagens"] --> B{"Qual mensagem e processada primeiro?"};
B -- "DeliveryTaxDocumentAuthorization" --> C["Gera erro de ordem incorreta"];
C --> D["Adiciona dados na tabela topnfe no DESKTOP"];
D --> E["Envia mensagem para forcar integracao da Remessa"];
B -- "DeliveryTicket" --> F["Inicia fluxo de integracao da remessa"];
E --> F;
end

subgraph "Fluxo de Integracao da Remessa"
F --> G{"Dados da remessa ja existem na topnfe?"};
G -- "Sim" --> H["Utiliza dados existentes da topnfe para preencher a fis_nf"];
G -- "Nao" --> I["Cria fis_nf com campos padroes (fluxo atual)"];
H --> J["Remessa integrada com sucesso"];
I --> J;
end
```


# Casos de teste

## ✅❌ Caso 1: Fluxo correto

- **Pré-condições:**
    - Configurar central para emitir nota
    - Entrega disponível para emissão de remessa
    - 
- **Passos do Teste:**
    1. Emitir remessa/nota no DISPATCH
    2. No DESKTOP, remover todos os registros integrados (fis_nf, con_nf, topnfe.nfe e seus respectivos items)
    3. Forçar integração do DeliveryTicket
    4. Verificar se a fis_nf foi inserida como padrão
    5. Forçar integração DeliveryTaxDocumentAuthorization
- **Resultado Esperado:** É esperado que inicialmente a fis_nf esteja inserida com valores padrões e em seguida, que os dados estejam todos preenchidos corretamente no DESKTOP
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** 

## ✅❌ Caso 1: Fluxo invertido

- **Pré-condições:**
    - Configurar central para emitir nota
    - Entrega disponível para emissão de remessa
    - 
- **Passos do Teste:**
    1. Emitir remessa/nota no DISPATCH
    2. No DESKTOP, remover todos os registros integrados (fis_nf, con_nf, topnfe.nfe e seus respectivos items)
    3. Forçar integração apenas do DeliveryTaxDocumentAuthorization
- **Resultado Esperado:** É esperado que no elastic seja exibido uma mensagem de DeliveryTaxDocumentAuthorization, em seguida uma de ForceDeliveryTicket, uma de DeliveryTicket e que os dados estejam todos preenchidos corretamente no DESKTOP
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** 

