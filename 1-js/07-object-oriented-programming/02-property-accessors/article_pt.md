
# Promotores e setters de propriedades

Existem dois tipos de propriedades.

O primeiro tipo é * propriedades de dados *. Nós já sabemos como trabalhar com eles. Na verdade, todas as propriedades que usamos até agora eram propriedades de dados.

O segundo tipo de propriedades é algo novo. É * propriedades de acesso *. Eles são essencialmente funções que funcionam ao obter e definir um valor, mas se parecem com propriedades comuns para um código externo.

[cortar]

## Getters e setters

As propriedades do acessador são representadas pelos métodos "getter" e "setter". Em um objeto literal são denotados por `get` e` set`:

`` `js
deixe obj = {
*! * get propName () * /! * {
// getter, o código executado na obtenção obj.propName
},

*! * set propName (value) * /! * {
// setter, o código executado na configuração obj.propName = value
}
};
`` `

O getter funciona quando `obj.propName` é lido, o setter - quando é atribuído.

Por exemplo, temos um objeto 'usuário' com 'nome' e `sobrenome ':

`` `js run
Deixe o usuário = {
Nome: "John",
sobrenome: "Smith"
};
`` `

Agora queremos adicionar uma propriedade "fullName", que deve ser "John Smith". Claro, não queremos copiar e colar informação existente, para que possamos implementá-la como um acessador:

`` `js run
Deixe o usuário = {
Nome: "John",
sobrenome: "Smith"

*! *
Get FullName () {
retornar `$ {this.name} $ {this.sempleame}`;
}
* /! *
};

*! *
alerta (user.fullName); // John Smith
* /! *
`` `

Do lado de fora, uma propriedade de acessório parece normal. Essa é a idéia de propriedades do acessor. Nós não chamamos * `user.fullName` como uma função, nós * lemos * normalmente: o getter corre atrás dos bastidores.

A partir de agora, `FullName` tem apenas um getter. Se tentarmos atribuir `user.fullName =`, haverá um erro.

Vamos consertar isso adicionando um setter para `user.fullName`:

`` `js run
Deixe o usuário = {
Nome: "John",
sobrenome: "Smith"

Get FullName () {
retornar `$ {this.name} $ {this.sempleame}`;
},

*! *
set fullName (value) {
[this.name, this.sendame] = value.split ("");
}
* /! *
};

// set fullName é executado com o valor fornecido.
user.fullName = "Alice Cooper";

alerta (user.name); // Alice
alerta (nome do usuário); // Cooper
`` `

Agora temos uma propriedade "virtual". É legível e gravável, mas na verdade não existe.

`` `cabeçalho inteligente =" As propriedades do acessório só são acessíveis com get / set "
Uma propriedade pode ser uma "propriedade de dados" ou uma "propriedade de acesso", mas não ambas.

Uma vez que uma propriedade é definida com `get prop ()` ou `set prop ()`, é uma propriedade de acesso. Então, deve haver um getter para lê-lo, e deve ser um setter se quisermos atribuí-lo.

Às vezes, é normal que haja apenas um setter ou apenas um getter. Mas a propriedade não será legível ou legível nesse caso.
`` `


## Descriptores do acessório

Os descritores para propriedades de acesso são diferentes - em comparação com as propriedades dos dados.

Para as propriedades do acessador, não existe nenhum "valor" e "gravável", mas, em vez disso, há funções `get` e` set`.

Portanto, um descritor de acesso pode ter:

- ** `get` ** - uma função sem argumentos, que funciona quando uma propriedade é lida,
- ** `set` ** - uma função com um argumento, que é chamado quando a propriedade está configurada,
- ** `enumerable` ** - o mesmo que para propriedades de dados,
- ** `configurável ** - o mesmo que para propriedades de dados.

Por exemplo, para criar um accessor `fullName` com` defineProperty`, podemos passar um descritor com `get` e` set`:

`` `js run
Deixe o usuário = {
Nome: "John",
sobrenome: "Smith"
};

*! *
Object.defineProperty (user, 'fullName', {
obter() {
retornar `$ {this.name} $ {this.sempleame}`;
},

set (value) {
[this.name, this.sendame] = value.split ("");
}
* /! *
});

alerta (user.fullName); // John Smith

para (permitir chave no usuário) alerta (chave);
`` `

Por favor, note mais uma vez que uma propriedade pode ser um acessador ou uma propriedade de dados, e não ambas.

Se tentarmos fornecer tanto `get` e` value` no mesmo descritor, haverá um erro:

`` `js run
*! *
// Erro: descritor de propriedade inválido.
* /! *
Object.defineProperty ({}, 'prop', {
obter() {
retornar 1
},

valor: 2
});
`` `

## Scherter getters / setters

Getters / setters podem ser usados ​​como invólucros sobre valores de propriedade "reais" para obter mais controle sobre eles.

Por exemplo, se queremos proibir nomes muito curtos para `usuário`, podemos armazenar` nome` em uma propriedade especial `_name`. E atribuições de filtro no setter:

`` `js run
Deixe o usuário = {
obter nome () {
devolva this._name;
},

nome do conjunto (valor) {
se (value.length <4) {
alerta ("Nome é muito curto, precisa de pelo menos 4 caracteres");
Retorna;
}
this._name = value;
}
};

user.name = "Pete";
alerta (user.name); // Pete

user.name = ""; // Nome muito curto...
`` `

Tecnicamente, o código externo ainda pode acessar o nome diretamente usando `user._name`. Mas existe um acordo amplamente conhecido de que as propriedades que começam com um sublinhado "_" são internas e não devem ser tocadas fora do objeto.


## Usando para compatibilidade

Uma das grandes idéias por trás de getters e setters - eles permitem assumir o controle sobre uma propriedade de dados "normal" e ajustá-la a qualquer momento.

Por exemplo, começamos a implementar objetos de usuário usando propriedades de dados `name` e` age`:

`` `js
função Usuário (nome, idade) {
this.name = name;
this.age = age;
}

Deixe John = novo Usuário ("John", 25);

alerta (john.age); // 25
`` `

... Mas, mais cedo ou mais tarde, as coisas podem mudar. Em vez de `age`, podemos decidir armazenar` birthday`, porque é mais preciso e conveniente:

`` `js
função Usuário (nome, aniversário) {
this.name = name;
this.birthday = aniversário;
}

deixe john = novo usuário ("John", nova data (1992, 6, 1));
`` `

Agora, o que fazer com o código antigo que ainda usa propriedade `age`?

Podemos tentar encontrar todos esses lugares e corrigi-los, mas isso leva tempo e pode ser difícil de fazer se esse código for escrito por outras pessoas. E, além disso, `age` é uma coisa boa para ter no` usuário`, certo? Em alguns lugares é exatamente o que queremos.

Adicionar um getter para `age` mitiga o problema:

`` `js run no-embellecer
função Usuário (nome, aniversário) {
this.name = name;
this.birthday = aniversário;

*! *
// a idade é calculada a partir da data atual e do aniversário
Object.defineProperty (isto, "idade", {
obter() {
deixe TodayYear = new Date (). getFullYear ();
volte hoje, ano - this.birthday.getFullYear ();
}
});
* /! *
}

deixe john = novo usuário ("John", nova data (1992, 6, 1));

alerta (john.birthday); // aniversário está disponível
alerta (john.age); // ... bem como a idade
`` `

Agora, o código antigo também funciona e nós temos uma boa propriedade adicional.
