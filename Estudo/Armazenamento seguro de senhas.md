---
Fonte: https://www.tabnews.com.br/rafael/armazenamento-seguro-de-senhas-um-guia-pratico-texto-puro-hash-salt-e-mais
---
# Sem tratamento

A primeira forma que aprendemos a salvar dados é salvando eles de forma pura, sem nenhum tratamento.
 Salvar assim é simples e rápido, não existe nenhum trabalho extra no desenvolvimento, qualquer sistema pode usar as credenciais salvas nesse banco de dados sem dificuldade, e o usuário pode recuperar a senha por e-mail caso esqueça.
 Só que salvar assim é o jeito mais fácil de ter a senha vazada. Qualquer pessoa com acesso ao banco de dados pode ver a senha de todos os usuários, e nem precisa ser um "hacker" para ver o banco: pode ser um desenvolvedor do sistema.
 
Então, na verdade, **conseguir recuperar a senha é um ponto negativo!** O ideal é que o usuário consiga acessar o sistema mesmo sem o sistema saber a senha original do usuário.

# Com criptografia

Salvar uma senha assim não é tão simples quanto salvar o texto puro, porque precisamos de uma função para criptografar, mas quem ver a senha criptografada não vai conseguir entrar no sistema. Então, acabamos de aumentar a segurança do sistema! Quer dizer, apenas se a pessoa com acesso às senhas criptografadas não tiver a chave/método de descriptografia.
Isso porque a criptografia é um processo reversível, e esse é o principal problema dela, quando falamos sobre salvar senhas. A criptografia não evitaria o problema da história que citei no tópico anterior.

# Solução não reversível: Hash

Um hash é uma função matemática: você dá a ele um valor de entrada e a função emite um valor de saída; e a mesma entrada sempre produz a mesma saída. A função hash é projetada de modo que a saída seja imprevisível, isto é, que não seja possível "quebrar" o hash. Como o hash é uma função unidirecional (_"one-way function"_), não existe um "deshash". Logo, não é possível obter a senha original a partir do hash.

Outra característica importante do hash para senhas é ser **altamente resistente a colisões**. Uma colisão ocorre quando duas entradas diferentes geram a mesma saída. Embora colisões sejam teoricamente possíveis em qualquer hash com saída finita, em um hash seguro, deve ser extremamente difícil (computacionalmente inviável) encontrar duas entradas que colidam. Se fosse fácil encontrar colisões, seria fácil acessar uma conta usando uma senha errada.

Inicialmente, o MD5 era um hash muito utilizado para armazenar senhas, e provavelmente ainda existe código não-mantido hoje em dia que utiliza o MD5. Ele é muito rápido de ser calculado, praticamente instantâneo, e isso não é bom para senhas. Imagine, mais uma vez, que o atacante tem acesso ao banco de dados. Se você utilizou MD5, ele pode tentar adivinhar as senhas "por força bruta" e cada tentativa de acertar a senha não demora nada!

O SHA-1, projetado pela Agência de Segurança Nacional dos Estados Unidos em 1995, era recomendado como substituto do MD5 já em 1996. O ponto é que é possível gerar o SHA-1 para várias senhas comumente utilizadas e salvar isso numa grande tabela, chamada de _rainbow table_.

# Hash + Salt

Se, para gerar hashes diferentes, precisamos de valores diferentes, então essencialmente temos que fazer senhas iguais, na verdade, serem diferentes. Pode parecer estranho, mas é bem simples: se concatenarmos um texto com a senha do usuário, por exemplo `TabNews` + `12345678`, significa que mudamos a senha do usuário. Mas, se o texto sempre for igual, então a mesma senha sempre irá gerar o mesmo hash. Portanto, **precisamos de um valor único por usuário para concatenar com a senha.**

E esse é o conceito de _salt_ (em português, "sal"). Como o salt irá mudar o texto que faremos hash, precisamos salvar o salt no banco de dados também para realizar a mesma verificação no login. Para cada senha, o salt deve ser único e aleatório.

# Hash + Salt + Pepper

Agora, se nós concatenarmos a senha do usuário com um valor secreto, que só a aplicação sabe, não precisamos armazenar esse valor no banco de dados. Na verdade, para ser ainda mais seguro e não vazar com o código fonte, podemos armazenar esse segredo nas variáveis de ambiente. Isso é chamado de _pepper_ (em português, "pimenta").

Se usarmos Hash + Pepper, perdemos o benefício de hashes diferentes. Se usarmos Hash + Salt, perdemos o benefício de ter algo secreto que dificulta que o atacante realize ataques offline de forma eficiente. Então, podemos juntar as duas técnicas e ter Hash + Salt + Pepper.

A comparação que fazemos para verificar a senha precisa da  concatenação do salt no início e do pepper no fim: `"e0c3a3babfb4fa7c4c8aa975722fc2ce78a5b77d" == sha1("kifa12p312345678TabNews")`.

# E o que é um hash forte?

Ao contrário dos hashes usados para verificar a integridade de arquivos ou dados (como o próprio SHA-1 ou SHA-256), que são projetados para serem rápidos, um hash para senhas é projetado propositalmente para ser **lento** e **computacionalmente caro de calcular**.

Essas características são importantes porque o principal ataque a senhas vazadas (mesmo com salt) é tentar adivinhar a senha original por força bruta ou dicionário. Se cada tentativa de adivinhar a senha leva muito tempo e consome muitos recursos do atacante, o ataque se torna inviável em larga escala.

A lentidão controlada é obtida através de "fatores de custo" ou "fatores de trabalho" configuráveis no algoritmo. Isso geralmente envolve realizar o processo de hashing milhares ou milhões de vezes (iterações), usar uma quantidade significativa de memória RAM (tornando ataques com hardware especializado como GPUs mais difíceis) ou aproveitar múltiplos núcleos de CPU.

Alguns dos algoritmos mais recomendados atualmente são:

- **[bcrypt](https://en.wikipedia.org/wiki/Bcrypt):** Criado em 1999, é um dos primeiros algoritmos populares a incorporar um fator de trabalho ajustável.
- **[scrypt](https://en.wikipedia.org/wiki/Scrypt):** Criado em 2009 com o objetivo de exigir bastante memória RAM, além de CPU, é mais resistente a ataques com hardware dedicado.
- **[Argon2](https://en.wikipedia.org/wiki/Argon2):** Criado em 2015 e vencedor da [Password Hashing Competition](https://password-hashing.net/), é considerado o estado da arte. Possui modos que permitem balancear o uso de CPU, memória e paralelismo, sendo o Argon2id o mais recomendado.