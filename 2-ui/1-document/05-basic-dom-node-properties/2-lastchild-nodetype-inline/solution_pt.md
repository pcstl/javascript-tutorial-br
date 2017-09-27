Há uma captura aqui.

No momento da execução `<script>` o último nó DOM é exatamente `<script>`, porque o navegador ainda não processou o resto da página.

Então, o resultado é `1` (elemento nó).

`` `html run height = 60
<html>

<corpo>
<script>

</ script>
</ body>

</ html>
`` `
