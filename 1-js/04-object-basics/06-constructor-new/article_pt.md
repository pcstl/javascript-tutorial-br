# Construtor, operador "novo"

A sintaxe regular `{...}` permite criar um objeto. Mas, muitas vezes, precisamos criar muitos objetos semelhantes, como vários usuários ou itens de menu, e assim por diante.

Isso pode ser feito usando as funções do construtor e o operador `` `novo ''.

[cortar]

## Função do construtor

As funções do construtor tecnicamente são funções regulares. Contudo, existem duas convenções:

1. Eles são nomeados com a letra maiúscula primeiro.
2. Eles devem ser executados apenas com o operador `` novo ''.

Por exemplo:

`` `js run
função Usuário (nome) {
this.name = name;
this.isAdmin = false;
}

*! *
Deixe o usuário = novo Usuário ("Jack");
* /! *

alerta (user.name); // Jack
alerta (user.isAdmin); // falso
`` `

Quando uma função é executada como `new User (...)`, ele faz as seguintes etapas:

1. Um novo objeto vazio é criado e atribuído a `this`.
2. O corpo da função é executado. Geralmente modifica `this`, adiciona novas propriedades a ele.
3. O valor de `this` é retornado.

Em outras palavras, `new User (...)` faz algo como:

`` `js
função Usuário (nome) {
*! *
// this = {}; (implicitamente)
* /! *

// adicionar propriedades a este
this.name = name;
this.isAdmin = false;

*! *
// retorna isso; (implicitamente)
* /! *
}
`` `

Então, o resultado de `new User (" Jack ")` é o mesmo objeto que:

`` `js
Deixe o usuário = {
nome: "Jack",
isAdmin: falso
};
`` `

Agora, se quisermos criar outros usuários, podemos chamar `new User (" Ann ")`, `new User (" Alice ")` e assim por diante. Muito mais curto do que usar literais sempre, e também fácil de ler.

Esse é o principal objetivo dos construtores - implementar o código de criação de objetos reutilizáveis.

Vamos notar mais uma vez - tecnicamente, qualquer função pode ser usada como um construtor. Isto é: qualquer função pode ser executada com `new`, e executará o algoritmo acima. A "letra maiúscula primeiro" é um acordo comum, para deixar claro que uma função deve ser executada com `new`.

`` `` cabeçalho inteligente = "nova função () {...}"
Se tivermos muitas linhas de código tudo sobre a criação de um único objeto complexo, podemos envolvê-los na função do construtor, como este:

`` `js
Deixe o usuário = nova função () {
this.name = "John";
this.isAdmin = false;

// ... outro código para criação de usuários
// talvez lógica e declarações complexas
// variáveis ​​locais etc.
};
`` `

O construtor não pode ser chamado de novo, porque não é salvo em qualquer lugar, apenas criado e chamado. Portanto, esse truque visa encapsular o código que constrói o objeto único, sem reutilização futura.
`` ``

## Construtores de dupla sintaxe: new.target

Dentro de uma função, podemos verificar se foi chamado com `new` ou sem ele, usando uma propriedade` new.target` especial.

Está vazio para chamadas regulares e é igual a função se chamado com `new`:

`` `js run
função Usuário () {
alerta (new.target);
}

// sem novo:
Do utilizador(); // Indefinido

// com novo:
novo usuário(); // função Usuário {...}
`` `

Isso pode ser usado para permitir que a sintaxe `new` e regular seja o mesmo:

`` `js run
função Usuário (nome) {
se (! new.target) {// se você me executar sem novo
retornar novo usuário (nome); // ... Vou adicionar novo para você
}

this.name = name;
}

Deixe John = Usuário ("John"); // redireciona chamada para novo usuário
alerta (john.name); // John
`` `

Essa abordagem às vezes é usada em bibliotecas para tornar a sintaxe mais flexível. Provavelmente não é bom usar em todos os lugares, porque omitir "novo" torna um pouco menos óbvio o que está acontecendo. Com `new` todos sabemos que o novo objeto está sendo criado, isso é uma coisa boa.

## Retorno dos construtores

Normalmente, os construtores não têm uma declaração de "retorno". Sua tarefa é escrever todas as coisas necessárias no `this`, e automaticamente se torna o resultado.

Mas se houver uma declaração de "retorno", então a regra é simples:

- Se o "return" for chamado com o objeto, ele será retornado em vez de `this`.
- Se o "retorno" for chamado com uma primitiva, é ignorado.

Em outras palavras, `return` com um objeto retorna esse objeto, em todos os outros casos` this` é retornado.

Por exemplo, aqui `return` substitui` this` retornando um objeto:

`` `js run
função BigUser () {

this.name = "John";

retornar {nome: "Godzilla"}; // <- retorna um objeto
}

alerta (novo nome BigUser ().); // Godzilla, conseguiu esse objeto ^^
`` `

E aqui está um exemplo com um "retorno" vazio (ou podemos colocar um primitivo depois dele, não importa):

`` `js run
function SmallUser () {

this.name = "John";

Retorna; // finaliza a execução, retorna isso

// ...

}

alerta (novo nome SmallUser ().); // John
`` `

Normalmente, os construtores não têm uma declaração de "retorno". Aqui mencionamos o comportamento especial com os objetos que retornam principalmente por causa da completude.

`` `` cabeçalho inteligente = "Omitir parênteses"
Por sinal, podemos omitir parênteses após `new`, se não tiver argumentos:

`` `js
deixe usuário = novo usuário; // <- sem parênteses
// igual a
deixe o usuário = novo usuário ();
`` `

Omitindo parênteses aqui não é considerado um "bom estilo", mas a sintaxe é permitida por especificação.
`` ``

## Métodos no construtor

Usar funções do construtor para criar objetos oferece uma grande flexibilidade. A função de construtor pode ter parâmetros que definem como construir o objeto e o que inserir.

Claro, podemos adicionar a `this` não apenas propriedades, mas também métodos.

Por exemplo, `new User (name)` abaixo cria um objeto com o `nome 'e o método` sayHi`:

`` `js run
função Usuário (nome) {
this.name = name;

this.sayHi = function () {
alerta ("Meu nome é:" + this.name);
};
}

*! *
Deixe John = novo Usuário ("John");

john.sayHi (); // Meu nome é John
* /! *

/ *
john = {
Nome: "John",
sayHi: function () {...}
}
* /
`` `

## Resumo

- As funções do construtor ou, em breve, os construtores, são funções regulares, mas há um acordo comum para nomeá-las com a letra maiúscula primeiro.
- As funções do construtor só devem ser chamadas usando `new`. Tal chamada implica uma criação de 'this` vazio no início e retornando o preenchido no final.

Podemos usar as funções do construtor para criar múltiplos objetos semelhantes.

O JavaScript fornece funções de construtor para muitos objetos de linguagem embutidos: como `Date` para datas,` Set` para conjuntos e outros que planejamos estudar.

`` `smart header =" Objects, voltaremos! "
Neste capítulo, apenas abordamos os conceitos básicos sobre objetos e construtores. Eles são essenciais para aprender mais sobre tipos de dados e funções nos próximos capítulos.

Depois de sabermos que, no capítulo <info: orientado a objetos-programação> retornamos a objetos e os cobrimos em profundidade, incluindo herança e aulas.
`` `
