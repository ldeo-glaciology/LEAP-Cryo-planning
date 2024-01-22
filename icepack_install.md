# Icepack installation steps
These are the steps that worked for me to install icepack on a 2018 Mac air running iOS 13.2.1 (22D68). 

## 1. Install firedrake 
It works to first install firedrake, then install icepack. 

I also found that a few extra steps were required to install firedrake compared to the instructions found on the project [website]([url](https://www.firedrakeproject.org/download.html)).

### 1.1. Install homebrew
If you have not got it on your system already, install homebrew from here https://brew.sh/

### 1.2. Update xcode command line tools 
I ran into an issue with a fortran compiler, which led me to update xcode command line tools, following guidelines [here](https://mac.install.guide/commandlinetools/3.html).

### 1.3. Avoid conda/anaconda
Comment out all lines in .zshrc and/or .bash_profile which mention conda/anaconda prior to installation.

### 1.4. Download the latest installation script
Even though I downloaded the installation script relatively recently (a few weeks ago), it was out of date after https://github.com/firedrakeproject/firedrake/pull/2838. So make sure you have the latest version of the script:

```curl -O https://raw.githubusercontent.com/firedrakeproject/firedrake/master/scripts/firedrake-install```

### 1.5. Install python using homebrew
```brew install python```

### 1.6. Make sure you are using the homebrew-installed version of python
The MacOS version of python is apparently not good for installing firedrake, so make sure that when you type python into the terminal you are getting the homebrew
version. Ensure this by adding 

```export PATH="/usr/local/opt/python/libexec/bin:$PATH"```

to .zshrc, as suggested [here](https://stackoverflow.com/questions/5157678/how-do-i-use-brew-installed-python-as-the-default-python). 
I think adding the same line to .bash_profile will work the same way if your terminal is set up to use bash.

### 1.7. Install firedrake
``` python firedrake-install```

then wait more than two hours (at least on my 2018 macbook air). 

### 1.8. Test the installation
Follow the steps on the firedrake website to test the installation:

Start the virtual environment:

```. /Users/jkingslake/firedrake/bin/activate```

change directory to the correct place to run the tests:

```cd $VIRTUAL_ENV/src/firedrake ```

run the tests:

```pytest tests/regression/ -k "poisson_strong or stokes_mini or dg_advection"```

## 2. Install icepack
### 2.1. Run the installation 
Icepack is installed as a package on top of firedrake: 

change directory back to where you installed firedrake and run

```python firedrake/bin/firedrake-update --install icepack```

### 2.2. Install packages

Comment out the last two lines in $VIRTUAL_ENV/src/icepack/requirements.txt, i.e. open the file and change the last two lines to 

```
#roltrilinos[opt]==0.0.9
#ROL[opt]==0.0.16
```

Also, while you have that file open, add 'jupyterlab' to the end of the list of packages. Then install them by running

```
cd $VIRTUAL_ENV/src/icepack
pip install -r requirements.txt
```

Install gmsh: 

```brew install gmsh```

### 2.3. Test icepack
```pytest -s test/ice_shelf_test.py```

## 3. Work through the tutorial notebooks
These steps should have successfully installed icepack. A good first step in learning how to use the software is to work through the tutorial notebooks:

```
cd $VIRTUAL_ENV/src/icepack/notebooks
jupyter-lab
```

## 4. Install icepack using docker
The current version of icepack seems to be broken (12/10/2023) because firedrake comes with an older version of icepack installed which causes pathing issues. To get around this we can use a docker image with an older version of firedrake already installed. We will follow the steps listed on the [icepack install page](https://icepack.github.io/install/) but switch out the image for an older image. The following steps were performed on Ubuntu 22.04.3 LTS. 

### 4.1 Install docker
Follow steps listed on the [docker website](https://docs.docker.com/engine/install/ubuntu/)

The method under `Install using the apt repository` was used which seems to work fine.

Instruction for Macs with Apple silicon or intel chips can be found [here](https://docs.docker.com/desktop/install/mac-install/)

### 4.2 Create Dockerfile
A Dockerfile contains a set of instructions which are followed so that we get an image which has everything we require. In this instance, we will create a dockerfile that uses an image with an older firedrake installation (from 09/2023) and install icepack on top of it. Copy paste the contents below into a textfile and name it `Dockerfile` (no file extension)

```
FROM firedrakeproject/firedrake-vanilla:2023-09
RUN sudo apt update && sudo apt install patchelf
RUN source firedrake/bin/activate && \
pip install git+https://github.com/icepack/Trilinos.git && \
pip install git+https://github.com/icepack/pyrol.git && \
git clone https://github.com/icepack/icepack.git && \
pip install --editable ./icepack && \
pip install jupyter lab
```
### 4.3 Create docker image with icepack installed
Run 

```docker build --tag icepack-image <directory>```

`<directory>` is the folder where the Dockerfile lives on your machine. If this give a permission denied error, run using `sudo`. Do this for all future docker commands if required. This [link](https://stackoverflow.com/questions/47854463/docker-got-permission-denied-while-trying-to-connect-to-the-docker-daemon-socke) might be useful if you do not want to use `sudo`.

### 4.4 Run the created image

Interactively run the image using

```
docker run \
    --interactive --tty \
    --publish 8888:8888 \
    icepack-image
```

if you want to access a local folder inside the image refer to the [icepack installation](https://icepack.github.io/install/) page. 

### 4.5 Test icepack installation
Activate the firedrake environment

```
source ~/firedrake/bin/activate
```

Run test script
```
pytest -s icepack/test/ice_shelf_test.py
```

### 4.6 Using jupyter lab notebooks
Make sure firedrake environment is activated. To ensure we use this environment in jupyter lab
```
pip install ipykernel
python -m ipykernel install --user --name=firedrake
```

install `gmsh` since they are used by many tutorials
```
sudo apt-get install gmsh
```

run jupyter lab
```
jupyter lab --ip 0.0.0.0 --no-browser
```

if that gives an error try
```
~/.local/bin/jupyter lab --ip 0.0.0.0 --no-browser
```
The server will print a bunch of things, at the end of which will be a URL. If you paste that URL into your browser you should have access from your host system to the notebook server that's now running in the container.
