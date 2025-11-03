---
Tarefa: TDP-
Desenvolvedor: Eu
Método de code review: 
Responsabilidade:
  - Desenvolver
Data de início: 
Data de fim:
---
# Quebra técnica (horas estimadas/total)

- [ ] Estudo do funcionamento do Webhook (3h)
- [ ] Replicar formato de envio de mensagem do TECH (10h)
- [ ] Adicionar envio da mensagem para pesagem no DISPATCH para fila (8h)
- [ ] Criar uma aplicação simples em Node para consumir esse webhook (2h)

# Casos de teste

## ✅❌ Caso 1: Envio de mensagem (concreto)

- **Pré-condições:**
    - Rodar a aplicação do DISPATCH
    - Cadastrar parâmetros para utilização do webhook
- **Passos do Teste:**
    1. Criar uma programação de concreto
    2. Clicar no botão de enviar para automação na expedição
    3. Verificar a mensagem na fila de webhook de QA
- **Resultado Esperado:** É esperado uma mensagem na fila de webhook com os dados enviados corretamente
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)
## ✅❌ Caso 2: Envio de mensagem (argamassa)

- **Pré-condições:**
    - Rodar a aplicação do DISPATCH
    - Cadastrar parâmetros para utilização do webhook
- **Passos do Teste:**
    1. Criar 3 programações de argamassa em um mesmo grupo
    2. Clicar no botão de enviar para automação na expedição
    3. Verificar a mensagem na fila de webhook de QA
- **Resultado Esperado:** É esperado uma mensagem na fila de webhook com os dados enviados corretamente
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)
## ✅❌ Caso 3: Teste ponta a ponta

- **Pré-condições:**
    - Aplicação para consumir o webhook pronta e rodando
    - Aplicação do DISPATCH rodando
- **Passos do Teste:**
    1. Criar uma programação de concreto
    2. Clicar no botão de enviar para automação na expedição
    3. Verificar se a aplicação foi acionada pelo webhook 
- **Resultado Esperado:** Verificar o resposta da aplicação de teste
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)





