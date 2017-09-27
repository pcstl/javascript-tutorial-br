# Métodos JSON, toJSON

Digamos que temos um objeto complexo, e gostaríamos de convertê-lo em uma string, enviá-lo através de uma rede ou apenas para exibi-lo para fins de registro.

Naturalmente, essa string deve incluir todas as propriedades importantes.

Poderíamos implementar a conversão como esta:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 30,

*! *
para sequenciar() {
retornar `{name: '$ {this.name}", idade: $ {this.age}} `;
}
* /! *
};

alerta (usuário); // {nome: "John", idade: 30}
`` `

... Mas no processo de desenvolvimento, novas propriedades são adicionadas, as propriedades antigas são renomeadas e removidas. Atualizar tais "toString" sempre pode se tornar uma dor. Poderíamos tentar ignorar as propriedades nele, mas e se o objeto for complexo e tiver objetos aninhados nas propriedades? Nós também precisamos implementar sua conversão. E, se estamos enviando o objeto através de uma rede, também precisamos fornecer o código para "ler" nosso objeto no lado receptor.

Por sorte, não há necessidade de escrever o código para lidar com tudo isso. A tarefa já foi resolvida.

[cortar]

## JSON.stringify

O [JSON] (http://en.wikipedia.org/wiki/JSON) (Notação de objeto JavaScript) é um formato geral para representar valores e objetos. É descrito como em [RFC 4627] (http://tools.ietf.org/html/rfc4627) padrão. Inicialmente, foi feito para JavaScript, mas muitas outras linguas possuem bibliotecas para lidar com isso também. Portanto, é fácil usar JSON para troca de dados quando o cliente usa JavaScript e o servidor está escrito em Ruby / PHP / Java / Whatever.

O JavaScript fornece métodos:

- `JSON.stringify` para converter objetos em JSON.
- `JSON.parse` para converter JSON novamente em um objeto.

Por exemplo, aqui nós `JSON.stringify` um aluno:
`` `js run
deixe student = {
Nome: 'John',
idade: 30,
isAdmin: falso,
cursos: ['html', 'css', 'js'],
esposa: nula
};

*! *
Deixe json = JSON.stringify (student);
* /! *

alerta (typeof json); // temos uma corda!

alerta (json);
*! *
/ * Objeto codificado em JSON:
{
"nome": "João",
"idade": 30,
"isAdmin": falso,
"cursos": ["html", "css", "js"],
"esposa": nula
}
* /
* /! *
`` `

O método `JSON.stringify (student)` leva o objeto e o converte em uma string.

A sequência resultante do `json` é um * chamado * JSON-codificado * ou * serializado * ou * stringified * ou * organizado * objeto. Estamos prontos para enviá-lo por cima do fio ou colocar na loja de dados simples.


Observe que o objeto codificado em JSON possui várias diferenças importantes do objeto literal:

- As strings usam aspas duplas. Nenhuma cotação ou backtick em JSON. Então, "John" se torna "John" `.
- Os nomes de propriedades do objeto também são citado duas vezes. Isso é obrigatório. Então, 'idade: 30' torna-se 'idade': 30 '.

O `JSON.stringify` também pode ser aplicado às primitivas.

Native supported JSON tipos são:

- Objetos `{...}`
- Arrays `[...]`
- Primitivas:
- cordas,
- números,
- valores booleanos `true / false`,
- `null`.

Por exemplo:

`` `js run
// um número no JSON é apenas um número
alerta (JSON.stringify (1)) // 1

// uma string em JSON ainda é uma string, mas cita duas vezes
alerta (JSON.stringify ('test')) // "teste"

alerta (JSON.stringify (true)); // verdade

alerta (JSON.stringify ([1, 2, 3])); // [1,2,3]
`` `

O JSON é especificação de linguagem cruzada somente de dados, portanto, algumas propriedades de objeto específicas de JavaScript são ignoradas por `JSON.stringify`.

Nomeadamente:

- Propriedades da função (métodos).
- Propriedades simbólicas.
- Propriedades que armazenam `indefinido`.

