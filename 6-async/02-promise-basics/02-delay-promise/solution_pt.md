`` `js run
atraso de função (ms) {
devolver nova Promise (resolve => setTimeout (resolve, ms));
}

atrasar (3000) .then (() => alert ('runs after 3 seconds'));
```

Observe que, nesta tarefa, "resolver" é chamado sem argumentos. Não devolvemos nenhum valor de "atraso", apenas assegure o atraso.
