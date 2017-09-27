
Para armazenar uma data, podemos usar `WeakMap`:

`` `js
deixe mensagens = [
{texto: "Olá", de: "John"},
{texto: "Como vai?", de: "John"},
{text: "Até breve", de: "Alice"}
];

deixe readMap = novo WeakMap ();

readMap.set (mensagens [0], nova Data (2017, 1, 1));
// Objeto de data que estudaremos mais tarde
`` `
