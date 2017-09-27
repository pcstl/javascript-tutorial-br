# Protótipos Nativos

A propriedade "protótipo" é amplamente utilizada pelo núcleo do próprio JavaScript. Todas as funções incorporadas do construtor o usam.

Veremos como é para objetos simples primeiro e, em seguida, para outros mais complexos.

## Object.prototype

Digamos que nós emitimos um objeto vazio:

`` `js run
deixe obj = {};
alerta (obj); // "[Object Object]"?
`` `

Onde está o código que gera a string `" [object Object] "`? Esse é um método incorporado `toString`, mas onde é? O `obj` está vazio!

... Mas a notação curta `obj = {}` é a mesma coisa que `obj = new Object ()`, onde `Object` - é uma função incorporada do construtor de objetos. E essa função tem `Object.prototype` que faz referência a um objeto enorme com` toString` e outras funções.

Assim (tudo isso é incorporado):

! [] (object-prototype.png)

Quando `new Object ()` é chamado (ou um objeto literal `{...}` é criado), o `[[Prototype]] 'é definido como` Object.prototype` pela regra que temos discutido no capítulo anterior:

! [] (object-prototype-1.png)

Depois, quando `obj.toString ()` é chamado - o método é tirado de `Object.prototype`.

Podemos verificá-lo assim:

`` `js run
deixe obj = {};

alerta (obj .__ proto__ === Object.prototype); // verdade
// obj.toString === obj .__ proto __. toString == Object.prototype.toString
`` `

Observe que não existe nenhum `[[Protótipo]] na cadeia acima` Object.prototype`:

`` `js run
alerta (Object.prototype .__ proto__); // nulo
`` `

## Outros protótipos internos

Outros objetos embutidos, como "Array", "Data", "Função" e outros também mantêm métodos em protótipos.

Por exemplo, quando criamos uma matriz `[1, 2, 3]`, o construtor 'novo Array () `padrão é usado internamente. Portanto, os dados da matriz são escritos no novo objeto e o `Array.prototype` torna-se seu protótipo e fornece métodos. Isso é muito eficiente em termos de memória.

Por especificação, todos os protótipos embutidos têm "Object.prototype" no topo. Às vezes, as pessoas dizem que "tudo herda dos objetos".

Aqui está a imagem geral (para 3 built-ins para caber):

! [] (native-prototypes-classes.png)

Vamos verificar os protótipos manualmente:

`` `js run
deixe arr = [1, 2, 3];

// herda do Array.prototype?
alerta (arr .__ proto__ === Array.prototype); // verdade

// então do Object.prototype?
alerta (arr.__ proto __.__ proto__ === Object.prototype); // verdade

// e nulo no topo.
alerta (arr.__ portanto __.__ portanto __.__ proto__); // nulo
`` `

Alguns métodos em protótipos podem se sobrepor, por exemplo, `Array.prototype` tem o seu próprio` toString` que lista elementos delimitados por vírgulas:

`` `js run
deixe arr = [1, 2, 3]
alerta (arr); // 1,2,3 <- o resultado de Array.prototype.toString
`` `

Como já vimos antes, o `Object.prototype` também tem` toString ', mas `Array.prototype` está mais próximo da cadeia, então a variante da matriz é usada.


! [] (native-prototypes-array-tostring.png)


As ferramentas no navegador, como o console do desenvolvedor do Chrome, também mostram herança (talvez seja necessário usar `console.dir` para objetos embutidos):

!] [] (console_dir_array.png)

Outros objetos embutidos também funcionam da mesma maneira. Mesmo funções. Eles são objetos de um construtor 'Function` incorporado e seus métodos: `call / apply` e outros são tirados de` Function.prototype`. As funções também têm "toString".

`` `js run
função f () {}

alerta (f .__ proto__ == Function.prototype); // verdade
alerta (f .__ proto __.__ proto__ == Object.prototype); // verdadeiro, herdar de objetos
`` `

## Primitivas

