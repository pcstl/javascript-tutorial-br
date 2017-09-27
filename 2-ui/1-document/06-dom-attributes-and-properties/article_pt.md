# Atributos e propriedades

Quando o navegador carrega a página, ele "lê" (outra palavra: "analisa") texto HTML e gera objetos DOM a partir dele. Para nós de elementos, a maioria dos atributos HTML padrão se tornam automaticamente propriedades de objetos DOM.

Por exemplo, se a tag for `<body id =" page ">`, então o objeto DOM tem `body.id =" page "`.

Mas o mapeamento de atributo-propriedade não é um para um! Neste capítulo, iremos prestar atenção para separar estas duas noções, para ver como trabalhar com elas, quando elas são iguais e quando elas são diferentes.

[cortar]

# Propriedades DOM

Já vimos propriedades de DOM incorporadas. Há muito. Mas tecnicamente ninguém nos limita, e se não é suficiente - podemos adicionar o nosso.

Os nós DOM são objetos JavaScript regulares. Podemos alterá-los.

Por exemplo, vamos criar uma nova propriedade em `document.body`:

`` `js run
document.body.myData = {
Nome: 'César',
título: 'Imperator'
};


`` `

Podemos também adicionar um método:

`` `js run
document.body.sayHi = function () {
alerta (this.tagName);
};

document.body.sayHi (); // BODY (o valor de "this" no método é document.body)
`` `

Podemos também modificar protótipos internos como `Element.prototype` e adicionar novos métodos a todos os elementos:

`` `js run
Element.prototype.sayHi = function () {
alerta ('Olá, eu sou $ {this.tagName} `);
};

document.documentElement.sayHi (); // Olá, eu sou HTML
document.body.sayHi (); // Olá, eu sou CORPO
`` `

Assim, as propriedades e métodos do DOM se comportam exatamente como os de objetos JavaScript comuns:

- Eles podem ter algum valor.
- Eles distinguem maiúsculas de minúsculas (escreva `elem.nodeType`, não` elem.NoDeTyPe`).

## atributos HTML

Em linguagem HTML, as tags podem ter atributos. Quando o navegador lê texto HTML e cria objetos DOM para tags, ele reconhece os atributos * standard * e cria propriedades do DOM deles.

Então, quando um elemento tem `id` ou outro * atributo * padrão *, a propriedade correspondente é criada. Mas isso não acontece se o atributo não for padrão.

Por exemplo:
`` `html run
<body id = "test" something = "non-standard">
<script>

*! *
// atributo não padrão não produz uma propriedade

* /! *
</ script>
</ body>
`` `

Observe que um atributo padrão para um elemento pode ser desconhecido para outro. Por exemplo, `" type "` é padrão para `<input>` ([HTMLInputElement] (https://html.spec.whatwg.org/#htmlinputelement)), mas não para `<body>` ([HTMLBodyElement] (https://html.spec.whatwg.org/#htmlbodyelement)). Atributos padrão são descritos na especificação para a classe de elemento correspondente.

Aqui podemos ver:
`` `html run
<body id = "body" type = "...">
<input id = "input" type = "text">
<script>
alerta (input.type); // texto
*! *
alerta (tipo body); // indefinido: propriedade DOM não criada, porque não é padrão
* /! *
</ script>
</ body>
`` `

Então, se um atributo não for padrão, não haverá propriedade DOM para isso. Existe uma maneira de acessar esses atributos?

Certo. Todos os atributos estão acessíveis usando os seguintes métodos:

- `elem.hasAttribute (name)` - verifica a existência.
- `elem.getAttribute (name)` - obtém o valor.
- `elem.setAttribute (name, value)` - define o valor.
- `elem.removeAttribute (name)` - remove o atributo.

Esses métodos funcionam exatamente com o que está escrito em HTML.

Também é possível ler todos os atributos usando `elem.attributes`: uma coleção de objetos pertencentes a uma classe incorporada [Attr] (https://dom.spec.whatwg.org/#attr), com` nome` e propriedades de "valor".

Aqui está uma demonstração de leitura de uma propriedade não padrão:

`` `html run
<body something = "non-standard">
<script>
*! *

* /! *
</ script>
</ body>
`` `

Os atributos HTML possuem os seguintes recursos:

- O nome deles é insensível a maiúsculas e minúsculas (isto é HTML: `id` é o mesmo que` ID`).
- São sempre cordas.

Aqui está uma demo detalhada de trabalhar com atributos:

`` `html run
<corpo>
<div id = "elem" sobre = "Elefante"> </ div>

