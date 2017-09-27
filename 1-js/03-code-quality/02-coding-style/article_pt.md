# Estilo de codificação

Nosso código deve ser tão limpo e fácil de ler quanto possível.

Essa é realmente uma arte de programação - para assumir uma tarefa complexa e codificá-la de uma forma que seja correta e legível por humanos.

Uma coisa para ajudar é o bom estilo de código.

[cortar]

## Sintaxe

Uma cheatsheet com as regras (mais detalhes abaixo):

! [] (code-style.png)
<! -
`` `js
função pow (x, n) {
Deixe o resultado = 1;

para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}

Deixe x = prompt ("x?", "");
Deixe n = prompt ("n?", "");

se (n <0) {
alerta ('Power $ {n} não é suportado,
digite um número inteiro, maior que 0`);
} outro {
alerta (pow (x, n));
}
`` `

->

Agora, vamos discutir as regras e os motivos em detalhes.

Nada é "esculpido em pedra" aqui. Tudo é opcional e pode ser alterado: são regras de codificação, e não dogmas religiosos.

### Suportes de figura

Na maioria dos projetos de JavaScript, os colchetes de figura estão escritos na mesma linha, não na nova linha. Um chamado estilo "egípcio". Há também um espaço antes de um suporte de abertura.

Como isso:

`` `js
se (condição) {
// faça isso
// ...e essa
// ...e essa
}
`` `

Uma construção de linha única é um importante exemplo de borda. Devemos usar colchetes? Se sim, então, onde?

Aqui estão as variantes anotadas, para que você possa avaliar sobre sua legibilidade por conta própria:

<! -
`` `js no-embellecer
se (n <0) {alert ('Power $ {n} não é suportado};}

se (n <0) alerta ('Power $ {n} não é suportado');

se (n <0)
alerta ("Power $ {n} não é suportado");

se (n <0) {
alerta ("Power $ {n} não é suportado");
}
`` `
->
!] (figura-bracket-style.png)

Como um resumo:
- Para um código muito curto, uma linha é aceitável: como `if (cond) return null`.
- Mas uma linha separada para cada declaração entre parênteses geralmente é melhor.

### Comprimento da linha

O comprimento máximo da linha deve ser limitado. Ninguém gosta de observar uma longa linha horizontal. É melhor dividi-lo.

O comprimento máximo da linha é acordado no nível da equipe. Normalmente são 80 ou 120 caracteres.

### Indents

Existem dois tipos de recuos:

- ** Um recuo horizontal: 2 (4) espaços. **

Um recuo horizontal é feito usando 2 ou 4 espaços ou o símbolo "Tab". O que escolher é uma antiga guerra santa. Os espaços são mais comuns hoje em dia.

Uma vantagem dos espaços sobre as abas é que os espaços permitem configurações mais flexíveis de recortes do que o símbolo "Tab".

Por exemplo, podemos alinhar os argumentos com o suporte de abertura, como este:

`` `js no-embellecer
mostrar (parâmetros,
alinhado, // 5 espaços de preenchimento à esquerda
1,
depois de,
outro
) {
// ...
}
`` `

- ** Um recuo vertical: linhas vazias para dividir o código em blocos lógicos. **

Mesmo uma única função pode ser dividida em blocos lógicos. No exemplo abaixo, a inicialização de variáveis, o loop principal e o retorno do resultado são divididos verticalmente:

`` `js
função pow (x, n) {
Deixe o resultado = 1;
// <-
para (vamos i = 0; i <n; i ++) {
resultado * = x;
}
// <-
resultado de retorno;
}
`` `

Insira uma nova linha extra onde ajuda a tornar o código mais legível. Não deve haver mais de nove linhas de código sem uma indentação vertical.

### Um ponto e vírgula

Um ponto e vírgula deve estar presente após cada indicação. Mesmo que possivelmente possa ser ignorado.

Existem idiomas em que um ponto-e-vírgula é verdadeiramente opcional. Raramente é usado lá. Mas, em JavaScript, há poucos casos em que um intervalo de linha às vezes não é interpretado como um ponto-e-vírgula. Isso deixa um lugar para erros de programação.

À medida que você se torna mais maduro como programador, você pode escolher um estilo sem semicolona, ​​como [StandardJS] (https://standardjs.com/), mas isso é apenas quando você conhece bem o JavaScript e entende possíveis armadilhas.

### Níveis de nidificação

Não deve haver muitos níveis de nidificação.

Às vezes, é uma boa idéia usar a diretiva ["continuar"] (info: while-for # continue) no loop para evitar o ninho extra em `if (...) {...}`:

Ao invés de:

`` `js
para (vamos i = 0; i <10; i ++) {
se (cond) {
... // <- mais um nível de nidificação
}
}
`` `

Nós podemos escrever:

`` `js
para (vamos i = 0; i <10; i ++) {
se (! cond) *! * continue * /! *;
... // <- sem nível de nidificação extra
}
`` `

Uma coisa semelhante pode ser feita com `if / else` e` return`.

Por exemplo, duas construções abaixo são idênticas.

O primeiro:

`` `js
função pow (x, n) {
se (n <0) {
alerta ("Negativo 'n' não é suportado");
} outro {
Deixe o resultado = 1;

para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}
}
`` `

E isto:

`` `js
função pow (x, n) {
se (n <0) {
alerta ("Negativo 'n' não é suportado");
Retorna;
}

Deixe o resultado = 1;

para (vamos i = 0; i <n; i ++) {
resultado * = x;
}

resultado de retorno;
}
`` `

... Mas o segundo é mais legível, porque o "caso de borda" de `n <0` é tratado antecipadamente, e então temos o fluxo de código" principal ", sem um aninhamento adicional.

## Funções abaixo do código

Se você estiver escrevendo várias funções "auxiliares" e o código para usá-las, então há três maneiras de colocá-las.

1. Funções acima do código que as usa:

`` `js
// *! * declarações de função * /! *
function createElement () {
...
}

Função Seitherander (LM) {
...
}

function walkAround () {
...
}

// *! * o código que os usa * /! *
deixe element = createElement ();
Seetherander (LM);
andar poraí();
`` `
2. Código primeiro, depois funções

`` `js
// *! * o código que usa as funções * /! *
deixe element = createElement ();
Seetherander (LM);
andar poraí();

// --- *! * funções auxiliares * /! * ---

function createElement () {
...
}

Função Seitherander (LM) {
...
}

function walkAround () {
...
}
`` `
3. Misto: uma função é descrita onde é usada pela primeira vez.

Na maior parte do tempo, a segunda variante é preferida.

Isso porque, ao ler um código, primeiro queremos saber "o que faz". Se o código for primeiro, então ele fornece essa informação. E então talvez não precisemos ler as funções, especialmente se seus nomes são adequados ao que estão fazendo.

## Guias de estilo

Um guia de estilo contém regras gerais sobre "como escrever": quais citações para usar, quantos espaços para recuar, onde colocar cortes de linha, etc. Muitas coisas menores.

No total, quando todos os membros de uma equipe usam o mesmo guia de estilo, o código parece uniforme. Não importa quem da equipe escreveu, ainda é o mesmo estilo.

Certamente, uma equipe pode pensar em um guia de estilo. Mas a partir de agora, não é necessário. Existem muitos guias de estilo tentados e elaborados, que são fáceis de adotar.

Por exemplo:

- [Guia de estilo do JavaScript do Google] (https://google.github.io/styleguide/javascriptguide.xml)
- [Airbnb JavaScript Style Guide] (https://github.com/airbnb/javascript)
- [Idiomatic.JS] (https://github.com/rwaldron/idiomatic.js)
- [StandardJS] (https://standardjs.com/)
- (há mais)

Se você é um desenvolvedor novato, então você poderia começar com a cheatsheet acima no capítulo e, mais tarde, procure os guias de estilo para escolher os princípios comuns e talvez escolha um.

## linters automatizados

Existem ferramentas que podem verificar o estilo do código automaticamente. Eles são chamados de "linters".

A grande coisa sobre eles é que a verificação de estilo também encontra alguns erros, como um erro de digitação em uma variável ou nome de função.

Por isso, recomenda-se instalar um, mesmo se você não quiser manter um "estilo de código". Eles ajudam a encontrar erros de digitação - e isso já é bom o suficiente.

As ferramentas mais conhecidas são:

- [JSLint] (http://www.jslint.com/) - um dos primeiros linters.
- [JSHint] (http://www.jshint.com/) - mais configurações do que JSLint.
- [ESLint] (http://eslint.org/) - provavelmente o mais novo.

Todos eles podem fazer o trabalho. O autor usa [ESLint] (http://eslint.org/).

A maioria dos linters está integrada com editores: apenas habilite o plugin no editor e configure o estilo.

Por exemplo, para ESLint você deve fazer o seguinte:

1. Instale [Node.JS] (https://nodejs.org/).
2. Instale ESLint com o comando `npm install -g eslint` (npm é o instalador do pacote Node.JS).
3. Crie um arquivo de configuração chamado `.eslintrc` na raiz do seu projeto JavaScript (na pasta que contém todos os seus arquivos).

Aqui está um exemplo de `.eslintrc`:

`` `js
{
"se estende": "Eslint: recomendado",
"env": {
"navegador": verdadeiro
"nó": verdadeiro
"es6": verdadeiro
},
"regras": {
"no-console": 0,
},
"travessão": 2
}
`` `

Aqui, a diretiva `" extends "significa que baseamos no conjunto de configurações" eslint: recommended ", e então especificamos o nosso.

Em seguida, instale / active o plugin para o seu editor que se integra com o ESLint. A maioria dos editores o possui.

É possível baixar conjuntos de regras de estilo da Web e estendê-las. Veja <http://eslint.org/docs/user-guide/getting-started> para obter mais detalhes sobre a instalação.

Usar um linter tem um ótimo efeito colateral: os linters capturam erros de digitação. Por exemplo, quando uma variável indefinida é acessada, um linter detecta-lo e (se integrado com um editor) o destaca. Na maioria dos casos, é um tipo de erro. Então, podemos corrigi-lo imediatamente.

Por essa razão, mesmo que você não esteja preocupado com os estilos, é altamente recomendável usar um linter.

Além disso, certos IDEs suportam o armazenamento interno, que também pode ser bom, mas não tão ajustável como o ESLint.

## Resumo

Todas as regras de sintaxe deste capítulo e os guias de estilo visam aumentar a legibilidade, de modo que todos eles são discutíveis.

Quando pensamos em "como escrever melhor?", O único critério é "o que torna o código mais legível e mais fácil de entender", o que ajuda a evitar erros? " Essa é a principal coisa a ter em mente ao escolher o estilo ou discutir qual é o melhor.

Leia guias de estilo para ver as últimas ideias sobre isso e siga as que você achou melhor.
