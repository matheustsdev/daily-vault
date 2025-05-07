---
Ticket: 500156
Status: Em análise
Data de início: 2025-05-06
Data de fim:
---
# Problemas
## Problema 1: alteração de status ao redosar

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

## Problema 4: retorno de step ao gerar erro de formulário

Está voltando a aba na edição de programação, quando falha a validação do yup na aba de entrega e é feito a tentativa de passar de etapa, ele volta pra aba de traço, logo não dá pra ver o que tá de errado, pois ao voltar pra aba de entrega limpa as validações  

### Solução 1



### Solução 2


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