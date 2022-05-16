# __Type of documentation__

Here are some of the key documents which is required for a software engineering project. We are going to use the following libraries for documentation,

1. [MKDocs](https://www.mkdocs.org/)
2. [MKDocstring](https://github.com/mkdocstrings/mkdocstrings)
3. [Swagger UI](https://swagger.io/) & [Fast API](https://fastapi.tiangolo.com/)

## __User guide or Project wiki__

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

## __Library API documentation__

### __MKDocstring: A MKDocs plugin to for Library API documentation__

### __How to use MKDocstring__

## __REST API documentation__

### __What is `Swagger UI` and `OpenAPI specifications`__

### __API level docs__

### __Tag level docs__

### __Endpoint level docs__

### __Adding sample request and response schema__
