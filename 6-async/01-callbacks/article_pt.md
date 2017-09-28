

# Introdução: callbacks

Muitas ações em Javascript são * assíncronas *.

Por exemplo, veja a função `loadScript (src)`:

`` `js
função loadScript (src) {
let script = document.createElement ('script');
script.src = src;
document.head.append (script);
}
```

O objetivo da função é carregar um novo script. Quando ele adiciona o `<script src =" ... ">` ao documento, o navegador carrega e executa.

Podemos usá-lo assim:

`` `js
// carrega e executa o script
loadScript ('/ my / script.js');
```

A função é chamada de "assíncrona", porque a ação (carregamento de script) não termina agora, mas depois.

A chamada inicia o carregamento do script, então a execução continua. Enquanto o script está sendo carregado, o código abaixo pode terminar de ser executado, e se o carregamento demorar, outros scripts podem ser executados enquanto isso.

`` `js
loadScript ('/ my / script.js');
// o código abaixo loadScript não aguarda a conclusão do script
// ...
```

Agora, digamos que queremos usar o novo script quando ele carrega. Provavelmente, declara novas funções, então gostaríamos de executá-las.

... Mas se fizermos isso imediatamente após a chamada `loadScript (...)`, isso não funcionaria:

`` `js
loadScript ('/ my / script.js'); // o script tem "function newFunction () {...}"

*!*
newFunction (); // nenhuma função desse tipo
*/!*
```

Naturalmente, o navegador provavelmente não teve tempo para carregar o script. Portanto, a chamada imediata para a nova função falha. A partir de agora, a função `loadScript` não fornece uma maneira de rastrear a conclusão da carga. O script carrega e, eventualmente, corre, isso é tudo. Mas gostaríamos de saber quando acontece, usar novas funções e variáveis ​​desse script.

Vamos adicionar uma função `callback` como um segundo argumento para` load Script` que deve ser executado quando o script é carregado:

`` `js
função loadScript (src, *! * callback * /! *) {
let script = document.createElement ('script');
script.src = src;

*!*
script.onload = () => retorno de chamada (script);
*/!*

document.head.append (script);
}
```

Agora, se quisermos chamar novas funções do script, devemos escrever isso no retorno de chamada:

`` `js
loadScript ('/ my / script.js', function () {
// o retorno de chamada é executado após o script ser carregado
newFunction (); // então agora funciona
...
});
```

Essa é a idéia: o segundo argumento é uma função (geralmente anônima) que é executada quando a ação é concluída.

Aqui está um exemplo executável com um script real:

`` `js run
função loadScript (src, callback) {
let script = document.createElement ('script');
script.src = src;
script.onload = () => retorno de chamada (script);
document.head.append (script);
}

*!*
Loadscript ('Http: //cdnjas.cloudflare.com/Ajax/Libs/loadshjs/3.2.0/loadshjs', script => {
alerta (`Cool, o $ {script.src} está carregado`);
alerta( _ ); // função declarada no script carregado
});
*/!*
```

Isso é chamado de "estilo baseado em chamada" de programação assíncrona. Uma função que faz algo de forma assíncrona deve fornecer um argumento de "retorno de chamada" onde colocamos a função para ser executada após a conclusão.

Aqui, fizemos isso no `loadScript`, mas é claro que é uma abordagem geral.

## Callback em retorno de chamada

Como carregar dois scripts sequencialmente: o primeiro e depois o segundo depois dele?

A solução natural seria colocar a segunda chamada `loadScript` dentro do retorno de chamada, assim:

`` `js
loadScript ('/ my / script.js', função (script) {

alerta ("Cool, o $ {script.src} está carregado, vamos carregar um mais");

*!*
loadScript ('/ my / script2.js', função (script) {
alerta (`Cool, o segundo script é carregado ');
});
*/!*

});
```

Depois que o `loadScript 'externo estiver completo, o retorno de chamada inicia o interior.

... E se quisermos um script mais?

`` `js
loadScript ('/ my / script.js', função (script) {

loadScript ('/ my / script2.js', função (script) {

*!*
loadScript ('/ my / script3.js', function (script) {
// ... continue depois de todos os scripts serem carregados
});
*/!*

})

});
```

Então, cada nova ação está dentro de uma chamada de retorno. Isso é bom para algumas ações, mas não é bom para muitos, então veremos outras variantes em breve.

