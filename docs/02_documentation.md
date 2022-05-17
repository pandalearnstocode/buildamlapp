# __Type of documentation__

Here are some of the key documents which is required for a software engineering project. We are going to use the following libraries for documentation,

1. [MKDocs](https://www.mkdocs.org/)
2. [MKDocstring](https://github.com/mkdocstrings/mkdocstrings)
3. [Swagger UI](https://swagger.io/) & [Fast API](https://fastapi.tiangolo.com/)

## __User guide or Project wiki__

### __Purpose__

### __Create MKDocs wiki__


```bash
conda create --name docs_engine python=3.8 -y # Create a env
conda activate docs_engine                    # activate env
pip install mkdocs                            # Install MKDocs
touch mkdocs.yml                              # Create a MKDocs config file
mkdir docs                                    # Create a `docs` dir. Here all the *.md files will be present
cd docs                                       # Navigate to the `docs` dir
touch index.md                                # Create a `index.md` which will be the landing page of the wiki
cd ..                                         # Navigate to the project root
```

Now, in the `mkdocs.yml` file we need to define the landing page of the wiki. In order to do that here in the `nav` section we are saying whenever there is a `GET` call to the landing page render content of `index.md` as HTML page.

```yaml
nav:
    - 'index.md'
```
To validate the above things are working or not, we can run `mkdocs build` this will generate all the static content. These files should not be part of the git repository. Hence we need to ignore the site folder in the `.gitignore`. Once this is done, to monitor the generated site we can use `mkdocs serve`. This will start a live web server and will host the content generate in the `site` directory. At the same time one can keep editing the content and check live preview in the hosted server.

```bash
mkdocs build
echo "site/" >> .gitignore
mkdocs serve
```

### __Add theme__

There are any themes are present in MKDocs ecosystem but we will use [mkdocs material](https://squidfunk.github.io/mkdocs-material/). To install the theme in the existing virtual env run,

```bash
pip install mkdocs-material
```
After installing the theme one has to add the following lines in `mkdocs.yml`,

```yaml
theme:
  name: material
```

Rest of the process for generating and serving the docs remains the same. 

### __How to add content to the wiki__

To add a new page we need to do the following things,

* Create a file in docs directory in markdown format.
* Link the file in `nav` section of `mkdocs.yml`
* After this generate the static content


### __CI Pipeline using GitHub actions__

* Create a file `.github/workflows/ci.yml`
* Copy paste the content from below
* Create PAT from GitHub
* Store in it in GitHub secrets with the name `PERSONAL_TOKEN`

```yaml
name: Publish docs via GitHub Pages
on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          persist-credentials: false
          fetch-depth: 0
      - name: Deploy docs
        uses: mhausenblas/mkdocs-deploy-gh-pages@master
        env:
          GITHUB_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
```

Now when ever there is some new code checked in to `master` it will build the static site and push it to `gh-pages` branch and will be deployed to GitHub pages. In addition to this you may have to enable GitHub pages. Set the brand to `gh-pages` and directory should be root.

### __Manual deployment__

If you want to directly build and push changes to GitHub pages, run the following command,

```bash
mkdocs gh-deploy --force
```

### __Additional settings__

* Install [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) extension in VS Code.
* Add the following lines in VS Code `settings.json` file,

```json
{
  "yaml.schemas": {
    "https://squidfunk.github.io/mkdocs-material/schema.json": "mkdocs.yml"
  }
}

```

## __Library API documentation__

### __Purpose__

### __MKDocstring: A MKDocs plugin to for Library API documentation__

When you have a project with MKDocs wiki and you want to add your code documentation along with your project documentation, this process becomes very useful. In oder to do this you need to install [autodocstring](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring) extension in VS Code. Once this is installed, add the following lines in the `settings.json` file,


```json
{
    "autoDocstring.docstringFormat": "sphinx"
}
```

Here are the list of formatters available in `autodocstring`,

* google
* sphinx
* numpy
* docBlockr
* one-line-sphinx
* pep257

Once this is configured write a function with docstring. For example,


```python
# my_library/my_module.py

def my_foo():
    """This is a docstring example"
    return "Sample function!!!"
```

### __How to use MKDocstring__

Once the above things are done, install the mkdocstring package using,


```bash
pip install mkdocstrings
```

Add the following lines in `mkdocs.yml` file,

```yaml
plugins:
- search
- mkdocstrings
```
To show any particular function in a markdown page, add the following block to the page and it will render the docstring as HTML content in the respective page,

```
::: my_library.my_module.my_foo
```

## __REST API documentation__

### __Purpose__

### __What is `Swagger UI` and `OpenAPI specifications`__

### __API level docs__

### __Tag level docs__

### __Endpoint level docs__

### __Adding sample request and response schema__


## __Reference__

* [Metadata and Docs URLs](https://fastapi.tiangolo.com/tutorial/metadata/)
* [Declare Request Example Data](https://fastapi.tiangolo.com/tutorial/schema-extra-example/)
* [Additional Responses in OpenAPI](https://fastapi.tiangolo.com/advanced/additional-responses/)
* [How to Document a FastAPI App with OpenAPI](https://www.linode.com/docs/guides/documenting-a-fastapi-app-with-openapi/)
* [Metadata and Additional Responses in FastAPI](https://betterprogramming.pub/metadata-and-additional-responses-in-fastapi-ea90a321d477)
