
# O indicador unicode

A bandeira unicode `/.../ u` permite o suporte correto de pares de substituição.

Os pares substituídos são explicados no capítulo <info: string>.

Vamos lembrá-los brevemente aqui. Em suma, normalmente os caracteres são codificados com 2 bytes. Isso nos dá 65536 caracteres no máximo. Mas há mais personagens no mundo.

Portanto, certos caracteres raros são codificados com 4 bytes, como `𝒳` (matemática X) ou` 😄` (um sorriso).

Aqui estão os valores unicode para comparar:

| Caráter | Unicode | Bytes |
|------------|---------|--------|
| `a` | 0x0061 |  2 |
| `≈` | 0x2248 | 2 |
|`𝒳`| 0x1d4b3 | 4 |
| `𝒴` | 0x1d4b4 | 4 |
`` `` `` `` `0x1f604 | 4 |

Então, personagens como `a` e` ≈ 'ocupam 2 bytes, e aqueles raros demoram 4.

O unicode é feito de tal maneira que os caracteres de 4 bytes apenas tenham um significado como um todo.

No passado, o JavaScript não sabia disso, e muitos métodos de string ainda têm problemas. Por exemplo, `length` acha que aqui estão dois caracteres:

`` `js run
alerta ('😄'.length); // 2
alerta ('𝒳'.length); // 2
```

... Mas podemos ver que há apenas um, certo? O ponto é que `length` trata 4 bytes como dois caracteres de 2 bytes. Isso é incorreto, porque eles devem ser considerados apenas juntos (o chamado "par substituto").

Normalmente, expressões regulares também tratam "caracteres longos" como dois de dois bytes.

Isso leva a resultados ímpares, por exemplo, vamos tentar encontrar `pattern: [𝒳𝒴]` na string `subject: 𝒳`:

`` `js run
alerta ('𝒳'.match (/ [𝒳𝒴] /)); // resultado ímpar
```

O resultado seria errado, porque, por padrão, o mecanismo regexp não entende pares de substituição. Ele pensa que `[𝒳𝒴]` não são dois, mas quatro caracteres: a metade esquerda de `𝒳`` (1) `, a metade direita de` 𝒳` `(2)`, a metade esquerda de `𝒴`` (3) `, a metade direita de` 𝒴` `(4)`.

Então, ele encontra a metade esquerda de `𝒳` na string` 𝒳`, não o símbolo inteiro.

Em outras palavras, a pesquisa funciona como `'12'.match (/ [1234] /)` - o `1 é retornado (metade esquerda de` 𝒳`).

A sintaxe `/.../ u` conserta isso. Permite pares de substituição no motor regex, então o resultado está correto:

`` `js run
alerta ('𝒳'.match (/ [𝒳𝒴] / u)); // 𝒳
```

Há um erro que pode acontecer se esquecermos a bandeira:

`` `js run
'𝒳'.match (/ [𝒳-𝒴] /); // SyntaxError: intervalo inválido na classe de caracteres
```

Aqui, o regexp `[𝒳-𝒴]` é tratado como `[12-34]` (onde `2` é a parte direita de` 𝒳` e `3` é a parte esquerda de` 𝒴`) e o intervalo entre duas metades "2" e "3" é inaceitável.

Usar a bandeira faria funcionar direito:

`` `js run
alerta ('𝒴'.match (/ [𝒳-𝒵] / u)); // 𝒴
```

Para finalizar, notemos que, se não lidarmos com pares de substituição, a bandeira não faz nada para nós. Mas no mundo moderno, muitas vezes nos encontramos.
