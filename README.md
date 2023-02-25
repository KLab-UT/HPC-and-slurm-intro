# HPC-and-slurm-intro
Orientation to high performance computing and slurm batch scheduling on the CHPC (University of Utah) advanced bioinformatics course at Utah Tech University


---

# Contents

-   [Objectives](#objectives)
-   [Getting Set Up](#getting-set-up)
-   [Logging On](#logging-on)
-   [Creating a Batch Script](#creating-a-batch-script)
-   [Submitting a Job](#submitting-a-job)

---

# <a name="objectives"></a>
# Objectives 

-  Learn how to login to a CHPC cluster
-  Learn the basics of the CHPC environment (modules available, login node, scratch)
-  Learn how to submit jobs to the computing nodes using slurm
---

# <a name="getting-set-up"></a>
# Getting set up
If you are here as a UTU student taking BIOL 4300, you should do the following:

1.  Login to your [Github](https://github.com/) account.

1.  Fork [this repository](https://github.com/rklabacka/HPC-and-slurm-intro), by
    clicking the 'Fork' button on the upper right of the page.

    After a few seconds, you should be looking at *your* 
    copy of the repo in your own Github account.

1.  Click the 'Clone or download' button, and copy the URL of the repo via the
    'copy to clipboard' button. **note: if you have an SSH key with your github account, make sure you select the ```SSH``` tab**

1.  In your terminal, navigate to where you want to keep this repo (you can
    always move it later, so just your home directory is fine). Then type:

        $ git clone the-url-you-just-copied

    and hit enter to clone the repository. Make sure you are cloning **your**
    fork of this repo.

1.  Next, `cd` into the directory:

        $ cd the-name-of-directory-you-just-cloned

1.  At this point, you should be in your own local copy of the repository.

    As you work on the exercise below, be sure to frequently `commit` your work
    and `push` changes to the *remote* copy of the repo hosted on Github.
---

# <a name="study-design"></a>
# Logging on and using the CHPC

The CHPC is the high performance computing (HPC) system owned and operated by the University of Utah
This guide will help you login to the supercomputer and learn some of the basic commands
Before using this guide, you should have completed the following:
1. Created a UNID account
2. Used the class token to be added to the CHPC class group
> note: if you are using the CHPC for research outside of the class, you don't need to use a token. Instead, a CHPC profile will be created for the K-Lab UTU group.

---

# <a name="logging-on"></a>
## Logging On 
'ssh' stands for 'secure shell'. By using ssh, you establish a secure connection between you and a remote computer

To use ssh for logging into the CHPC, do the following:

```
ssh <your-UNID>@lonepeak.chpc.utah.edu
```

- 'your-UNID' is the ID number you obtained from the University of Utah
- you will use the number that begins with a 'u'
- This should prompt you to insert your password. Your password is the same as your UNID password.
- if it is your first time logging on, you will have to answer 'yes' to a security question

This will take you to your personal directory on the login node for the cluster. Look at your location using the following command:

```pwd```

You'll see that you are within a directory named after your UNID. This is your home directory on the login node. Any time you use the ```~``` symbol in a path, you'll see that it is referring to your home directory.


Try this out by moving to the root directory and then move back to your home directory:

```
cd /
pwd
cd ~
pwd
```

IMPORTANT NOTE: You should only read/create/edit files within your home directory. You shouldn't have access to anything else, but just-in-case I felt I should mention this here.

Within your home directory you can create your own environment with directories and script files. Create a directory within your home directory called ```BIOL_4310```

```mkdir -p ~/BIOL_4310```

Now the directory ```BIOL_4310``` is within your home directory. Check it out (make sure you are in your home directory when you do this)

```ls```

Now create a bash script within the 'BIOL_4310' directory that writes the text 'Hello World' to a text file called 'output.txt' (you can do this using a linux terminal text editor [such as vim] or you can use cat to create it with one line)

```
cd ~/BIOL_4310
echo 'echo "hello world\n" >> output.txt' > BIOL_4310.sh
```

You can then run this script as you would on your local computer, but you are doing it on the cluster!

```
bash BIOL_4310.sh
```

IMPORTANT NOTE: You should never run demanding commands from the login node, regardless of the directory you are in. The login node is for editing files, submitting jobs, and analyzing output. Very inexpensive computation is okay (e.g., installing software packages is usually okay), but be careful. If you notice a job is taking a long time to run, you should kill it. Any computation that requires lots of time/power should be conducted on the compute nodes- not the login nodes.

The CHPC has multiple clusters for high performance computing. These can all be seen on the [CHPC website](https://chpc.utah.edu/documentation/gettingstarted.php) in the "Accessing the HPC Systems" section. If you ever have questions about using the CHPC, use the link above for accessing the CHPC documentation. We only have access to the general clusters (not the protected environment), but you can log into any of these using your UNID.


# <a name="creating-a-batch-script"></a>
## Creating a Batch Script 

As stated above, when you login to a CHPC cluster, you land at the login node for that cluster. On this node, you should never run programs that are computationally intensive (i.e., require lots of power or long run times). However, you might be asking yourself, "They why the heck are we using the supercomputer if we can't run big jobs? That doesn't sound very super!" Computationally intensive jobs will be submitted from your login node to a compute node. The software on the CHPC that enables these submissions is called 'slurm'.

Slurm is a workload manager used for job submission on HPC clusters so that multiple people can access compute nodes for their high-intensity computing needs.

Move to the ```BIOL_4310``` directory you created earlier

```
cd ~/BIOL_4310
```

You should have a bash script here called 'BIOL_4310.sh' that appends 'hello world\n' to a an output file. You can run this script on a compute node by converting your simple script to a batch script as described below (For more detailed instructions, see the [CHPC doc page](https://chpc.utah.edu/documentation/software/slurm.php#submit)

To make your batch script, create a new bash script called ```q.SlurmNewbie.sh``` and add the following lines to the top of the file:

```
#!/bin/sh
#SBATCH --account=utu
#SBATCH --partition=lonepeak
#SBATCH --nodes=1
#SBATCH --mem=1
#SBATCH --ntasks=1
#SBATCH -o slurm-%j.out-%N
#SBATCH -e slurm-%j.err-%N

cd ~/BIOL_4310
bash BIOL_4310.sh
```

Every line that begins with ```#SBATCH``` is an option that will be interpreted by slurm. Any line that starts with ```#``` and is not imediately followed with ```BATCH``` will simply be interpreted as a comment.

To see the values you can include for ```account``` and ```partition```, enter the following command while on the cluster:

```
myallocation
```

- The account is the group name you are a part of (it is NOT your UNID). For those in the advanced bioinformatics class, it is ```utu-biol4310```. For those in the research lab who are linked to the CHPC, it is ```utu```.

- The partition is the cluster containing the compute node(s) you will run the job on.

The ```--nodes``` (```-N```) option specifies the number of nodes you are requesting. You can think of a node as a single computer. When we refer to the supercomputer as "the cluster", we are referring to the "cluster" of nodes that are used. In other words, the collection of interconnected computers that can be accessed for computing tasks. For computationally intensive jobs, you may want to use multiple nodes. The login node is essentially a node with low memory dedicated to user interaction (e.g., writing scripts). The compute nodes have more memory and are dedicated to computation. A node is made up of multiple processors.

The ```--mem``` option specifies the number of memory you will use in the node (e.g., ```--mem=32``` means you will use 32GB of memory)

The ```-o``` option specifies the name of the standard output file (stdout). In this example, we use the job number (```%j```) and the first node (```%N```) as components of the filename (this is a good practice, but is not necessary). Anytime you use a command such as 'echo' that typically prints something to your terminal screen, it will be appended to this file.

The ```-e``` option specifies the name of the error file. In this example, we use the job number (```%j```) and the first node (```%N```) as components of the filename (this is a good practice, but is not necessary). If your script runs into errors, the error messages will be printed to this file.

The ```--ntasks``` (```-n```) option specifies the number of parallelized tasks you will utilize.
> When running a command in parallel, the SLURM built in ```$SLURM_NTASKS``` variable can then be used to denote the number of MPI tasks to run. In case of a plain MPI job, this number should equal number of nodes (```$SLURM_NNODES```) times number of cores per node.


Once you have completed the worksheet, add, commit, and push the worksheet and the logfile to your forked repository.
```
add worksheet.md logfile
git commit -m "ran script and answered worksheet questions"
git push
```

# <a name="submitting-a-job"></a>
## Submitting a Job

Rather than running your script locally, you're now ready to submit the script as a batch to the slurm queue. Submit the script using the following command:

```
sbatch q.SlurmNewbie.sh
```

It may take some time to run if the queue is long. Check back periodically for the output files. 



