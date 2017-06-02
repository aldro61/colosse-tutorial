# Colosse Tutorial

This tutorial explains how to configure a basic Python environment and how to launch jobs on the Colosse super computer @ Calcul Quebec. The official Colosse wiki can be found at: [https://wiki.calculquebec.ca](https://wiki.calculquebec.ca/w/Accueil).

**Table of contents:**
1. [Connecting to the supercomputer](#connecting-to-the-supercomputer)
2. [Modules](#modules)
3. [Configuring your development environment](#configuring-your-development-environment)
4. [Submitting jobs](#submitting-jobs)

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
mkdir $SCRATCH/$USER
mkdir $RAP/$USER
ln -s $SCRATCH/$USER scratch
ln -s $RAP/$USER rap
```
This will create symbolic links to you scratch and rap folders, which have complicated paths.

## Modules

Colosse provides a lot of preinstalled software, which is made available through modules.

### Listing all available modules
`module avail`

### Searching for a specific module
`module spider [keyword]`

For example, running `module spider gcc` returns:
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

Manually loading modules each time you log in is tedious and can be avoided by using a `.bashrc` file. 

1. Copy the [bashrc](.bashrc) file provided with this tutorial to the root of your home directory. 
2. Load it by running the following command, which will be run automatically on login.
```
source ~/.bashrc
```

## Configuring your development environment

### Directory structure

In addition to the *scratch* and *rap* directories, I like to have a **dev** directory, where I keep all my code repositories. Run the following commands at the root of your home directory.
```
mkdir dev
mkdir dev/git
```

### Configuring Python

#### Creating a virtual environment

First, create a virtual environment by using the following command at the root of your home directory.
```
virtualenv env
```
Then, open up your *.bashrc* file and uncomment the following line in the *Software* section.
```
source ~/env/bin/activate
```
This will load you python environment when you login. Now, run `source ~/.bashrc` followed by `which python`. The last command should point to an executable in your virtual environment. 

#### Installing numpy

Run the following command, which will tells numpy where the MKL library is located.
```
cat > ~/.numpy-site.cfg << EOF
[mkl]
library_dirs = $MKLROOT/lib/intel64
include_dirs = $MKLROOT/include
mkl_libs = mkl_rt
lapack_libs =
EOF
```

Then, go to the *~/dev/git* directory and run the following commands.
```
git clone https://github.com/numpy/numpy.git
cd numpy
python setup.py install
```

#### Installing other useful packages

Run the following commands.
```
pip install --upgrade pip
pip install ipython scipy scikit-learn h5py pandas
```
You can install any other package using pip.

Now, your environment is all set and you are ready to launch experiments!

## Submitting jobs

### Ressource allocation project

First, determine what your ressource allocation project is by running `colosse-info`. This will print a lot of stuff, including your various computation allocations. In my case, it prints
```
RAPI nne-790-aa: 0 used cores / 30 allocated cores (recent history)
RAPI nne-790-ae: 39.2049 used cores / 180 allocated cores (recent history)
RAPI agq-973-aa: 0 used cores / 30 allocated cores (recent history)
RAPI kyk-164-aa: 0 used cores / 30 allocated cores (recent history)
```
but you might only have one. Pick the allocation you want to use and remember its identifier, e.g., nne-790-ae.

### Submitting a job to the scheduler
Now, open the [example_job.msub](example_job.msub) file provided with this tutorial. The file header gives the scheduler some information about your job. For example, the header could be
```
#!/bin/bash
#PBS -l nodes=2:ppn=8,walltime=24:00:00
#PBS -o stdout.out
#PBS -e stderr.err
#PBS -V
#PBS -N myjob
#PBS -A nne-790-ae
```
In this case, the requested computing time is 24 hours. The job requires 2 nodes, with 8 CPUs each. The stderr and stdout are redirected to user specified files. The name of the job is *myjob*. The ressource allocation to use is *nne-790-ae*.

Copy the [example_job.msub](example_job.msub) file to a directory called *~/scratch/example_job*. Replace the ressource allocation project number by yours. Then, submit the job using the following command.
```
msub example_job.msub
```
Our example job will run for 5 minutes, so it should not be queued for a long time.

Once the job is submitted, you can use the `i` command to list the jobs that are in the waiting queue, i.e., the IDLE state. The `r` command shows all the jobs that are running and the `b` command shows all the jobs that are blocked, i.e., that the server refuses to run for the moment.

That's it! You can now submit jobs on Colosse.
