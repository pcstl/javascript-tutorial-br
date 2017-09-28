# Encontre pares bbtag

Um "bb-tag" parece "[tag] ... [/ tag]`, onde `tag` é um de:` b`, `url` ou` quote`.

Por exemplo:
```
[b] text [/ b]
[url] http://google.com [/ url]
```

As tags BB podem ser aninhadas. Mas uma etiqueta não pode ser aninhada em si mesma, por exemplo:

```
Normal:
[url] [b] http://google.com [/ b] [/ url]
[citar] [b] text [/ b] [/ quote]

Impossível:
[b] [b] text [/ b] [/ b]
```

As tags podem conter quebras de linha, isso é normal:

```
[citar]
[b] text [/ b]
[/citar]
```

Crie um regex para encontrar todas as tags BB com seus conteúdos.

Por exemplo:

`` `js
Deixe reg = / your regexp / g;

Deixe str = ".. [url] http://google.com [/ url] ..";
alerta (str.match (reg)); // [url] http://google.com [/ url]
```

Se as tags são aninhadas, então precisamos da etiqueta externa (se quisermos, podemos continuar a pesquisa em seu conteúdo):

`` `js
Deixe reg = / your regexp / g;

deixe str = ".. [url] [b] http://google.com [/ b] [/ url] ..";
alerta (str.match (reg)); // [url] [b] http://google.com [/ b] [/ url]
```