`` `js run
Deixe o usuário = {
sayHi () {// ignorado

},
[Símbolo ("id")]: 123, // ignorado
algo: indefinido // ignorado
};

alerta (JSON.stringify (usuário)); // {} (objeto vazio)
`` `

Normalmente, está bem. Se não é o que queremos, então, logo veremos como personalizar o processo.

O grande é que os objetos aninhados são suportados e convertidos automaticamente.

Por exemplo:

`` `js run
Deixe meetup = {
Título: "Conferência",
*! *
quarto: {
número: 123,
participantes: ["john", "ann"]
}
* /! *
};

alerta (JSON.stringify (meetup));
/ * Toda a estrutura é stringificada:
{
"título": "Conferência",
"quarto": {"número": 23, "participantes": ["john", "ann"]},
}
* /
`` `

A limitação importante: não deve haver referências circulares.

Por exemplo:

`` `js run
deixe room = {
número: 23
};

Deixe meetup = {
Título: "Conferência",
participantes: ["john", "ann"]
};

meetup.place = room; // sala de referências de reunião
room.occupiedBy = meetup; // reunião de referências de sala

*! *
JSON.stringify (meetup); // Erro: Convertendo a estrutura circular para JSON
* /! *
`` `

Aqui, a conversão falha, por causa da referência circular: `room.occupiedBy` faz referência` meetup` e `meetup.place` references` room`:

!] [] (json-meetup.png)


## Excluindo e transformando: substituição

A sintaxe completa de `JSON.stringify` é:

`` `js
Deixe json = JSON.stringify (value [, replace, space])
`` `

valor
: Um valor a codificar.

substituto
: Array de propriedades para codificar ou uma função de mapeamento `função (chave, valor)`.

espaço
: Quantidade de espaço a ser usada para formatação

Na maioria das vezes, `JSON.stringify` é usado apenas com o primeiro argumento. Mas se precisarmos ajustar o processo de substituição, gosta de filtrar referências circulares, podemos usar o segundo argumento de `JSON.stringify`.

Se nós passamos uma série de propriedades para ele, apenas essas propriedades serão codificadas.

Por exemplo:

`` `js run
deixe room = {
número: 23
};

Deixe meetup = {
Título: "Conferência",
participantes: [{nome: "John"}, {nome: "Alice"}],
lugar: quarto // sala de referências de reunião
};

room.occupiedBy = meetup; // reunião de referências de sala

alerta (JSON.stringify (meetup, *! * ['title', 'participants'] * /! *));
// {"título": "Conferência", "participantes": [{}, {}]}
`` `

Aqui, provavelmente, somos muito rígidos. A lista de propriedades é aplicada a toda a estrutura do objeto. Então, os participantes estão vazios, porque `name` não está na lista.

Vamos incluir todos os bens, exceto `room.occupiedBy` que causariam a referência circular:

`` `js run
deixe room = {
número: 23
};

Deixe meetup = {
Título: "Conferência",
participantes: [{nome: "John"}, {nome: "Alice"}],
lugar: quarto // sala de referências de reunião
};

room.occupiedBy = meetup; // reunião de referências de sala

alerta (JSON.stringify (meetup, *! * ['title', 'participantes', 'lugar', 'nome', 'número'] * /! *));
/ *
{
"título": "Conferência",
"participantes": [{"nome": "João"}, {"nome": "Alice"}],
"lugar": {"número": 23}
}
* /
`` `

Agora, tudo exceto `occupiedBy` é serializado. Mas a lista de propriedades é bastante longa.

Felizmente, podemos usar uma função em vez de uma matriz como o `substituto '.

A função será chamada para cada par `(chave, valor)` e deve retornar o valor "substituído", que será usado em vez do original.

No nosso caso, podemos retornar "valor" como está "para tudo, exceto` occupiedBy`. Para ignorar `occupiedBy`, o código abaixo retorna` undefined`:

`` `js run
deixe room = {
número: 23
};

Deixe meetup = {
Título: "Conferência",
participantes: [{nome: "John"}, {nome: "Alice"}],
lugar: quarto // sala de referências de reunião
};

room.occupiedBy = meetup; // reunião de referências de sala

