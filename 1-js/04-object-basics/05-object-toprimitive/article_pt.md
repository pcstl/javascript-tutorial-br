
# Objeto para conversão primitiva

O que acontece quando os objetos são adicionados `obj1 + obj2`, subtraídos` obj1 - obj2` ou impressos usando `alert (obj)`?

Existem métodos especiais em objetos que fazem a conversão.

No capítulo <info: type-conversions> vimos as regras para conversões numeric, string e booleanas de primitivas. Mas deixamos uma lacuna para os objetos. Agora, como sabemos sobre métodos e símbolos, torna-se possível fechá-lo.

[cortar]

Para objetos, não há conversão para booleano, porque todos os objetos são `verdadeiros 'em um contexto booleano. Portanto, há apenas conversões de cadeia e numéricas.

A conversão numérica ocorre quando subtraimos objetos ou aplicamos funções matemáticas. Por exemplo, os objetos `Date` (para serem cobertos no capítulo <info: date>) podem ser subtraídos e o resultado de` date1 - date2` é a diferença horária entre duas datas.

Quanto à conversão de cordas - geralmente acontece quando emitimos um objeto como `alert (obj)` e em contextos semelhantes.

## ToPrimitive

Quando um objeto é usado no contexto em que uma primitiva é necessária, por exemplo, em um "alerta" ou operações matemáticas, é convertido em um valor primitivo usando o algoritmo `ToPrimitive` ([especificação] (https: //tc39.github .io / ecma262 / # sec-toprimitive)).

Esse algoritmo nos permite personalizar a conversão usando um método de objeto especial.

Dependendo do contexto, a conversão tem uma chamada "dica".

Existem três variantes:

`" string "`
: Quando uma operação espera uma string, para conversões de objeto a string, como `alert`:

`` `js
// saída
alerta (obj);

// usando o objeto como uma chave de propriedade
OutroObj [obj] = 123;
`` `

`" número "
: Quando uma operação espera um número, para conversões de objeto a número, como matemática:

`` `js
// conversão explícita
Deixe num = Número (obj);

// matemática (exceto binário mais)
deixe n = + obj; // unary plus
deixe delta = data1 - data2;

// comparação menor / maior
deixe maior = user1> user2;
`` `

`" padrão "`
: Ocorre em casos raros quando o operador "não tem certeza" do tipo a qual espera.

Por exemplo, o binário plus `+` pode funcionar tanto com strings (concatena-los) quanto com números (os adiciona), de modo que ambas as seqüências e os números fariam. Ou quando um objeto é comparado usando `==` com uma string, um número ou um símbolo.

`` `js
// binário mais
deixe total = car1 + car2;

// obj == string / number / symbol
se (usuário == 1) {...};
`` `

O operador maior / menor `<>` pode trabalhar com ambas as strings e números também. Ainda assim, ele usa a dica de "número", não "padrão". Isso é por razões históricas.

Na prática, todos os objetos embutidos, exceto para um caso (objeto `Date`, o aprenderemos mais tarde) implementamos a conversão` `default '' da mesma maneira que` `number" `. E provavelmente devemos fazer o mesmo.

Por favor note - há apenas três dicas. É simples assim. Não há sugestão "booleana" (todos os objetos são `verdadeiros 'no contexto booleano) ou qualquer outra coisa. E se tratarmos `" default "` e `" number "` o mesmo, como a maioria dos built-in, então existem apenas duas conversões.

** Para fazer a conversão, o JavaScript tenta encontrar e chamar três métodos de objeto: **

1. Ligue `obj [Symbol.toPrimitive] (dica)` se o método existe,
2. Caso contrário, se a dica for `" string "`
- tente `obj.toString ()` e `obj.value Of ()`, seja lá o que for existir.
3. Caso contrário, se a dica for `" número "ou" padrão "
- tente `obj.valueOf ()` e `obj.toString ()`, seja lá o que for existir.

## Symbol.toPrimitive

Vamos começar pelo primeiro método. Há um símbolo embutido chamado `Symbol.toPrimitive` que deve ser usado para nomear o método de conversão, como este:

`` `js
obj [Symbol.toPrimitive] = função (dica) {
// retorna um valor primitivo
// dica = um de "string", "número", "padrão"
}
`` `

Por exemplo, aqui o objeto 'usuário' o implementa:

`` `js run
Deixe o usuário = {
Nome: "John",
dinheiro: 1000,

[Symbol.toPrimitive] (dica) {
alerta (`dica: $ {dica}`);
Dica de retorno == "string"? `{name:" $ {this.name} "}`: this.money;
}
};

// demonstração de conversões:
alerta (usuário); // dica: string -> {nome: "John"}
alerta (+ usuário); // dica: número -> 1000
alerta (usuário + 500); // dica: padrão -> 1500
`` `

Como podemos ver a partir do código, o "usuário" torna-se uma seqüência auto-descritiva ou um valor monetário dependendo da conversão. O método único `user [Symbol.toPrimitive]` lida com todos os casos de conversão.


## toString / valueOf

Os métodos `toString` e` valueOf` vêm da antiguidade. Eles não são símbolos (os símbolos não existiram há muito tempo), mas sim os métodos "regulares" com nomes de string. Eles fornecem uma maneira alternativa de "estilo antigo" para implementar a conversão.

Se não houver `Symbol.toPrimitive`, o JavaScript tenta encontrá-los e tentar na ordem:

- `toString -> valueOf` para a sugestão" string ".
- `valueOf -> toString` caso contrário.

Por exemplo, aqui `usuário` faz o mesmo acima usando uma combinação de` toString` e `valueOf`:

`` `js run
Deixe o usuário = {
Nome: "John",
dinheiro: 1000,

// para dica = "string"
para sequenciar() {
retornar `{name: '$ {this.name}"} `;
},

// para dica = "número" ou "padrão"
valor de() {
devolva isto. dinheiro;
}

};

