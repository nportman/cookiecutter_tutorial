# cookiecutter_tutorial

## Cookiecutter dependencies that have to be installed on your Windows 10 machine
### Step 1. Check if Python is installed by issueing a command python --version 
### Step 2. If Python is an unrecognized command, install python as follows:
#### Browse to https://www.python.org/downloads/release/python-394 and click on "Windows installer(64-bit)". 
#### It will automatically download .exe file on your computer. Click on the downloaded executable file. 
#### Select "Install now" option that will install Python with default settings.
### Step 3. Usually, Python 3.9 comes with "pip" pre-installed. If you get an error "pip command not found", install Python package installer "pip" by typing at your Windows Command Prompt:
### curl https://bootstrap.pypa.io/get-pip/py -o C:\Users\yourusername\Desktop\get-pip.py
### Step 4. cd C:\Users\yourusername\Desktop 
### Step 5. python3 get-pip.py 

## Install cookiecutter Python package >= 1.4.0:
### pip install cookiecutter

## Start a new project
cookiecutter https://github.com/drivendata/cookiecutter-data-science
