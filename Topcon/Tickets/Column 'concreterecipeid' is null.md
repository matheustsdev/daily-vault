---
Ticket: 501007
Status: Concluído
Data de início: 2025-05-27
Data de fim: 2025-06-02
---
# Problemas
# Problema 1: Programação sem produto
Programações estão ficando sem item product mesmo após emissão de remessa. 

Com os exemplos disponibilizados não foi possível reproduzir o erro, porém segue o problema que fica exibindo erro quando existe uma programação sem produto.
## Solução 1
Adicionar validação no final da inclusão de programação quando for criar uma programação sem produto alertando o cliente disso.
## Solução 2
Adicionar validação no backend que retorna um alerta ao tentar alterar bomba ou traço de programação que já possui remessa de concreto ou bomba, respectivamente, emitida
# Problema 2: Programação sem produto e bomba

É possível criar programações sem produto e sem bombeamento, o que não faz sentido no quesito produto
## Solução 1

Adicionar validação se existe produto ou bombeamento vinculado. Caso não exista ambos, exibir alerta.
# Problema 3: Disponibilidade do Gantt

A query de disponibilidade tanto de betoneiras quanto de ponto de carga não está considerando a possibilidade do traço nulo.
## Solução 1

Adicionar validação para o campo nulo e não conta-lo como ocupação.
# Casos de teste

## ✅ Caso 1: alerta de programação sem concreto

- **Pré-condições:**
    - Ser capaz de criar programações
- **Passos do Teste:**
    1. Iniciar processo de inclusão de programação
    2. Adicionar um horário que não irá ultrapassar o dia
    3. Não adicionar nenhum traço
    4. Clicar no botão de incluir programação no final do processo
- **Resultado Esperado:** Deve ser exibido um modal alertando sobre a criação de programação sem produto vinculado
- **Status:** ✅ PASSOU
- **Observações:**
## ✅ Caso 2: não exibir alerta de programação sem concreto

- **Pré-condições:**
    - Ser capaz de criar programações
- **Passos do Teste:**
    1. Iniciar processo de inclusão de programação
    2. Adicionar um horário que não irá ultrapassar o dia
    3. Adicionar um traço
    4. Clicar no botão de incluir programação no final do processo
- **Resultado Esperado:** Não deve ser exibido nenhum modal alertando sobre a criação de programação sem produto vinculado
- **Status:** ✅ PASSOU
- **Observações:**
## ✅ Caso 3: alerta de programação sem concreto e ultrapassando o dia

- **Pré-condições:**
    - Ser capaz de criar programações
- **Passos do Teste:**
    1. Iniciar processo de inclusão de programação
    2. Adicionar um horário que irá ultrapassar o dia
    3. Não adicionar nenhum traço
    4. Clicar no botão de incluir programação no final do processo
- **Resultado Esperado:** Deve ser exibido um modal alertando sobre a criação de programação sem produto vinculado em seguida o modal sobre a programação ultrapassar um dia
- **Status:** ✅ PASSOU
- **Observações:** 
## ✅ Caso 4: programação comum com traço e em um único dia

- **Pré-condições:**
    - Ser capaz de criar programações
- **Passos do Teste:**
    1. Iniciar processo de inclusão de programação
    2. Adicionar um horário que irá ultrapassar o dia
    3. Adicionar um traço
    4. Clicar no botão de incluir programação no final do processo
- **Resultado Esperado:** Não deve ser exibido nenhum modal alertando sobre a criação de programação sem produto vinculado porém exibir o alerta de programação ultrapassando um dia
- **Status:** ✅ PASSOU
- **Observações:** 
## ✅ Caso 5: programação sem traço nem bomba

- **Pré-condições:**
    - Ser capaz de criar programações
- **Passos do Teste:**
    1. Iniciar processo de inclusão de programação
    2. Não adicionar uma bomba
    3. Não adicionar um traço
    4. Clicar no botão de incluir programação no final do processo
- **Resultado Esperado:** Deve ser exibido um alerta de programação inválida
- **Status:** ✅ PASSOU
- **Observações:** 
## ✅ Caso 6: programação sem traço nem bomba, ultrapassando o dia

- **Pré-condições:**
    - Ser capaz de criar programações
- **Passos do Teste:**
    1. Iniciar processo de inclusão de programação
    2. Adicionar um horário que irá ultrapassar o dia
    3. Não adicionar uma bomba
    4. Não adicionar um traço
    5. Clicar no botão de incluir programação no final do processo
- **Resultado Esperado:** Deve ser exibido um alerta de programação inválida
- **Status:** ✅ PASSOU
- **Observações:**
## ✅ Caso 7: remover produto de programação com remessa emitida

- **Pré-condições:**
    - Pelo menos 1 programação com produto vinculado
    - Pelo menos 1 remessa de concreto expedida para a mesma programação
    - Remover TEMPORARIAMENTE a validação no Frontend de remover produto selecionado
- **Passos do Teste:**
    1. Iniciar processo de alteração da programação
    2. Remover o produto/traço vinculado
    3. Clicar no botão de atualizar programação no final do processo
    4. Readicionar a validação no Frontend de remover o produto selecionado
- **Resultado Esperado:** Deve ser exibido um erro na requisição do back alertando sobre a remoção do produto
- **Status:** ✅ PASSOU
- **Observações:**
## ✅ Caso 8: remover bombeamento de programação com remessa de bomba emitida

- **Pré-condições:**
    - Pelo menos 1 programação com bombeamento vinculado
    - Pelo menos 1 remessa de bomba expedida para a mesma programação
    - Remover TEMPORARIAMENTE a validação no Frontend de remover bomba selecionado
- **Passos do Teste:**
    1. Iniciar processo de alteração da programação
    2. Remover o produto/traço vinculado
    3. Clicar no botão de atualizar programação no final do processo
    4. Readicionar a validação no Frontend de remover o produto selecionado
- **Resultado Esperado:** Deve ser exibido um erro na requisição do back alertando sobre a remoção do produto
- **Status:** ✅ PASSOU
- **Observações:**