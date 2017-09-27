Quando o navegador lê o atributo 'on * `como' onclick`, ele cria o manipulador de seu conteúdo.

Para 'onclick = "handler ()" `a função será:

`` `js
função (evento) {
manipulador () // o conteúdo do onclick
}
`` `

Agora, podemos ver que o valor retornado pelo `handler ()` não é usado e não afeta o resultado.

A correção é simples:

`` `html run
<script>
manipulador de função () {
alerta("...");
retornar falso;
}
</ script>

<a href="http://w3.org" onclick="*!**********************************************************
`` `

Também podemos usar `event.preventDefault ()`, assim:

`` `html run
<script>
*! *
manipulador de função (evento) {
alerta("...");
event.preventDefault ();
}
* /! *
</ script>

<a href="http://w3.org" onclick="*!*handler(event)*/!*"> w3.org </a>
`` `
