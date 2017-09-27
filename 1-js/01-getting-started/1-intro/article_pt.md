# Uma introdução ao JavaScript

Vamos ver o que é tão especial sobre o JavaScript, o que podemos conseguir com ele e quais outras tecnologias funcionam bem com ele.

## O que é o JavaScript?

* O JavaScript * foi inicialmente criado para * "tornar as páginas vivas" *.

Os programas neste idioma são chamados * scripts *. Eles podem ser escritos diretamente no HTML e executar automaticamente à medida que a página é carregada.

Scripts são fornecidos e executados como um texto simples. Eles não precisam de uma preparação especial ou uma compilação para executar.

Neste aspecto, o JavaScript é muito diferente de outro idioma chamado [Java] (http://en.wikipedia.org/wiki/Java).

`` `smart header =" Por que <u> Java </ u> Script? "
Quando o JavaScript foi criado, inicialmente tinha outro nome: "LiveScript". Mas a linguagem Java era muito popular naquela época, então decidiu-se que o posicionamento de um novo idioma como um "irmão mais novo" do Java ajudaria.

Mas, à medida que evoluiu, o JavaScript tornou-se uma linguagem totalmente independente, com sua própria especificação chamada [ECMAScript] (http://en.wikipedia.org/wiki/ECMAScript), e agora não tem relação com o Java.
`` `

Atualmente, o JavaScript pode ser executado não só no navegador, mas também no servidor, ou mesmo em qualquer dispositivo onde exista um programa especial chamado [o mecanismo de JavaScript] (https://en.wikipedia.org/wiki/JavaScript_engine) .

O navegador possui um mecanismo incorporado, às vezes também é chamado de "máquina virtual JavaScript".

Diferentes motores têm diferentes "nomes de código", por exemplo:

- [V8] (https://en.wikipedia.org/wiki/V8_ (JavaScript_engine)) - no Chrome e no Opera.
- [SpiderMonkey] (https://en.wikipedia.org/wiki/SpiderMonkey) - no Firefox.
- ... Existem outros nomes de código como "Trident", "Chakra" para diferentes versões do IE, "ChakraCore" para Microsoft Edge, "Nitro" e "SquirrelFish" para Safari etc.

Os termos acima são bons para lembrar, porque eles são usados ​​em artigos para desenvolvedores na internet. Nós também os usaremos. Por exemplo, se "um recurso X é suportado pelo V8", então provavelmente funciona no Chrome e no Opera.

`` `smart header =" Como funcionam os motores? "

Os motores são complicados. Mas o básico é fácil.

1. O motor (incorporado se for um navegador) lê ("analisa") o script.
2. Então ele converte ("compila") o script para o idioma da máquina.
3. E, em seguida, o código da máquina é executado, muito rápido.

O motor aplica otimizações em todas as etapas do processo. Ele mesmo assiste ao script compilado conforme ele é executado, analisa os dados que o atravessam e aplica otimizações ao código da máquina com base nesse conhecimento. No final, os scripts são bastante rápidos.
`` `

## O que o JavaScript do navegador pode fazer?

O JavaScript moderno é uma linguagem de programação "segura". Não fornece acesso de baixo nível à memória ou à CPU, porque foi inicialmente criado para navegadores que não o exigem.

Os recursos dependem muito do ambiente que executa JavaScript. Por exemplo, [Node.JS] (https://wikipedia.org/wiki/Node.js) suporta funções que permitem JavaScript para ler / escrever arquivos arbitrários, executar solicitações de rede etc.

O JavaScript no navegador pode fazer tudo relacionado à manipulação da página, a interação com o usuário e o servidor web.

Por exemplo, JavaScript no navegador é capaz de:

- Adicionar novo HTML à página, alterar o conteúdo existente, modificar estilos.
- Reagir às ações do usuário, executar em cliques do mouse, movimentos de ponteiro, pressionar teclas.
- Enviar solicitações através da rede para servidores remotos, baixar e fazer upload de arquivos (o chamado [AJAX] (https://en.wikipedia.org/wiki/Ajax_ (programação)) e [COMET] (https: // pt). tecnologias wikipedia.org/wiki/Comet_(programming))).
- Obter e definir cookies, fazer perguntas ao visitante, mostrar mensagens.
- Lembre-se dos dados no lado do cliente ("armazenamento local").

## O que não pode fazer o JavaScript no navegador?

As habilidades do JavaScript no navegador são limitadas por causa da segurança do usuário. O objetivo é evitar que uma página mal-intencionada acesse informações privadas ou prejudique os dados do usuário.

Os exemplos de tais restrições são:

- O JavaScript em uma página da Web pode não ler / escrever arquivos arbitrários no disco rígido, copiá-los ou executar programas. Não possui acesso direto às funções do sistema operacional.

Os navegadores modernos permitem que ele funcione com arquivos, mas o acesso é limitado e apenas fornecido se o usuário fizer determinadas ações, como "soltar" um arquivo em uma janela do navegador ou selecioná-lo através de uma tag `<input>`.

Existem maneiras de interagir com câmera / microfone e outros dispositivos, mas eles exigem permissão explícita de um usuário. Portanto, uma página habilitada para JavaScript pode não habilmente ativar uma câmera web, observar os ambientes e enviar as informações para a [NSA] (https://en.wikipedia.org/wiki/National_Security_Agency).
- Diferentes separadores / janelas geralmente não sabem um sobre o outro. Às vezes, eles fazem, por exemplo, quando uma janela usa o JavaScript para abrir a outra. Mas, mesmo neste caso, o JavaScript de uma página pode não acessar o outro se eles vierem de sites diferentes (de um domínio, protocolo ou porta diferente).

Isso é chamado de "A mesma Política de Origem". Para contornar isso, * ambas as páginas * devem conter um código JavaScript especial que lida com a troca de dados.

A limitação é novamente para a segurança do usuário. Uma página de `http: // anysite.com` que um usuário abriu não deve acessar outra guia do navegador com a URL` http: // gmail.com` e roubar informações a partir daí.
- O JavaScript pode se comunicar facilmente pela rede para o servidor do qual a página atual veio. Mas a capacidade de receber dados de outros sites / domínios é prejudicada. Embora possivel, requer um acordo explícito (expresso em cabeçalhos HTTP) do lado remoto. Mais uma vez, são limitações de segurança.

! [] (limits.png)

Esses limites não existem se o JavaScript for usado fora do navegador, por exemplo, em um servidor. Os navegadores modernos também permitem instalar o plugin / extensões que podem obter permissões estendidas.

## O que torna o JavaScript único?

Existem pelo menos * três * coisas excelentes sobre o JavaScript:

`` `comparar
+ Integração completa com HTML / CSS.
+ Simples coisas feitas simplesmente.
+ Suportado por todos os navegadores principais e habilitado por padrão.
`` `

Combinado, estas três coisas existem apenas em JavaScript e nenhuma outra tecnologia de navegador.

Isso é o que torna o JavaScript único. É por isso que é a ferramenta mais difundida para criar interfaces de navegador.

Enquanto planeja aprender uma nova tecnologia, é benéfico verificar suas perspectivas. Então, vamos para as tendências modernas que incluem novos idiomas e habilidades do navegador.


## Languages ​​"over" JavaScript

A sintaxe do JavaScript não é adequada às necessidades de todos. Pessoas diferentes querem características diferentes.

Isso é de se esperar, porque projetos e requisitos são diferentes para todos.

Recentemente, surgiu uma infinidade de novos idiomas, que são * transpilaram * (convertido) para JavaScript antes de serem executados no navegador.

As ferramentas modernas tornam a transpilação muito rápida e transparente, permitindo que os desenvolvedores se codificem em outro idioma e a conversão automática "sob o capô".

Exemplos de tais linguagens:

- [CoffeeScript] (http://coffeescript.org/) é um "açúcar de sintaxe" para JavaScript, ele apresenta uma sintaxe mais curta, permitindo escrever código mais preciso e claro. Geralmente, os devedores Ruby gostam.
- [TypeScript] (http://www.typescriptlang.org/) concentra-se na adição de "digitação de dados rigorosa", para simplificar o desenvolvimento e suporte de sistemas complexos. É desenvolvido pela Microsoft.
- [Dart] (https://www.dartlang.org/) é um idioma autônomo que possui seu próprio mecanismo que é executado em ambientes que não são navegador (como aplicativos para dispositivos móveis). Inicialmente foi oferecido pelo Google como um substituto para o JavaScript, mas, a partir de agora, os navegadores exigem que ele seja transpilar para o JavaScript, assim como aqueles acima.

Há mais. Claro que, mesmo que usemos uma dessas línguas, também devemos saber o JavaScript, para realmente entender o que estamos fazendo.

## Resumo

- O JavaScript foi inicialmente criado como um idioma apenas para o navegador, mas agora também é usado em muitos outros ambientes.
- Neste momento, o JavaScript tem uma posição única como o idioma de navegador mais amplamente adotado com integração total com HTML / CSS.
- Existem muitos idiomas que são "transpilados" para JavaScript e fornecem determinados recursos. Recomenda-se dar uma olhada neles, pelo menos brevemente, depois de dominar o JavaScript.
