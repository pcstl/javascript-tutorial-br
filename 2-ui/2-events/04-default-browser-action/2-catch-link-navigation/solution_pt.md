Esse é um ótimo uso do padrão de delegação de eventos.

Na vida real em vez de perguntar, podemos enviar uma solicitação de "logging" para o servidor que salva as informações sobre onde o visitante deixou. Ou podemos carregar o conteúdo e mostrá-lo diretamente na página (se permitido).

Tudo o que precisamos é pegar o `contents.onclick` e usar` confirm` para perguntar ao usuário. Uma boa idéia seria usar `link.getAttribute ('href')` em vez de `link.href` para o URL. Veja a solução para obter detalhes.
