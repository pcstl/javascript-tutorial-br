# Data e hora

Vamos encontrar um novo objeto interno: [Data] (mdn: js / Date). Ele armazena a data, hora e fornece métodos para gerenciamento de data / hora.

Por exemplo, podemos usá-lo para armazenar tempos de criação / modificação, ou para medir o tempo, ou apenas para imprimir a data atual.

[cortar]

## Criação

Para criar um novo `Date` object call` new Date () `com um dos seguintes argumentos:

`new Date ()`
: Sem argumentos - crie um objeto `Date` para a data e hora atuais:

`` `js run
Deixe agora = nova data ();
alerta (agora); // mostra a data / hora atual
`` `

`new Date (milissegundos)`
: Crie um objeto `Date` com o tempo igual ao número de milissegundos (1/1000 de segundo) passado após 1 de janeiro de 1970 UTC + 0.

`` `js run
// 0 significa 01.01.1970 UTC + 0
deixe Jan01_1970 = nova data (0);
alerta (Jan01_1970);

// agora adicione 24 horas, receba 02.01.1970 UTC + 0
deixe Jan02_1970 = nova data (24 * 3600 * 1000);
alerta (Jan02_1970);
`` `

O número de milissegundos que passou desde o início de 1970 é chamado de * timestamp *.

É uma representação numérica leve de uma data. Nós sempre podemos criar uma data a partir de um timestamp usando `new Date (timestamp)` e converter o objeto `Date` existente em um timestamp usando o método` date.getTime () `(veja abaixo).

`new Date (datestring)`
: Se houver um único argumento, e é uma string, então é analisado com o algoritmo 'Date.parse` (veja abaixo).


`` `js run
deixar data = nova data ("2017-01-26");
alerta (data); // Qui 26 de janeiro de 2017 ...
`` `

`new Data (ano, mês, data, horas, minutos, segundos, ms)`
: Crie a data com os componentes fornecidos no fuso horário local. Apenas dois primeiros argumentos são obrigatórios.

Nota:

- O `ano` deve ter 4 dígitos:` 2013` está ok, `98` não é.
- A contagem de "mês" começa com `0` (Jan), até` 11` (dezembro).
- O parâmetro `date` é realmente o dia do mês, se ausente, então '1' é assumido.
- Se `horas / minutos / segundos / ms` estiver ausente, eles são assumidos como iguais a` 0`.

Por exemplo:

`` `js
nova data (2011, 0, 1, 0, 0, 0, 0); // // 1 de janeiro de 2011, 00:00:00
nova data (2011, 0, 1); // o mesmo, horas, etc. são 0 por padrão
`` `

A precisão mínima é de 1 ms (1/1000 seg):

`` `js run
let date = new Date (2011, 0, 1, 2, 3, 4, 567);
alerta (data); // 1.01.2011, 02: 03: 04.567
`` `

## Access date components

Existem muitos métodos para acessar o ano, mês e assim por diante do objeto `Date`. Mas eles podem ser facilmente lembrados quando categorizados.

[getFullYear ()] (mdn: js / Date / getFullYear)
: Obter o ano (4 dígitos)

[getMonth ()] (mdn: js / Date / getMonth)
: Obter o mês, ** de 0 a 11 **.

[getDate ()] (mdn: js / Date / getDate)
: Obter o dia do mês, de 1 a 31, o nome do método parece um pouco estranho.

[getHours ()] (mdn: js / Date / getHours), [getMinutes ()] (mdn: js / Date / getMinutes), [getSeconds ()] (mdn: js / Date / getSeconds), [getMilliseconds ()] (mdn: js / Date / getMilliseconds)
: Obter os componentes de tempo correspondentes.

`` `warn header =" Not `getYear ()`, mas `getFullYear ()` "
Muitos mecanismos de JavaScript implementam um método não padrão `getYear ()`. Esse método está obsoleto. Retorna um ano de 2 dígitos às vezes. Nunca use isso. Há `getFullYear ()` para o ano.
`` `

Além disso, podemos obter um dia da semana:

