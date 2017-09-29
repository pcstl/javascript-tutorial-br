importância: 5

---

# Editar TD no clique

Tornar as células da tabela editáveis ​​por clique.

- No clique - a célula deve se tornar "editável" (a área de texto aparece no interior), podemos alterar o HTML. Não deve haver redimensionamento, toda geometria deve permanecer a mesma.
- Botões OK e CANCELAR aparecem abaixo da célula para finalizar / cancelar a edição.
- Apenas uma célula pode ser editável em um momento. Enquanto um `<td>` está no "modo de edição", os cliques em outras células são ignorados.
- A tabela pode ter muitas células. Use a delegação de eventos.

A demo:

[iframe src = "solução" height = 400]
