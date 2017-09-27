O HTML na tarefa está incorreto. Esse é o motivo da coisa estranha.

O navegador precisa corrigi-lo automaticamente. Mas pode não haver texto dentro da `<tabela>`: de acordo com a especificação, somente as tags específicas da tabela são permitidas. Então, o navegador adiciona `" aaa "` * antes * a `<tabela>`.

Agora é óbvio que quando removemos a mesa, ela permanece.

A questão pode ser facilmente respondida explorando DOM usando as ferramentas do navegador. Eles mostram `" aaa "` antes da `<tabela>`.

O padrão HTML especifica detalhadamente como processar HTML incorreto, e esse comportamento do navegador está correto.
