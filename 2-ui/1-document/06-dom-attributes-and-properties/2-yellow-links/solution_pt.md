
Primeiro, precisamos encontrar todas as referências externas.

Há duas maneiras.

O primeiro é encontrar todos os links usando `document.querySelectorAll ('a')` e depois filtrar o que precisamos:

`` `js
Deixe links = document.querySelectorAll ('a');

para (deixe o link dos links) {
*! *
Deixe href = link.getAttribute ('href');
* /! *
se (! href) continuar; // nenhum atributo

se (! href.includes (': //')) continue; // nenhum protocolo

se (href.startsWith ('http://internal.com')) continue; // interno

link.style.color = 'orange';
}
`` `

Por favor note: usamos `link.getAttribute ('href')`. Não `link.href`, porque precisamos do valor do HTML.

... Outra maneira mais simples seria adicionar as verificações ao seletor CSS:

`` `js
// procure todos os links que têm: // in href
// mas href não começa com http://internal.com
deixe setor = 'a [href * = ": //"]: not ([href ^ = "http://internal.com"])';
Deixe links = document.querySelectorAll (seletor);

links.forEach (link => link.style.color = 'orange');
`` `
