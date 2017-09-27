A fita de imagens pode ser representada como lista de imagens `ul / li`` <img> `.

Normalmente, essa fita é larga, mas colocamos um `` div divisão 'de tamanho fixo para "cortá-lo", de modo que apenas uma parte da fita é visível:

! [] (carousel1.png)

Para que a lista mostre horizontalmente, precisamos aplicar as propriedades CSS corretas para `<li>`, como `display: inline-block`.

Para `<img>` devemos também ajustar `display`, porque, por padrão, é` inline`. Há espaço extra reservado sob elementos `inline` para" tails de cartas ", para que possamos usar` display: block` para removê-lo.

Para fazer a rolagem, podemos mudar `<ul>`. Existem muitas maneiras de fazê-lo, por exemplo, alterando `margin-left` ou (melhor desempenho) use` transform: translateX () `:

! [] (carousel2.png)

O `<div>` externo tem uma largura fixa, então as imagens "extra" são cortadas.

O carrossel inteiro é um "componente gráfico" autônomo na página, então seria melhor envolvê-lo em um único <div class = "carousel"> `e coisas de estilo dentro dele.
