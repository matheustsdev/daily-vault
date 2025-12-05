---
Tipo de estudo: Grupo de estudo
Fonte: Topcon
---
A reunião foi um debate técnico aprofundado sobre os desafios e as evoluções nos projetos TopconDISPATCH ("Dispet") e TopconTECH ("TEC"). O foco principal foi discutir problemas arquiteturais, práticas de codificação e as estratégias adotadas para corrigir e escalar as aplicações.

A seguir, os principais temas discutidos, detalhados para seu estudo.

#### **1. Arquitetura e Organização de Projetos**

Um dos pontos centrais foi a dificuldade em manter uma arquitetura limpa (Clean Architecture) e aplicar os conceitos de Domain-Driven Design (DDD) no projeto Dispet.

- **Problema Identificado:** A implementação falhou em aderir aos princípios. Um exemplo claro foi a presença de **handlers na camada de domínio**, quando, na verdade, eles são serviços de aplicação e deveriam estar na camada de Aplicação (API).
    - **Conceito-chave (Handlers):** Handlers são responsáveis por orquestrar ações. Eles carregam uma entidade (do domínio), executam as regras de negócio nela e persistem as alterações. Esse fluxo é uma responsabilidade da camada de aplicação, que serve como uma porta de entrada para o domínio, mas não é o domínio em si.
- **Referência Circular:** Foi identificado um problema grave de dependência circular: o projeto `domain` precisa do `data` para injeção do `DataContext`, mas o `data` já possui uma dependência do `domain`. Isso cria um impasse que impede a reorganização correta dos projetos e a aplicação de padrões ideais.
- **Estratégia Atual:** Devido à impossibilidade de uma reestruturação completa por falta de tempo, a equipe adotou a estratégia de fazer um "**puxadinho bem comportado**". Ou seja, implementar as funcionalidades críticas de uma maneira que minimize efeitos colaterais, mesmo que não seja a ideal, adiando a refatoração completa.

#### **2. Injeção de Dependência (ID)**

A configuração correta da injeção de dependência foi um tópico crucial, pois impacta diretamente o desempenho, o uso de memória e o comportamento da aplicação.

- **DataContext: `Transient` vs. `Scoped`**
    - **Problema:** O `DataContext` do Entity Framework estava registrado como `Transient`. Isso significa que uma nova instância era criada toda vez que o `DataContext` era solicitado, mesmo dentro do mesmo request HTTP.
    - **Consequências:**
        1. **Memory Leak:** Instâncias não eram descartadas corretamente.
        2. **Erro de Tracking:** Se você carregasse a mesma entidade em diferentes momentos do request, o Entity Framework tentaria "rastrear" duas instâncias diferentes do mesmo objeto, gerando o clássico erro "_The instance of entity type cannot be tracked because another instance with the same key value is already being tracked_".
    - **Solução:** Alterar o registro do `DataContext` para **`Scoped`**. Isso garante que **apenas uma instância** do `DataContext` seja utilizada durante todo o ciclo de vida de uma requisição, resolvendo os problemas de tracking e de gerenciamento de memória.
- **Injeção no GraphQL:**
    - **Problema:** O esquema do GraphQL é, por natureza, um **`Singleton`** (uma única instância para toda a vida da aplicação). Injetar serviços com ciclo de vida mais curto (como `Scoped` ou `Transient`) diretamente em um `Singleton` pode "aprisioná-los", fazendo com que vivam mais do que deveriam.
    - **Solução:** O ajuste visa garantir que a resolução dos serviços necessários pelo GraphQL ocorra em um **contêiner `Scoped`**, criado a cada requisição, e não no escopo do `Singleton` do GraphQL.

#### **3. Gerenciamento de Transações e Mensageria**

A consistência dos dados foi um grande foco, especialmente em relação à forma como as transações de banco de dados e o envio de mensagens eram tratados.

- **O Problema da Inconsistência:** Muitas operações de alteração de dados (mutações) no Dispet ocorriam **fora de uma transação**. Além disso, eventos eram disparados para o Service Bus antes que a transação no banco de dados fosse confirmada (`commit`). Isso gerava inconsistências graves: um evento poderia notificar sobre uma alteração que, no fim, falhou e não foi salva no banco.
- **Solução (Outbox Pattern):** Foi implementado o **Outbox Pattern**.
    - **Como funciona:** Em vez de disparar a mensagem diretamente, a ação de enviar a mensagem é salva em uma tabela no mesmo banco de dados da transação principal. Somente **após o `commit`** bem-sucedido da transação principal, um outro processo lê essa tabela "outbox" e envia a mensagem de fato. Isso garante que as mensagens só sejam enviadas se a alteração nos dados for confirmada.
- **TransactionScope (Legado):** Foi mencionado que o Dispet utiliza `TransactionScope`, uma forma mais antiga de gerenciar transações. Rogério destacou a importância de usar `TransactionScopeAsyncFlowOption.Enabled` em operações assíncronas (`async/await`) para garantir que o contexto da transação "flua" corretamente entre as threads, evitando que parte do código execute fora da transação.

#### **4. Más Práticas e Efeitos Colaterais**

Rogério alertou sobre práticas no código que, embora pareçam funcionar, geram sérios problemas ocultos.

- **Uso de `Update` com `AsNoTracking()` e Deep Copy:**
    - **Prática:** Carregar uma entidade com `AsNoTracking()` (para não ser rastreada pelo EF), fazer uma cópia profunda do objeto (`deep copy`), realizar alterações e depois forçar a atualização com o método `Update()`.
    - **Por que é um problema?** Essa abordagem contorna o mecanismo de rastreamento de mudanças do Entity Framework, que é uma de suas principais vantagens. O uso do `Update()` em um objeto com várias entidades relacionadas (carregadas com `.Include()`) marca **toda a árvore de objetos** como `Modified`, mesmo que apenas um campo de um objeto tenha sido alterado.
- **Emissão Excessiva de Eventos:**
    - **Consequência direta do problema acima:** Ao marcar toda a árvore de objetos como `Modified`, o sistema dispara dezenas de eventos desnecessários, em vez de apenas um para a alteração que realmente ocorreu.
    - **Impacto:** Isso sobrecarrega a infraestrutura (Service Bus), pode estourar limites e, como os eventos são processados de forma assíncrona e fora de ordem, pode levar a problemas de integridade de dados (ex: tentar criar um "filho" antes do "pai").
    - **Mitigação:** Foi mencionado o uso de um evento intermediário com sessão para garantir a ordem de processamento, mas isso não resolve a causa raiz da emissão excessiva.

#### **5. Multitenancy e Escalabilidade**

As melhorias no TEC foram apresentadas como um modelo para o futuro do Dispet, visando a escalabilidade.

- **Multitenancy no TEC:** O projeto foi preparado para ser multitenant, com a separação dos bancos de dados por cliente. Isso é fundamental para a **escala horizontal**, permitindo distribuir a carga entre diferentes servidores.
- **Desafios de Escala do Dispet:** O Dispet era `stateful` (mantinha estado entre as chamadas), o que impedia adicionar mais máquinas para distribuir a carga. As melhorias recentes o tornaram mais próximo de ser `stateless`, permitindo um ganho na escala de computação, embora a escala de dados (particionamento em múltiplos bancos) ainda seja um passo futuro.