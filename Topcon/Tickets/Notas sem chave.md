---
Ticket: 489811
Status: Em análise
Data de início: 2025-03-20
Data de fim:
---

# Análises
1. Muitas notas da Maxmohr não estavam integrando. No fim descobrimos que o appsettings da Supremo estava com dados da Maxmohr, lendo a mesma fila, resultando em mensagens não sendo lidas pelo cliente certo. 
   Xela assumiu esse erro pois tinha copiado dados da Maxmohr e jogado na Supremo sem verificar


# Soluções
1. Foi corrigido os dados do appsettings
2. Passos para resolver no banco
	Maxmohr
	-> Forçar integração do legado > Dispatch (menos contrato)  
	-> Forçar integração do Dispatch > Legado  
	  
	OBS: Alinhar com cliente para poder forçar a integração e não ter problema com a data de update.  
	  
	Supremo  
	-> Apagar os dados que vieram do Dispatch para o legado (con_nf, etc).  
	-> Forçar integração do Legado > Dispatch  
	-> Forçar integração do Dispatch > Legado

