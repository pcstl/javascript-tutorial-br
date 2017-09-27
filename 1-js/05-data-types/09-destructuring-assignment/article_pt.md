# Desestruturação

As duas estruturas de dados mais utilizadas em JavaScript são `Object` e` Array`.

Os objetos permitem embalar muitas informações em uma única entidade e os arrays permitem armazenar coleções ordenadas. Então, podemos fazer um objeto ou uma matriz e lidar com isso como uma única coisa, talvez passar para uma chamada de função.

* Desestruturação atribuição * é uma sintaxe especial que permite "descompactar" arrays ou objetos em um monte de variáveis, como às vezes eles são mais convenientes. A desestruturação também funciona de forma excelente com funções complexas que possuem muitos parâmetros, valores padrão e, em breve, veremos como eles também são tratados.

[cortar]

## Arrazoamento desestruturação

Um exemplo de como a matriz é desestruturada em variáveis:

`` `js
// temos uma matriz com o nome e sobrenome
deixe arr = ["Ilya", "Escritório"]

*! *
// tarefa de desestruturação
Deixe [firstName, sobrenome] = arr;
* /! *

alerta (primeiro nome); // Ilya
alerta (sobrenome); // Office
`` `

Agora, podemos trabalhar com variáveis ​​em vez de membros da matriz.

Parece ótimo quando combinado com `split` ou outros métodos de retorno de matrizes:

`` `js
deixe [primeiro nome, sobrenome] = "Ilya Kantor" .split ('');
`` `

`` `` cabeçalho inteligente = "\" Desestruturação \ "não significa \" destrutivo \ ""
Isso é chamado de "atribuição de desestruturação", porque "destruiça" copiando itens em variáveis. Mas a matriz em si não é modificada.

Essa é apenas uma maneira mais curta de escrever:
`` `js
// let [firstName, sobrenome] = arr;
deixe firstName = arr [0];
let sobrenome = arr [1];
`` `
`` ``

`` `` cabeçalho inteligente = "Ignorar os primeiros elementos"
Elementos indesejados da matriz também podem ser descartados através de uma vírgula extra:

`` `js run
*! *
// primeiro e segundo elementos não são necessários
Deixe [,, título] = ["Julius", "César", "Cônsul", "da República Romana"];
* /! *

alerta (título); // Cônsul
`` `

No código acima, os primeiro e segundo elementos da matriz são ignorados, o terceiro é atribuído ao `título ', e o resto também é ignorado.
`` ``

`` `` cabeçalho inteligente = "Funciona com qualquer iterável no lado direito"

... Na verdade, podemos usá-lo com qualquer iterável, não apenas uma matriz:

`` `js
deixe [a, b, c] = "abc"; // ["a", "b", "c"]
Deixe [um, dois, três] = conjunto novo ([1, 2, 3]);
`` `

`` ``


`` `` cabeçalho inteligente = "Atribuir a qualquer coisa no lado esquerdo"

Podemos usar qualquer "atribuível" no lado esquerdo.

Por exemplo, uma propriedade de objeto:
`` `js run
Deixe o usuário = {};
[user.name, user.sendame] = "Ilya Kantor" .split ('');

alerta (user.name); // Ilya
`` `

`` ``

`` `` cabeçalho inteligente = "Looping com .entries ()"

No capítulo anterior, vimos o método [Object.entries (obj)] (mdn: js / Object / entries).

Podemos usá-lo com desestruturação para ignorar chaves e valores de um objeto:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30
};

// loop over keys-and-values
*! *
para (deixe [chave, valor] de Object.entries (usuário)) {
* /! *
alerta (`$ {key}: $ {value}`); // nome: John, então idade: 30
}
`` `

... E o mesmo para um mapa:

`` `js run
deixe o usuário = novo Mapa ();
user.set ("name", "John");
user.set ("idade", "30");

*! *
para (deixe [chave, valor] de user.entries ()) {
* /! *
alerta (`$ {key}: $ {value}`); // nome: John, então idade: 30
}
`` `
`` ``
### O resto '...'

