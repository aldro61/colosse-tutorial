# Colosse Tutorial
Configuring your environment on the Colosse super computer @ Calcul Quebec

## Connecting to the supercomputer

To connect to Colosse, open a terminal and use the following command.
```
ssh username@colosse.calculquebec.ca
```
This will connect you to a login node. You should be greeted with a message like this:
```
===========================================================================
Vous êtes sur un noeud de login de colosse (Calcul Québec).
 - Nous n'effectuons pas de sauvegarde de vos fichiers.
 - N'utilisez pas le noeud de login pour executer votre code.

This is a Calcul Québec login node for colosse.
 - There is no backup of users files.
 - Do not use this node to run code.

Rapportez tout problème à / Report any problems to: colosse@calculquebec.ca
Documentation: https://wiki.calculquebec.ca/
Suivre sur Twitter/Follow on Twitter: https://twitter.com/CQ_Colosse
État des serveurs: http://serveurscq.computecanada.ca
===========================================================================
```

Login nodes are used to prepare/launch/monitor jobs and to move files around. Do not use them to run code.

## File system

Colosse has various file systems, which have different properties listed [here](https://wiki.calculquebec.ca/w/Utiliser_l%27espace_de_stockage/en#tab=tab2). Basically, I use SCRATCH to run experiments and RAP to store results and data that must not be lost.

* Scratch: This directory is placed on a parallel filesystem, Lustre (for Colosse, Mp2, Ms2 and Cottos) or GPFS (for Briarée and Guillimin). It is generally visible from all nodes. Using it is very fast for large files, but not very efficient for many small files. This is the appropriate place to store **large files** that you use for a **few days or weeks only**. Periodically, it may be **automatically cleaned** (files being deleted).

In your home directory, run the following commands:
```
ln -s $SCRATCH scratch
ln -s $RAP rap
```
This will create symbolic links to you scratch and rap folders, which have complicated paths.

## Modules

Colosse provides a lot of preinstalled software, which is made available through modules.

### Listing all available modules
`module avail`

### Searching for a specific module
`module spider [keyword]`

For example, running `module spider python` returns:
```
-----------------
  compilers/gcc:
-----------------
     Versions:
        compilers/gcc/4.5
        compilers/gcc/4.6
        compilers/gcc/4.8
        compilers/gcc/4.8.5
        compilers/gcc/4.9
        compilers/gcc/5.4
```

### Loading modules

To use a module, you must first load it using the following command
```
module load [module name]
```

For example, `module load compilers/gcc/4.8.5` loads version 4.8.5 of the gcc compiler.

### Loading modules on login

Manually loading modules each time you log in is tedious and can be avoided by using a `.bashrc` file. Copy the [bashrc](.bashrc) file provided with this tutorial to the root of your home directory.

## Configuring your development environment


### Installing Python

There a two ways of getting a working Python installation on Colosse. The first is using the Python installation available through the `apps/python/x.y.z` module, where `x.y.z` is the version, to create a virtual environment in your home directory. The second, which I prefer, is using the [Anaconda](https://www.continuum.io/downloads) package manager.

| Virtual Environment                                                                        | Anaconda |
|--------------------------------------------------------------------------------------------|----------|
| Pros: <ul><li>Some packages are optimized for the hardware</li><li>Don't need to download all packages, some are pre-downloaded</li></ul> | Pros: <ul><li>All dependencies are installed with the packages</li>  <li>Package version are updated if needed</li><li>Installs more than just Python packages (e.g.: libraries)</li></ul>        |
| Cons: Doesn't use the libraries provided by Colosse, which can sometimes be more efficient.<ul></ul>         |

The regular version of the Anaconda package comes with a bunch of software preinstalled. To avoid using unnecessary disk space, we will use a lighter version called Miniconda. 

### Step 1: Downloading the Miniconda installer

* Python 2.7 (the one I use): 
```
https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
```
* Python 3.6: 
```
https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

### Step 2: Installing Anaconda

* Python 2.7 (the one I use): 
```
bash Miniconda2-latest-Linux-x86_64.sh
```
* Python 3.6: 
```
bash Miniconda3-latest-Linux-x86_64.sh
```

Follow the steps and answer all the questions. Most default answers are perfectly fine.