[getDay ()] (mdn: js / Date / getDay)
: Obter o dia da semana, de `0` (domingo) a` 6 (sábado). O primeiro dia é sempre domingo, em alguns países, não é assim, mas não pode ser alterado.

** Todos os métodos acima retornam os componentes em relação ao fuso horário local. **

Há também suas contrapartes UTC, que retornam dia, mês, ano e assim por diante para o fuso horário UTC + 0: [getUTCFullYear ()] (mdn: js / Date / getUTCFullYear), [getUTCMonth ()] (mdn: js / Date / getUTCMonth), [getUTCDay ()] (mdn: js / Date / getUTCDay). Basta inserir o `" UTC "` logo depois `" get "`.

Se o seu fuso horário local for deslocado em relação a UTC, o código abaixo mostra horas diferentes:

`` `js run
// data atual
Deixe data = nova Data ();

// a hora no seu fuso horário atual
alerta (date.getHours ());

// a hora no fuso horário UTC + 0 (hora de Londres sem horário de verão)
alerta (date.getUTCHours ());
`` `

Além dos métodos dados, existem dois especiais, que não possuem uma variante UTC:

[getTime ()] (mdn: js / Date / getTime)
: Retorna o carimbo de data / hora para a data - um número de milissegundos passado do 1 de janeiro de 1970 UTC + 0.

[getTimezoneOffset ()] (mdn: js / Date / getTimezoneOffset)
: Retorna a diferença entre o fuso horário local e UTC, em minutos:

`` `js run
// se você estiver no fuso horário UTC-1, as saídas 60
// se você estiver no fuso horário UTC + 3, as saídas -180
alerta (nova Data (). getTimezoneOffset ());

