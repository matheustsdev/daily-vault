---
Tarefa: TDP-1675
Desenvolvedor: Eu
Método de code review:
  - Misto
Responsabilidade:
  - Desenvolver
Data de início: 2025-04-02
Data de fim: 2025-05-02
---
# Quebra técnica (24h/37h)

- [x] Criar parâmetro de "origem do interveniente" e seu log (8h)
- [x] Adicionar campo de "ClientId" na remessa e nota (4h)
- [x] Criar script para atualizar todos os dados anteriores das remessas e notas com o "ClientId" (4h)
- [x] Implementar integração e envio de nota baseado nos novos modelos (10h)

## Criar parâmetro de "origem do interveniente" e seu log (8h)

- [x] Adicionar o campo IntervenerSource na entidade DeliveryTaxDocumentParams
- [x] Adicionar log na alteração da entidade DeliveryTaxDocumentParams
- [x] Adicionar campo para alterar o dado no Frontend

# Adicionar campo de "ClientId" na remessa e nota (4h)

Sem muitos problemas

# Criar script para atualizar todos os dados anteriores das remessas e notas com o "ClientId" (4h)

```sql
UPDATE reg_delivery_tickets rdt
SET id_client_fk = rc.id_client_fk
FROM reg_schedules rs
INNER JOIN reg_contracts rc ON rs.id_contract_fk = rc.id_contract
WHERE rdt.id_scheduling_fk = rs.id_schedule;

UPDATE reg_delivery_tax_documents rdtd
SET id_client_fk = rsfd.id_client_fk
FROM reg_subsidiaries_fiscal_data rsfd 
WHERE rsfd.id_subsidiary_fiscal_data = rdtd.id_subsidiary_fiscal_data_fk;
```

# Implementar integração e envio de nota baseado nos novos modelos (10h)

Devido a opção de utilizar o remetente com endereço da obra foi decidido criar uma nova tabela que irá armazenar os dados de endereço da cada nota, dessa forma a validação do parâmetro só precisa ser feita no DISPATCH e refatorado na FUNCTION para que utilize os dados dessa tabela para gerar a nota.

Foi criado também um script para preencher essa tabela com os dados das notas já emitidas.

```sql
INSERT INTO reg_delivery_tax_document_address 
(
    id_tenant,
    id_delivery_tax_document_fk,
    zip_code,
    address,
    "number",
    complement,
    district,
    id_municipality_fk,
    phone,
    created_at
)
SELECT 
    rdtd.id_tenant ,
    rdtd.id_delivery_tax_document,
    rsfd.zip_code,
    rsfd.address, 
    rsfd.number,
    rsfd.complement,
    rsfd.district,
    rsfd.id_municipality_fk,
    CONCAT(rsfd.ddd, rsfd.phone_number),
    rdtd.created_at
FROM 
    reg_delivery_tax_documents rdtd
INNER JOIN reg_subsidiaries_fiscal_data rsfd ON rdtd.id_subsidiary_fiscal_data_fk = rsfd.id_subsidiary_fiscal_data;
```

No caso do contato:
	- Caso o parâmetro seja "DESTINATÁRIO" deve ser usado o contato registrado no cliente, utilizando o primeiro contato do tipo "TELEFONE". Caso não exista nenhum deve ser gerado um erro erro tratado que exibirá o motivo: "Erro ao emitir nota fiscal. Telefone do cliente não encontrado"
	- Caso o parâmetro seja diferente de "DESTINATÁRIO" deve ser utilizado o contato da subsidiaria (filial)

```ad-failure
Tive problema ao adicionar a entidade endereço devido à entidade de município dela possuir filhos trackeados. Ajustei removendo esse filho manualmente
```

---
# Documentação

## Resumo

Com essa implementação é possível selecionar o cliente que será utilizado para emissão de nota, adicionando nas configurações da subsidiária se será utilizado dados do remetente, destinatário ou o remetente com endereço da obra.

