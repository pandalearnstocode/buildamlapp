# __Introduction to web apps, CRUD & REST APIs__

## __Topics to be discussed during this session:__

Here in this session we are going to discuss the following topics,

* History of web apps
* Introduction of SOAP, REST & GraphQL
* Introduction to JSON
* Structure of a REST API
* REST API frameworks in Python
* Methods of REST APIs
* Usage of status code
* Virtual envs
* Introduction of containerization
* Creating virtual envs using anaconda
* Installation of git
* VS Code extensions and usage

## __Session 1: Introduction Web App, CRUD & REST APIs__

&nbsp;

![type:video](https://www.youtube.com/embed/_KL52Z_-j1Y)


## __System setup:__

### __Install anaconda:__

Here for most of our work we are going to use Ubuntu. To install anacona in a Ubuntu system, either download the latest anaconda installation scipt from the official website and install using `bash <https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh>`. To download and run the anaconda from terminal execute the following set of commands,

```bash
sudo apt update  -y 
sudo apt install curl -y
curl --output anaconda.sh https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh
bash anaconda.sh 
source ~/.bashrc
conda list 
conda update --all
```
__Note:__ Here we are using `https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh` url, because this is the latest version. Check the latest version from [this](https://www.anaconda.com/products/distribution#linux) url while doing the setup.

While installing make sure that the `PATH` of anaconda is registered in `.bashrc`. It will ensure that whenever you are executing `conda` from terminal your OS knows when conda is located. If everything goes well run the following commands to create a virtual env name as `tou_app`,

```bash
conda create --name toy_app python=3.8
conda activate toy_app
pip install pandas jupyter notebook
conda deactivate
```
If the above lines work, you have a conda env called `toy_app` with python 3.8. Also you have pandas and jupyter notebook installed in the same.


## __Install git__

```bash
sudo apt install git-all
```
Execute the above line to install `git` in your system. After installing configure your user name and email in git using the following commands,

```bash
git config --global user.name "Aritra Biswas"
git config --global user.email pandalearnstocode@gmail.com
```

__Note:__ This has nothing to do with your GitHub login. To login to GitHub you need use your PAT. You can register RSA token so that everytime you make a push you do not need to enter user name and PAT. Since, we are going to use VS Code this will not be required.



* Install VS Code
* Install VS Code extensions
* Install Docker
* Install Docker Compose

## __Creating virtual envs:__


## __`poetry` as alternative of conda__

&nbsp;

![type:video](https://www.youtube.com/embed/G-OAVLBFxbw)

### __Why we should use `poetry`?__

`poetry` is very useful from two points,

* poetry can easily work with multiple version of python
* poetry is good for building and distributing python libraries


### __Some useful `poetry` commands__

```bash
# Download poetry in Ubuntu
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
source $HOME/.poetry/env                                    # Add to PATH
poetry --version                                            # Check version of poetry
poetry self update                                          # Update version
poetry new project1                                         # Create a new project
cd project1
tree . 
poetry run pytest                                           # Run pytest for the project
poetry add pandas                                           # Add a package as dependency of a project
poetry remove pandas                                        # Delete a project from the file
poetry add --dev pytest                                     # Add a package as dev dependency in a poetry project
poetry add -D coverage[toml] pytest-cov                     # --dev & -D same
poetry install                                              # Install all the dependencies for a project
poetry build                                                # Build a python library using poetry
poetry publish                                              # Publish library to PyPI
poetry export - requirements.txt --output requirements.txt  # Generate requirements.txt
poetry use python3.8                                        # Use specific version of python in the project
```

## __Reference__:

* [Reading list 1](https://www.google.com/)
* [Reading list 1](https://www.google.com/)
* [Reading list 1](https://www.google.com/)
* [Reading list 1](https://www.google.com/)
