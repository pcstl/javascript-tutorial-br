A resposta: ** `BODY` **.

`` `html run
<script>
deixe body = document.body;

body.innerHTML = "<! -" + body.tagName + "->";

alerta (body.firstChild.data); // CORPO
</ script>
`` `

O que está acontecendo passo a passo:

1. O conteúdo de `<body>` é substituído pelo comentário. O comentário é <code> & lt;! - BODY - & gt; </ code>, porque `body.tagName ==" BODY "`. Como lembramos, `tagName` é sempre maiúscula em HTML.
2. O comentário é agora o único nó filho, então nós o entendemos no `body.firstChild`.
3. A propriedade `data` do comentário é o seu conteúdo (dentro de` <! - ....--> `):` "BODY" `.
