# CU supercomputer

We have free access to amazing computing resources for machine learning. CU has a supercomputer with NVIDIA 80GB A100 tensor core GPU nodes. These are specialized GPUs for machine learning. These are roughly \$20K each. Each compute node on CU's Alpine supercomputing cluster has three of these, so \$60K worth of compute. You should use them!!

Full documentation for using CU Research Computing resources is here:

https://curc.readthedocs.io



## Sign up

Signing up for a Research Computing account only takes a few minutes and you will get approval almost right away. It may ask for a reason. Put "EBIO 5460 Machine Learning for Ecology class, spring 2026". To sign up, go [here](https://www.colorado.edu/rc/) and click "Request an Account to use RC Resources". You also need to have set up [Duo 2 factor authentication](https://oit.colorado.edu/services/identity-access-management/duo-multi-factor-authentication#useduo) for your CU Boulder identikey.



## Login to computing resources

Login to the server via ssh, which establishes a secure shell session. Do this from a terminal on your desktop or laptop. Mac OS has a built in **Terminal** application generally found in the Utilities folder in Applications. Windows 11 has a built in **Windows Terminal** application (click start, search for "Terminal"), or you can use the open source standalone terminal program [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Linux distributions will have at least one terminal option.

```bash
ssh jbsmith@login.rc.colorado.edu
```

You'll be prompted for username and password. If pasting your password, use a right click or middle click, then press enter. Since the password is not shown, this can be a bit fussy and system dependent to ensure you paste only your password. Once you enter your password, it will seem that nothing is happening. You now must open the DUO app on your phone and approve the push notification (or verify the DUO 2FA another way depending on how you set it up). Then in the terminal you'll get a **prompt** that looks something like this:

```bash
You are using login node: login12
[jbsmith@login12 ~]$
```

When you first login, you are on a login node of the supercomputer. From here you can organize files and submit jobs but you can't do any live computing work in Python or R.

Once done with your work on the supercomputer, you'll navigate back to this login node and type `logout` to close the connection to the supercomputer.



## Transfer files

It's useful to have a GUI app to transfer files back and forth from your laptop to the supercomputer. You can also transfer files with command line tools (e.g. `scp`, `rsync`) but it's often faster and more convenient to work with a GUI. The following are trusted open source options.

* [WinSCP](http://winscp.net). Win, best option.
* [Filezilla](https://filezilla-project.org/). Win, Mac, Linux, recommended by CU research computing.
* [Cyberduck](https://cyberduck.io/). Win, Mac.

These applications use sftp (secure file transfer protocol). You'll need to put in the hostname or supercomputer address as `sftp://dtn.rc.colorado.edu` (dtn = data transfer node), your username and password, and you'll need to verify the Duo 2-factor notification (e.g. on your phone) to start the connection.

In WinSCP and Filezilla the file browser has two window panes, one showing files on the client, the other showing files on the server. You can manually transfer files back and forth or set it to synchronize client and host. Cyberduck presents a browser for the server's file system and you can drag and drop files to/from there, or set it to synchronize a folder.

Other ways to transfer files are described [here](https://curc.readthedocs.io/en/latest/compute/data-transfer.html).



## Linux commands

On the supercomputer and other linux servers, manage files using linux commands. When you login you'll be in your home directory, e.g. `/home/jbsmith`, which is indicated by the `~` symbol at the prompt. If you transferred files already you most likely transferred them here. To list the files in the current directory, type:
```bash
ls
```

To view the contents of a file, type:

```bash
cat filename
```

The `cat` command concatenates files but is also the most common way to print files to the terminal. Sometimes a file is so large that once it has printed you can't scroll back enough to see all of it. You can use the `more` command instead:

```bash
more filename
```

Press the spacebar to print the next screen's worth of text.

To edit a file you can use the command line tool `nano`.

```bash
nano filename
```

Within nano use the arrow keys to move the cursor. When you're done editing, exit nano, saving the file:
```
ctrl-x
# choose yes to save buffer when prompted
# press enter to save the file as original filename
```

It's natural to organize files in directories. To make a new directory

```bash
mkdir mydirectory
```
Move files using
```bash
mv myfile mydirectory
```

Navigate to the new directory using
```bash
cd mydirectory       #change directory
```

To go back, or "up" a directory

```bash
cd ..
```

Some other useful linux commands:
```bash
pwd                  #path of the current ("present working") directory
ls -al               #list files, including hidden files
rm myfile            #delete (remove) file
cp myfile mynewfile  #copy file
rmdir mydirectory    #remove directory
rm -r mydirectory    #remove directory with all its subfolders and files
man <command>        #help (manual) on a linux command
man rm               #e.g. help for remove
```



## Projects directory

It is generally best to work from your projects directory on RC computing because this has much more storage for large files, which you'll likely need for machine learning projects. We don't need to do that just yet but here is how you get there;

```bash
cd /projects/jbsmith
```

You can then set up directories here for specific projects. For example, you could make a directory for this class, and navigate to it:

```
mkdir ml4e
cd ml4e
```



## Manage environments

Typically, each computing project needs a unique set of R and/or Python libraries. The supercomputer uses the **Conda** package manager to set up isolated computing environments with unique combinations of software and libraries. This eases a lot of pain caused by incompatible package versions, and specific versions of R and Python, that are needed for different projects. It's possible to load preinstalled versions of R or Python outside of the conda system (via the module system) but I find it cleanest to always use conda environments.

We can't do any compute on a login node.  We must first transfer to a compute node. To set up conda environments, the custom is to use the compile nodes set aside especially for this (although it often makes sense to use an interactive compute node instead to meet certain requirements, such as testing code or compiling for GPU). Transfer to a CPU compile node with:

```bash
acompile
```

The prompt in your terminal will change to something like:

```
[jbsmith@c3cpu-a2-u32-2 ~]$
```

where `c3cpu-a2-u32-2` is the name of the compute node. There is a time limit to being on any compute node. If you exceed the time limit, you'll be kicked back to the login node. This can be important to plan for.

Software on the supercomputer is provided as modules that need to be loaded. Each time we switch to a compute node we need to load the conda software:

```bash
module load anaconda
```

The prompt in your terminal will change to something like:

```
(base) [jbsmith@c3cpu-a2-u32-2 ~]$
```

where `(base)` indicates that we are in the base conda environment. The base conda environment is a basic installation of a recent python version. To determine which version of Python is installed:

```bash
python --version
```

or start an interactive Python session:

```bash
python
```

We can then type Python commands at the Python `>>>` prompt

```python
print("hello world")
```

To exit Python

```python
exit()
```

I tend to leave the base conda environment alone and always create new environments to interact with. There are a number of different ways that one can set up conda environments. The most common approach, recommended by Research Computing, uses a slightly different strategy for setting up Python versus R environments.

### Update warning message (ignore this)

The conda installation and version is controlled by the system administrators of the supercomputer. The version installed might be older than the current version. Thus, when using conda you might often get the following message: `WARNING: A newer version of conda exists.` and to `Please update conda`. This is not something that you can do, as regular users do not have permission to write to the installation location, so please ignore the warning message.

### Python

For Python environments, start by setting up a Python environment and install packages using conda where possible. If you have a project that needs several specific packages, often it's necessary to install everything at once so that dependencies can be resolved. It's very common for machine learning libraries to have dependency issues since the codebase changes fast.

To initiate a new conda environment:

```bash
conda create --name py-datasci
```

The environment can be any name. I called it `py-datasci` to remind me that it's a general data science setup. Once the environment is created, activate it:

```bash 
conda activate py-datasci
```

Now install packages into the environment

```bash
conda install numpy pandas matplotlib scikit-learn -c conda-forge
```

where `-c conda-forge` says to use the conda-forge channel. This might take some time. If all goes well, start an interactive Python session.

```bash
python
```

Now you can type or copy/paste python commands, e.g.

```python
import numpy as np
rng = np.random.default_rng()
rng.uniform(size=10)
```

You can't plot any graphics in this text-only session but you can save plots to file. Generally, I consider the supercomputer to be only for compute. Visualization is best done on your local computer.

To quit python type `quit()`. Deactivate the conda environment:

```bash
conda deactivate
```

which will return you to the `base` environment. 



### R

For R environments, start by setting up a conda environment for R but then use R itself to install packages. To initiate a new conda environment:

```bash
conda create --name r-latest
```

I called it `r-latest` to remind me that it's the latest R version at the time it was set up. Once the environment is created, activate it:

```bash 
conda activate r-latest
```

Now install R into the environment

```bash
conda install r-base -c conda-forge
```

This might take some time. If all goes well, you can now start an interactive R session.

```bash
R
```

Now you can type or copy/paste R commands, e.g.

```R
runif(10)
```

As for Python, or any other software, you can't plot any graphics in this text-only session but you can save plots to file.

To install packages, do so from within R. These are installed into the current conda environment. They are independent from other environments.

```R
install.packages("dplyr")
```

Since packages need to be compiled, this takes some time. Occasionally you'll find a package that won't compile because it has a linux dependency. You can often install the dependency from conda. If that isn't possible, you might be able to install the R package directly from conda.

Quit R by typing `q()` and choose to not save the workspace. Deactivate the conda environment:

```bash
conda deactivate
```

which will return you to the `base` environment.



### Conda commands

Managing environments

```bash
conda create --name myexample
conda activate myexample
conda deactivate
conda env list
conda rename --name existing_environment_name new_environment_name
conda env remove --name myexample
conda create --name new_environment_name --clone existing_environment_name
```

Managing packages within an activated environment

```bash
conda install pkgnames -c conda-forge
conda list                                 #list installed packages
conda remove pkgname
conda search pkgname -c conda-forge        #find available versions
conda install pkgname=2.13 -c conda-forge  #install specific versions
conda list --revisions                     #history
conda restore --revision <revision-number> #revert to a previous state
```

More commands in the conda [documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/index.html)



### Return to a login node

Type `exit` to close the compute session. You might want to type `clear` to clear the screen as the prompt will often jump to somewhere randomly in the printed output.



## Transfer to a compute node

We can't do any compute on a login node.  We must first transfer to a compute node.

Available compute resources:

https://curc.readthedocs.io/en/latest/clusters/alpine/alpine-hardware.html#

### Compiling

For compiling software and straight-forward conda environments intended for CPU workflows

* **acompile**

From a login node, type

```bash
acompile
```

### Interactive work

For interactive work, which should be limited to experimenting with one-off runs, testing your code, or compiling more complex workflows, we'll use these nodes:

* **atesting**: CPU workflows
  * max: 2 nodes x 8 cores, 3 hours

From a login node, type:

```bash
sinteractive --partition=atesting --time=0:60:00 --nodes=1 --ntasks=4 --qos=testing
```

* **atesting_a100**: GPU workflows
  * max: 1 node x 10 cores, 1 GPU (20GB VRAM), 1 hour

From a login node, type:

```bash
sinteractive --partition=atesting_a100 --time=0:60:00 --nodes=1 --ntasks=4 --gres=gpu:1 --qos=testing
```

In the above, adjust `time`, `nodes`, `ntasks`, and `gres` as needed. A node is essentially a full computer (server) that lives in the data center. Within a node are multiple CPUs or GPUs: `ntasks` specifies the number of CPUs requested per node `gres=gpu:` specifies the number of GPUs requested per node.

### Submitted jobs

For longer or larger computing jobs, we need to use the supercomputer's batch processing mechanism. This requires us to submit a job via **slurm**, which is scheduling software that manages all the jobs submitted by users of the supercomputer. These nodes are used for jobs submitted to the scheduler:

* **amilan**: CPU workflows
* **aa100**: GPU workflows
  * max: 16 GPUs

#### Template slurm script

To submit a job, we need to write a script to ask for the resources we need, set up the computing environment, and run our code. In the following example, we are asking for a job on `aa100` , the GPU partition of the CU supercomputer. Each node has 64 CPU cores and 3 GPUs. We'll ask for 1 node with 4 cores (we are using the GPUs for compute and don't need much CPU compute) and 3 GPUs. We'll ask for a maximum time of 15 minutes. Specify a quality of service (qos). Enter your email address. Enter a filename for the output file. This is set up for a conda environment. Enter the name of the conda environment. Delete the lines related to a conda environment if not using. The lines with `echo` will print to the output file along with any text sent to the output stream. Enter the command for the line that runs the actual scientific code.


```bash
#!/bin/bash

#SBATCH --partition=aa100        #GPU=aa100, CPU=amilan
#SBATCH --nodes=1
#SBATCH --ntasks=4               #number of cores per node (max 64)
#SBATCH --gres=gpu:3             #number of GPUs per node (max 3)
#SBATCH --time=00:15:00          #hours:minutes:seconds
#SBATCH --qos=normal             #for jobs longer than 1 day this needs to be qos=long
#SBATCH --mail-type=ALL
#SBATCH --mail-user=put_your_email_address_here
#SBATCH --output=my_job_%j.out #output file (%j adds job id)

echo "== Starting Job =="

echo "== Loading conda module =="
module purge
module load anaconda

echo "== Activating conda environment =="
conda activate put_name_of_conda_env_here

echo "== Starting scientific code =="
put bash command to start your parallel code here

echo "== End of Job =="
```

Save the above to a script file, e.g. `my_long_job.sh`

Check the file

```
cat my_long_job.sh
```

Now we can submit the job to the slurm system, which will put it in the queue.

```
sbatch my_long_job.sh
```

When the job starts and again when it finishes, you'll be sent an email. We can also check on its status in the queue:

```bash
squeue --user=put_your_user_name_here --start
```

See explanatory codes for squeue [here](https://curc.readthedocs.io/en/latest/running-jobs/squeue-status-codes.html).

When the job is finished, the output will be in the file named `my_job_<job_number>.out`, where the job number is appended to the file name by slurm. Type `ls` to see the current files in the working directory, and type:

```bash
cat my_job_<job_number>.out
```

Useful commands for managing your jobs

https://curc.readthedocs.io/en/latest/running-jobs/slurm-commands.html#

https://curc.readthedocs.io/en/latest/running-jobs/squeue-status-codes.html

```
squeue --user=your_username   #check status of jobs in queue
scancel your_job-id           #cancel a job
sstat --jobs=your_job-id      #info about an actively running job
```