## Erros de manipulação

Nos exemplos acima, não consideramos erros. E se o carregamento do script falhar? Nosso retorno de chamada deve ser capaz de reagir a isso.

Aqui está uma versão melhorada do `loadScript` que rastreia erros de carregamento:

`` `js run
função loadScript (src, callback) {
let script = document.createElement ('script');
script.src = src;

*!*
script.onload = () => retorno de chamada (nulo, script);
script.onerror = () => retorno de chamada (novo erro (`Erro de carga do script para $ {src}`));
*/!*

document.head.append (script);
}
```

Ele chama `callback (null, script)` para carregar com êxito e 'callback (erro)' caso contrário.

O uso:
`` `js
loadScript ('/ my / script.js', função (erro, script) {
se (erro) {
// lidar com erro
} outro {
// script carregado com sucesso
}
});
```

Mais uma vez, a receita que usamos para `loadScript 'é bastante comum. É chamado o estilo de "retorno de erro primeiro".

A convenção é:
1. O primeiro argumento de `callback` é reservado para um erro se ocorrer. Em seguida, chamado callback (err) é chamado.
2. O segundo argumento (e os próximos se necessário) são para o resultado bem-sucedido. Em seguida, o callback (null, result1, result2 ...) `é chamado.

Portanto, a única função 'callback` é usada tanto para reportar erros quanto para reverter os resultados.

## Pyramid of doom

Desde o primeiro aspecto, é uma maneira viável de codificação assíncrona. E, na verdade, é. Para uma ou talvez duas chamadas aninhadas, parece bem.

Mas, para várias ações assíncronas que seguem uma após a outra, teremos um código como este:

`` `js
loadScript ('1.js', função (erro, script) {

se (erro) {
handleError (erro);
} outro {
// ...
loadScript ('2.js', função (erro, script) {
se (erro) {
handleError (erro);
} outro {
// ...
loadScript ('3.js', função (erro, script) {
se (erro) {
handleError (erro);
} outro {
*!*
// ... continue depois que todos os scripts são carregados (*)
*/!*
}
});

}
})
}
});
```

No código acima:
1. Carregamos `1.js`, então, se não houver nenhum erro.
2. Nós carregamos `2.js`, então, se não houver nenhum erro.
3. Carregamos `3.js`, então, se não houver nenhum erro - faça outra coisa` (*) `.

À medida que as chamadas se tornam mais aninhadas, o código torna-se mais profundo e cada vez mais difícil de gerenciar, especialmente se tivermos um código real em vez de `...`, que pode incluir mais loops, declarações condicionais e assim por diante.

Às vezes é chamado de "inferno de retorno" ou "pirâmide da desgraça".

! [] (callback-hell.png)

A "pirâmide" de chamadas aninhadas cresce para a direita com cada ação assíncrona. Logo, espira fora de controle.

Então, esta maneira de codificar não é muito boa.

Podemos tentar aliviar o problema fazendo de cada ação uma função autônoma, como esta:

`` `js
loadScript ('1.js', passo 1);

função step1 (erro, script) {
se (erro) {
handleError (erro);
} outro {
// ...
loadScript ('2.js', passo 2);
}
}

função step2 (erro, script) {
se (erro) {
handleError (erro);
} outro {
// ...
loadScript ('3.js', passo 3);
}
}

função passo3 (erro, script) {
se (erro) {
handleError (erro);
} outro {
// ... continue depois que todos os scripts são carregados (*)
}
};
```

Vejo? Ele faz o mesmo, e agora não há aninhamento profundo, porque fizemos de cada ação uma função de nível superior separada.

Ele funciona, mas o código parece uma planilha separada. É difícil de ler, você provavelmente percebeu isso. É preciso saltar os olhos entre as peças enquanto a lê. Isso é inconveniente, especialmente o leitor não está familiarizado com o código e não sabe por onde saltar os olhos.

Além disso, as funções denominadas `passo *` são todas de um único uso, elas são criadas apenas para evadir a "pirâmide da destruição". Ninguém vai reutilizá-los fora da cadeia de ação. Então, há um pouco de espaço para nome que está aqui.

Gostaríamos de ter algo melhor.

Por sorte, existem outras maneiras de evadir tais pirâmides. Uma das melhores maneiras é usar "promessas", descritas no próximo capítulo.
