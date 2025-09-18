---
Tarefa: TDP-2318
Desenvolvedor: Eu
Método de code review:
Responsabilidade:
  - Desenvolver
Data de início: 2025-09-17
Data de fim:
---
# Quebra técnica (13h/20h)

- [x] Validação sobre criação de nova versão (1h)
- [ ] Implementar nova versão (2h)
- [ ] Implementar redirecionamento automático para v1 (2h)
- [ ] Criar DTO de retorno que receberá os includes (4h)
- [ ] Ajuste da documentação pública (4h)

# Casos de teste

## ✅❌ Caso 1: Requisição à API sem alteração

- **Pré-condições:**
    - Um serviço para requisitar à uma API (insomnia, postman e etc)
    - Projeto deve estar em execução
- **Passos do Teste:**
    1. Executar chamada API à rota pré-existente
    2. Verificar se a chamada será direcionada à v1 com break point
    3. Liberar fluxo do código
- **Resultado Esperado:** Retorno dos dados conforme a API já existente
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

## ✅❌ Caso 2: Requisição à API v2

- **Pré-condições:**
    - Um serviço para requisitar à uma API (insomnia, postman e etc)
    - Projeto deve estar em execução
- **Passos do Teste:**
    1. Executar chamada API à rota v2
    2. Verificar se a chamada será direcionada à v2 com break point
    3. Liberar fluxo do código
- **Resultado Esperado:** Retorno dos dados conforme o DTO criado
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

<%* 
await tp.file.move("Topcon/Tickets/Novo ticket");
%>