alerta (usuário); // toString -> {nome: "John"}
alerta (+ usuário); // valorOf -> 1000
alerta (usuário + 500); // valorOf -> 1500
`` `

Muitas vezes, queremos um único lugar "catch-all" para lidar com todas as conversões primitivas. Neste caso, podemos implementar apenas o `toString`, assim:

`` `js run
Deixe o usuário = {
Nome: "John",

para sequenciar() {
devolva this.name;
}
};

alerta (usuário); // toString -> John
alerta (usuário + 500); // toString -> John500
`` `

Na ausência de `Symbol.toPrimitive` e` valueOf`, `toString` irá lidar com todas as conversões primitivas.


## ToPrimitive e ToString / ToNumber

O importante saber sobre todos os métodos de conversão primitiva é que eles não necessariamente retornam o primitivo "insinuado".

Não há nenhum controle se `toString ()` retorna exatamente uma string, ou se o método `Symbol.toPrimitive` retorna um número para um" número "de dica.

** A única coisa obrigatória: esses métodos devem retornar uma primitiva. **

Uma operação que iniciou a conversão obtém essa primitiva e, em seguida, continua trabalhando com ela, aplicando novas conversões, se necessário.

Por exemplo:

- Operações matemáticas (exceto binário mais) realizam a conversão `ToNumber`:

`` `js run
deixe obj = {
toString () {// toString lida com todas as conversões na ausência de outros métodos
retornar "2";
}
};

alerta (obj * 2); // 4, ToPrimitive dá "2", então torna-se 2
`` `

- O binário mais verifica o primitivo - se for uma string, então faz concatenação, caso contrário, executa `ToNumber` e funciona com números.

Exemplo de cadeia:
`` `js run
deixe obj = {
para sequenciar() {
retornar "2";
}
};

alerta (obj + 2); // 22 (ToPrimitive retornou cadeia => concatenação)
`` `

Exemplo de número:
`` `js run
deixe obj = {
para sequenciar() {
retornar verdadeiro;
}
};

alerta (obj + 2); // 3 (ToPrimitive retornou booleano, não string => ToNumber)
`` `

`` `cabeçalho inteligente =" notas históricas "
Por motivos históricos, os métodos `toString` ou` valueOf` * devem * retornar um primitivo: se algum deles retorna um objeto, então não há erro, mas esse objeto é ignorado (como se o método não existisse).

Em contraste, `Symbol.toPrimitive` * deve * retornar uma primitiva, caso contrário, haverá um erro.
`` `

## Resumo

A conversão de objeto a primitivo é chamada automaticamente por muitas funções internas e operadores que esperam um primitivo como um valor.

Existem 3 tipos (sugestões) dele:
- `" string "` (para `alert` e outras conversões de string)
- `` number "` (para matemática)
- `" default "` (poucos operadores)

A especificação descreve explicitamente qual operador usa essa sugestão. Há muito poucos operadores que "não sabem o que esperar" e usam a dica "padrão". Normalmente, para objetos incorporados, a dica "padrão" "é tratada da mesma maneira que" número ", então, na prática, as duas últimas são muitas vezes juntas.

O algoritmo de conversão é:

1. Ligue `obj [Symbol.toPrimitive] (dica)` se o método existe,
2. Caso contrário, se a dica for `" string "`
- tente `obj.toString ()` e `obj.value Of ()`, seja lá o que for existir.
3. Caso contrário, se a dica for `" número "ou" padrão "
- tente `obj.valueOf ()` e `obj.toString ()`, seja lá o que for existir.

Na prática, muitas vezes é suficiente implementar apenas `obj.toString ()` como um método "catch-all" para todas as conversões que retornam uma representação "legível por humanos" de um objeto, para fins de logging ou depuração.
