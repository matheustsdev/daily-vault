---
Ticket: 489776
Status: Concluído
Data de início: 2025-03-21
Data de fim: 2025-03-24
---

# Análises
1. Foi gerado o adicional referente à taxa de permanência um mês após a emissão da nota. 
2. A remessa possui data de atualização no mesmo período
3. O cálculo de taxa de permanência só é feito na atualização da DeliveryTicketConcrete. Porém não faz sentido o cálculo já que 


# Soluções
1. Foi adicionado validação no DeliveryTicketConcreteHandler para que não seja atualizado/criado um adicional depois da remessa já faturada;
2. Foi adicionado no formulário de canhoto de nota as informações referentes à saída de obra (leaveConstruction) levando em consideração ela para calcular o tempo de permanência na obra.
3. Foi adicionado também log para a AdditionalFeeContract

