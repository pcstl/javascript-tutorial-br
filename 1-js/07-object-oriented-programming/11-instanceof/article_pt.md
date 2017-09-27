# Verificação de classe: "instanceof"

O operador `instanceof` permite verificar se um objeto pertence a uma determinada classe. Também leva em conta a herança.

Tal verificação pode ser necessária em muitos casos, aqui vamos usá-lo para construir uma * função polimórfica *, a qual trata os argumentos de maneira diferente, dependendo do tipo deles.

[cortar]

## The instanceof operator [# ref-instanceof]

A sintaxe é:
`` `js
Obj instanceof Class
`` `

Retorna `true` se` obj` pertence à `Classe` (ou uma classe que herda dela).

Por exemplo:

`` `js run
classe Rabbit {}
deixe coelho = coelho novo ();

// é um objeto da classe Rabbit?
*! *
alerta (coelho instanceof Rabbit); // verdade
* /! *
`` `

Também funciona com as funções do construtor:

`` `js run
*! *
// em vez de classe
função Rabbit () {}
* /! *

alerta (novo Coelho () instância de coelho); // verdade
`` `

... E com classes embutidas como `Array`:

`` `js run
deixe arr = [1, 2, 3];
alerta (arr instanceof Array); // verdade
alerta (arr instanceof Object); // verdade
`` `

Por favor, note que `arr` também pertence à classe` Object`. Isso porque o `Array` herdeia prototípica de` Object`.

O operador `instanceof` examina a cadeia de protótipo para a verificação e também é ajustável usando o método estático` Symbol.hasInstance`.

O algoritmo de `obj instanceof Class` funciona aproximadamente da seguinte forma:

1. Se houver um método estático `Symbol.hasInstance`, use-o. Como isso:

`` `js run
// assume qualquer coisa que canEat é um animal
classe Animal {
estática [Symbol.hasInstance] (obj) {
se (obj.canEat) retornar verdadeiro;
}
}

deixe obj = {canEat: true};
alerta (obj instanceof Animal); // true: Animal [Symbol.hasInstance] (obj) é chamado
`` `

2. A maioria das classes não tem `Symbol.hasInstance`. Nesse caso, verifique se `Class.prototype` é igual a um dos protótipos na cadeia de protótipo 'obj`.

Em outras palavras, compare:
`` `js
obj .__ proto__ == Class.prototype
obj .__ proto __.__ proto__ == Class.prototype
portanto __.__ portanto __.__ proto__ == Class.prototype
...
`` `

No exemplo acima `Rabbit.prototype == coelho .__ proto__`, de modo que dá a resposta imediatamente.

No caso de uma herança, `rabbit` também é uma instância da classe pai:

`` `js run
Classe Animal {}
classe Rabbit extends Animal {}

deixe coelho = coelho novo ();
*! *
alerta (caso de coelho de Animal); // verdade
* /! *
// coelho .__ proto__ == Rabbit.prototype
// coelho .__ proto __.__ proto__ == Animal.prototype (match!)
`` `

Aqui está a ilustração do que `bunbit instanceof Animal` se compara com` Animal.prototype`:

! [] (instanceof.png)

Por sinal, também existe um método [objA.isPrototypeOf (objB)] (mdn: js / object / isPrototypeOf), que retorna `true` se` objA` estiver em algum lugar na cadeia de protótipos para `objB`. Portanto, o teste de `obj instanceof Class` pode ser reformulado como 'Class.prototype.isPrototypeOf (obj)`.

Isso é engraçado, mas o construtor `Class` não participa no cheque! Apenas a cadeia de protótipos e `Class.prototype` são importantes.

Isso pode levar a consequências interessantes quando o `protótipo 'é alterado.

Como aqui:

`` `js run
função Rabbit () {}
deixe coelho = coelho novo ();

// mudou o protótipo
Rabbit.prototype = {};

