---
Ticket: 472914
Status: Concluído
Data de início: 2025-01-03
Data de fim: 2025-01-06
---
- No dia 03/01 forcei a integração das notas
	- 376
	- 2371
	- 2393
	- 647 (Duas notas)
	- 849
	- 403
	- 2345
	- 2351
	- 2358
	- 2388
	- 2254

```sql
select rdtd.issue_date, rdtda.cancellation_protocol, rdtda.receipt_date , rsfd.code, rsfd."name" , rdtd.concrete_shipment_document_series, rdtd.document_number, rdtda.document_key, rc2.code, rc2."name" from reg_delivery_tax_document_authorizations rdtda
inner join reg_delivery_tax_documents rdtd on rdtd.id_delivery_tax_document = rdtda.id_delivery_tax_document_fk
inner join reg_subsidiaries_fiscal_data rsfd on rsfd.id_subsidiary_fiscal_data = rdtd.id_subsidiary_fiscal_data_fk
inner join reg_clients rc2 on rc2.id_client = rsfd.id_client_fk
inner join reg_schedules rs on rs.id_schedule = rdtd.id_scheduling_fk
inner join reg_contracts rc on rc.id_contract = rs.id_contract_fk
where rdtd.id_subsidiary_fiscal_data_fk = '2b4e144e-ca62-4da8-ac1c-7a9f16c3e702'
and rdtd.document_number <= 410
and rdtd.concrete_shipment_document_series = '4';
```

```sql
UPDATE reg_delivery_tax_documents rdtd SET id_subsidiary_fiscal_data_fk = '2b4e144e-ca62-4da8-ac1c-7a9f16c3e702'
WHERE rdtd.id_subsidiary_fiscal_data_fk = '185e54a7-9815-4953-9158-010b36ba6579'
AND rdtd.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52'
AND rdtd.document_number <= 410
AND rdtd.concrete_shipment_document_series = '4';
```
