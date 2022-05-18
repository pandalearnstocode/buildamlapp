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
touch requirements.txt                        # Create a dependency file
```

Now, in the `mkdocs.yml` file we need to define the landing page of the wiki. In order to do that here in the `nav` section we are saying whenever there is a `GET` call to the landing page render content of `index.md` as HTML page.

```yaml
site_name: Wiki
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

* Update all the dependencies in `requirements.txt` file.
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
          REQUIREMENTS: requirements.txt
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
    "autoDocstring.docstringFormat": "google"
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
# project/hello_world.py

def say_hello_world(name:str)-> str:
    """Hello world function
    Args:
        name (str): persons name
    Returns:
        str: say hello to person
    """
    return f"Hello {name}!!!!"
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
::: project.hello_world.say_hello_world
```

## __REST API documentation__

### __Purpose__

### __What is `Swagger UI` and `OpenAPI specifications`__

### __Getting things up and running__

Lets create a new conda env for Fast API and documentation related work. Install `fastapi` and `uvicorn`. Create two key files `main.py` and `description.py`.

```bash
conda create --name doc_engine python=3.8 -y
conda activate doc_engine
touch main.py
touch description.py
pip install "fastapi[all]"
pip install "uvicorn[standard]"
```

```python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```

Save the above content in `main.py`. 

```bash
uvicorn main:app --reload
```
Execute the above lines to check the Fast API server is running or not. Check `http://127.0.0.1:8000` and `http://127.0.0.1:8000/docs`. If the landing page and Swagger UI page is coming up, we are good to go.


### __API level documentation__

Here in API level documentation we are going capture the what this API collection is about. What are the overall endpoints present in this collection and the high level objective of these endpoints. Also we can keep track of a what all methods we have implemented and what other we need to develop.

We will update the following code in `description.py`. As it is visible here that it is used to describe the high level objectives `Items` and `Users`.

```python
# description.py
description = """
ChimichangApp API helps you do awesome stuff. 🚀

## Items

You can **read items**.

## Users

You will be able to:

* **Create users** (_not implemented_).
* **Read users** (_not implemented_).
"""
```

Along with the description we can use the following information regarding the API collection in `main.py`.

```python
# main.py
from description import description

app = FastAPI(
    title="ChimichangApp",
    description=description,
    version="0.0.1",
    terms_of_service="http://example.com/terms/",
    contact={
        "name": "Deadpoolio the Amazing",
        "url": "http://x-force.example.com/contact/",
        "email": "dp@x-force.example.com",
    },
    license_info={
        "name": "Apache 2.0",
        "url": "https://www.apache.org/licenses/LICENSE-2.0.html",
    },
)

@app.get("/items/")
async def read_items():
    return [{"name": "Katana"}]
```


### __Tag level documentation__

Here we can create sub-collection of APIs which are known as `tags`. This can be a group of APIs doing operation related to a business logic or objective. Some example of these can be `user-management`, `project-management`, `data-management`. These are quite similar to the routes we have in Fast API.

```python
# description.py
tags_metadata = [
    {
        "name": "users",
        "description": "Operations with users. The **login** logic is also here.",
    },
    {
        "name": "items",
        "description": "Manage items. So _fancy_ they have their own docs.",
        "externalDocs": {
            "description": "Items external docs",
            "url": "https://fastapi.tiangolo.com/",
        },
    },
]
```

```python
# main.py
from description import description, tags_metadata

app = FastAPI(
    title="ChimichangApp",
    description=description,
    version="0.0.1",
    terms_of_service="http://example.com/terms/",
    contact={
        "name": "Deadpoolio the Amazing",
        "url": "http://x-force.example.com/contact/",
        "email": "dp@x-force.example.com",
    },
    license_info={
        "name": "Apache 2.0",
        "url": "https://www.apache.org/licenses/LICENSE-2.0.html",
    },
    openapi_tags=tags_metadata)

@app.get("/users/", tags=["users"])
async def get_users():
    return [{"name": "Harry"}, {"name": "Ron"}]


@app.get("/items/", tags=["items"])
async def get_items():
    return [{"name": "wand"}, {"name": "flying broom"}]
```

### __Schema or Data Model level documentation__

Here is an example how we can create a DB model, request and response level documentation. There are other ways to do the same thing we have done here using `Field`. Check the reference section for including multiple schema example.

```python
# main.py

from pydantic import BaseModel, Field
from typing import Union

class Item(BaseModel):
    name: str = Field(example="Foo")
    description: Union[str, None] = Field(default=None, example="A very nice Item")
    price: float = Field(example=35.4)
    tax: Union[float, None] = Field(default=None, example=3.2)

@app.put("/items/{item_id}", tags=["items"])
async def update_item(item_id: int, item: Item):
    results = {"item_id": item_id, "item": item}
    return results
```


### __Endpoint level documentation__

If we want to add details regarding a endpoint we can use `description` arg in any method to do that. We can capture the inner working of a endpoint using this argument. Also, note that we can use `title` and `description` for a `query_parameter` in `GET` method to understand how it is being used in the endpoint.

```python
# main.py

from fastapi import FastAPI, Query

@app.get("/new_items/", tags=["items"], description = "This API is for creating new items.")
async def read_items(
    q: Union[str, None] = Query(
        default=None,
        title="Query string",
        description="Query string for the items to search in the database that have a good match",
        min_length=3,
    )
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

### __Response documentation__

We can use `response` argument to pass status code along with the description about the response. Check [this](https://betterprogramming.pub/metadata-and-additional-responses-in-fastapi-ea90a321d477) blog post for details.

```python
users = {"000": "admin","001": "Wai Foong", "002": "Jane", "003": "Jessie", "007": "Five Six Seven"}

@app.get("/get-user", tags=["get-user"], response_model=Result,
    responses={
        403: {
            "model": Message, 
            "description": "Insufficient privileges for this action"
        },
        404: {
            "model": Message,
            "description": "No user with this ID in the database",
        },
        200: {
            "description": "Successfully retrieved information of the user",
            "content": {
                "application/json": {
                    "example":  {'status_code': '0', 'status_message' : 'Success', 'data': {'id': '001', 'name': 'John Doe'}}
                }
            },
        },
    },)
async def get_user(id: str = Query(..., title="3-digit identity number of the user", example="010")):
    if id in users:
        if id == "007":
            return JSONResponse(status_code=403, content={"message": "Insufficient privileges!"})

        return {"status_code": "0", "status_message" : "Success", "data": {"id": id, "name": users[id]}}
    else:
        return JSONResponse(status_code=404, content={"message": "User not found!"})
```

Deactivate conda env.

```bash
conda deactivate
```
Check minimal implimentation of above discussed topic [here](https://github.com/pandalearnstocode/docs-toy-sample).


## __Reference__

* [Metadata and Docs URLs](https://fastapi.tiangolo.com/tutorial/metadata/)
* [Declare Request Example Data](https://fastapi.tiangolo.com/tutorial/schema-extra-example/)
* [Additional Responses in OpenAPI](https://fastapi.tiangolo.com/advanced/additional-responses/)
* [How to Document a FastAPI App with OpenAPI](https://www.linode.com/docs/guides/documenting-a-fastapi-app-with-openapi/)
* [Metadata and Additional Responses in FastAPI](https://betterprogramming.pub/metadata-and-additional-responses-in-fastapi-ea90a321d477)
* [Sample repository](https://github.com/pandalearnstocode/docs-toy-sample)
