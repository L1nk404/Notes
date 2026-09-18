### Find
#find
1. `-exec`:
Usamos `{}` como placeholder do resultado do **find** e podemos terminar com `+` para concatenar todos resultados e fazer um único comando ou `;\` para quebrar alinha e realizar um comando por resultado:

```bash
❯ find . -iname "Ubuntu*" -exec rm -rf {} +  # Exclui todos resultados que começam com "Ubuntu"
```
