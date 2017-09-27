importância: 5

---

# Guarde as bandeiras "não lidas"

Há uma série de mensagens:

`` `js
deixe mensagens = [
{texto: "Olá", de: "John"},
{texto: "Como vai?", de: "John"},
{text: "Até breve", de: "Alice"}
];
`` `

Seu código pode acessá-lo, mas as mensagens são gerenciadas pelo código de outra pessoa. Novas mensagens são adicionadas, as antigas são removidas regularmente por esse código e você não conhece os momentos exatos quando acontece.

Agora, qual a estrutura de dados que você poderia usar para armazenar informações se a mensagem "foi lida"? A estrutura deve ser bem adaptada para dar a resposta "foi lido?" para o objeto de mensagem dado.

P.S. Quando uma mensagem é removida de "mensagens", ela também deve desaparecer da sua estrutura.

P.P.S. Não devemos modificar objetos de mensagem diretamente. Se eles são gerenciados pelo código de outra pessoa, adicionar propriedades extras para eles pode ter conseqüências ruins.
