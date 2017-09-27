Há muitas maneiras de fazê-lo.

Aqui estão alguns deles:

`` `js
// 1. A tabela com `id =" age-table "`.
Deixe table = document.getElementById ('age-table')

// 2. Todos os elementos do rótulo dentro dessa tabela
table.getElementsByTagName ('label')
// ou
document.querySelectorAll ('# etiqueta da idade-tabela')

// 3. O primeiro td nessa tabela (com a palavra "Idade").
table.rows [0] .cells [0]
// ou
table.getElementsByTagName ('td') [0]
// ou
table.querySelector ('td')

// 4. O formulário com o nome "pesquisa".
// assumindo que há apenas um elemento com name = "search"
Deixe form = document.getElementsByName ('search') [0]
// ou, especificamente
document.querySelector ('form [name = "search"]')

// 5. A primeira entrada nesse formulário.
form.getElementsByTagName ('input')
// ou
form.querySelector ('input')

// 6. A última entrada nesse formulário.
// não há uma consulta direta para isso
Deixe input = form.querySelectorAll ('input') // procure todos
entradas [inputs.length-1] // pegue o último
`` `