Se queremos não apenas obter os primeiros valores, mas também reunir tudo a seguir - podemos adicionar um parâmetro mais que recebe "o resto" usando os três pontos "..." `:

`` `js run
Deixe [name1, name2, *! * ... rest * /! *] = ["Julius", "César", *! * "Consul", "da República Romana" * /! *];

alerta (nome1); // Julius
alerta (nome2); // César

*! *
alerta (repouso [0]); // Cônsul
alerta (repouso [1]); // da República Romana
alerta (rest.length); // 2
* /! *
`` `

O valor de `rest` é a matriz dos elementos de matriz restantes. Podemos usar qualquer outro nome de variável no lugar de `rest``, apenas certifique-se de ter três pontos antes dele e vai o último.

### Valores padrão

Se houver menos valores na matriz do que as variáveis ​​na atribuição - não haverá erro, os valores ausentes são considerados indefinidos:

`` `js run
*! *
deixe [primeiro nome, sobrenome] = [];
* /! *

alerta (primeiro nome); // Indefinido
`` `

Se quisermos um valor "padrão" para substituir o desaparecido, podemos fornecê-lo usando `=`:

`` `js run
*! *
// valores padrão
Deixe [name = "Guest", sobrenome = "Anônimo"] = ["Julius"];
* /! *

alerta (nome); // Julius (da matriz)
alerta (sobrenome); // Anônimo (padrão usado)
`` `

Os valores padrão podem ser expressões mais complexas ou mesmo chamadas de função. Eles são avaliados apenas se o valor não for fornecido.

Por exemplo, aqui usamos a função `prompt` para dois padrões. Mas ele só será executado apenas para o desaparecido:

`` `js run
// executa apenas um pedido de sobrenome
Deixe [name = prompt ('name?'), sobrenome = prompt ('sobrenome?')] = ["Julius"];

alerta (nome); // Julius (da matriz)
alerta (sobrenome); // o que for solicitado
`` `



## Desestruturação de objetos

A tarefa de desestruturação também funciona com objetos.

A sintaxe básica é:

`` `js
Deixe {var1, var2} = {var1: ..., var2 ...}
`` `

Temos um objeto existente no lado direito, que queremos dividir em variáveis. Para o lado esquerdo contém um "padrão" para as propriedades correspondentes. No caso simples, essa é uma lista de nomes de variáveis ​​em `{...}`.

Por exemplo:

`` `js run
Deixe opções = {
título: "Menu",
largura: 100,
altura: 200
};

*! *
Deixe {title, width, height} = options;
* /! *

alerta (título); // Cardápio
alerta (largura); // 100
alerta (altura); // 200
`` `

As propriedades `options.title`,` options.width` e `options.height` são atribuídas às variáveis ​​correspondentes. A ordem não importa, isso também funciona:

`` `js
// mudou a ordem de propriedades em let {...}
Deixe {altura, largura, título} = {título: "Menu", altura: 200, largura: 100}
`` `

O padrão no lado esquerdo pode ser mais complexo e especificar o mapeamento entre propriedades e variáveis.

Se queremos atribuir uma propriedade a uma variável com outro nome, por exemplo, `options.width` para entrar na variável denominada` w`, então podemos configurá-lo usando dois pontos:

`` `js run
Deixe opções = {
título: "Menu",
largura: 100,
altura: 200
};

*! *
// {sourceProperty: targetVariable}
Deixe {largura: w, altura: h, título} = opções;
* /! *

// largura -> w
// altura -> h
// título -> título

alerta (título); // Cardápio
alerta (w); // 100
alerta (h); // 200
`` `

O cólon mostra "o que: vai para onde". No exemplo acima, a propriedade `width` vai para` w`, a propriedade `height` vai para` h` e `title` é atribuído ao mesmo nome.

Para propriedades potencialmente perdidas, podemos definir valores padrão usando `" = "`, assim:

