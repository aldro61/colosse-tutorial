# Colosse Tutorial
Configuring your environment on the Colosse super computer @ Calcul Quebec

## Modules

## Configuring your development environment


### Installing Python

There a two ways of getting a working Python installation on Colosse. The first is using the Python installation available through the `apps/python/x.y.z` module, where `x.y.z` is the version, to create a virtual environment in your home directory. The second, which I prefer, is using the [Anaconda](https://www.continuum.io/downloads) package manager.

| Virtual Environment                                                                        | Anaconda |
|--------------------------------------------------------------------------------------------|----------|
| Pros: <ul><li>Some packages are optimized for the hardware</li><li>Don't need to download all packages, some are pre-downloaded</li></ul> | Pros: <ul><li>All dependencies are installed with the packages</li>  <li>Package version are updated if needed</li>  <li>Most scientific computing packages are preinstalled</li></ul>        |
| Cons: <ul><li>Dependencies that are not available through modules must be installed manually</li></ul> | Cons: <ul><li>Uses a lot of disk space (~3 Gb)</li></ul>         |

### Step 1: Downloading the Anaconda installer

* Python 2.7 (the one I use): 
```
wget https://repo.continuum.io/archive/Anaconda2-4.4.0-Linux-x86_64.sh
```
* Python 3.6: 
```
wget https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
```

### Step 2: Installing Anaconda

* Python 2.7 (the one I use): 
```
bash Anaconda3-4.4.0-Linux-x86_64.sh 
```
* Python 3.6: 
```
bash Anaconda2-4.4.0-Linux-x86_64.sh 
```

Follow the steps and answer all the questions. Most default answers are perfectly fine.
