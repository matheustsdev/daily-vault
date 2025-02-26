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

# Soluções
1. Adição de parenteses em cada membro da divisão

