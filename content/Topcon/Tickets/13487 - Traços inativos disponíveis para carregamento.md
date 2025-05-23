---
Ticket: 500154
Status: Pendente
Data de início: 2025-05-16
Data de fim:
---
# Problemas
# Problema 1

Falha ao tentar integrar versão da familia pois o custo do insumo não existe e não há tratativa para adiciona-lo no fluxo do update.

## Solução 1

Adicionar manipulate para essa entidade ao atualizar a versão da familia 
## Solução 2




# Problema 2



## Solução 1



## Solução 2


# Casos de teste

## ✅❌ Caso 1: Fluxo de integração

- **Pré-condições:**
    - Existir uma família de traço 
- **Passos do Teste:**
    1. Iniciar atualização da família de traço
    2. Atualizar o status para inativo
    3. Criar um novo custo de insumo
    4. Atualizar o custo de insumo para um novo
    5. Finalizar atualização da familia
- **Resultado Esperado:** É esperado que seja criado uma mensagem de integração que atualize o status no DISPATCH e crie o novo custo
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)