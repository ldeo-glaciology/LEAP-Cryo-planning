# Icepack installation steps
These are the stepps that worked for me to install icepack on a 2018 Mac iOS 13.2.1 (22D68). 

## 1. Install firedrake 
We have found that it works to first install firdrake, then install icepack afterwards. 

We also found that a few extra steps were required to intall firedrake compared ot the instruction on the project [website]([url](https://www.firedrakeproject.org/download.html)).

### 1.1. Install homebrew
If you havent got it on your system already, install homebrew from here https://brew.sh/

### 1.2. Update xcode command line tools 
I  ran into an issue with a fortran compiler, which led me to update xcode command line tools, following guidelines [here](https://mac.install.guide/commandlinetools/3.html).

### 1.3. Avoid conda/anaconda
Comment out all line in .zshrc and/or .bash_profile which mention conda/anaconda during the installation. 

### 1.4. Download the latest installation script
Even though I downloaded the installatin script relatively recently (a few weeks ago), it was out of date after https://github.com/firedrakeproject/firedrake/pull/2838. So make sure you have the latest version of the script:

```curl -O https://raw.githubusercontent.com/firedrakeproject/firedrake/master/scripts/firedrake-install```

### 1.5. Install python using homebrew
```brew install python```

### 1.6. make sure you are using the homebrew-installed verison of python
The MacOS version of python is apparently not good for installing firedrake, so make sure that when you type python into the terminal you are getting the homebrew
version. Ensure this by adding 
```export PATH="/usr/local/opt/python/libexec/bin:$PATH"```
to .zshrc, as suggested [here](https://stackoverflow.com/questions/5157678/how-do-i-use-brew-installed-python-as-the-default-python). 
I think adding the same line to .bash_profile will work the same way if your terminal is set up to use bash.

### 1.7. Install firedrake
``` python firedrake-install```
then wait more than two hours (at least on my 2018 macbook air). 

### 1.8. Test the installation
Follow the steps ont he firedrake website to test the installation. 

Start the virtual environment:
```. /Users/jkingslake/firedrake/bin/activate```

change directory to the correct place to run the tests:
```cd $VIRTUAL_ENV/src/firedrake ```

run the tests:
```pytest tests/regression/ -k "poisson_strong or stokes_mini or dg_advection"```

## 2. Install icepack
### 2.1. Install icepack
Icepack is installed as a package on top of firedrake: 

change directory back to where you installed firdrake and run

```python firedrake/bin/firedrake-update --install icepack```

### 2.2. Install useful packages

Comment out the last two lines in $VIRTUAL_ENV/src/icepack and add jupyterlab to the list, then run

```
cd $VIRTUAL_ENV/src/icepack
pip install -r requirements.txt
```

Install gmsh: ```brew install gmsh```

### 2.3. Test icepack
```pytest -s test/ice_shelf_test.py```

## 3. Work through the tutorial notebooks

```
cd $VIRTUAL_ENV/src/icepack/notebooks
jupyter-lab
```

