importância: 5

---

# Finalmente ou apenas o código?

Compare os dois fragmentos de código.

1. O primeiro usa `finally` para executar o código após` try..catch`:

`` `js
experimentar {
trabalho Trabalho
} catch (e) {
lidar com erros
} finalmente {
*! *
limpeza do espaço de trabalho
* /! *
}
`` `
2. O segundo fragmento coloca a limpeza logo após `try..catch`:

`` `js
experimentar {
trabalho Trabalho
} catch (e) {
lidar com erros
}

*! *
limpeza do espaço de trabalho
* /! *
`` `

Definitivamente precisamos da limpeza após o início do trabalho, não importa se houve um erro ou não.

Existe alguma vantagem aqui ao usar `finally` ou ambos os fragmentos de código são iguais? Se existe tal vantagem, então dê um exemplo quando interessa.
