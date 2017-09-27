importância: 4

---

# Adicione a decoração "adiar ()" para funções

Adicione ao protótipo de todas as funções o método `defer (ms)`, que retorna um wrapper, atrasando a chamada por milissegundos de "ms".

Aqui está um exemplo de como deveria funcionar:

`` `js
função f (a, b) {
alerta (a + b);
}

f.defer (1000) (1, 2); // mostra 3 após 1 segundo
`` `

Observe que os argumentos devem ser passados ​​para a função original.
