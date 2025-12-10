---
Ticket: 544051
Status: Em análise
Data de início: 2025-12-10
Data de fim:
---
# Problemas
## Problema 1
Erro ao emitir a nota devido não existir contato do tipo telefone
### Causa
Erro na integração do contato do tipo telefone e não obrigatoriedade dele no legado
### Solução 1
Validar a integração dos contatos
### Solução 2
Ajustar emissão de nota para tentar usar telefone, mas caso não exista, usar fixo

# Casos de teste

## ✅❌ Caso 1: emissão apenas com telefone fixo

- **Passos do Teste:**
    1. Garantir apenas um contato do tipo telefone fixo no legado
    2. Forçar integração
    3. Emitir remessa
- **Resultado Esperado:** Sucesso na emissão
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)
## ✅❌ Caso 2: emissão apenas com celular

- **Passos do Teste:**
    1. Garantir apenas um contato do tipo celular no legado
    2. Forçar integração
    3. Emitir remessa
- **Resultado Esperado:** Sucesso na emissão
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)
## ✅❌ Caso 3: emissão com celular e telefone fixo

- **Passos do Teste:**
    1. Garantir contato do tipo celular e telefone fixo no legado
    2. Forçar integração
    3. Emitir remessa
- **Resultado Esperado:** Sucesso na emissão com dados do celular na nota
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)