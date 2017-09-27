
`` `js run
deixe room = {
número: 23
};

Deixe meetup = {
Título: "Conferência",
ocupado por: [{nome: "João"}, {nome: "Alice"}],
lugar: sala
};

room.occupiedBy = meetup;
meetup.self = meetup;

alerta (JSON.stringify (meetup, substituição de função (chave, valor) {
retorno (chave! = "" && value == meetup)? indefinido: valor;
}));

/ *
{
"título": "Conferência",
"ocupado": [{"nome": "João"}, {"nome": "Alice"}],
"lugar": {"número": 23}
}
* /
`` `

Aqui também precisamos testar `key ==" "para excluir a primeira chamada onde é normal que` value` seja `meetup`.

