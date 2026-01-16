.DEFAULT_GOAL := help


generate-classifiers: ## Generate question files for classifiers
	@# NOTE: we use the pattern `echo "$$(printf "%s" "" "test\n" ...)"` to split the str over multiline for readability while preserving indentation

	@# Create `generated_classifiers_include.yaml` and start it with `---` for yaml separation"
	@cd ./generated; \
	echo '---' > generated_classifiers_include.yaml

	@# Get the official classifiers from `https://pypi.org/classifiers/`
	@# (no automatic printing `-n`) print only the content between `<h2>List of classifiers</h2>` and `</ul>`
	@# (no automatic printing `-n`, `-E`=`--regexp-extended`) print content after extracting the text inside each `a` tag
	@# result is put into `test.txt`
	@cd ./generated; \
	curl https://pypi.org/classifiers/ | \
	sed -n '/<h2>List of classifiers<\/h2>/,/<\/ul>/ p' | \
	sed -n -E 's/.*?>(.*?)<\/a>/\1/ p' \
	> test.txt

	@# display content of `test.txt`
	@# (`-F`:Delimit using regular expression) select 1rst column as delimited by the regexp
	@# (`-u`=`--unique`) `sort` unique lines of text files
	@# `while` over extracted `category`, see the `do` comments
	@cd ./generated; \
	cat test.txt | \
	awk -F '\\s*::\\s*' '{print $$1;}' | \
	sort -u | \
	while IFS= read -r category; do\
		: # print name of category to keep track of progress; \
		echo "###### $$category ######";\
		: # get the filename prefix/root to be used for that category, simply the `category` where space ` ` are replaced by `_`; \
		filename_root=$$(echo "$$category" | sed 's/ /_/');\
		: # display the `filename_root`; \
		echo "$$filename_root";\
		: # ([sed] `\L`:Turn the replacement to lowercase until a `\U` or `\E` is found) ; \
		: # 1rst sed: take `filename_root` and create str `project_<lowercase(filename_root)>`; \
		: # 2nd sed: take `category` and create str `project <lowercase(category)>`; \
		: # 3rd sed: take `filename_root` and create str `project_<lowercase(category)>.yaml`; \
		: # cmd populate file `project_<lowercase(category)>.yaml` with `copier` `Question` name, type, help, multiselect, choices; \
		echo "$$(printf "%s" ""\
			"$$(echo "$$filename_root" | sed -n 's/\(.*\?\)/project_\L\1/ p'):\n"\
			"  type: str\n"\
			"  help: What is the $$(echo "$$category" | sed -n 's/\(.*\?\)/project \L\1/ p')?\n"\
			"  multiselect: true\n"\
			"  use_search_filter: true\n"\
			"  choices:\n"\
		)" > $$(echo "$$filename_root" | sed -n 's/\(.*\?\)/project_\L\1.yaml/ p');\
		: # display content of `test.txt`; \
		: # (`-F`:Delimit using regular expression) search `category` in 1rst column as delimited by the regexp & print full line with match; \
		: # (`-u`=`--unique`) `sort` unique lines of text files; \
		: # `while` over extracted `subcat`, see the `do` comments; \
		cat test.txt | \
		awk -F '\\s*::\\s*' -v var="$$category" '$$1 ~ var {print $$0;}' | \
		sort -u |\
		while IFS2= read -r subcat; do\
			: # ([sed] `\L`:Turn the replacement to lowercase until a `\U` or `\E` is found) ; \
			: # 1rst sed: take `subcat` and remove the first field before the first `::` to not display the `category` name in the choice name; \
			: # 2nd sed: take `filename_root` and create str `project_<lowercase(category)>.yaml`; \
			: # write the `copier` `Question` choices to `project_<lowercase(category)>.yaml`; \
			echo "$$(printf "%s" ""\
				"    \"$$(echo "$$subcat" | sed -n 's/^[^:]* :: // p')\":\n"\
				"      value: \"$$subcat\""\
			)" >> $$(echo "$$filename_root" | sed -n 's/\(.*\?\)/project_\L\1.yaml/ p');\
		done;\
		: # ([sed] `\L`:Turn the replacement to lowercase until a `\U` or `\E` is found) ; \
		: # write the `!include project_<lowercase(category)>.yaml` to `generated_classifiers_include.yaml` followed by YAML separator; \
		echo "$$(printf "%s" ""\
			"!include $$(echo "$$filename_root" | sed -n 's/\(.*\?\)/copier_files\/generated\/project_\L\1.yaml/ p')\n"\
			"---"\
		)" >> generated_classifiers_include.yaml;\
	done 2>/dev/null

	@# delete file `test.txt`
	@cd ./generated; \
	rm test.txt
	@# delete in-place 4th line of `./generated/project_development_status.yaml`
	@sed -i '4d' ./generated/project_development_status.yaml
	@# delete in-place 4th line of `./generated/project_typing.yaml`
	@sed -i '4d' ./generated/project_typing.yaml
	@# Append str after 3rd line of `./generated/project_typing.yaml` (note the escape of the space characters)
	@sed -i '3a\ \ default: "Typing :: Typed"' ./generated/project_typing.yaml


###### Help ######

# https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
.PHONY: help

help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