// ... não é um coelho mais!
*! *
alerta (coelho instanceof Rabbit); // falso
* /! *
`` `

Essa é uma das razões para evitar a mudança de "protótipo". Apenas para manter a segurança.

## Bonus: Object toString para o tipo

Nós já sabemos que objetos simples são convertidos em seqüência de caracteres como '[Objeto objeto] `:

`` `js run
deixe obj = {};

alerta (obj); // [Object Object]
alerta (obj.toString ()); // o mesmo
`` `

Essa é a implementação do `toString`. Mas há um recurso oculto graças a `` toString` realmente muito mais poderoso do que isso. Podemos usá-lo como "typeof" e uma alternativa para `instanceof`.

Soa estranho? De fato. Vamos desmistificar.

Por [especificação] (https://tc39.github.io/ecma262/#sec-object.prototype.tostring), o `toString 'integrado pode ser extraído do objeto e executado no contexto de qualquer outro valor. E o seu resultado depende desse valor.

- Para um número, será `[número do objeto]`
- Para um booleano, será `[object Boolean]`
- Para `null`:` [object Null] `
- Para `undefined`:` [object Undefined] `
- Para arrays: `[object Array]`
- ... etc (customizável).

Vamos demonstrar:

`` `js run
// copiar o método toString para uma variável por conveniência
deixe objectToString = Object.prototype.toString;

// de que tipo é esse?
deixe arr = [];

alerta (objectToString.call (arr)); // [Object Array]
`` `

Aqui usamos [call] (mdn: js / function / call) conforme descrito no capítulo [] (info: call-apply-decorators) para executar a função `objectToString` no contexto` this = arr`.

Internamente, o algoritmo `toString` examina` this` e retorna o resultado correspondente. Mais exemplos:

`` `js run
deixe s = Object.prototype.toString;

alerta (s.call (123)); // [número do objeto]
alerta (s.call (null)); // [object Null]
alerta (s.call (alerta)); // [função do objeto]
`` `

### Symbol.toStringTag

O comportamento do Object `toString` pode ser personalizado usando uma propriedade de objeto especial` Symbol.toStringTag`.

Por exemplo:

`` `js run
Deixe o usuário = {
[Symbol.toStringTag]: 'Usuário'
};

alerta ({} .toString.call (usuário)); // [objeto Usuário]
`` `

Para a maioria dos objetos específicos do ambiente, existe tal propriedade. Aqui estão alguns exemplos específicos do navegador:

`` `js run
// toStringTag para o objeto e a classe específicos do ambiente:
alerta (janela [Symbol.toStringTag]); // janela
alerta (XMLHttpRequest.prototype [Symbol.toStringTag]); // XMLHttpRequest

alerta ({} .toString.call (janela)); // [janela do objeto]
alerta ({} .toString.call (novo XMLHttpRequest ())); // [objeto XMLHttpRequest]
`` `

Como você pode ver, o resultado é exatamente `Symbol.toStringTag` (se existir), envolvido em` [object ...] `.

No final, temos "tipo de esteróides" que não só funciona para tipos de dados primitivos, mas também para objetos embutidos e até mesmo podem ser personalizados.

Ele pode ser usado em vez de `instanceof` para objetos internos quando queremos obter o tipo como uma seqüência de caracteres em vez de apenas para verificar.

## Resumo

Vamos recapitular os métodos de verificação de tipo que conhecemos:

| | funciona para | retorna |
| --------------- | ------------- | --------------- |
| `typeof` | primitivas | corda |
| `{} .toString` | primitivas, objetos embutidos, objetos com `Symbol.toStringTag` | corda |
| `instanceof` | objetos | verdadeiro / falso |

Como podemos ver, `{} .toString` é tecnicamente um" tipoof "mais avançado.

E o operador `instanceof` realmente brilha quando trabalhamos com uma hierarquia de classes e queremos verificar a classe tendo em conta a herança.
