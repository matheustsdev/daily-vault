---
Tarefa: TDP-2115
Desenvolvedor: Eu
Método de code review:
Responsabilidade:
  - Desenvolver
Data de início: 2025-11-04
Data de fim:
---
# Quebra técnica (9h/12h)
 
- [x] Exibir nome do vendedor e contato no card de contrato (2h)
- [x] Adicionar contato no vendedor (1h)
- [x] Tornar claro a visualização do contrato selecionado (1h)
- [x] Adicionar um campo de link na entidade *Construction*  (2h)
- [x] Deve ser exibido o *external id* da obra no card de contrato (1h)
- [x] O *external id* da obra no card de contrato deve ser clicável, direcionando para o link vinculado na *Construction* (1h)
- [ ] Documentar alteração das API (2h)

# Casos de teste

## ✅❌ Caso 1: Fluxo API

- **Pré-condições:**
    - Possuir contratos cadastrados
- **Passos do Teste:**
    1. Cadastrar um vendedor
    2. Atualizar uma obra adicionando o link
    3. Iniciar processo de inclusão de programação
    4. Checar contrato alterado
- **Resultado Esperado:** É esperado que o card de contrato exiba os dados adicionados via API
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)