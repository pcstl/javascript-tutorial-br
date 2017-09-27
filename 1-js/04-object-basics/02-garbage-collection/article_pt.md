# Coleta de lixo

O gerenciamento de memória em JavaScript é executado de forma automática e invisível para nós. Criamos primitivas, objetos, funções ... Tudo isso leva memória.

O que acontece quando algo não é mais necessário? Como o motor de JavaScript descobre e limpa?

[cortar]

## Acessibilidade

O conceito principal de gerenciamento de memória em JavaScript é * reachability *.

Simplificando, os valores "acessíveis" são aqueles que são acessíveis ou utilizáveis ​​de alguma forma. Eles são garantidos para serem armazenados na memória.

1. Existe um conjunto básico de valores inerentemente acessíveis, que não podem ser excluídos por razões óbvias.

Por exemplo:

- Variáveis ​​e parâmetros locais da função atual.
- Variáveis ​​e parâmetros para outras funções na cadeia atual de chamadas aninhadas.
- Variáveis ​​globais.
- (há outros, internos também)

Esses valores são chamados * roots *.

2. Qualquer outro valor é considerado acessível se for acessível a partir de uma raiz por uma referência ou por uma cadeia de referências.

Por exemplo, se houver um objeto em uma variável local e esse objeto tiver uma propriedade referenciando outro objeto, esse objeto é considerado acessível. E aqueles que ele faz referência também são acessíveis. Exemplos detalhados a seguir.