`` `js run
Deixe opções = {
título: "Menu"
};

*! *
Deixe {width = 100, height = 200, title} = opções;
* /! *

alerta (título); // Cardápio
alerta (largura); // 100
alerta (altura); // 200
`` `

Assim como com arrays ou parâmetros de função, os valores padrão podem ser quaisquer expressões ou mesmo chamadas de função. Eles serão avaliados se o valor não for fornecido.

O código abaixo pede largura, mas não sobre o título.

`` `js run
Deixe opções = {
título: "Menu"
};

*! *
Deixe {width = prompt ("width?"), title = prompt ("title?")} = opções;
* /! *

alerta (título); // Cardápio
alerta (largura); // (seja qual for o resultado do prompt é)
`` `

Também podemos combinar o ponto e a igualdade:

`` `js run
Deixe opções = {
título: "Menu"
};

*! *
Deixe {largura: w = 100, altura: h = 200, título} = opções;
* /! *

alerta (título); // Cardápio
alerta (w); // 100
alerta (h); // 200
`` `

### O operador restante

E se o objeto tiver mais propriedades do que as variáveis? Podemos levar algum e depois atribuir o "descanso" em algum lugar?

A especificação para usar o operador restante (três pontos) aqui é quase no padrão, mas a maioria dos navegadores ainda não o suporta.

Parece assim:

`` `js run
Deixe opções = {
título: "Menu",
altura: 200,
largura: 100
};

*! *
Deixe {title, ... rest} = opções;
* /! *

// now title = "Menu", rest = {height: 200, width: 100}
alerta (rest.height); // 200
alerta (rest.width); // 100
`` `



`` `` smart header = "Gotcha without` let` "
Nos exemplos acima, as variáveis ​​foram declaradas antes da atribuição: `let {...} = {...}`. Claro, podemos usar variáveis ​​existentes também. Mas há uma captura.

Isso não funcionará:
`` `js run
deixe o título, largura, altura;

// erro nesta linha
{título, largura, altura} = {título: "Menu", largura: 200, altura: 100};
`` `

O problema é que o JavaScript trata `{...}` no fluxo de código principal (não dentro de outra expressão) como um bloco de código. Esses blocos de código podem ser usados ​​para fazer declarações de grupo, como este:

`` `js run
{
// um bloco de código
Deixe a mensagem = "Olá";
// ...
alerta (mensagem);
}
`` `

Para mostrar ao JavaScript que não é um bloco de código, podemos envolver toda a tarefa entre parênteses `(...)`:

`` `js run
deixe o título, largura, altura;

// Okay agora
*! * (* /! * {título, largura, altura} = {título: "Menu", largura: 200, altura: 100} *! *) * /! *;

alerta (título); // Cardápio
`` `

`` ``

## Desestruturação aninhada

Se um objeto ou uma matriz contém outros objetos e arrays, podemos usar padrões mais complexos do lado esquerdo para extrair porções mais profundas.

No código abaixo, `options` tem outro objeto na propriedade` size` e uma matriz na propriedade `items`. O padrão no lado esquerdo da tarefa tem a mesma estrutura:

`` `js run
Deixe opções = {
Tamanho: {
largura: 100,
altura: 200
},
itens: ["Cake", "Donut"],
extra: verdadeiro // algo extra que não destruiremos
};

// atribuição de desestruturação em várias linhas para maior clareza
deixei {
tamanho: {// coloque o tamanho aqui
largura,
altura
},
itens: [item1, item2], // atribuir itens aqui
title = "Menu" // não presente no objeto (o valor padrão é usado)
} = opções;

alerta (título); // Cardápio
alerta (largura); // 100
alerta (altura); // 200
alerta (item1); // Bolo
alerta (item2); // Rosquinha
`` `

Todo o objeto `options`, exceto` extra` que não foi mencionado, é atribuído às variáveis ​​correspondentes.

! [] (desestruturação-complex.png)

Finalmente, temos `width`,` height`, `item1`,` item2` e `title` do valor padrão.

Isso geralmente acontece com tarefas de desestruturação. Temos um objeto complexo com muitas propriedades e queremos extrair apenas o que precisamos.

