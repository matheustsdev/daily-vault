- [ ] Ticket [[Exibir na barra da entrega na programação as cores e o tempo em cada etapa da entrega]]
- [ ] Ticket [[Expedição de Argamassa gerou duas remessas de cada cliente]]
- [x] Code review tarefa do Jonatha (TDP - 1478)
	- No arquivo organisms/MainAndSupportConcreteBatchingPlantPicker o regionalId é sempre enviado, então no caso de "Exibir todas as centrais" ele sempre filtra por regional
	- No arquivo organisms/MainAndSupportConcreteBatchingPlantPicker, na linha 264, isDisableAutoComplete está escrito errado
	- No arquivo molecules/FormGeneralParameters ainda é feito a importação do EScheduleConcreteBatchingPlantMethod do arquivo errado, resultando em erro total ao abrir a página
	- Ao executar yarn lint-check ainda são exibidos 8 erros que podem ser resolvidos com yarn lint-fix
