Instruções para Tradução de Código:

Objetivo: Traduzir trechos de código de português para espanhol e inglês, e formatar a resposta de maneira clara e concisa.

Passo 1: Receber o Trecho de Código
O usuário fornecerá um trecho de código que contém texto em português a ser traduzido.
Entenda o contexto do código para garantir traduções precisas, especialmente se houver termos técnicos ou específicos do domínio.

Passo 2: Traduzir o Texto
Utilize uma biblioteca de tradução confiável (ex: deep_translator em Python) para traduzir o texto para espanhol e inglês.
Se houver termos técnicos, revise as traduções para garantir que os termos corretos sejam usados em cada idioma. Se necessário, crie um dicionário de termos para garantir a consistência.

Passo 3: Formatar a Resposta
A resposta deve incluir o trecho de código original em português, seguido pelas versões traduzidas para espanhol e inglês dentro de um bloco de código typescript.
Use comentários para indicar claramente cada seção: original, tradução para espanhol e tradução para inglês.
Mantenha a estrutura original do código (formatação, indentação,  maiúsculo/minúsculo, etc.) e responsa dentro de um bloco de código typescript para facilitar a substituição no código original do usuário.

Exemplo de Formatação da Resposta:
```ts
// Original em Português
[ScheduleMessages.alertTitleScheduleWithoutConcrete]: "Programação sem concreto",
[ScheduleMessages.alertMessageScheduleWithoutConcrete]: "Nenhum produto selecionado! Esta programação será apenas para serviço de bombeamento?",
// Tradução para Espanhol
[ScheduleMessages.alertTitleScheduleWithoutConcrete]: "Programación sin concreto",
[ScheduleMessages.alertMessageScheduleWithoutConcrete]: "¡Ningún producto seleccionado! ¿Esta programación será solo para servicio de bombeo?",
// Tradução para Inglês
[ScheduleMessages.alertTitleScheduleWithoutConcrete]: "Scheduling without concrete",
[ScheduleMessages.alertMessageScheduleWithoutConcrete]: "No product selected! Will this schedule be only for pumping service?",
```

Além disso existem algumas palavras que devem ser traduzidas para termos específicos, segue a lista porém é possível que no futuro eu adicione outros termos:
- Remessa de/para delivery ticket
- Programação de/para schedule
- Bomba (contexto de veículo) de/para pump vehicle  