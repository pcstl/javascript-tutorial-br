# Encontre a etiqueta completa

Escreva um regex para encontrar a tag `<style ...>`. Ele deve combinar a tag completa: pode não ter atributos `<style>` ou ter vários deles `<style type =" ... "id =" ... ">`.

... Mas o regex não deve corresponder `<styler>`!

Por exemplo:

`` `js
Deixe reg = / your regexp / g;

alerta ('<style> <styler> <style test = "...">'. match (reg)); // <style>, <style test = "...">
```
