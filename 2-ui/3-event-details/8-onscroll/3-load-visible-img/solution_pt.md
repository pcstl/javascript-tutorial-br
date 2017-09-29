O manipulador `onscroll` deve verificar quais imagens são visíveis e mostrá-las.

Também podemos querer executá-lo quando a página carregar, detectar imagens visíveis imediatamente antes de qualquer rolagem e carregá-las.

Se colocarmos na parte inferior `<body>`, então ela é executada quando o conteúdo da página é carregado.

`` `js
// ... o conteúdo da página está acima ...

função isVisible (elem) {

let coords = elem.getBoundingClientRect ();

deixe windowHeight = document.documentElement.clientHeight;

// a borda do topo superior está visível ou a borda inferior do elem está visível
Deixe topVisible = coords.top> 0 && coords.top <windowHeight;
Deixe bottomVisible = coords.bottom <windowHeight && coords.bottom> 0;

voltar topVisível || bottomVisible;
}

*! *
showVisible ();
window.onscroll = showVisible;
* /! *
`` `

Para imagens visíveis, podemos pegar `img.dataset.src` e atribuí-lo a` img.src` (se não o fizesse ainda).

P.S. A solução também possui uma variante de `isVisible` que" carrega pré-cargas "imagens que estão dentro de 1 página acima / abaixo (a altura da página é` document.documentElement.clientHeight`).
