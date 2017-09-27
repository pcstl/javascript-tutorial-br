A solução é:

`` `js
Deixe scrollBottom = element.scrollHeight - element.scrollTop - elem.clientHeight;
`` `

Em outras palavras: (altura total) menos (parte superior deslocada) menos (parte visível) - é exatamente a parte inferior deslocada.