A coisa mais intrincada acontece com cordas, números e booleanos.

Como lembramos, eles não são objetos. Mas se tentarmos acessar suas propriedades, os objetos temporários do wrapper são criados usando construtores incorporados `String`,` Number`, `Boolean`, eles fornecem os métodos e desaparecem.

Esses objetos são criados invisivelmente para nós e a maioria dos mecanismos o otimiza, mas a especificação descreve exatamente dessa maneira. Os métodos desses objetos também residem em protótipos, disponíveis como `String.prototype`,` Number.prototype` e `Boolean.prototype`.

`` `warn header =" Values ​​`null` e` undefined` não têm wrappers de objeto "
Valores especiais "nulos" e "indefinidos" estão separados. Eles não têm invólucros de objetos, portanto, métodos e propriedades não estão disponíveis para eles. E também não há protótipos correspondentes.
`` `

## Alterando protótipos nativos [# native-prototype-change]

Os protótipos nativos podem ser modificados. Por exemplo, se adicionarmos um método a `String.prototype`, ele fica disponível para todas as seqüências de caracteres:

`` `js run
String.prototype.show = function () {
alerta (isto);
};

"BOOM!". Show (); // ESTRONDO!
`` `

Durante o processo de desenvolvimento, podemos ter ideias sobre os novos métodos internos que gostaríamos de ter. E pode haver uma pequena tentação de adicioná-los aos protótipos nativos. Mas isso geralmente é uma má idéia.

Os protótipos são globais, por isso é fácil conseguir um conflito. Se duas bibliotecas adicionar um método `String.prototype.show`, então uma delas substitui a outra.

Na programação moderna, há apenas um caso ao modificar os protótipos nativos. Isso é polyfills. Em outras palavras, se houver um método em especificação de JavaScript que ainda não seja suportado pelo nosso mecanismo de JavaScript (ou qualquer um dos que queremos suportar), então pode implementá-lo manualmente e preencher o protótipo interno com ele.

Por exemplo:

`` `js run
se (! String.prototype.repeat) {// se não existe tal método
// adicione-o ao protótipo

String.prototype.repeat = function (n) {
// repete a string n vezes

// na verdade, o código deve ser mais complexo do que isso,
// lança erros para valores negativos de "n"
// o algoritmo completo está na especificação
retornar nova matriz (n + 1). juntar (esta);
};
}

alerta ("La" .repeat (3)); // LaLaLa
`` `

# Empréstimo de protótipos

No capítulo <info: call-apply-decorators # method-borrowing> falamos sobre empréstimo de método:

`` `js run
função showArgs () {
*! *
// emprestar juntar-se da matriz e chamar no contexto dos argumentos
alerta ([] .join.call (argumentos, "-"));
* /! *
}

showArgs ("John", "Pete", "Alice"); // John - Pete - Alice
`` `

Porque `join` reside em` Array.prototype`, podemos chamá-lo de lá diretamente e reescrevê-lo como:

`` `js
função showArgs () {
*! *
alerta (Array.prototype.join.call (argumentos, "-"));
* /! *
}
`` `

Isso é mais eficiente, porque evita a criação de um objeto de matriz extra `[]`. Por outro lado, é mais longo para escrever.

## Resumo

- Todos os objetos embutidos seguem o mesmo padrão:
- Os métodos são armazenados no protótipo (`Array.prototype`,` Object.prototype`, `Date.prototype` etc.).
- O próprio objeto armazena apenas os dados (itens da matriz, propriedades do objeto, a data).
- Primitivos também armazenam métodos em protótipos de objetos wrapper: `Number.prototype`,` String.prototype`, `Boolean.prototype`. Não há objetos wrapper apenas para `undefined` e` null`.
- Os protótipos internos podem ser modificados ou preenchidos com novos métodos. Mas não é recomendável alterá-los. Provavelmente, a única causa admissível é quando adicionamos um novo padrão, mas ainda não suportado pelo método JavaScript do mecanismo.
