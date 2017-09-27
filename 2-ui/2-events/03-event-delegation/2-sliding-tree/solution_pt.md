A solução tem duas partes.

1. Enrole cada título de nó de árvore em `<span>`. Então nós podemos estilo CSS em `: hover` e lidar com cliques exatamente no texto, porque a largura` <span> `é exatamente a largura do texto (ao contrário de não).
2. Defina um manipulador no nó raiz `árvore` e manipule os cliques nesses títulos` <span> `.
