# Comentários

Como sabemos do capítulo <info: structure>, os comentários podem ser de uma única linha: começando por `//` e multiline: `/ * ... * /`.

Nós usamos usualmente para descrever como e por que o código funciona.

Desde a primeira vista, os comentários podem ser óbvios, mas os novatos na programação costumam ter errado.

## Comentários ruins

Novatos tendem a usar comentários para explicar "o que está acontecendo no código". Como isso:

`` `js
// Este código fará isso (...) e essa coisa (...)
// ... e quem sabe o que mais ...
muito;
complexo;
código;
`` `

Mas, em um bom código, a quantidade de comentários "explicativos" deve ser mínima. Sério, um código deve ser fácil de entender sem eles.

Há uma grande regra sobre isso: "se o código é tão pouco claro que exige um comentário, então talvez ele devesse ser reescrito em vez disso".

### Receita: funções de fator

Às vezes, é benéfico substituir uma peça de código por uma função, como aqui:

`` `js
show de funçãoPrimes (n) {
nextPrime:
para (deixe i = 2; i <n; i ++) {

*! *
// verifique se eu é um número primo
para (let j = 2; j <i; j ++) {
se (i% j == 0) continue nextPrime;
}
* /! *

alerta (i);
}
}
`` `

A melhor variante, com uma função fatorada `isPrime`:


`` `js
show de funçãoPrimes (n) {

para (deixe i = 2; i <n; i ++) {
*! * if (! isPrime (i)) continue; * /! *

alerta (i);
}
}

function isPrime (n) {
para (deixe i = 2; i <n; i ++) {
se (n% i == 0) retornar falso;
}
retornar verdadeiro;
}
`` `

Agora podemos entender o código com facilidade. A própria função se torna o comentário. Esse código é chamado * auto-descritivo *.

### Receita: crie funções

E se tivermos uma "folha de código" longa como esta:

`` `js
// aqui adicionamos whisky
para (vamos i = 0; i <10; i ++) {
Deixe drop = getWhiskey ();
cheiro (gota);
adicionar (gota, vidro);
}

// aqui adicionamos suco
para (vamos t = 0; t <3; t ++) {
deixe tomate = getTomato ();
examinar (tomate);
Deixe o sumo = pressione (tomate);
adicionar (suco, vidro);
}

// ...
`` `

Então, pode ser uma variante melhor para refatorá-lo em funções como:

`` `js
addWhiskey (vidro);
addJuice (vidro);

função addWhiskey (container) {
para (vamos i = 0; i <10; i ++) {
Deixe drop = getWhiskey ();
// ...
}
}

função addJuice (container) {
para (vamos t = 0; t <3; t ++) {
deixe tomate = getTomato ();
// ...
}
}
`` `

Mais uma vez, as próprias funções contam o que está acontecendo. Não há nada para comentar. E também a estrutura do código é melhor quando dividida. Está claro o que faz cada função, o que é preciso e o que ela retorna.

Na realidade, não podemos evadir totalmente comentários "explicativos". Existem algoritmos complexos. E há "ajustes" inteligentes para fins de otimização. Mas geralmente devemos tentar manter o código simples e auto-descritivo.

## Good comments

Então, os comentários explicativos geralmente são ruins. Quais comentários são bons?

Descreva a arquitetura
: Fornecer uma visão geral de alto nível dos componentes, como eles interagem, qual o fluxo de controle em várias situações ... Em suma - a visão do código do pássaro. Existe uma linguagem de diagrama especial [UML] (http://wikipedia.org/wiki/Unified_Modeling_Language) para diagramas de arquitetura de alto nível. Definitivamente vale a pena estudar.

Documentar um uso de função
: Existe uma sintaxe especial [JSDoc] (http://en.wikipedia.org/wiki/JSDoc) para documentar uma função: uso, parâmetros, valor retornado.

Por exemplo:
`` `js
/ **
* Retorna x aumentado para a n-ésima potência.
*
* @param {número} x O número a aumentar.
* @param {number} n O poder, deve ser um número natural.
* @ retomar {número} x aumentado para a n-ésima potência.
* /
função pow (x, n) {
...
}
`` `

Esses comentários nos permitem entender o propósito da função e usá-lo da maneira correta sem olhar em seu código.

By the way, muitos editores como [WebStorm] (https://www.jetbrains.com/webstorm/) também podem compreendê-los e usá-los para fornecer autocompletar e alguns códigos de verificação automática.

Além disso, existem ferramentas como [JSDoc 3] (https://github.com/jsdoc3/jsdoc) que podem gerar documentação HTML dos comentários. Você pode ler mais informações sobre JSDoc em <http://usejsdoc.org/>.

Por que a tarefa foi resolvida dessa maneira?
: O que está escrito é importante. Mas o que é * não * escrito talvez seja ainda mais importante para entender o que está acontecendo. Por que a tarefa foi resolvida exatamente assim? O código não dá resposta.

Se houver muitas maneiras de resolver a tarefa, por que essa? Especialmente quando não é o mais óbvio.

Sem tais comentários, a seguinte situação é possível:
1. Você (ou seu colega) abre o código escrito há algum tempo, e veja que é "suboptimal".
2. Você pensa: "Quão estúpido eu era então, e quanto mais inteligente eu estou agora", e reescreva usando a variante "mais óbvia e correta".
3. ... O desejo de reescrever foi bom. Mas, no processo, você vê que a solução "mais óbvia" está faltando. Você até mesmo lembrou pouco porquê, porque já tentou há muito tempo. Você reverte para a variante correta, mas o tempo foi desperdiçado.

Os comentários que explicam a solução são muito importantes. Eles ajudam a continuar o desenvolvimento da maneira correta.

Quaisquer características sutis do código? Onde eles são usados?
: Se o código tem algo sutil e contra-óbvio, definitivamente vale a pena comentar.

## Resumo

Um sinal importante de um bom desenvolvedor são comentários: sua presença e até mesmo a ausência deles.

Os bons comentários nos permitem manter o código bem, volte para ele depois de um atraso e usá-lo de forma mais eficaz.

** Comente isso: **

- Arquitetura geral, visão de alto nível.
- Uso da função.
- Soluções importantes, especialmente quando não são imediatamente óbvias.

** Evade comentários: **

- Isso diz "como o código funciona" e "o que ele faz".
- Coloque-os apenas se for impossível tornar o código tão simples e auto-descritivo que não exige esses.

Os comentários também são usados ​​para ferramentas de documentação automática, como JSDoc3: elas as lêem e geram HTML-docs (ou documentos em outro formato).
