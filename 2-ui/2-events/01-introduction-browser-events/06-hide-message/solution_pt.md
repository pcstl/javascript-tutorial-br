
Para adicionar o botão, podemos usar `position: absolute` (e fazer o painel` position: relative`) ou `float: right`. O `float: right` tem o benefício de que o botão nunca se sobrepõe ao texto, mas` position: absolute` dá mais liberdade. Então a escolha é sua.

Então, para cada painel o código pode ser como:
`` `js
pane.insertAdjacentHTML ("afterbegin", '<button class = "remove-button"> [x] </ button>');
`` `

Então o `<botão>` se torna `pane.firstChild`, então podemos adicionar um manipulador assim:

`` `js
pane.firstChild.onclick = () => pane.remove ();
`` `
