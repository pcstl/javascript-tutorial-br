# F.protótipo

No JavaScript moderno, podemos definir um protótipo usando `__proto__`, conforme descrito no artigo anterior. Mas não era assim o tempo todo.

[cortar]

O JavaScript teve herança prototípica desde o início. Foi uma das principais características do idioma.

Mas, nos velhos tempos, havia outra (e a única) maneira de configurá-lo: usar uma propriedade "protótipo" da função do construtor. E ainda há muitos scripts que o usam.

## A propriedade "protótipo"

Como já sabemos, `new F ()` cria um novo objeto.

Quando um novo objeto é criado com `new F ()`, o `[[Prototype]]` do objeto está definido como `F.prototype`.

Em outras palavras, se `F` tiver uma propriedade` prototype` com um valor do tipo de objeto, o operador `new` usará para configurar` [[Prototype]] `para o novo objeto.

Observe que `F.prototype` aqui significa uma propriedade regular denominada` "protótipo" `em` F`. Parece algo semelhante ao termo "protótipo", mas aqui realmente queremos dizer uma propriedade regular com esse nome.

Aqui está o exemplo:

`` `js run
deixe animal = {
come: true
};

função Coelho (nome) {
this.name = name;
}

*! *
Rabbit.prototype = animal;
* /! *

deixe coelho = coelho novo ("coelho branco"); // coelho .__ proto__ == animal

alerta (rabbit.eats); // verdade
`` `

Definir `Rabbit.prototype = animal` literalmente indica o seguinte:" Quando um `novo Rabbit` é criado, atribua o` [[Prototype]] `` `animal`".

Essa é a imagem resultante:

! [] (proto-constructor-animal-rabbit.png)

Na imagem, o "protótipo" é uma seta horizontal, é uma propriedade regular, e `[[Protótipo]] é vertical, ou seja, a herança de` coelho 'de' animal '.


## Padrão F.protótipo, propriedade do construtor

Toda função possui a propriedade "protótipo" mesmo se não fornecemos.

O "protótipo" padrão é um objeto com a única propriedade `construtor 'que retorna à própria função.

Como isso:

`` `js
função Rabbit () {}

/ * protótipo padrão
Rabbit.prototype = {constructor: Rabbit};
* /
`` `

! [] (function-prototype-constructor.png)

Podemos verificá-lo:

`` `js run
função Rabbit () {}
// por padrão:
// Rabbit.prototype = {constructor: Rabbit}

alerta (Rabbit.prototype.constructor == Rabbit); // verdade
`` `

Naturalmente, se não fizermos nada, a propriedade `constructor` está disponível para todos os coelhos através de` [[Prototype]] `:

`` `js run
função Rabbit () {}
// por padrão:
// Rabbit.prototype = {constructor: Rabbit}

deixe coelho = coelho novo (); // herda de {constructor: Rabbit}

alerta (coelho.constructor == Coelho); // verdadeiro (do protótipo)
`` `

! [] (rabbit-prototype-constructor.png)

Podemos usar a propriedade `constructor` para criar um novo objeto usando o mesmo construtor que o existente.

Como aqui:

`` `js run
função Coelho (nome) {
this.name = name;
alerta (nome);
}

deixe coelho = coelho novo ("coelho branco");

*! *
Deixe rabbit2 = novo coelho.constructor ("Black Rabbit");
* /! *
`` `

Isso é útil quando temos um objeto, não sabemos qual construtor foi usado para ele (por exemplo, vem de uma biblioteca de terceiros), e precisamos criar outro do mesmo tipo.

Mas provavelmente a coisa mais importante sobre "construtor" é que ...

** ... O JavaScript em si não garante o valor "construtor" correto. **

Sim, existe no "protótipo" padrão para funções, mas isso é tudo. O que acontece com isso mais tarde - está totalmente em nós.

Em particular, se substituirmos o protótipo padrão como um todo, então não haverá nenhum "construtor" nisso.

Por exemplo:

`` `js run
função Rabbit () {}
Rabbit.prototype = {
saltos: verdadeiro
};

deixe coelho = coelho novo ();
*! *
alerta (coelho.constructor === Coelho); // falso
* /! *
`` `

Então, para manter o "construtor" correto, podemos escolher adicionar / remover propriedades ao "protótipo" padrão em vez de substituí-lo como um todo:

`` `js
função Rabbit () {}

// Não substitua Rabbit.prototype totalmente
// apenas adicione a ele
Rabbit.prototype.jumps = true
// o Rabbit.prototype.constructor padrão é preservado
`` `

Ou, alternativamente, recrie a propriedade `constructor` manualmente:

`` `js
Rabbit.prototype = {
saltos: verdade
*! *
construtor: Coelho
* /! *
};

// agora o construtor também está correto, porque nós o adicionamos
`` `


## Resumo

Neste capítulo, descrevemos brevemente o modo de configurar um `[[Protótipo]] para objetos criados através de uma função de construtor. Mais tarde, veremos padrões de programação mais avançados que dependerão disso.

Tudo é bastante simples, apenas algumas notas para deixar as coisas claras:

- A propriedade `F.prototype` não é a mesma que` [[Prototype]] `. A única coisa que `F.prototype` faz: define` [[Prototype]] `de novos objetos quando` new F () `é chamado.
- O valor de `F.prototype` deve ser um objeto ou nulo: outros valores não funcionarão.
- A propriedade "protótipo" possui apenas um efeito especial quando é configurada para uma função de construtor e é invocada com `new`.

Em objetos regulares, o `protótipo 'não é nada especial:
`` `js
Deixe o usuário = {
Nome: "John",
protótipo: "Bla-bla" // nenhuma mágica
};
`` `

Por padrão, todas as funções têm `F.prototype = {constructor: F}`, para que possamos obter o construtor de um objeto ao acessar sua propriedade `` constructor ''.
