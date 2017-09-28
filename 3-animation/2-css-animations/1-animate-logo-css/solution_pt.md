
CSS para animar `width` e` height`:
`` `css
/ * classe original * /

#flyjet {
transição: todos os 3s;
}

/ * JS adiciona .growing * /
# flyjet.growing {
largura: 400px;
altura: 240px;
}
```

Observe que `transitionend` dispara duas vezes - uma vez por cada propriedade. Então, se não realizarmos uma verificação adicional, a mensagem apareceria duas vezes.
