---
Ticket: 500156
Status: Concluído
Data de início: 2025-05-06
Data de fim: 2025-05-08
---
# Problemas
## ✅Problema 1: alteração de status ao redosar

Status fica como enviado pra automação, se emitir a redosagem após a emissão da nota (não necessariamente tá errado, tem que confirmar se vai ser assim) 

### Solução 1



### Solução 2


## ✅ Problema 2: validação da sequência de expedição

A validação que não permiti expedição fora da sequencia, está de forma que pode gerar erro, acredito que deve pensar uma forma nova de validar. 

### Solução 1
Foi ajustado a validação do valor da sequência da próxima entrega que deveria ser expedida, ignorando entregas canceladas.

## ✅ Problema 3: volume ao editar programação com redosagem

E está considerando o saldo de redosagem, para colocar o mínimo no valor de entrega da programação.

### Solução 1
Correção no frontend para não considerar o volume de load tickets que possuem um reuso do tipo redosagem e esteja utilizado
### Solução 2
Correção no backend para validar corretamente quais entregas e o volume total que deve ser preservado.

## ✅ Problema 4: retorno de step ao gerar erro de formulário

Está voltando a aba na edição de programação, quando falha a validação do yup na aba de entrega e é feito a tentativa de passar de etapa, ele volta pra aba de traço, logo não dá pra ver o que tá de errado, pois ao voltar pra aba de entrega limpa as validações.

O erro se deu pois na atualização utilizamos a propriedade invisible para ocultar a step de contrato. A validação não considerava os steps invisiveis, filtrando-os, porém isso gerava uma inconsistência na atualização da step pois no caso da atualização da programação caso um erro ocorresse na step 3 (index 2) era retornado que a step do erro era o index 1 (pois filtrou a step do contrato que está invisível) mas a step atual é atualizada com base em todas as steps e não só as visíveis.

### Solução 1
Adição de um mapeamento dos steps filtrados e não filtrados para retornar o index correto

# Casos de teste

## ✅ Caso 1: Permitir expedição de uma entrega ignorando canceladas na validação de sequência

- **Pré-condições:**
    - Possuir pelo menos 3 entregas em uma programação
    - A primeira deve estar pesada, a segunda cancelada e a terceira será onde o teste será executado
- **Passos do Teste:**
    1. Clicar em expedição na entrega
- **Resultado Esperado:** Abrir a tab de expedição normalmente
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 2: Não permitir expedição de uma entrega cujo a anterior não esteja pesada

- **Pré-condições:**
    - Possuir pelo menos 2 entregas liberadas em uma programação
- **Passos do Teste:**
    1. Clicar em expedição na segunda entrega liberada
- **Resultado Esperado:** Deve ser exibido um modal não alertando sobre a expedição fora de ordem
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 3: Permitir expedição de uma entrega cujo a anterior esteja pesada

- **Pré-condições:**
    - Possuir pelo menos 2 entregas em uma programação
    - A primeira deve estar, pelo menos, pesada e a segunda apenas liberada
- **Passos do Teste:**
    1. Clicar em expedição na segunda entrega liberada
- **Resultado Esperado:** Abrir a tab de expedição normalmente
- **Status:** ✅ PASSOU
- **Observações:**

## ✅ Caso 4: Editar volume da programação com redosagem para um volume inferior ao somatório da redosagem

- **Pré-condições:**
    - 1 programação com pelo menos 2 viagens
- **Passos do Teste:**
    1. Gerar um load ticket em uma entrega
    2. Fechar a aba da expedição
    3. Criar uma redosagem na tela de reaproveitamento
    4. Clicar na programação para edita-la
    5. Alterar o volume programado para um volume que respeite: volume de remessas emitidas > volume inserido < volume de remessas emitidas + volume da redosagem
- **Resultado Esperado:** O sistema não deve bloquear a edição da programação para este volume e atualizar corretamente a programação
- **Status:** ✅ PASSOU
- **Observações:** Após primeiro teste houve um problema na criação das entregas devido um erro na validação de entregas à preservar para redosagem.

## ✅ Caso 5: Editar volume da programação sem redosagem

- **Pré-condições:**
    - 1 programação com pelo menos uma viagem
- **Passos do Teste:**
    1. Clicar na programação para edita-la
    2. Alterar o volume programado
- **Resultado Esperado:** O sistema deve alterar corretamente o volume da programação
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 6: Erro na step de entrega não retornar step na atualização de programação

- **Pré-condições:**
    - Uma programação cadastrada com mais de uma entrega
    - Possuir uma entrega com nota emitida
- **Passos do Teste:**
    1. Clicar no botão "Editar" na programação no Gantt
    2. Pular para o step de entregas
    3. Informar um volume inferior ao já emitido no campo volume
    4. Clicar no botão da seta para a direita (próximo step)
- **Resultado Esperado:** É esperado que seja exibido um erro e que não seja alterado o step visualizado atual
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 7: Erro na step de entrega não retornar step na inserção de programação

- **Pré-condições:**
	- 
- **Passos do Teste:**
    1. Clicar no botão "+" no canto inferior direito em seguida no botão "Incluir programação"
    2. Preencher os campos adequadamente até o de entrega
    3. Informar um volume superior ao disponível para o traço
    4. Clicar no botão da seta para a direita (próximo step)
- **Resultado Esperado:** É esperado que seja exibido um erro e que não seja alterado o step visualizado atual
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅❌ Caso 2: redosar remessa já emitida não deve alterar o status

- **Pré-condições:**
    - 
    - 
    - 
- **Passos do Teste:**
    1. 
    2. 
    3. 
- **Resultado Esperado:** 
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

## ✅❌ Caso 2: nome do teste

- **Pré-condições:**
    - 
    - 
    - 
- **Passos do Teste:**
    1. 
    2. 
    3. 
- **Resultado Esperado:** 
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

## ✅❌ Caso 2: nome do teste

- **Pré-condições:**
    - 
    - 
    - 
- **Passos do Teste:**
    1. 
    2. 
    3. 
- **Resultado Esperado:** 
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)