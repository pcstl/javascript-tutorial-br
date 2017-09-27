# Métodos de primitivas

O JavaScript nos permite trabalhar com primitivas (strings, números, etc.) como se fossem objetos.

Eles também fornecem métodos para chamar e tal. Estudaremos aqueles em breve, mas primeiro veremos como funciona, porque, claro, as primitivas não são objetos (e aqui vamos torná-lo ainda mais claro).

[cortar]

Vejamos a distinção chave entre primitivas e objetos.

Um primitivo

Um objeto
: É capaz de armazenar vários valores como propriedades.
Pode ser criado com `{}`, por exemplo: `{name:" John ", age: 30}`. Existem outros tipos de objetos em JavaScript, e. as funções são objetos.

Uma das melhores coisas sobre os objetos é que podemos armazenar uma função como uma das suas propriedades:

`` `js run
deixe john = {
Nome: "John",
sayHi: function () {
alerta ("Olá amigo!");
}
};

john.sayHi (); // Oi Cara!
`` `

Então, aqui fizemos um objeto `john` com o método` sayHi`.

Muitos objetos incorporados já existem, como aqueles que funcionam com datas, erros, elementos HTML, etc. Eles têm diferentes propriedades e métodos.

Mas, esses recursos vêm com um custo!

Os objetos são "mais pesados" do que primitivos. Eles requerem recursos adicionais para suportar a maquinaria interna. Mas como propriedades e métodos são muito úteis na programação, os motores JavaScript tentam otimizá-los para reduzir a carga adicional.

## Um primitivo como um objeto

Aqui está o paradoxo enfrentado pelo criador de JavaScript:

- Há muitas coisas que se deseja fazer com uma primitiva como uma string ou um número. Seria ótimo acessá-los como métodos.
- Primitivas devem ser tão rápidas e leves quanto possível.

A solução parece um pouco estranha, mas aqui está:

1. Primitivas ainda são primitivas. Um valor único, conforme desejado.
2. O idioma permite o acesso a métodos e propriedades de strings, números, booleanos e símbolos.
3. Quando isso acontece, é criado um "wrapper de objeto" especial que fornece a funcionalidade extra e, em seguida, é destruído.

Os "wrappers de objeto" são diferentes para cada tipo primitivo e são chamados: `String`,` Number`, `Boolean` e` Symbol`. Assim, eles fornecem diferentes conjuntos de métodos.

Por exemplo, existe um método [str.toUpperCase ()] (https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) que retorna uma seqüência maiúscula.

Veja como funciona:

`` `js run
Deixe str = "Olá";

alerta (str.toUpperCase ()); // OLÁ
`` `

Simples, certo? Aqui está o que realmente acontece no `str.toUpperCase ()`:

1. A string `str` é uma primitiva. Portanto, no momento de acessar sua propriedade, é criado um objeto especial que conhece o valor da seqüência e possui métodos úteis, como `toUpperCase ()`.
2. Esse método é executado e retorna uma nova string (mostrada por `alert`).
3. O objeto especial é destruído, deixando o primitivo `str` sozinho.

Então, os primitivos podem fornecer métodos, mas eles ainda permanecem leves.

O motor de JavaScript altamente otimiza esse processo. Pode até ignorar a criação do objeto extra. Mas ainda deve aderir à especificação e se comportar como se criasse uma.

Um número tem métodos próprios, por exemplo, [toFixed (n)] (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) arredonda o número para a precisão dada:

`` `js run
deixe n = 1.23456;

alerta (n.toFixed (2)); // 1.23
`` `

Veremos métodos mais específicos nos capítulos <info: number> e <info: string>.


`` `` cabeçalho de aviso = "Construtores` String / Number / Boolean` são apenas para uso interno "
Algumas linguagens como o Java nos permitem criar "objetos wrapper" para primitivas usando explicitamente uma sintaxe como `new Number (1)` ou `new Boolean (false)`.

Em JavaScript, isso também é possível por razões históricas, mas altamente ** não recomendado **. As coisas ficam loucas em vários lugares.

Por exemplo:

`` `js run
alerta (tipo de 1); // "número"

alerta (typeof novo número (1)); // "objeto"!
`` `

E porque o que se segue, `zero`, é um objeto, o alerta aparecerá:

`` `js run
deixe zero = novo número (0);

se (zero) {// zero for verdadeiro, porque é um objeto
alerta ("zero is truthy?!?");
}
`` `

Por outro lado, usar as mesmas funções `String / Number / Boolean` sem` new` é uma coisa totalmente sã e útil. Eles convertem um valor para o tipo correspondente: para uma string, um número ou um booleano (primitivo).

Por exemplo, isso é inteiramente válido:
`` `js
deixe num = número ("123"); // converte uma string para o número
`` `
`` ``


`` `` warn header = "null / undefined have no methods"
As primitivas especiais 'nulo' e 'indefinido' são exceções. Eles não têm "objetos wrapper" correspondentes e não fornecem métodos. Em certo sentido, eles são "os mais primitivos".

Uma tentativa de acessar uma propriedade desse valor daria o erro:

`` `js run
alerta (null.test); // erro
`` ``

## Resumo

- Primitivas, exceto `null` e` undefined`, fornecem muitos métodos úteis. Estudaremos os próximos capítulos.
- Formalmente, esses métodos funcionam através de objetos temporários, mas os motores JavaScript estão bem ajustados para otimizar isso internamente, então eles não são caros de ligar.
