
# Polyfills

A linguagem JavaScript evolui de forma constante. Novas propostas para o idioma aparecem regularmente, são analisadas e, se consideradas dignas, são anexadas à lista em <https://tc39.github.io/ecma262/> e, em seguida, progridem para a [especificação] (http: // www.ecma-international.org/publications/standards/Ecma-262.htm).

Equipes por trás dos motores JavaScript têm suas próprias ideias sobre o que implementar primeiro. Eles podem decidir implementar propostas que estão em rascunho e adiar coisas que já estão na especificação, porque são menos interessantes ou são mais difíceis de fazer.

Portanto, é bastante comum que um mecanismo implemente apenas a parte do padrão.

Uma boa página para ver o estado atual de suporte para recursos de linguagem é <https://kangax.github.io/compat-table/es6/> (é grande, temos muito a estudar ainda).

# Babel

Quando usamos recursos modernos do idioma, alguns mecanismos podem não suportar esse código. Tal como foi dito, nem todos os recursos são implementados em todos os lugares.

Aqui Babel vem ao resgate.

[Babel] (https://babeljs.io) é um [transpiler] (https://en.wikipedia.org/wiki/Source-to-source_compiler). Ele reescreve o código JavaScript moderno para o padrão anterior.

Na verdade, existem duas partes em Babel:

1. Primeiro, o programa transpiler, que regrava o código. O desenvolvedor executa isso em seu próprio computador. Ele reescreve o código no padrão mais antigo. E, em seguida, o código é entregue ao site para usuários. O sistema de construção de projeto moderno como [webpack] (http://webpack.github.io/) ou [brunch] (http://brunch.io/) fornece meios para executar o transpiler automaticamente em cada mudança de código, de modo que não envolver qualquer perda de tempo do nosso lado.

2. Em segundo lugar, o polyfill.

O transpiler reescreve o código, então os recursos de sintaxe são cobertos. Mas, para novas funções, precisamos escrever um script especial que as implemente. O JavaScript é uma linguagem altamente dinâmica, os scripts podem não apenas adicionar novas funções, mas também modificar incorporados, para que se comportem de acordo com o padrão moderno.

Existe um termo "polyfill" para scripts que "preencha" a lacuna e adicione implementações faltantes.

Dois polyfills interessantes são:
- [babel polyfill] (https://babeljs.io/docs/usage/polyfill/) que suporta muito, mas é grande.
- [polyfill.io] (http://polyfill.io) serviço que permite carregar / construir polifrenos sob demanda, dependendo dos recursos que precisamos.

Então, precisamos configurar o transpiler e adicionar o polyfill para motores antigos para suportar recursos modernos.

Se orientamos para motores modernos e não usamos recursos, exceto aqueles suportados em todos os lugares, então não precisamos usar o Babel.

## Exemplos no tutorial


`` `` online
A maioria dos exemplos são executáveis ​​no local, assim:

`` `js run
alerta ('Pressione o botão "Reproduzir" no canto superior direito para executar');
`` `

Exemplos que usam o JS moderno funcionarão somente se o seu navegador o suportar.
`` ``

`` `desconectado
Como você está lendo a versão offline, os exemplos não são executáveis. Mas eles geralmente trabalham :)
`` `

[Chrome Canary] (https://www.google.com/chrome/browser/canary.html) é bom para todos os exemplos, mas outros navegadores modernos também são excelentes.

Note-se que, na produção, podemos usar a Babel para traduzir o código para adequado para navegadores menos recentes, portanto, não haverá tal limitação, o código será executado em todos os lugares.
