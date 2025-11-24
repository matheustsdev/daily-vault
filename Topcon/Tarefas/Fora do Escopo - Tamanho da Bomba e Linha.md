---
Tarefa: 2125TDP-
Desenvolvedor: Eu
Método de code review:
Responsabilidade:
  - Desenvolver
Data de início: 2025-11-12
Data de fim:
---
# Quebra técnica (5h/20h)

- [x] Adicionar propriedade de tamanho de bomba (1h)
- [x] Adicionar campo com validações para tamanho de bomba (2h)
- [x] Garantir/validar a disponibilidade da propriedade nas APIs (2h) 

# Casos de teste

## ✅❌ Caso 1: nome do teste

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

# Documentação

---

> Documentação técnica sobre a alteração de **TDP-2125**

## Resumo

Foi implementada a funcionalidade de cadastro e visualização do tamanho da bomba. Agora, ao cadastrar ou editar um veículo, é possível informar o **Tamanho da Bomba (em metros)**.

Essa informação é condicional ao tipo de veículo, só sendo habilitado caso o veículo seja do tipo "Bomba", e reflete diretamente na seleção de bomba durante a etapa de programação, onde o tamanho da bomba agora é exibido nos _cards_ de seleção, facilitando a identificação do equipamento adequado pelo operador.

## Funcionamento
1. **Alteração no Cadastro de Veículos**
    No formulário de manutenção de veículos, foi adicionado o campo **"Tamanho da bomba (m)"**.
    
    - **Comportamento Condicional:** O campo se torna habilitado e obrigatório **apenas** quando o **Tipo de Veículo** selecionado for **"Caminhão Bomba"**.
    - Caso o tipo de veículo seja alterado para outro que não seja bomba, o campo é limpo automaticamente e desabilitado.
2. **Visualização na Seleção de Bomba (Programação)**
    
    Os _cards_ de seleção de bomba na etapa de programação foram atualizados para exibir essa nova informação:
    - Agora é exibido o tamanho da bomba (ex: `36m`) logo abaixo da placa do veículo.
    - Caso o veículo não possua tamanho cadastrado, é exibido um caractere de traço ("-").