Existe um processo em segundo plano no mecanismo de JavaScript denominado [coletor de lixo] (https://en.wikipedia.org/wiki/Garbage_collection_ (computer_science)). Ele monitora todos os objetos e remove aqueles que se tornaram inalcançáveis.

## Um exemplo simples

Aqui está o exemplo mais simples:

`` `js
// usuário tem uma referência ao objeto
Deixe o usuário = {
Nome: "John"
};
`` `

! [] (memory-user-john.png)

Aqui, a seta representa uma referência de objeto. A variável global `" usuário "` faz referência ao objeto `{nome: 'João'}` `(chamaremos John por brevidade). A propriedade `` name '' de John armazena um primitivo, então é pintado dentro do objeto.

Se o valor de `usuário 'for substituído, a referência será perdida:

`` `js
usuário = nulo;
`` `

! [] (memory-user-john-lost.png)

Agora, John torna-se inalcançável. Não há como acessá-lo, sem referências a ele. O coletor de lixo irá juntar os dados e liberar a memória.

## Duas referências

Agora vamos imaginar que copiamos a referência de `usuário 'para' admin ':

`` `js
// usuário tem uma referência ao objeto
Deixe o usuário = {
Nome: "John"
};

*! *
Deixe admin = user;
* /! *
`` `

! [] (memory-user-john-admin.png)

Agora, se fizermos o mesmo:
`` `js
usuário = nulo;
`` `

... Então o objeto ainda é acessível através da variável global `admin`, então está na memória. Se substituirmos 'admin' também, então ele pode ser removido.

## Interlinked objects

Agora, um exemplo mais complexo. A família:

`` `js
Funcionar se casar (homem, mulher) {
woman.husband = man;
man.wife = woman;

Retorna {
pai: homem
mãe: mulher
}
}

Deixe family = marry ({
Nome: "John"
}, {
nome: "Ann"
});
`` `

Função `casar`" casa "dois objetos, dando-lhes referências entre si e retorna um novo objeto que os contém.

A estrutura de memória resultante:

! [] (family.png)

A partir de agora, todos os objetos são acessíveis.

Agora vamos remover duas referências:

`` `js
delete family.father;
excluir family.mother.husband;
`` `

!] [family-delete-refs.png)

Não basta excluir apenas uma dessas duas referências, pois todos os objetos ainda podem ser alcançados.

Mas se excluímos ambos, então podemos ver que John não tem nenhuma referência recebida:

! [] (family-no-father.png)

As referências de saída não são importantes. Somente os recebidos podem tornar acessível um objeto. Então, John está agora inacessível e será removido da memória com todos os seus dados que também se tornaram inacessíveis.

Após a coleta de lixo:

! [] (family-no-father-2.png)

## Ilha inacessível

É possível que toda a ilha de objetos interligados se torne inacessível e seja removida da memória.

O objeto de origem é o mesmo que acima. Então:

`` `js
family = null;
`` `

A imagem na memória torna-se:

! [] (family-no-family.png)

Este exemplo demonstra a importância do conceito de acessibilidade.

É óbvio que John e Ann ainda estão ligados, ambos têm referências recebidas. Mas isso não é suficiente.

O antigo "objeto" familiar foi desvinculado da raiz, não há mais referência a ele, de modo que toda a ilha fica inacessível e será removida.

## Algoritmos internos

O algoritmo básico de coleta de lixo é chamado de "mark-and-sweep".

As seguintes etapas de "coleta de lixo" são realizadas regularmente:

- O coletor de lixo toma raízes e "marca" (os lembra).
- Então ele visita e "marca" todas as referências deles.
- Então ele visita objetos marcados e marca * suas * referências. Todos os objetos visitados são lembrados, para não visitar o mesmo objeto duas vezes no futuro.
- ... E assim por diante até que haja referências não visitadas (acessíveis a partir das raízes).
- Todos os objetos, exceto os marcados, são removidos.

Por exemplo, deixe nossa estrutura de objetos parecer assim:

! [] (lixo-coleta-1.png)

Podemos ver claramente uma "ilha inacessível" no lado direito. Agora vejamos como o coletor de lixo "mark-and-sweep" lida com ele.

O primeiro passo marca as raízes:

! [] (lixo-coleta-2.png)

Em seguida, suas referências são marcadas:

! [] (lixo-coleta-3.png)

... E suas referências, enquanto possível:

!] [] (lixo-coleta-4.png)

Agora, os objetos que não puderam ser visitados no processo são considerados inacessíveis e serão removidos:

! [] (lixo-coleta-5.png)

Esse é o conceito de como a coleta de lixo funciona.

Os motores de JavaScript aplicam muitas otimizações para fazê-lo funcionar mais rápido e não afetar a execução.

Algumas das otimizações:

- ** Coleção geracional ** - os objetos são divididos em dois conjuntos: "novos" e "velhos". Muitos objetos aparecem, fazem seu trabalho e morrem rápido, eles podem ser limpos de forma agressiva. Aqueles que sobrevivem por tempo suficiente, tornam-se "velhos" e são examinados com menos frequência.
- ** Cobrança incremental ** - se houver muitos objetos, e tentamos caminhar e marcar todo o objeto configurado de uma vez, pode levar algum tempo e apresentar atrasos visíveis na execução. Então, o mecanismo tenta dividir a coleção de lixo em pedaços. Em seguida, as peças são executadas uma a uma, separadamente. Isso requer uma contabilidade extra entre eles para rastrear mudanças, mas temos muitos pequenos atrasos em vez de um grande.
- ** Coleta de tempo de inatividade ** - o coletor de lixo tenta executar somente enquanto a CPU está ociosa, para reduzir o possível efeito na execução.

Existem outras otimizações e sabores de algoritmos de coleta de lixo. Tanto quanto eu gostaria de descrevê-los aqui, eu tenho que aguentar, porque diferentes motores implementam diferentes ajustes e técnicas. E, o que é ainda mais importante, as coisas mudam à medida que os motores se desenvolvem, então, aprofundar "antecipadamente", sem uma necessidade real provavelmente não vale a pena. A menos que, é claro, é uma questão de puro interesse, então haverá alguns links para você abaixo.

## Resumo

As principais coisas a serem conhecidas:

- A coleta de lixo é realizada automaticamente. Não podemos forçá-lo nem evitá-lo.
- Os objetos são mantidos na memória enquanto eles são alcançáveis.
- Ser referenciado não é o mesmo que ser acessível (a partir de uma raiz): um pacote de objetos interligados pode tornar-se inacessível como um todo.

Os motores modernos implementam algoritmos avançados de coleta de lixo.

Um livro geral "The Garbage Collection Handbook: The Art of Automatic Memory Management" (R. Jones et al) abrange alguns deles.

Se você está familiarizado com a programação de baixo nível, as informações mais detalhadas sobre o colector de lixo V8 estão no artigo [Um passeio de V8: Garbage Collection] (http://jayconrod.com/posts/55/a-tour-of- v8-lixo-coleta).

[V8 blog] (http://v8project.blogspot.com/) também publica artigos sobre mudanças no gerenciamento de memória de tempos em tempos. Naturalmente, para aprender a coleta de lixo, é melhor você se preparar aprendendo sobre os internos do V8 em geral e ler o blog de [Vyacheslav Egorov] (http://mrale.ph) que trabalhou como um dos engenheiros da V8. Estou dizendo: "V8", porque está melhor coberto com artigos na internet. Para outros motores, muitas abordagens são semelhantes, mas a coleta de lixo difere em muitos aspectos.

O conhecimento aprofundado dos motores é bom quando você precisa de otimizações de baixo nível. Seria sábio planejar isso como o próximo passo depois de você estar familiarizado com o idioma.
