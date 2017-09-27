Há várias maneiras, por exemplo:


O nó DOM `<div>`:

`` `js
document.body.firstElementChild
// ou
document.body.children [0]
// ou (o primeiro nó é espaço, então tomamos 2º)
document.body.childNodes [1]
`` `

O nó `<ul>` DOM:

`` `js
document.body.lastElementChild
// ou
document.body.children [1]
`` `

O segundo `<li>` (com Pete):

`` `js
// obtenha <ul>, e depois obtenha seu filho do último elemento
document.body.lastElementChild.lastElementChild
`` `