<script>
alerta (elem.getAttribute ('About')); // (1) 'Elefante', lendo

elem.setAttribute ('Test', 123); // (2), escrevendo

alerta (elem.outerHTML); // (3), veja que está lá

para (deixe attr de element.attributes) {// (4) listar todos
alerta (attr.name + "=" + attr.value);
}
</ script>
</ body>
`` `

Observe:

1. `getAttribute ('About')` - a primeira letra é maiúscula aqui, e em HTML é tudo em minúsculas. Mas isso não importa: nomes de atributos são sensíveis a maiúsculas e minúsculas.
2. Podemos atribuir qualquer coisa a um atributo, mas isso se torna uma string. Então, aqui temos "123" como o valor.
3. Todos os atributos, incluindo os que definimos, são visíveis no `outerHTML`.
4. A coleção `attributes` é iterável e tem todos os atributos com` nome` e `valor`.

## Sincronização de atributo de propriedade

Quando um atributo padrão muda, a propriedade correspondente é atualizada automaticamente e (com algumas exceções) vice-versa.

No exemplo abaixo, `id` é modificado como um atributo, e também podemos ver a alteração de propriedade. E então, o mesmo para trás:

`` `html run
<input>

<script>
deixe input = document.querySelector ('input');

// atributo => propriedade
input.setAttribute ('id', 'id');
alerta (input.id); // id (atualizado)

// property => attribute
input.id = 'newId';
alerta (input.getAttribute ('id')); // newId (atualizado)
</ script>
`` `

Mas há exclusões, por exemplo, `input.value` sincroniza apenas de atributo -> para propriedade, mas não de volta:

`` `html run
<input>

<script>
deixe input = document.querySelector ('input');

// atributo => propriedade
input.setAttribute ('value', 'text');
alerta (input.value); // texto

*! *
// NÃO propriedade => atributo
input.value = 'newValue';
alerta (input.getAttribute ('value')); // texto (não atualizado!)
* /! *
</ script>
`` `

No exemplo acima:
- Alterar o atributo `value` atualiza a propriedade.
- Mas a alteração de propriedade não afeta o atributo.

Esse "recurso" pode realmente ser útil, porque o usuário pode modificar o "valor", e depois, se quisermos recuperar o valor "original" do HTML, está no atributo.

## propriedades DOM são digitadas

As propriedades do DOM nem sempre são strings. Por exemplo, a propriedade `input.checked` (para caixas de seleção) é booleana:

`` `html run
<input id = "input" type = "checkbox" checked> checkbox

<script>
alerta (input.getAttribute ('checked')); // o valor do atributo é: string vazia
alerta (input.checked); // o valor da propriedade é: true
</ script>
`` `

Existem outros exemplos. O atributo `style` é uma string, mas a propriedade` style` é um objeto:

`` `html run
<div id = "div" style = "color: red; font-size: 120%"> Olá </ div>

<script>
// corda
alerta (div.getAttribute ('style')); // cor: vermelho; tamanho da fonte: 120%

// objeto
alerta (div.style); // [objeto CSSStyleDeclaration]
alerta (div.style.color); // vermelho
</ script>
`` `

Essa é uma diferença importante. Mas mesmo que um tipo de propriedade DOM seja uma string, pode diferir do atributo!

Por exemplo, a propriedade DOM `href` é sempre um URL * completo *, mesmo que o atributo contenha um URL relativo ou apenas um` # hash`.

Aqui está um exemplo:

`` `html height = 30 run
<a id="a" href="#hello"> link </a>
<script>
// atributo
alerta (a.getAttribute ('href')); // #Olá

// propriedade
alerta (a.href); // URL completo no formulário http://site.com/page#hello
</ script>
`` `

Se precisamos do valor de `href` ou de qualquer outro atributo exatamente como escrito no HTML, podemos usar` getAttribute`.


## Atributos não-padrão, conjunto de dados

Ao escrever HTML, usamos muitos atributos padrão. Mas e os não padronizados e personalizados? Primeiro, vamos ver se eles são úteis ou não? Pelo que?

Às vezes, os atributos não padrão são usados ​​para passar dados personalizados de HTML para JavaScript ou para "marcar" elementos HTML para JavaScript.

Como isso:

