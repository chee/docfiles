# formatters

## editorconfig

i keep an [editorconfig](https://editorconfig.org) in my `~` so i have good
default settings even if there is no `.editorconfig` in the project.

```ini filename=".editorconfig"
root = true

[*]
charset = utf-8
end_of_line = lf
indent_style = tab
insert_final_newline = true
trim_trailing_whitespace = true
tab_width = 3

[*.rs]
indent_style = space
indent_size = 4

[*.py]
indent_style = space
indent_size = 4

[*.el]
indent_style = space
indent_size = 2

[*.rb]
indent_style = space
indent_size = 2

[*.yml]
indent_style = space
indent_size = 2

[*.yaml]
indent_style = space
indent_size = 2

[*.lisp]
indent_style = space
indent_size = 2
tab_width = 2

[*.el]
indent_style = space
indent_size = 2

[*.org]
indent_size = 8
tab_width = 8
```

## prettier

like [editorconfig](#editorconfig), i keep [prettier](https://prettier.io/)
config in my `~` so i have good default settings even if there is no
`.prettierrc` in the project.

this is sometimes annoying if i forget to include one in the project, and then
somebody else tries to contribute.

```toml filename=".prettierrc.toml"
printWidth = 80
trailingComma = "es5"
semi = false
singleQuote = false
useTabs = true
bracketSpacing = false
jsxBracketSameLine = true
arrowParens = "avoid"
```

## [prettierignore](https://prettier.io/docs/en/ignore.html)

prettier should leave html and markdown alone, because it is very bad at them.

```gitignore filename=".prettierignore"
*.md
*.html
```
