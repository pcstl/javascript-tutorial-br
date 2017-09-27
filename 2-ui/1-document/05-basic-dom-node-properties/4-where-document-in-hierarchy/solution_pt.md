
Podemos ver qual a classe que pertence ao gerá-la, como:

`` `js run
alerta (documento); // [object HTMLDocument]
`` `

Ou:

`` `js run

`` `

Então, `document` é uma instância da classe` HTMLDocument`.

Qual é o seu lugar na hierarquia?

Sim, poderíamos procurar a especificação, mas seria mais rápido descobrir manualmente.

Vamos percorrer a cadeia de protótipo através de `__proto__`.

Como sabemos, os métodos de uma classe estão no "protótipo" do construtor. Por exemplo, `HTMLDocument.prototype` possui métodos para documentos.

Além disso, há uma referência à função do construtor dentro do `protótipo ':

`` `js run
alerta (HTMLDocument.prototype.constructor === HTMLDocument); // verdade
`` `

Para classes integradas em todos os protótipos, há uma referência `construtor 'e podemos obter` constructor.name` para ver o nome da classe. Vamos fazê-lo para todos os objetos na cadeia protótipo `document`:

`` `js run
alerta (HTMLDocument.prototype.constructor.name); // HTMLDocument
alerta (HTMLDocument.prototype .__ proto__.constructor.name); // Documento
alerta (HTMLDocument.prototype .__ proto__.__proto__.constructor.name); // Nó
`` `

Também podemos examinar o objeto usando `console.dir (documento)` e ver esses nomes abrindo `__proto__`. O console leva-os do `construtor 'internamente.