alerta (JSON.stringify (meetup, substituição de função (chave, valor) {
alerta (`$ {key}: $ {value}`); // para ver o que o substituto obtém
retorno (chave == 'ocupadoBy')? indefinido: valor;
}));

/ *: pares de valores que vêm para o substituto:
: [Object Object]
Título: Conferência
participantes: [object Object], [object Object]
0: [Object Object]
nome: John
1: [Object Object]
nome: Alice
lugar: [object Object]
número: 23
* /
`` `

Observe que a função `replacer` obtém cada par de chaves / valores, incluindo objetos aninhados e itens de matriz. É aplicado de forma recursiva. O valor de `this` dentro de` replace 'é o objeto que contém a propriedade atual.

A primeira ligação é especial. É feito usando um "objeto wrapper especial": `{" ": meetup}`. Em outras palavras, o primeiro par '(chave, valor) `tem uma chave vazia e o valor é o objeto alvo como um todo. É por isso que a primeira linha é `": [Object Object] "no exemplo acima.

A idéia é fornecer o máximo de poder para o "substituto" possível: tem a chance de analisar e substituir / ignorar todo o objeto, se necessário.


## Formatação: espaçador

O terceiro argumento de `JSON.stringify (value, replacer, spaces)` é o número de espaços a serem usados ​​para formatação bonita.

Anteriormente, todos os objetos de stringificação não apresentavam recortes e espaços extras. Isso é bom se queremos enviar um objeto através de uma rede. O argumento `spacer` é usado exclusivamente para uma boa saída.

Aqui, `spacer = 2` diz ao JavaScript que mostre objetos aninhados em várias linhas, com indentação de 2 espaços dentro de um objeto:

`` `js run
Deixe o usuário = {
Nome: "John",
idade: 25,
papéis: {
isAdmin: falso,
isEditor: true
}
};

alerta (JSON.stringify (user, null, 2));
/ * recuos de dois espaços:
{
"nome": "João",
"idade": 25,
"papéis": {
"isAdmin": falso,
"isEditor": true
}
}
* /

/ * para JSON.stringify (usuário, nulo, 4) o resultado seria mais recuado:
{
"nome": "João",
"idade": 25,
"papéis": {
"isAdmin": falso,
"isEditor": true
}
}
* /
`` `

O parâmetro `spaces` é usado unicamente para fins de logging e nice-output.

## Custom "toJSON"

Como `toString` para uma conversão de seqüência de caracteres, um objeto pode fornecer o método` toJSON` para a conversão para JSON. `JSON.stringify` automaticamente o chama se disponível.

Por exemplo:

`` `js run
deixe room = {
número: 23
};

Deixe meetup = {
Título: "Conferência",
data: nova data (Date.UTC (2017, 0, 1)),
quarto
};

alerta (JSON.stringify (meetup));
/ *
{
"título": "Conferência",
*! *
"data": "2017-01-01T00: 00: 00.000.000", // (1)
* /! *
"sala": {"número": 23} // (2)
}
* /
`` `

Aqui podemos ver que `data`` (1) `se tornou uma string. Isso porque todas as datas possuem um método 'toJSON' interno que retorna esse tipo de string.

Agora vamos adicionar um `` toJSON 'personalizado para o nosso objeto `room`:

`` `js run
deixe room = {
número: 23,
*! *
toJSON () {
devolva este.número;
}
* /! *
};

Deixe meetup = {
Título: "Conferência",
quarto
};

*! *
alerta (JSON.stringify (quarto)); // 23
* /! *

alerta (JSON.stringify (meetup));
/ *
{
"título": "Conferência",
*! *
"sala": 23
* /! *
}
* /
`` `

Como podemos ver, `toJSON` é usado tanto para a chamada direta` JSON.stringify (room) `quanto para o objeto aninhado.


## JSON.parse

Para decodificar uma string JSON, precisamos de outro método chamado [JSON.parse] (mdn: js / JSON / parse).

A sintaxe:
`` `js
Deixe value = JSON.parse (str [, reviver]);
`` `

str
: JSON-string para analisar.

reviver
: Função opcional (chave, valor) que será chamada para cada par `(chave, valor)` e pode transformar o valor.

Por exemplo:

`` `js run
// matriz de stringificação
Deixe números = "[0, 1, 2, 3]";

números = JSON.parse (números);

alerta (números [1]); // 1
`` `

Ou para objetos aninhados:

`` `js run
Deixe o usuário = "{" nome ":" João "," idade ": 35," isAdmin ": falso," amigos ": [0,1,2,3]} ';

user = JSON.parse (usuário);

alerta (user.friends [1]); // 1
`` `

O JSON pode ser tão complexo quanto necessário, objetos e arrays podem incluir outros objetos e arrays. Mas eles devem obedecer o formato.

Aqui estão erros típicos em JSON escrito à mão (às vezes temos que escrevê-lo para fins de depuração):

`` `js
Deixe json = `{
*! * name * /! *: "John", // erro: nome da propriedade sem aspas
"sobrenome": *! * 'Smith' * /! *, // erro: cotações simples em valor (deve ser o dobro)
*! * 'isAdmin' * /! *: false // error: cotações simples na chave (deve ser o dobro)
"aniversário": *! * nova data (2000, 2, 3) * /! *, // erro: não é permitido "novo", apenas valores nulos
"amigos": [0,1,2,3] // aqui tudo bem
} `;
`` `

Além disso, a JSON não suporta comentários. Adicionar um comentário ao JSON torna inválido.

Há outro formato chamado [JSON5] (http://json5.org/), que permite chaves não-marcadas, comentários, etc. Mas esta é uma biblioteca autônoma, não na especificação do idioma.

O JSON regular é tão estrito, não porque seus desenvolvedores são preguiçosos, mas também permitem implementações fáceis, confiáveis ​​e muito rápidas do algoritmo de análise.

## Utilizando o reviver

Imagine, obtivemos um objeto `meetup` com fio do servidor.

Parece assim:

`` `js
// título: (título de reunião), data: (data de reunião)
Deixe str = '{"title": "Conference", "date": "2017-11-30T12: 00: 00.000.000"}';
`` `

... E agora precisamos deserializá-lo, retornar para o objeto JavaScript.

Vamos fazê-lo chamando `JSON.parse`:

`` `js run
Deixe str = '{"title": "Conference", "date": "2017-11-30T12: 00: 00.000.000"}';

Deixe meetup = JSON.parse (str);

*! *
alerta (meetup.date.getDate ()); // Erro!
* /! *
`` `

Ups! Um erro!

O valor de `meetup.date` é uma string, não um objeto` Date`. Como poderia "JSON.parse" saber que deveria transformar essa string em uma 'data'?

Passemos para `JSON.parse` a função revivendo que retorna todos os valores" tal como está ", mas` data` se tornará uma 'Data':

`` `js run
Deixe str = '{"title": "Conference", "date": "2017-11-30T12: 00: 00.000.000"}';

*! *
let meetup = JSON.parse (str, function (key, value) {
se (chave == 'data') retornar nova Data (valor);
valor de retorno;
});
* /! *

alerta (meetup.date.getDate ()); // agora funciona!
`` `

Por sinal, isso também funciona para objetos aninhados:

`` `js run
Deixe schedule = `{
"meetups": [
{"título": "Conferência", "data": "2017-11-30T12: 00: 00.000.000"},
{"título": "Aniversário", "data": "2017-04-18T12: 00: 00.000.000"}
]
} `;

schedule = JSON.parse (programação, função (chave, valor) {
se (chave == 'data') retornar nova Data (valor);
valor de retorno;
});

*! *
alerta (schedule.meetups [1] .date.getDate ()); // trabalho!
* /! *
`` `



## Resumo

- JSON é um formato de dados que possui seu próprio padrão independente e bibliotecas para a maioria das linguagens de programação.
- JSON suporta objetos simples, arrays, strings, números, booleanos e `null`.
- O JavaScript fornece métodos [JSON.stringify] (mdn: js / JSON / stringify) para serializar em JSON e [JSON.parse] (mdn: js / JSON / parse) para ler a partir do JSON.
- Ambos os métodos suportam funções do transformador para leitura / escrita inteligentes.
- Se um objeto tem `toJSON`, então ele é chamado por` JSON.stringify`.
