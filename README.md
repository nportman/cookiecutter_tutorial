# cookiecutter_tutorial
## Cookiecutter dependencies that have to be installed on your Windows 10 machine
### Step 1. Check if Python is installed by issueing a command python --version 
### Step 2. If Python is an unrecognized command, install python as follows:
#### Browse to https://www.python.org/downloads/release/python-394 and click on "Windows installer(64-bit)". 
#### It will automatically download .exe file on your computer. Click on the downloaded executable file. 
#### Select "Install now" option that will install Python with default settings.
### Step 3. Usually, Python 3.9 comes with "pip" pre-installed. If you get an error "pip command not found", install Python package installer "pip" by typing at your Windows Command Prompt:
### curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
### Step 4. python3 get-pip.py 

## Install cookiecutter Python package >= 1.4.0:
### pip install cookiecutter

## Start a new project
### 1. Go to bitbucket.org/your_workspace_name
### 2. Create a project name
### 3. Create a repository name (eg. kitchen) (alternatively, you can create a github repository) on a master branch
### 4. Git clone your empty repository:
#### git clone https://your_user_name@bitbucket.org/your_workspace_name/kitchen.git
#### alternatively, git clone https://github.com/your_user_name/cookiecutter_tutorial.git
### 5. cd kitchen

## Create project skeleton
### cookiecutter https://github.com/drivendata/cookiecutter-data-science
### git add .
### git commit -m "Initial skeleton."
### git push 

## Explore the structure of your project 

### Raw data folder treats data stored in it as immutable. Avoid creating multiple versions of raw data. Raw data is an input to your pipeline that produces the final results. Everyone should be able to reproduce the final results with only the code in src and raw data.  
### If data is immutable, it does not need source control. The data folder is included in the .gitignore file. If your data sample is small, you may want to include it into your repository. Github file size limit is usually 100Mb. If you store big data in AWS S3 bucket, you can sync it with AWS syncing tool ("s3cmd"). There are other syncing tools such as Git Large File Storage and dat, to name a few. By default, this repo is set up for AWS S3 data storing/syncing.

### Jupyter notebooks are effective for exploratory analysis. A common practice is to subdivide the notebooks folder into exploratory and reports subfolders containing initial explorations and polished work, respectively. Notebooks are challenging for source control (eg., merging is near impossible). As such, colaborative coding of Jupyter notebooks is not the best practice. Here are 2 steps for the effective use of notebooks:

#### 1. Follow a naming convention that shows the owner and the order the analysis was done in. The proposed format is <step>-<bbuser>-<description>.ipynb (e.g., 0.1-NataliyaP-process_rbc_data.ipynb).

#### 2. Refactor the code that you will reuse in multiple notebooks. If it's a data assembly task, put it in the pipeline at src/data/make_dataset.py. If it's useful utility code, refactor it to src.

### Using the setup.py file, the project can be turned into a Python package. You can import your code and use it in notebooks with a cell like the following:
#### IPython extension to reload modules before executing user code. autoreload reloads modules automatically before entering the execution of code typed at the IPython prompt.
#### %load_ext autoreload
#### %autoreload 2
#### from src.data import make_dataset

### Managing processing pipelines
#### Often your pipeline for data preparation for training consists of a number of long-running steps. If you have run these steps already and stored the outputs in the data/interim directory, you don't want to wait when rerunning them. "Make" is a common tool on Unix-based systems that manages pipeline steps that depend on each other, especially the long-running ones. Makefiles can work effectively across systems if you follow makefile conventions and portability guide.

### Reproduce the computational environment
#### In order for your analysis to be reproducible, you need to provide your colleagues with the same tools, the same libraries, and the same versions that you used to create it. One effective approach is using virtualenv. By listing all of your requirements in the requirements.txt file, you can easily track the package versions needed to recreate the analysis. Here is a typical workflow:
#### 1. virtualenv visualization (create a new project within this repo:)
#### 2. cd visualization
#### 3. Scripts\activate (activate virtual environment)
#### 4. pip install (the packages that your analysis needs)
#### 5. pip freeze > requirements.txt (to pin the exact package versions)
#### If you find you need to install another package, run pip freeze > requirements.txt again and commit the changes to your bb or gh repo. If you have more complex requirements for recreating your environment, consider using Docker container.

### Store your credentials in a special file
#### You want to keep your Azure access keys or cloud database username and password top secret. You can safely store them in a special .env file in the project root folder.
#### Thanks to the .gitignore, this file never gets committed into the version control repository. Here's an example:
#### example .env file
#### DATABASE_URL=jdbc:mysql://username:password@localhost:5432/dbname
#### AZ_ACCESS_KEY=myaccesskey
#### AZ_SECRET_ACCESS_KEY=mysecretkey
#### OTHER_VARIABLE=something
#### If you look at src/data/make_dataset.py, it uses a package called python-dotenv to load up all the entries in this file and make them accessible with os.environ.get.
#### Here is a snippet code that uses dotenv library to read environment variable values
##### #src/data/dotenv_example.py
##### import os
##### from dotenv import load_dotenv, find_dotenv
##### # find .env automagically by walking up directories until it's found
##### dotenv_path = find_dotenv()
##### # load up the entries as environment variables
##### load_dotenv(dotenv_path)
##### database_url = os.environ.get("DATABASE_URL")
##### other_variable = os.environ.get("OTHER_VARIABLE")