## Funcionamento

Para acessar a funcionalidade basta, na aplicação de "Programação",  na tela de listagem de "Centrais cadastradas", clicar no ícone de configurações (engrenagem). 

![[Pasted image 20250502101225.png]]

Por fim selecionar a aba de "Parâmetros de documento fiscal".

![[Pasted image 20250502102538.png]]

Nessa aba foi adicionado 2 campos: "Origem do interveniente" e "Cliente da filial". 

![[Pasted image 20250502103225.png]]

O primeiro possui as opções: "Remetente", "Destinatário" e "Remete com endereço da obra". Alterando esse campo, o envio de nota fiscal será condicionado à ele.

![[Pasted image 20250502104040.png]]

O segundo possibilita a seleção de qual cliente é vinculado à subsidiária, ou seja, qual cliente será utilizado para preencher os dados necessários quando a "Origem do interveniente" for "Remetente" ou "Remetente com endereço da obra".

## Documentação técnica

A propriedade que define a origem do interveniente foi adicionada na entidade DeliveryTaxDocumentParams e adicionado essa entidade para registrar suas alterações (log)

Foi criado uma propriedade nas entidades de DeliveryTicket e DeliveryTaxDocument de ClientId, para garantir o histórico e rastreabilidade do cliente utilizado para emissão das notas. Para preencher os dados já salvo em banco das notas, foi criado os scripts abaixo para atualizar todos os dados:

```sql
UPDATE reg_delivery_tickets rdt
SET id_client_fk = rc.id_client_fk
FROM reg_schedules rs
INNER JOIN reg_contracts rc ON rs.id_contract_fk = rc.id_contract
WHERE rdt.id_scheduling_fk = rs.id_schedule;

UPDATE reg_delivery_tax_documents rdtd
SET id_client_fk = rsfd.id_client_fk
FROM reg_subsidiaries_fiscal_data rsfd 
WHERE rsfd.id_subsidiary_fiscal_data = rdtd.id_subsidiary_fiscal_data_fk;
```


Além disso com a possibilidade de utilizar parte dos dados de um cliente e o endereço de outro, optamos por desacoplar os endereços para emissão de nota dos clientes criando a entidade DeliveryTaxDocumentAddress e refatorando a forma como a FUNCTION do Dispatch recebe e utiliza esses dados. Para criar os dados referentes às notas já salvas em banco, foi criado os scripts abaixo:

```sql
INSERT INTO reg_delivery_tax_document_address 
(
    id_tenant,
    id_delivery_tax_document_fk,
    zip_code,
    address,
    "number",
    complement,
    district,
    id_municipality_fk,
    phone,
    created_at
)
SELECT 
    rdtd.id_tenant ,
    rdtd.id_delivery_tax_document,
    rsfd.zip_code,
    rsfd.address, 
    rsfd.number,
    rsfd.complement,
    rsfd.district,
    rsfd.id_municipality_fk,
    CONCAT(rsfd.ddd, rsfd.phone_number),
    rdtd.created_at
FROM 
    reg_delivery_tax_documents rdtd
INNER JOIN reg_subsidiaries_fiscal_data rsfd ON rdtd.id_subsidiary_fiscal_data_fk = rsfd.id_subsidiary_fiscal_data;
```


Por fim, com a emissão de notas não ser sempre pro mesmo cliente, surge a possibilidade  de emissão de notas para estados diferentes do emissor, por isso foi adicionado tratativa na FUNCTION para lidar com o CFOP:

```cs
var cfop = deliveryTaxDocumentItem.ShipmentFiscalOperation?.FiscalOperationCode?.Code ?? 0;  
  
if (deliveryTaxDocumentAddress?.Municipality?.State != subsidiaryFiscalData?.Municipality?.State  
    && cfop > 5000 && cfop < 6000)  
{  
    cfop += 1000;  
}
```
