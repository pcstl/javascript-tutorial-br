A escolha certa aqui é um `WeakSet`:

`` `js
deixe mensagens = [
{texto: "Olá", de: "John"},
{texto: "Como vai?", de: "John"},
{text: "Até breve", de: "Alice"}
];

deixe readMessages = new WeakSet ();

// duas mensagens foram lidas
readMessages.add (mensagens [0]);
readMessages.add (mensagens [1]);
// readMessages tem 2 elementos

// ... vamos ler a primeira mensagem novamente!
readMessages.add (mensagens [0]);
// readMessages ainda possui 2 elementos exclusivos

// resposta: a mensagem [0] foi lida?
alerta ("Leia a mensagem 0:" + readMessages.has (mensagens [0])); // verdade

messages.shift ();
// agora readMessages tem 1 elemento (tecnicamente, a memória pode ser limpa mais tarde)
`` `

O `WeakEst` permite armazenar um conjunto de mensagens e verificar facilmente a existência de uma mensagem nele.

Ele se limpa automaticamente. A compensação é que não podemos repetir sobre isso. Não podemos obter "todas as mensagens lidas" diretamente. Mas podemos fazê-lo, iterando sobre todas as mensagens e filtrando aqueles que estão no conjunto.

P.S. Adicionar uma propriedade nossa a cada mensagem pode ser perigoso se as mensagens forem gerenciadas pelo código de outra pessoa, mas podemos torná-lo um símbolo para evitar conflitos.

Como isso:
`` `js
// a propriedade simbólica é conhecida apenas pelo nosso código
deixa isRead = Symbol ("isRead");
mensagens [0] [isRead] = true;
`` `

Agora, mesmo se o código de outra pessoa usar o `for..in 'loop para propriedades da mensagem, nossa bandeira secreta não aparecerá.
