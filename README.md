# buildamlapp


```bash
conda create --name buildamlapp python=3.8
conda activate buildamlapp
pip install mkdocs-material
mkdocs new .
```

Add the following lines in `mkdocs.yml`,

```yml
theme:
  name: material
```
Add this following lines in `settings.json`,


```json
{
  "yaml.schemas": {
    "https://squidfunk.github.io/mkdocs-material/schema.json": "mkdocs.yml"
  }
}
```

```bash
mkdocs serve
mkdocs build
```


```bash
conda deactivate
```