Mesmo assim:
`` `js
// pegue o tamanho como um todo em uma variável, ignore o resto
Deixe {tamanho} = opções;
`` `

## Parâmetros da função inteligente

Há momentos em que uma função pode ter muitos parâmetros, a maioria dos quais são opcionais. Isso é especialmente verdadeiro para interfaces de usuário. Imagine uma função que crie um menu. Pode ter uma largura, uma altura, um título, uma lista de itens e assim por diante.

Aqui está uma má maneira de escrever essa função:

`` `js
function showMenu (title = "Untitled", width = 200, height = 100, items = []) {
// ...
}
`` `

O problema da vida real é como lembrar a ordem dos argumentos. Normalmente, os IDEs tentam nos ajudar, especialmente se o código estiver bem documentado, mas ainda assim ... Outro problema é como chamar uma função quando a maioria dos parâmetros está correto por padrão.

Como isso?

`` `js
showMenu ("Meu Menu", indefinido, indefinido, ["Item1", "Item2"])
`` `

Isso é feio. E torna-se ilegível quando lidamos com mais parâmetros.

A desestruturação vem ao resgate!

Podemos passar parâmetros como um objeto, e a função imediatamente os destrói em variáveis:

`` `js run
// passamos objeto para funcionar
Deixe opções = {
título: "Meu menu",
itens: ["Item1", "Item2"]
};

// ... e imediatamente expande para variáveis
função showMenu (*! * {title = "Untitled", width = 200, height = 100, items = []} * /! *) {
// título, itens - tirados das opções,
// largura, altura - padrões usados
alerta (título + '' + largura + '' + altura); // Meu Menu 100 200
alerta (itens); // Item1, Item2
}

showMenu (opções);
`` `

Também podemos usar uma desestruturação mais complexa com objetos aninhados e mapeamentos de dois pontos:

`` `js run
Deixe opções = {
título: "Meu menu",
itens: ["Item1", "Item2"]
};

*! *
show de funçãoMenu ({
title = "Untitled",
largura: w = 100, // largura vai para w
altura: h = 200, // altura vai para h
itens: [item1, item2] // item primeiro elemento vai ao item1, em segundo lugar ao item2
}) {
* /! *
alerta (título + '' + w + '' + h); // Meu Menu 100 200
alerta (item1); // Item 1
alerta (item2); // Item2
}

showMenu (opções);
`` `

A sintaxe é a mesma que para uma atribuição de desestruturação:
`` `js
função({
entradaProperty: parameterName = defaultValue
...
})
`` `

Observe que tal desestruturação pressupõe que `showMenu ()` tenha um argumento. Se quisermos todos os valores por padrão, devemos especificar um objeto vazio:

`` `js
showMenu ({});

// que daria um erro
showMenu ();
`` `

Podemos corrigir isso fazendo `{}` o valor padrão para toda a coisa de desestruturação:


`` `js run
// parâmetros simplificados um pouco para maior clareza
função showMenu (*! * {title = "Menu", largura = 100, altura = 200} = {} * /! *) {
alerta (título + '' + largura + '' + altura);
}

showMenu (); // Menu 100 200
`` `

No código acima, todo o objeto de argumentos é `{}` por padrão, então sempre há algo para desestruturar.

## Resumo

- A atribuição de desestruturação permite mapear instantaneamente um objeto ou matriz em muitas variáveis.
- A sintaxe do objeto:
`` `js
Deixe {prop: varName = default, ...} = objeto
`` `

Isso significa que a propriedade `prop` deve entrar na variável` varName` e, se nenhuma dessas propriedades existir, o valor "padrão" deve ser usado.

- A sintaxe da matriz:

`` `js
deixe [item1 = padrão, item2, ... restante] = matriz
`` `

O primeiro item vai para `item1`, o segundo passa para` item2`, todo o resto torna a matriz `rest '.

- Para casos mais complexos, o lado esquerdo deve ter a mesma estrutura que a direita.
