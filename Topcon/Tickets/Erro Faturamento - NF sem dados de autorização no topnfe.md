---
Ticket: 484050
Status: Concluído
Data de início: 2025-02-21
Data de fim: 2025-02-25
---
# Análises
1. Ordem parece estar incoerente
	![[Pasted image 20250224140537.png]]
	Na imagem dá pra perceber que teve um update só de um campo em seguida atualização com dados.
	Geovane sugeriu que fizesse só a correção tratando a não inserção caso já exista dados

# Soluções
1. Validação da mensagem. Caso seja vazia e já exista dados inseridos, a mensagem será ignorada e logada


# Processo de teste