---
Tarefa: TDP-1926
Desenvolvedor: Eu
Método de code review: 
Responsabilidade:
  - Desenvolver
Data de início: 
Data de fim:
---
# Quebra técnica (50h/60h)

- [x]  Adicionar tipo de funcionario de laboratorista (1h)
- [x]  Adicionar campo de carga horário diária (1h)
- [ ]  Alocação laboratorista (30h)
	- [x] Criar nova aba/aplicação (1h)
	- [x] Criar tabela de alocação (1h)
	- [x] Criar queries de alocação como a de veículo (2h)
	- [x] Criar tela de alocação como a de veículos (26h)
- [x] Adição do campo tempo de permanência em obra pro laboratorista (1h)
- [x] Adição do campo tempo de permanência em obra como parametro geral (1h)
- [x] Adicionar na inclusão de programação o local de ensaio (rompimento), podendo ser: Laboratório Interno ou Externo e no Gantt - Padrão interno (1h)
- [x] Adicionar na inclusão de programação o campo Necessidade de permanecer em obra que pode ser Sim ou Não e no Gantt - Padrão Não (1h)
- [x] Exibir na inclusão de programação uma mensagem de ajuda informando a quantidade recomendada e idade dos CPs baseado no parâmetro (2h)

- [ ] Tratar disponibilidade do laboratorista ao emitir programação (2h)
	- [ ] Verificar/criar API para os dados de corpo de prova (incluindo local do ensaio, quantidade de CPs e amostras, capacidade, vínculos e disponibilidade do técnico) (6h)
		- [ ] Validar se na API de Employee informa o tipo UNAVAILABLE e remover
- [ ] TENDO TEMPO - Agrupar os helpers
- [ ] TENDO TEMPO - Ajustar o clique fechando accordion da média de CPs e melhorar (4h)

# Dúvidas
- O técnico de laboratório deve ser um funcionário? Seria apenas um tipo de funcionário? Ou seria um vinculo do funcionário com dados do mesmo?
- Onde a aplicação de laboratório ficaria? Nova aplicação
- A alocação de laboratorista vai ser só dele ou de funcionarios como um todo?

# Casos de teste

- [ ] 
- [ ] 
- [ ] 



O sistema calcula a quantidade de moldagem que pode ser realizada por cada laboratorista alocado naquela central.
Para isso é considerado
	- O tempo disponivel em alocação para o dia
	- O tempo indisponível em alocação para o dia
	- A carga horária cadastrada
	- O tempo de permanência em obra cadastrada no laboratorista
	- O tempo de permanência em obra cadastrada no parâmetro geral
O sistema utiliza busca todos os funcionários alocados para aquela central no dia da programação, em seguida faz o somatório da carga horária do dia (descontando possíveis indisponibilidades) e limitando à carga horária cadastrada. Por exemplo:
	Matheus - Carga horária: 8h - Alocação cadastrada: Disponível 08:00 -> 18:00 e Indisponível de 10:00 as 12:00
		Portanto o


- [x] A moldagem é uma por progração
- [x] Tratar indisponibilidade sem fim e disponibilidade limitada
- [x] Caso exista uma indisponibilidade sem data de fim e uma disponbilidade com data de fim, a indisponibilidade deve ser desconsiderada
- [ ] 