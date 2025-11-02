---
Tarefa: TDP-2273
Desenvolvedor: Eu
Método de code review:
Responsabilidade:
  - Desenvolver
Data de início: 2025-09-17
Data de fim:
---
# Quebra técnica (19h/30h)

- [x] Adicionar propriedade SupplyProvidedByTheClient no CostSupply (1h)
- [x] Garantir que a integração do TECH vai retornar esse dado (2h)
- [x] Adicionar propriedade de CFOP para insumo fornecido pelo cliente (4h)
- [x] Adicionar propriedade de movimentação de estoque para insumo fornecido pelo cliente (2h)
- [x] Adicionar propriedade no LoadTicket de insumo fornecido pelo cliente (4h)
- [x] Adicionar validação que, caso exista pelo menos um insumo fornecido pelo cliente e não exista CFOP parametrizado, retornar um erro (2h)
- [ ] Adicionar tratativa na integração quando sinalizado que não haverá movimentação de estoque para insumos fornecidos pelo cliente (2h)
- [ ] Ajustar o fluxo de integração da remessa que está diferente do force pro normal

------------
O problema da integração da familia associado a forma como estava executando na minha tarefa se dá por que ao adicionar o corpo do CostSupply em todos os payloads da integração, faz com que sejam enviados CostSupply em CREATEs de versão mesmo não sendo um novo custo. Dessa forma ao tentar adicionar a familia, tenta adicionar também o custo e gera um erro de duplicata. 

Porém está funcionando em produção por que não consegui até o momento identificar uma maneira de alterar o insumo para fornecido pelo cliente se não via tela de insumos, que gera uma mensagem de familia já existente, entrando no manipulate do custo e não gerando duplicamento.

Ou seja, caso essa seja realmente a única maneira de adicionar a informação de fornecimento por cliente, então está 100% funcional. Porém isso torna a integração FORTEMENTE acoplada à regra de negocio do TECH, o que me da a ideia de criar um fluxo para integração individual do CostSupply agora ou em outro momento

# Casos de teste

- [ ] 
- [ ] 
- [ ] 



