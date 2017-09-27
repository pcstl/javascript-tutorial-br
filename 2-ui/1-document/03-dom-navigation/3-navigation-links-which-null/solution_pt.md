1. Sim, é verdade. O elemento `elem.lastChild` é sempre o último, não tem` nextSibling`, então, se houver filhos, então sim.
2. Não, errado, porque `elem.children [0]` é o primeiro filho entre os elementos. Mas pode haver nós não-elementos antes dele. Portanto, `previousSibling` pode ser um nó de texto.

Por favor, note que, para ambos os casos, se não houver filhos, haverá um erro. Por exemplo, se `elem.lastChild` for` null`, não podemos acessar `elem.lastChild.nextSibling`.