`` `html run
<! - marque a div para mostrar "nome" aqui ->
<div *! * show-info = "name" * /! *> </ div>
<! - e idade aqui ->
<div *! * show-info = "age" * /! *> </ div>

<script>
// o código encontra um elemento com a marca e mostra o que é solicitado
Deixe o usuário = {
Nome: "Pete",
idade: 25
};

para (deixe div de document.querySelectorAll ('[show-info]')) {
// insira a informação correspondente no campo
deixe field = div.getAttribute ('show-info');
div.innerHTML = user [field]; // Pete, então idade
}
</ script>
`` `

Também podem ser usados ​​para modelar um elemento.

Por exemplo, aqui para o estado da ordem, o atributo `order-state` é usado:

`` `html run
<style>
/ * estilos dependem do atributo personalizado "order-state" * /
.order [order-state = "new"] {
cor: verde;
}

.order [order-state = "pending"] {
cor azul;
}

.order [order-state = "cancelado"] {
cor vermelha;
}
</ style>

<div class = "order" order-state = "new">
Uma nova ordem.
</ div>

<div class = "order" order-state = "pending">
Uma ordem pendente.
</ div>

<div class = "order" order-state = "cancelado">
Um pedido cancelado.
</ div>
`` `

Por que o atributo pode ser preferível a classes como `.order-state-new`,` .order-state-pending`, `order-state-canceled`?

Isso porque um atributo é mais conveniente para gerenciar. O estado pode ser alterado tão fácil como:

`` `js
// um pouco mais simples do que remover antigo / adicionar uma nova classe
div.setAttribute ('order-state', 'cancelado');
`` `

Mas pode haver um problema possível com atributos personalizados. E se usarmos um atributo não padrão para nossos propósitos e depois o padrão o introduzir e faz com que ele faça algo? A linguagem HTML está viva, ela cresce, mais atributos parecem adequar-se às necessidades dos desenvolvedores. Pode haver efeitos inesperados nesse caso.

Para evitar conflitos, existem atributos [data- *] (https://html.spec.whatwg.org/#embedding-custom-non-visible-data-with-the-data-*-attributes).

** Todos os atributos que começam com "dados-" são reservados para o uso dos programadores. Eles estão disponíveis na propriedade `dataet`. **

Por exemplo, se um `elemento 'tiver um atributo chamado` "dados" sobre "`, ele está disponível como `elem.dataset.about`.

Como isso:

`` `html run
<body data-about = "Elephants">
<script>

</ script>
`` `

Os atributos Multiword como `data-order-state` tornam-se camel-encapsulados:` dataset.orderState`.

Aqui está um exemplo de "estado de ordem" reescrito:

`` `html run
<style>
.order [data-order-state = "new"] {
cor: verde;
}

.order [data-order-state = "pending"] {
cor azul;
}

.order [data-order-state = "cancelado"] {
cor vermelha;
}
</ style>

<div id = "order" class = "order" data-order-state = "new">
Uma nova ordem.
</ div>

<script>
// ler
alerta (order.dataset.orderState); // Novo

// modifica
order.dataset.orderState = "pending"; // (*)
</ script>
`` `

Usar atributos `data- *` é uma maneira válida e segura de passar dados personalizados.

Por favor, note que não podemos apenas ler, mas também modificar atributos de dados. Então o CSS atualiza a visualização de acordo: no exemplo acima, a última linha `(*)` altera a cor para o azul.

## Resumo

- Atributos - é o que está escrito em HTML.
- Propriedades - é o que está em objetos DOM.

Uma pequena comparação:

| | Propriedades | Atributos |
| ------------ | ------------ | ------------ |
| Tipo | Qualquer valor, as propriedades padrão possuem tipos descritos na seqüência de especificação | A |
| Nome | Nome é sensível a maiúsculas e minúsculas | O nome não diferencia maiúsculas de minúsculas |

Métodos para trabalhar com atributos são:

- `elem.hasAttribute (name)` - para verificar a existência.
- `elem.getAttribute (name)` - para obter o valor.
- `elem.setAttribute (nome, valor)` - para definir o valor.
- `elem.removeAttribute (name)` - para remover o atributo.
- `elem.attributes` é uma coleção de todos os atributos.

Para a maioria das necessidades, as propriedades DOM podem nos servir bem. Devemos nos referir apenas aos atributos somente quando as propriedades do DOM não nos atendem, quando precisamos exatamente de atributos, por exemplo:

- Precisamos de um atributo não padrão. Mas se ele começar com `data-`, então devemos usar `dataset`.
- Queremos ler o valor "como escrito" em HTML. O valor da propriedade DOM pode ser diferente, por exemplo, a propriedade `href` é sempre um URL completo, e podemos querer obter o valor" original ".
