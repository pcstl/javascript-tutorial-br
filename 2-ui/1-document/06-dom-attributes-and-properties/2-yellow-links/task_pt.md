importância: 3

---

# Faça links externos de laranja

Faça todos os links externos de laranja alterando sua propriedade `style`.

Um link é externo se:
- É `href` tem`: // `nele
- Mas não começa com `http: // internal.com`.

Exemplo:

`` `html run
<a name="list"> a lista </a>
<Ul>
<li> <a href="http://google.com"> http://google.com </a> </ li>
<li> <a href="/tutorial"> /tutorial.html </a> </ li>
<li> <a href="local/path"> local / caminho </a> </ li>
<li> <a href="ftp://ftp.com/my.zip"> ftp://ftp.com/my.zip </a> </ li>
<li> <a href="http://nodejs.org"> http://nodejs.org </a> </ li>
<li> <a href="http://internal.com/test"> http://internal.com/test </a> </ li>
</ Ul>

<script>
// estilo de configuração para um único link
Deixe link = document.querySelector ('a');
link.style.color = 'orange';
</ script>
`` `

O resultado deve ser:

[iframe border = 1 height = 180 src = "solution"]
