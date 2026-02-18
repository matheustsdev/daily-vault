## 1. Gestão Central e Configurações

Este módulo engloba as definições estruturais e cadastros base do sistema.

### ⚙️ Configurações e Arquitetura

- [x] Configurações iniciais do projeto.
- [x] **DDD:** Converter rotas para utilizarem os _Use Cases_.
- [ ] Implementar controle de direitos e permissões de usuário.

### 👤 Gestão de Usuários

- [ ] Cadastro de novos usuários.
- [ ] Atualização de dados de usuários existentes.
### 📁 Categorias

- [ ] Cadastro de categorias de produtos.
- [ ] Atualização de categorias existentes.
- [ ] Permitir categorias que não possuem medidas (ex: bolsas, sapatos).
## 2. Módulo de Produtos

Gestão do inventário e catálogo.

### ✅ Funcionalidades Realizadas

- [x] Cadastro de produtos.
- [x] Atualização de produtos.
- [x] Barra de pesquisa (Desktop e Mobile).
- [x] **Padronização de Medidas:** - Ordem Vestido: Busto, Cintura, Quadril e Ombro.
    - Ordem Geral: Manga, Costas, Altura, Cintura.

### 🛠️ Funcionalidades Pendentes

- [ ] Botão para deleção de produtos.
- [ ] Paginação no Mobile.
- [ ] **Tratativa de Referência:** A referência/código agora não precisa ser única, mas sua existência é obrigatória para o vínculo.
- [ ] **Validação de Medidas:** Garantir que valores sejam sempre maiores ou iguais a zero.
- [ ] Melhorar UI geral da tela de produtos.
## 3. Fluxo de Aluguel

Regras e interface para a operação principal do negócio.

### ✅ Funcionalidades Realizadas

- [x] Cadastro e atualização de aluguel.
- [x] Ordenação de dados de aluguel.
- [x] Melhoria visual nos cards e navegação.
- [x] Barra de pesquisa (Desktop e Mobile).
- [x] Funcionalidade de **"Baixa"** no aluguel (conclusão).

### 🛠️ Funcionalidades Pendentes

- [ ] **Busca Avançada:** Adicionar busca rápida por disponibilidade e busca insensível a acentos.
- [ ] **Resumo do Aluguel:**
    - Ajustar layout para suportar textos longos (descrições).
    - Exibir medidas do item selecionado no resumo.
- [ ] Validar integridade da função de atualização de aluguel.

## 4. Interface (UI) e Experiência do Usuário (UX)

Ajustes finos para facilitar o uso diário.

- [x] Melhorar mensagens de erro para campos não preenchidos.
- [ ] Adicionar DDD padrão automaticamente nos campos de contato.
- [ ] Renomear todos os campos/botões de "Devolução" para **"Fazer Baixa"**.
- [ ] Revisão geral da UI em todas as telas pendentes.

## 5. Documentos e Financeiro

Emissão de comprovantes e regras de pagamento.

- [ ] **Emissão de Recibos:**
    - Gerar recibo em folha A5 contendo a via do cliente e a via da loja na mesma página.
    - Exibir data de criação/reserva no documento.
    - Melhorar a legibilidade dos dados de pagamento.
- [ ] **Regra de Desconto:** Alterar o campo de desconto para operar exclusivamente como porcentagem (%).

## 6. Dashboard e Logística

Visão macro da operação e alertas rápidos.

- [ ] Implementar tela de Dashboard.
- [ ] **Listas de Controle:**
    - Aluguéis previstos para entrega hoje.
    - Aluguéis com entrega atrasada.
- [ ] Permitir realizar a "Baixa" do aluguel diretamente pelas listas do Dashboard.

## 7. Relatórios

Extração de dados para gestão de pessoas e financeira.

- [ ] **Relatório de Aluguéis:** Listagem completa de movimentações por período.
- [ ] **Relatório de Costureira:** Lista de ajustes e demandas para o setor de costura.