`` `

## Configurando os componentes da data

Os seguintes métodos permitem definir componentes de data / hora:

- [`setFullYear (ano [, mês, data]` `(por exemplo: js / Date / setFullYear)
- [`setMonth (month [, date])`] (mdn: js / Date / setMonth)
- [`setDate (data)`] (mdn: js / Date / setDate)
- [`setHours (hora [, min, sec, ms])`] (mdn: js / Date / setHours)
- [`setMinutes (min [, sec, ms])`] (mdn: js / Date / setMinutes)
- [`setSeconds (sec [, ms])`] (mdn: js / Date / setSeconds)
- [`setMilliseconds (ms)`] (mdn: js / Date / setMilliseconds)
- [`setTime (milissegundos)`] (mdn: js / Date / setTime) (define a data inteira em milissegundos desde 01.01.1970 UTC)

Cada um deles exceto `setTime ()` tem uma variante UTC, por exemplo: `setUTCHours ()`.

Como podemos ver, alguns métodos podem configurar vários componentes ao mesmo tempo, por exemplo `setHours`. Os componentes que não são mencionados não são modificados.

Por exemplo:

`` `js run
Deixe hoje = nova data ();

today.setHours (0);
alerta (hoje); // ainda hoje, mas a hora é alterada para 0

today.setHours (0, 0, 0, 0);
alerta (hoje); // ainda hoje, agora 00:00:00 sharp.
`` `

## Auto correção

A * autocorreção * é uma característica muito útil dos objetos `Date`. Nós podemos definir valores fora de alcance, e ele se auto-ajustará.

Por exemplo:

`` `js run
Deixe data = nova data (2013, 0, *! * 32 * /! *); // 32 de janeiro de 2013?!?
alerta (data); // ... é 1 de fevereiro de 2013!
`` `

Os componentes da data fora do alcance são distribuídos automaticamente.

Digamos que precisamos aumentar a data "28 de fevereiro de 2016" por 2 dias. Pode ser "2 Mar" ou "1 Mar" em caso de um ano bissexto. Não precisamos pensar sobre isso. Apenas adicione 2 dias. O objeto `Date` fará o resto:

`` `js run
Data da data = nova (2016, 1, 28);
*! *
date.setDate (date.getDate () + 2);
* /! *

alerta (data); // 1 de março de 2016
`` `

Esse recurso é freqüentemente usado para obter a data após o período de tempo dado. Por exemplo, vamos receber a data para "70 segundos depois":

`` `js run
Deixe data = nova Data ();
date.setSeconds (date.getSeconds () + 70);

alerta (data); // mostra a data correta
`` `

Também podemos definir zero ou mesmo valores negativos. Por exemplo:

`` `js run
Data da data = nova (2016, 0, 2); // 2 de janeiro de 2016

date.setDate (1); // set dia 1 do mês
alerta (data);

date.setDate (0); // min dia é 1, então o último dia do mês anterior é assumido
alerta (data); // 31 de dezembro de 2015
`` `

## Data para número, data de dif

Quando um objeto `Date` é convertido em número, ele se torna o timestamp igual a` date.getTime () `:

`` `js run
Deixe data = nova Data ();
alerta (+ data); // o número de milissegundos, igual a date.getTime ()
`` `

O efeito colateral importante: as datas podem ser resgatadas, o resultado é a diferença em ms.

Isso pode ser usado para medições de tempo:

`` `js run
Deixe começar = nova Data (); // começa a contar

// faça o trabalho
para (vamos i = 0; i <100000; i ++) {
deixe doSomething = i * i * i;
}

Deixe fim = nova Data (); // feito

alerta (`O loop levou $ {fim - início} ms`);
`` `

## Date.now ()

Se quisermos apenas medir a diferença, não precisamos do objeto `Date`.

Existe um método especial `Date.now ()` que retorna o timestamp atual.

É semanticamente equivalente a `new Date (). GetTime ()`, mas não cria um objeto intermediário 'Date`. Portanto, é mais rápido e não pressiona a coleta de lixo.

É usado principalmente por conveniência ou quando o desempenho é importante, como em jogos em JavaScript ou outros aplicativos especializados.

Então, isso provavelmente é melhor:

`` `js run
*! *
deixe start = Date.now (); // milissegundos contagem a partir de 1 de janeiro de 1970
* /! *

// faça o trabalho
para (vamos i = 0; i <100000; i ++) {
deixe doSomething = i * i * i;
}

*! *
Deixe fim = Date.now (); // feito
* /! *

alerta (`O loop levou $ {fim - início} ms`); // subtraia números, não data
`` `

## Avaliação comparativa

Se quisermos um benchmark confiável da função de CPU-fome, devemos ter cuidado.

Por exemplo, vamos medir duas funções que calculam a diferença entre duas datas: qual delas é mais rápida?

`` `js
// temos data1 e data2, qual função mais rápida retorna sua diferença em ms?
função diffSubstract (date1, date2) {
data de retorno2 - data1;
}

// ou
função diffGetTime (date1, date2) {
return date2.getTime () - date1.getTime ();
}
`` `

Esses dois fazem exatamente a mesma coisa, mas um deles usa um `data.getTime ()` explícito para obter a data em ms e o outro depende de uma transformação de data a número. O resultado é sempre o mesmo.

Então, qual é o mais rápido?

A primeira idéia pode ser executá-los muitas vezes seguidas e medir a diferença horária. Para o nosso caso, as funções são muito simples, então devemos fazê-lo em torno de 100000 vezes.

Vamos medir:

`` `js run
função diffSubstract (date1, date2) {
data de retorno2 - data1;
}

função diffGetTime (date1, date2) {
return date2.getTime () - date1.getTime ();
}

banco de função (f) {
Deixe data1 = data nova (0);
Deixe data2 = nova Data ();

deixe start = Date.now ();
para (vamos i = 0; i <100000; i ++) f (date1, date2);
Retornar Date.now () - start;
}

alerta ('Time of diffSubstract:' + bench (diffSubstract) + 'ms');
alerta ('Tempo de diffGetTime:' + bench (diffGetTime) + 'ms');
`` `

Uau! Usar `getTime ()` é muito mais rápido! Isso porque não há conversão de tipo, é muito mais fácil para otimizar os motores.

Ok, temos algo. Mas essa não é uma boa referência ainda.

Imagine que, no momento da execução do banco de dados (diffSubstract), a CPU estava fazendo algo em paralelo, e estava tomando recursos. E, no momento da execução do banco (diffGetTime), o trabalho terminou.

Um cenário bastante real para um sistema operacional moderno com vários processos.

Como resultado, o primeiro benchmark terá menos recursos de CPU do que o segundo. Isso pode levar a resultados errados.

** Para um benchmarking mais confiável, todo o pacote de benchmarks deve ser repetido várias vezes. **

Aqui está o exemplo de código:

`` `js run
função diffSubstract (date1, date2) {
data de retorno2 - data1;
}

função diffGetTime (date1, date2) {
return date2.getTime () - date1.getTime ();
}

banco de função (f) {
Deixe data1 = data nova (0);
Deixe data2 = nova Data ();

deixe start = Date.now ();
para (vamos i = 0; i <100000; i ++) f (date1, date2);
Retornar Date.now () - start;
}

Deixe time1 = 0;
Deixe time2 = 0;

*! *
// executar banco (UpperSlice) e banco (upperLoop) cada 10 vezes alternando
para (vamos i = 0; i <10; i ++) {
time1 + = bench (diffSubstract);
time2 + = bench (diffGetTime);
}
* /! *

alerta ('Tempo total para diffSubstract:' + time1);
alerta ('Tempo total para diffGetTime:' + time2);
`` `

Os motores de JavaScript modernos começam a aplicar otimizações avançadas apenas para "hot code" que executa muitas vezes (não é necessário otimizar as coisas raramente executadas). Então, no exemplo acima, as primeiras execuções não estão bem otimizadas. Podemos querer adicionar uma corrida de aquecimento:

`` `js
// adicionado para "aquecimento" antes do loop principal
banco (diffSubstract);
banco (diffGetTime);

// agora benchmark
para (vamos i = 0; i <10; i ++) {
time1 + = bench (diffSubstract);
time2 + = bench (diffGetTime);
}
`` `

`` `warn header =" Tenha cuidado ao fazer microbenchmarking "
Os motores JavaScript modernos executam muitas otimizações. Eles podem ajustar os resultados de "testes artificiais" em comparação com o "uso normal", especialmente quando comparamos algo muito pequeno. Então, se você deseja seriamente entender o desempenho, então, estuda como funciona o mecanismo de JavaScript. E então você provavelmente não precisará de microbenchmarks.

O grande pacote de artigos sobre o V8 pode ser encontrado em <http://mrale.ph>.
`` `

## Date.parse de uma string

O método [Data.parse (str)] (mdn: js / Date / parse) pode ler uma data a partir de uma string.

O formato de seqüência deve ser: `AAAA-MM-DDTHH: mm: ss.sssZ`, onde:

- `YYYY-MM-DD` - é a data: ano-mês-dia.
- O caractere "T" é usado como o delimitador.
- `HH: mm: ss.sss` - é o tempo: horas, minutos, segundos e milissegundos.
- A parte opcional `` Z '' designa o fuso horário no formato `+ -hh: mm`. Uma única letra `Z` significaria UTC + 0.

Variáveis ​​mais curtas também são possíveis, como `YYYY-MM-DD` ou` YYYY-MM` ou mesmo `YYYY`.

A chamada para `Date.parse (str)` analisa a string no formato dado e retorna o carimbo de data / hora (número de milissegundos a partir de 1 de janeiro de 1970 UTC + 0). Se o formato for inválido, retorna `NaN`.

Por exemplo:

`` `js run
let ms = Date.parse ('2012-01-26T13: 51: 50.417-07: 00');

alerta (ms); // 1327611110417 (timestamp)
`` `

Podemos criar instantaneamente um objeto `new Date` do timestamp:

`` `js run
Deixe data = nova Data (Data.parse ('2012-01-26T13: 51: 50.417-07: 00'));

alerta (data);
`` `

## Resumo

- A data e a hora em JavaScript são representadas com o objeto [Data] (mdn: js / Date). Não podemos criar "apenas data" ou "apenas tempo": os objetos `Date` sempre carregam ambos.
- Os meses são contados a partir de zero (sim, janeiro é um mês zero).
- Os dias da semana em `getDay ()` também são contados a partir de zero (que é domingo).
- `Date` corrige-se automaticamente quando os componentes fora do alcance estão configurados. Bom para adicionar / subtrair dias / meses / horas.
- As datas podem ser resgatadas, dando a diferença em milissegundos. Isso ocorre porque um `Date` se torna o timestamp se convertido em um número.
- Use `Date.now ()` para obter o timestamp atual rápido.

Observe que, ao contrário de muitos outros sistemas, timestamps em JavaScript são em milissegundos, e não em segundos.

Além disso, às vezes precisamos de medidas de tempo mais precisas. O próprio JavaScript não possui uma maneira de medir o tempo em microssegundos (1 milhão de segundo), mas a maioria dos ambientes o fornece. Por exemplo, o navegador tem [performance.now ()] (mdn: api / Performance / now) que dá o número de milissegundos do início da página com precisão de microssegundos (3 dígitos após o ponto):

`` `js run
alerta (`Carregando iniciado $ {performance.now ()} ms ago`);
// Algo como: "Loading started 34731.26000000001ms ago"
// .26 são microssegundos (260 microssegundos)
// mais de 3 dígitos após o ponto decimal são erros de precisão, mas apenas 3 primeiro são corretos
`` `

Node.JS possui o módulo "microtime" e outras formas. Tecnicamente, qualquer dispositivo e ambiente permite obter mais precisão, não é apenas em 'Data'.
