importância: 5

---

# Encontre as coordenadas da janela do campo

No iframe abaixo, você pode ver um documento com o "campo" verde.

Use JavaScript para encontrar as coordenadas da janela dos cantos apontados com as setas.

Há um pequeno recurso implementado no documento por conveniência. Um clique em qualquer lugar mostra coordenadas lá.

[iframe border = 1 height = 360 src = "source" link edit]

Seu código deve usar o DOM para obter as coordenadas da janela de:

1. Esquina superior esquerda-superior (isso é simples).
2. Esquina externa direita-direita (simples também).
3. Esquina interna esquerda-superior (um pouco mais difícil).
4. O canto interno inferior direito (existem várias maneiras, escolha um).

As coordenadas que você calcula devem ser as mesmas que retornadas pelo clique do mouse.

P.S. O código também deve funcionar se o elemento tiver outro tamanho ou borda, não vinculado a valores fixos.
