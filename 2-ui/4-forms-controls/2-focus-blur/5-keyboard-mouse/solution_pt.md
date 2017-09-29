
Podemos usar `mouse.onclick` para lidar com o clique e fazer o mouse" móvel "com` position: fixed`, então então `mouse.keydown` para lidar com as teclas de seta.

A única armadilha é que o "keydown" desencadeia somente elementos com foco. Então, precisamos adicionar `tabindex` ao elemento. Como estamos proibidos de alterar o HTML, podemos usar a propriedade `mouse.tabIndex` para isso.

P.S. Também podemos substituir `mouse.click` por` mouse.onfocus`.
