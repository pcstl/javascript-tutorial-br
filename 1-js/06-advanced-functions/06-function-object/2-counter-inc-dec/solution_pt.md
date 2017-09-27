
A solução usa `count` na variável local, mas os métodos de adição são escritos diretamente no` counter`. Eles compartilham o mesmo ambiente lexical externo e também podem acessar o atual `count`.
