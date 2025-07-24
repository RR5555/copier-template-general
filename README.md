# copier-template-general

General Python Project Scaffolding Copier Template

## Docstring templates

https://github.com/NilsJPWerner/autoDocstring/tree/master/src/docstring/templates

```bash
find *.mustache | awk -F '.' '{print "mv "$1".mustache \"{% if docstring_template == '\''"$1"'\'' %}"$1".mustache{% endif %}\""}'
```



