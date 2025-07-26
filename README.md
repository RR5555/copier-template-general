# copier-template-general

General Python Project Scaffolding Copier Template

## Docstring templates

https://github.com/NilsJPWerner/autoDocstring/tree/master/src/docstring/templates

```bash
find *.mustache | awk -F '.' '{print "mv "$1".mustache \"{% if docstring_template == '\''"$1"'\'' %}"$1".mustache{% endif %}\""}'
```



## classifiers

```bash
curl https://pypi.org/classifiers/ | sed -n '/<h2>List of classifiers<\/h2>/,/<\/ul>/ p' | sed -n -E 's/.*?>(.*?)<\/a>/\1/ p' > test.txt
cat test.txt | awk -F '\\s*::\\s*' '{print $1;}' | sort -u
```