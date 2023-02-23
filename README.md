# HPC-and-slurm-intro
Orientation to high performance computing and slurm batch scheduling on the CHPC (Univ
ersity of Utah) advanced bioinformatics course at Utah Tech University

---

# Contents

-   [Objectives](#objectives)
-   [Getting Set Up](#getting-set-up)
-   [Exercise](#exercise)

---

# <a name="objectives"></a>
# Objectives 

-  Learn how to login to a CHPC cluster
-  Learn the basics of the CHPC environment (modules available, login node, scratch)
-  Learn how to submit jobs to the computing nodes using slurm
---

# <a name="getting-set-up"></a>
# Getting set up
If you are here as a UTU student taking BIOL 3300, you should do the following:

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

# <a name="logging-on"></a>
## Logging on 
'ssh' stands for 'secure shell'. By using ssh, you establish a secure connection between you and a remote computer

To use ssh for logging into the CHPC, do the following:

```
ssh <your-UNID>@lonepeak.chpc.utah.edu
```

- 'your-UNID' is the ID number you obtained from the University of Utah
- you will use the number that begins with a 'u'
- if it is your first time logging on, you will have to answer 'yes' to a security question


Once you have completed the worksheet, add, commit, and push the worksheet and the logfile to your forked repository.
```
add worksheet.md logfile
git commit -m "ran script and answered worksheet questions"
git push
```
