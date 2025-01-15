---
Ticket: 476781
Status: Em análise
Data de início: 2025-01-14
Data de fim:
---
Deletar veículo de código 375M. 
Muitos vínculos com o veículo, quando removi quebrou o gantt por que tinha alocação.

CONTRATO - CLIENTE - DATA (PROG e NOTA)

--- 


Boa tarde.  
   
Após deletar o veículo e abrir a aplicação para verificar a solução foi observado uma quebra da aplicação devido outros vínculos já existentes desse veículo no DISPATCH (**O veículo foi restaurado para evitar a quebra da aplicação**). Segue os vínculos encontrados:  
   
**Query:**  

```
SELECT rs.code AS Scheduling_Code, rs.volume, rs.scheduled_date, rv.code AS Vehicle_Code, rv.license_plate FROM reg_vehicles rvINNER JOIN reg_schedules rs ON rs.id_pump_fk = rv.id_vehicle WHERE rv.code = '375M';
```

   
**Resultado:**  
![](https://telluria.zendesk.com/attachments/token/wb3VRag84eugAee0B6NrETkHr/?name=image.png)  

---

**Query:**  

```
SELECT rdt.code AS Delivery_Ticket_Code, rdtp.volume_pumped, rv.code AS Vehicle_Code, rv.license_plate FROM reg_vehicles rvINNER JOIN reg_delivery_tickets_pumpings rdtp ON rdtp.id_vehicle_fk = rv.id_vehicleINNER JOIN reg_delivery_tickets rdt on rdt.id_delivery_ticket = rdtp.id_delivery_ticket_fkWHERE rv.code = '375M';
```

   
**Resultado:**  
![](https://telluria.zendesk.com/attachments/token/ey5fjTRYIOAQScL1DD1iQczMP/?name=image.png)  

---

   
Vejo, no momento, duas abordagens: remover esses dados vinculados juntamente do veículo ou adicionar o veículo no legado para que os bancos fiquem sincronizados quanto à isso. Porém, devido a necessidade de definição de como proceder, colocarei esse ticket como PENDENTE.  
   
Atenciosamente