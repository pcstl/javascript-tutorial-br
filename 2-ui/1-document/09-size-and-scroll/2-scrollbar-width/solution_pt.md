Para obter a largura da barra de rolagem, podemos criar um elemento com o pergaminho, mas sem bordas e preenchimentos.

Em seguida, a diferença entre a sua largura total `offsetWidth` e a largura da área de conteúdo interno` clientWidth` será exatamente a barra de rolagem:

`` `js run
// crie uma div com o pergaminho
deixe div = document.createElement ('div');

div.style.overflowY = 'scroll';
div.style.width = '50px';
div.style.height = '50px';

// deve colocá-lo no documento, caso contrário os tamanhos serão 0
document.body.append (div);
Deixe scrollWidth = div.offsetWidth - div.clientWidth;

div.remove ();

alerta (scrollWidth);
`` `
