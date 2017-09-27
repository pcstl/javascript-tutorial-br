importância: 3

---

# Por que "return false" não funciona?

Por que no código abaixo `return false` não funciona?

`` `html execução automática
<script>
manipulador de função () {
alerta( "..." );
retornar falso;
}
</ script>

<a href="http://w3.org" onclick="handler()"> o navegador irá para w3.org </a>
`` `

O navegador segue o URL clicando, mas não o queremos.

Como consertar?
