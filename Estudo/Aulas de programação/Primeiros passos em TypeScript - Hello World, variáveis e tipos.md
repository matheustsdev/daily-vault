---
Número da aula: "1"
---
# Objetivos de aprendizagem

• Entender o que é TypeScript e por que usá-lo
• Criar e executar um arquivo .ts
• Declarar variáveis e identificar tipos primitivos

# Introdução (5 min)

• “Hoje vamos ver o que é TS, como ele se diferencia do JavaScript e criar nosso primeiro programa.”
• Mostre um trecho de código JS sem tipos vs. TS com tipos explícitos.
• Explique: segurança de tipos, autocomplete e manutenção.
# Conceitos básicos (20 min)

## 1. Criar um projeto

1. Instalar NodeJs
2. Instalar VSCode
3. Abrir VSCode na pasta do projeto
4. Executar comandos
```bash
npm init -y
npm install typescript --save-dev
npm init -y
```
5. Criar arquivo index.tsx
6. Executar projeto
```bash
npx tsc
npm init -y
```
## 1. Hello World

• Crie o arquivo hello.ts
• Código:
```typescript
// hello.ts
function sayHello(name: string): string {
	return Olá, ${name}!;
}

 console.log(sayHello('Mundo'));
 ```

• Explique: compilação (tsc hello.ts), node hello.js.

## 2. Variáveis e tipos primitivos

• number, string, boolean, any, array:
```ts
let idade: number = 25; 
let nome: string = 'Ana'; 
let ativo: boolean = true; 
let notas: number[] = [8, 9, 7.5]; 
let qualquerCoisa: any = 'pode ser qualquer coisa';
```

• Demonstre erros de tipo (ex.: atribuir string a idade).

# Exercício prático (25 min)

**Proposta:** crie um arquivo user.ts que:
- Declare uma interface User com campos id: number, name: string, isAdmin: boolean.
- Crie um array de usuários (3 objetos).
- Escreva uma função que receba o array e retorne somente os admins.
- Exiba no console o resultado.

**Níveis de dificuldade:**
• **Fácil**: implementar interface e array.
• **Intermediário**: filtrar admins com filter.
• **Avançado**: tipar a função genérica para filtrar qualquer campo.

> [!note] Nota
> Durante a atividade, o instrutor passe pelas mesas, revisando erros de compilação, sugerindo uso de VSCode e atalho para console.


# Discussão e revisão (10 min)

• Peça que dois alunos mostrem suas soluções.
• Aponte boas práticas (nomes claros, uso de interfaces).
• Entregue desafio para casa:
	- Implementar função que adicione um novo usuário (checando tipos).


Solução sugerida (user.ts)

```ts
interface User {
  id: number;
  name: string;
  isAdmin: boolean;
}

const users: User[] = [
  { id: 1, name: 'Ana', isAdmin: true },
  { id: 2, name: 'Bruno', isAdmin: false },
  { id: 3, name: 'Carla', isAdmin: true },
];

function getAdmins(list: User[]): User[] {
  return list.filter(u => u.isAdmin);
}

console.log(getAdmins(users));
```
────────────────────────────────────────────────────

## A. Projeto de curso (to-do list):

• Cada aula adiciona uma feature (criar tarefas, listar, marcar como feita, remover).
• Ao final, deploy simples com Node.js + TS + JSON.

## B. Recursos adicionais:

• Livro: “Programming TypeScript” de Boris Cherny.
• Site: https://www.typescriptlang.org/docs/
• Tutorial online: https://www.freecodecamp.org/news/tag/typescript/

# C. Dicas de engajamento:

• Gamificação: pontos por erros corrigidos.
• Duplas de pair-programming para troca de conhecimento.
• “Kahoot” rápido no fim de cada aula para fixar termos.

Com essa estrutura e exemplos, você terá uma sequência de aulas envolventes, práticas e alinhadas às boas práticas de ensino de programação com TypeScript. Bom curso!
