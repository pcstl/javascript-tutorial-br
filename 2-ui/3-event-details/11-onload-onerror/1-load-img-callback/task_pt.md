importância: 4

---

# Carrega imagens com um retorno de chamada

Normalmente, as imagens são carregadas quando são criadas. Então, quando adicionamos `<img>` à página, o usuário não vê a imagem imediatamente. O navegador precisa carregá-lo primeiro.

Para mostrar uma imagem imediatamente, podemos criá-la "antecipadamente", assim:

`` `js
deixe img = document.createElement ('img');
img.src = 'my.jpg';
`` `

O navegador começa a carregar a imagem e a lembra no cache. Mais tarde, quando a mesma imagem aparecer no documento (não importa como), ele aparece imediatamente.

** Crie uma função `preloadImages (sources, callback)` que carrega todas as imagens da matriz `sources` e, quando estiver pronto, executa` callback`. **

Por exemplo, isso mostrará um 'alerta' depois que as imagens forem carregadas:

`` `js
função carregada () {
alerta ("Imagens carregadas")
}

preloadImages (["1.jpg", "2.jpg", "3.jpg"], carregado);
`` `

Em caso de erro, a função ainda deve assumir a imagem "carregada".

Em outras palavras, o `callback` é executado quando todas as imagens são carregadas ou erradas.

A função é útil, por exemplo, quando planejamos mostrar uma galeria com muitas imagens roláveis ​​e queremos ter certeza de que todas as imagens são carregadas.

No documento de origem, você pode encontrar links para testar imagens e também o código para verificar se eles estão carregados ou não. Deve produzir `300`.
