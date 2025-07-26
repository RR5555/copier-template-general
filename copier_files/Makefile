.DEFAULT_GOAL := help


generate-classifiers: ## Generate question files for classifiers
	cd ./generated;\
	echo '---' > generated_classifiers_include.yaml;\
	curl https://pypi.org/classifiers/ | sed -n '/<h2>List of classifiers<\/h2>/,/<\/ul>/ p' | sed -n -E 's/.*?>(.*?)<\/a>/\1/ p' > test.txt;\
	cat test.txt | awk -F '\\s*::\\s*' '{print $$1;}' | sort -u |\
	while IFS= read -r category; do\
		echo "###### $$category ######";\
		filename_root=$$(echo "$$category" | sed 's/ /_/');\
		echo "$$filename_root";\
		echo "$$(echo "$$filename_root" | sed -n 's/\(.*\?\)/project_\L\1/ p'):\n  type: str\n  help: What is the project $$(echo "$$category" | sed -n 's/\(.*\?\)/project \L\1/ p')?\n  multiselect: true\n  choices:\n" > $$(echo "$$filename_root" | sed -n 's/\(.*\?\)/project_\L\1.yaml/ p');\
		cat test.txt | awk -F '\\s*::\\s*' -v var="$$category" '$$1 ~ var {print $$0;}' | sort -u |\
		while IFS2= read -r subcat; do\
			echo "    \"$$(echo "$$subcat" | sed -n 's/^[^:]* :: // p')\":\n      value: \"$$subcat\"" >> $$(echo "$$filename_root" | sed -n 's/\(.*\?\)/project_\L\1.yaml/ p');\
		done;\
		echo "!include $$(echo "$$filename_root" | sed -n 's/\(.*\?\)/project_\L\1.yaml/ p')\n---" >> generated_classifiers_include.yaml;\
	done 2>/dev/null;\
	rm test.txt










###### Help ######

# https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
.PHONY: help

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

