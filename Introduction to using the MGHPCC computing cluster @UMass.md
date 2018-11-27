# Introduction to using the MGHPCC computing cluster @UMass
The Massachusetts Green High Performance Computing Cluster is a shared computing resource available to UMass Amherst students, faculty, and staff. In our lab, we primarily use the cluster is useful for analyzing high-throughput sequence data but it can be useful for other large data sets as wells. see the [umass cluster user wiki](http://wiki.umassrc.org/wiki/index.php/Main_Page#Welcome_to_the_University_of_Massachusetts_Green_High_Performance_Computing_Cluster) for addtional and up-to-date info. For questions related to the UMass GHPCC cluster please contact: hpcc-support@umassmed.edu
## Connecting to the cluster for the first time
- step 1: request access to cluster (must be approved by PI) http://wiki.umassrc.org/wiki/index.php/Requesting_Access
- step 2: get an email with your user name and password
- step 3: download and install putty from https://www.chiark.greenend.org.uk/~sgtatham/putty/ (windows only)
- step 4: open putty
- step 5: type ghpcc06.umassrc.org into  "Host Name (or IP address)" box make sure port is set to 22 and connection type SSH is seldcted and click open
- step 6: type your user name into login as:
- step 7: type your given password NOTE: you will not be able to see anything appearing as you type your pw but you are still typing, press enter when you are done
- step 8:type your given password again
- step 9: type new password (Your password must contain:at least 9 characters, at least one digit, at least one other character (non alpha numeric))
- step 10: type new password again
- step 11: putty will automatically close, don't panic
- step 12: open putty again and type ghpcc06.umassrc.org into  "Host Name (or IP address)" box and click open
- step 13: type your new user name
- step 14: type your new password (you just set)
- step 15: You're ready to go!

#### Repeat steps 12-14 each time you want to use the cluster
## Basic Tips For Beginners

- When you are typing in your password, you won't be able to see anything, not even those little dots, but you are still typing.

- Copy and Paste in putty:
    - to copy text from putty just highlight it and it will be available to paste; Ctrl+C will cancel and commands you may have running (but will not affect a submitted job)
    - To paste copied text (from anywhere) into putty just right click. Clicking the right mouse button twice will act as an enter and submit your line of code. If you have copied and "enter" the command you paste will automatically run even if you only click once
- It is always best to make your commands/scripts/jobsubmissions in  a text editor/hackMD beforehand so you do not accidentally submit an job 
- You can also use the `echo` command and `#` to comment parts of the command out to have a script tell you what it is about to do before you actually submit it
- you can find resources about computing on the cluster on the [umass mghpcc wiki](http://wiki.umassrc.org/wiki/index.php/Main_Page#Welcome_to_the_University_of_Massachusetts_Green_High_Performance_Computing_Cluster)
- you can find resources about coding in unix in the MEC lab shared resources folder in Box

## Uploading to the cluster and sharing files

There are a variety of ways to transfer files to and from the cluster. Different techniques are convenient for different purposes. Each user has a home folder with a limited amount of space. Lab groups can request project folders with more space. We are currently doing most of our work in `project/uma_lisa_komoroske/`. However, we don't have permission to upload things from our own computers to the project space, so we have to upload to files our home space then move files to the project space

#### uploading files from your own computer via command line
this is most useful for mac users (or linux users) who have a command line terminal built into their computer. 

rsync allows you to upload folders containing mulitple files.the flags `-av` and `-e` require you to type yes before files transfer.
```
rsync -av -e ssh ./test.file user1@ghpcc06.umassrc.org:/home/user1/testfolder/
```
you can also use scp to copy files from your computer to the cluster
```
scp ./test.file /home/user1/testfolder
```
There is a Box app for rsync that we do not currently have access to but can look into if we want a different way to upload files.
#### uploading files from your own computer using a sftp client
This method allows you to use a GUI interface and search for files on your computer in the way you normally do.

Two commonly used sftp clients are [filezilla](https://filezilla-project.org/) and [cyberduck](https://cyberduck.io/)

These programs allow you to log into the cluster (using the same IP address, port, and login info you use to access the cluster) and drag and drop files from your computer to you home folder in the cluster.

Instructions fir filezilla
1. Download filezilla https://sourceforge.net/projects/filezilla/
2. look at the top bar on the program
    Host: ghpcc06.umassrc.org
    Username: user1
    Password: password1
    Port: 22
3. Click Quickconnect. The right column should populate with your cluster files
4. right click on the HTML report desired, then click view/edit
#### moving files from your home folder to the shared project space
You can use the `mv` command to move files once you are in the ghpcc cluster. 
```
mv ~/file.to.move /project/uma_lisa_komoroske/folder
```
It very important not to make a typo in the destination part of this command because if you move a file to a fodler that does not exist you cannot find that file again. It is best to use tab complete to type out paths. You can also use the `scp` command to move files.
```
scp ~/file.to.move /project/uma_lisa_komoroske/folder
```
this will create duplicate files so you can use the `rm` command to remove files. Be very careful with this command because you canot recover files that have been removed.
```
rm ~/file.to.delete
```
you can also use `mv` to rename files.
```
mv badname.txt goodname.txt
```
#### downloading files from the internet
As always, use caution when downloading files from the internet. 

You can use the `wget` or `curl` commands to download files from the web. You will need a file's address. See this [blog post](https://www.thegeekstuff.com/2012/07/wget-curl/) or google in general when to use each command. For the most part,they seem to be interchangeable
#### transferring files between users (only necessary if one user doesn't have access to shared project space)
In general, this is not very useful because the user you want to send something to will have to type their password into your computer nefore the file will send. So you have to be in the same room as them.

```  
rsync -av -e ssh /home/user1/testfolder/send.file user2@ghpcc06.umassrc.org:/home/user2/folder
```

NOTE about rsync:
This creates copies, we are not sharing one file. This is good because we cannot accidentally delete a file from all of our accounts. We need to be aware that one person's work on a file in their account will not update the file in other accounts.



## Making scripts 
Scripts are text files that contain commands that will run in order when the script in run. writing scripts allows you to submit a job that includes multiple commands and helps you keep track of your code to help facilitate troubleshooting and remond others and your future self what you did. The Following are some tips:
- write your script in a text editor or hackmd. programs like MS word can add hidden characters to your code 
- Text editors to download:
    - https://atom.io/
    - https://www.barebones.com/products/textwrangler/ (available from Apple App store)
- it's common for bash scripts to end with `.sh` so your script would be called `xxxscript.sh`
- all bash scripts begin with ```#!/bin/bash``` 
- you can upload/download scripts to the cluster as described above
- you can also create and edit scripts in console using ```nano xxxscript.sh```
- you can read your script in console using the command ```less xxxscript.sh``` 
- to make your script executable (give yourself permission to run it) use ```chmod u+x xxxscript.sh``` 
- to run your script in console use ```./path/xxxscript.sh```
- to submit a script as a job (see below for more details about bsub) to the cluster run ```bsub < ./path/xxxscript.sh```

## Submitting a job 

To run commands that will take a large amount of memory and/or time you will need to request resources/submit a job to the cluster. To submit a job to mghpcc you must use the `bsub` command. see the [wiki job submission page](http://wiki.umassrc.org/wiki/index.php/Submitting_Cluster_Jobs) for additonal and up-to-date info.

Jobs are run from the directory/environment that the job is submitted from and unless specified otherwise, output files will be written into the directory the job was submitted from. For your script to work, you will need to call files by the path relative to the directory you submit your job from. To avoid this you can use a files full path (use `pwd` to get full path). 

This is an example job submission for a command
```
#cd your.working.directory
bsub -q long -W 1:00 -R rusage[mem=16000] -n 4 -R span\[hosts=1\] #command# #FILENAME#
```
This is an example job submission for a scrip
```
#cd your.working.directory
bsub -q long -W 1:00 -R rusage[mem=16000] -n 4 -R span\[hosts=1\] < ./path/xxxscript.sh
```
The default resources requested by bsub are 1 hour, 1GB, and 1 core. You can adjust your resources using the flags below.
```
   #BSUB -n X                   # Where X is in the set {1..X}
   #BSUB -J Bowtie_job          # Job Name
   #BSUB -o myjob.out           # Append to output log file
   #BSUB -e myjob.err           # Append to error log file
   #BSUB -oo myjob.log          # Overwrite output log file 
   #BSUB -eo myjob.err          # Overwrite error log file 
   #BSUB -q short               # Which queue to use {short, long, parallel, GPU, interactive}
   #BSUB -W 0:15                # How much time does your job need (HH:MM)
   #BSUB -L /bin/sh             # Shell to use
   #BSUB -R span\[hosts=1\]     #says you want to use cores on the same host!
   #BSUB -R rusage[mem=16000]   # max amount of memory the cluster will use (in megabytes! so 16000 = 16GB)
   #BSUB -n 1                   # specifies number of cores you want to use
```
*Note: anytime you request more than one core you should use the flag `-R span\[hosts=1\]` to avoid issues with running the job in parallel*

Currently the smallest memory nodes in the long queue have 128G of memory, the largest 512G, so you can specify larger amounts, though the tradeoff tends to be a longer time pending for resources before the job can be dispatched.

To check job status and job number of all jobs you currently have running use:
```
bjobs 
```
If you notice a typo, or need to cancel a job for some reason you can use the following command:
```
bkill xxxx
# xxxx = your job number
```
In general, you should not have to do this often.

#### General thoughts about errors
- exit code 1 means there is an issue with your script/command code
- so far all other exit codes I've seen are related to job submission (i.e. job hit wall time, hit memory limit, etc.)
- if you get an error saying something like "lsbatch: command not found" try adding the program path to the command, or if the program is a python/java program you may need to add code about that. So far I just google and see what other people use, but I would try syntaxes used for programs used in the RNA seq markdown on our github first.
- if you get an error saying something like "lsbatch: xxx is/is not a directory or file not found" check for typos and that you submitted your job from the correct directory

## Using and Installing programs
In general, there are three main ways we can install/use programs to the cluster, module load, downloading files, and using the program bioconda.
#### module load
There are a variety of programs already installed and available for use on the cluster. You can also email  hpcc-support@umassmed.edu to see if they can make a new program available through module load. 

To see what programs are available use the command `module avail` to print a list of programs to your console.

To load a program use the command `module load program.v.1`. The program is now ready to use, but this command does not necessarily load the programs dependencies. Any program available through module load will have it's dependencies available through module load as well, so you just need to look at the program documentation (google) or error messages to figure out what prigrams to module load.

When you run `module load program.v.1` the program's location will print to your console. This path is needed for some programs.

#### installing and using programs downloaded from files

You can also install programs by downloading zipped files from the internet. You can download files using `wget`. You can unzip tar files using the following command
```
tar xzf program.v.1.tar.gz
```
you can then remove the zipped file. We typically download programs to /project/uma_lisa_komoroske/bin so everyone can use them. You will likely need the path to the program to use it. For example:
```
/project/uma_lisa_komoroske/bin/program.v.1/command1 file.txt
```

#### Installing and using programs with bioconda

[bioconda](https://bioconda.github.io/) is a program (available through module load) that makes downloading programs that have a lot of dependencies a lot easier. You can see what programs are available to install and use via biocionda [here](https://bioconda.github.io/recipes.html#recipes).

To install a program to our shared bin folder use the following commands:
```
cd /project/uma_lisa_komoroske/bin
module load anaconda2/4.4.0
conda create --prefix /project/uma_lisa_komoroske/bin/#PROGRAM#
source activate /project/uma_lisa_komoroske/bin/#PROGRAM#

conda config --add channels defaults
conda config --add channels conda-forge
conda config --add channels bioconda
conda install #PROGRAM#
source deactivate /project/uma_lisa_komoroske/bin/#PROGRAM#
```
To use the program in the future use the commands
```
cd /working.directory
module load anaconda2/4.4.0
source activate /project/uma_lisa_komoroske/bin/#PROGRAM#
#comands
source deactivate /project/uma_lisa_komoroske/bin/#PROGRAM#
```
You cna include these commands into a script to submit as a job.
#### General thoughts/opinons about program use and installation
- using module load for programs seems easiest and also doesn't use up our shared project space
- While at first glance it looks like using bioconda to install programs is more challenging than just downloading executable files, bioconda is usually easier because you don't have to worry about dependencies, it works the same way for all the programs it's compatible with, and you can easily update it (but updating is not automatic).
- if you are using a java based program you usually have to start your command with java -jar /path/to/program/program.jar