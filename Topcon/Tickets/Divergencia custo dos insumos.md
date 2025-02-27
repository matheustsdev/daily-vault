---
Ticket: 485867
Status: Em análise
Data de início: 2025-02-26
Data de fim:
---

# Análises
1. O custo unitário está sendo salvo errado na DeliveryTicketSupply. Com Geovane vimos que provavelmente é erro de lógica de cálculo
	![[Pasted image 20250226143657.png]]
	Dessa forma, como não o CostValue não era nulo, já usava ele direto.
2. Outros campos que usavam o CostValue no cálculo também estavam errados

# Soluções
1. Adição de parenteses em cada membro da divisão
2. Foi forçado a integração de todas as notas porém não foi corrigido já que o problema é no DISPATCH e não na integração
	Após testar em QA muitos problemas foram resolvidos e outros não. Depois de um bom tempo de análise Geovane e eu chegamos a conclusão que provavelmente é devido ao banco de QA do legado não estar sincronizado com o DISPATCH. 
3. Foi forçado integração em HML a fim de teste