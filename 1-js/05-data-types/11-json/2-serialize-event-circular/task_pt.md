importância: 5

---

# Excluir backreferences

Em casos simples de referências circulares, podemos excluir uma propriedade ofensiva da serialização por seu nome.

Mas, por vezes, há muitas referências. E os nomes podem ser usados ​​em referências circulares e propriedades normais.

Escreva a função `replace 'para restringir tudo, mas remova as propriedades que referem` meetup`:

`` `js run
deixe room = {
número: 23
};

Deixe meetup = {
Título: "Conferência",
ocupado por: [{nome: "João"}, {nome: "Alice"}],
lugar: sala
};

*! *
// referências circulares
room.occupiedBy = meetup;
meetup.self = meetup;
* /! *

alerta (JSON.stringify (meetup, substituição de função (chave, valor) {
/* seu código */
}));

/ * resultado deve ser:
{
"título": "Conferência",
"ocupado": [{"nome": "João"}, {"nome": "Alice"}],
"lugar": {"número": 23}
}
* /
`` `

