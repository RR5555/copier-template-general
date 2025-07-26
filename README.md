# copier-template-general

General Python Project Scaffolding Copier Template

## Docstring templates

https://github.com/NilsJPWerner/autoDocstring/tree/master/src/docstring/templates

```bash
find *.mustache | awk -F '.' '{print "mv "$1".mustache \"{% if docstring_template == '\''"$1"'\'' %}"$1".mustache{% endif %}\""}'
```



## classifiers

```bash
cd copier_files
make generate-classifiers
```

## Nested inclusions

```python
def _include(loader: yaml.Loader, node: yaml.Node) -> Any:
	if not isinstance(node, yaml.ScalarNode):
	    raise ValueError(f"Unsupported YAML node: {node!r}")
	include_file = str(loader.construct_scalar(node))
	print(f"include_file: {include_file}")
	if PurePosixPath(include_file).is_absolute():
	    raise ValueError("YAML include file path must be a relative path")
	print(f"{list(conf_path.parent.glob(include_file))=}")
	return [
	    #yaml.load(path.read_bytes(), Loader=type(loader))
	    #for path in conf_path.parent.glob(include_file)
	    lflatten(filter(None, yaml.load_all(path.read_bytes(), Loader=type(loader))))
	    for path in conf_path.parent.glob(include_file)
	]
```

NOTE: There might be a way to keep the path relative to the include file instead of relative to root file

