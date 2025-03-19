---
Tarefa: TDP-1688
Desenvolvedor: Jonathas
Método de code review:
  - Commit
Responsabilidade:
  - Code review
Data de início: 2025-03-14
Data de fim:
---

# Code review 1 (14/03)
**Problemas de funcionalidade:**
- Ao selecionar a frente de obra e selecionar outro contrato, a frente de obra é salva mesmo em outro contrato que não possui vínculo com essa frente de obra.
- Ao selecionar a frente de obra o título do campo está ficando sobreposto pelo valor do campo. Talvez seja melhor utilizar o componente molecules/TextFieldController e criar um formulário para conseguir tratar melhor o preenchimento dos dados. Isso é apenas uma ideia de correção, já que outros formulários desse tipo não possuem esse erro.

# Code review 2 (18/03)
**Melhorias**:
- No arquivo molecules/AutocompleteVehicle é utilizado o a propriedade defaultValues porém o useEffect em seguida faz o mesmo processo. Dessa forma a utilização do defaultValues não é necessária.

**Problemas de funcionalidade:**
- No arquivo molecules/AutocompleteVehicle, o useEffect é utilizado para setar os valores dos campos do formulário, porém ao remover a seleção ele não é limpo, exibindo campos preenchidos mesmo que sem nenhuma frente de obra.
- O erro de gerar uma programação com frente de obra de outro contrato se mantém. Para reproduzir o problema fiz:
	1. Selecionei a frente de obra do contrato A
	2. Selecionei o contrato B para prosseguir com a programação
	3. Inclui a programação com o contrato B
	4. No Gantt, clicando no ícone na barra lateral, é exibido os dados da programação e é possível ver que a frente de obra foi inserida mesmo não possuindo vínculo com a obra do contrato B