A diferença torna-se óbvia quando olhamos para o código dentro de uma função.

O comportamento é diferente se houver um "salto para fora" de `try..catch`.

Por exemplo, quando há um 'retorno' dentro de `try..catch`. A cláusula `finally` funciona no caso de * any * exit from` try..catch`, mesmo através da instrução `return`: logo após` try..catch` é feito, mas antes que o código de chamada obtenha o controle.

`` `js run
função f () {
experimentar {
alerta ('início');
*! *
retornar "resultado";
* /! *
} catch (e) {
/// ...
} finalmente {
alerta ('limpeza' ');
}
}

f (); // Limpar!
`` `

... Ou quando há um "throw", como aqui:

`` `js run
função f () {
experimentar {
alerta ('início');
lança um novo erro ("um erro");
} catch (e) {
// ...
se ("não pode lidar com o erro") {
*! *
jogue e;
* /! *
}

} finalmente {
alerta ('limpeza'!)
}
}

f (); // Limpar!
`` `

É "finalmente" que garante a limpeza aqui. Se acabos de colocar o código no final de `f`, não seria executado.
