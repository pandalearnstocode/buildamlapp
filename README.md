# Build a ML app

[![Publish docs via GitHub Pages](https://github.com/pandalearnstocode/buildamlapp/actions/workflows/ci.yml/badge.svg)](https://github.com/pandalearnstocode/buildamlapp/actions/workflows/ci.yml)

URL: https://pandalearnstocode.github.io/buildamlapp/

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

If a new code is push to `main` or `master` CI pipeline (`.github/workflows/ci.yml`) will deploy this in GitHub pages. If you want to deploy site manually,

```bash
mkdocs gh-deploy --force
```

will deploy the latest changes from any branch to GitHub pages. 


After the initial project is generated, the project folder will be as follows,

```bash
.
├── docs
│   └── index.md
├── mkdocs.yml
├── README.md
├── requirements.txt
└── site
    ├── 404.html
    ├── assets
    ├── index.html
    ├── search
    ├── sitemap.xml
    └── sitemap.xml.gz
```

Use the follow command to rebase with a target branch (master in this case),

```bash
git rebase origin/master
```
