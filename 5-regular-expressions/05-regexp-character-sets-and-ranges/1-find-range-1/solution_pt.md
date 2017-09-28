Respostas: ** não, sim **.

- No script `subject: Java` não corresponde a nada, porque` pattern: [^ script] `significa" qualquer caractere exceto os dados ". Então, o regexp procura "Java" seguido de um desses símbolos, mas há um fim de string, sem símbolos depois dele.

`` `js run
alerta ("Java" .match (/ Javascript] /)); // nulo
```
- Sim, porque o regexp não é sensível a maiúsculas e minúsculas, a parte `pattern: [^ script]` corresponde ao personagem `" S "`.

`` `js run
alerta ("JavaScript" .match (/ Java [^ script] /)); // "JavaS"
```
