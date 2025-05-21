---
Tipo de estudo: Diário
Fonte: https://medium.com/@romanglushach/domain-driven-design-ddd-a-guide-to-building-scalable-high-performance-systems-5314a7fe053c
---
DDD é um design de software que foca em modelar o software para se encaixar ao domínio, ou seja, modelado para seu intuito real.

## Benefícios

- **Comunicação**: facilita a comunicação entre desenvolvedores, gerente, product owner fornecendo uma linguagem comum sobre o domínio do negócio
- **Produtividade**: fornece um claro entendimento do negócio e encapsula a lógica de negócio de uma maneira de simples entendimento e manutenabilidade;
- **Manutenabilidade**: fornece um design modular e orientado a objeto de fácil compreensão e modificação
- **Escalabilidade**: melhora a escalabilidade fornecendo uma maneira de quebrar grandes monólitos em pequenas parte gerenciáveis;

## Arquitetura

![[Pasted image 20250520173724.png]]

### Interface do usuário

É a camada de interação do usuário como paginas web, aplicativos de celular, aplicativos desktop e etc.

Essa camada deve ser desacoplada do domínio, ou seja, não deve depender diretamente dos objetos e conceitos do domínio. Deve se utilizar de DTOs e views que forneçam os dados necessários para o usuário.

### Camada de aplicação

É a camada responsável por coordenar as ações e fluxos da aplicação, sendo mediador entre a interface do usuário e a camada de domínio.

Essa camada não deve depender diretamente de nenhum serviço ou recursos externos, apenas interfaces e abstrações definidas pela camada de domínio.