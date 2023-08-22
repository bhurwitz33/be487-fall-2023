# Introducing the Shell

### Questions:
- "What is a command shell and why would I use one?"
- "How can I move around in a computer?"
- "How can I see what files and directories I have?"
- "How can I specify the location of a file or directory on my computer?"

### Objectives:
- "Describe key reasons for learning shell."
- "Learn how to access a remote machine."
- "Navigate your file system using the command line."
- "Access and read help files for `bash` programs and use help files to identify useful command options."
- "Demonstrate the use of tab completion, and explain its advantages."

### Keypoints:
- "The shell gives you the ability to work more efficiently by using keyboard commands rather than a GUI."
- "Useful commands for navigating your file system include: `ls`, `pwd`, and `cd`."
- "Most commands take options (flags) which begin with a `-`."
- "Tab completion can reduce errors from mistyping and make work more efficient in the shell."


## What is a shell and why should I care?

A *shell* is a computer program that presents a command line interface
which allows you to control your computer using commands entered
with a keyboard instead of controlling graphical user interfaces
(GUIs) with a mouse/keyboard combination.

There are many reasons to learn about the shell.

* Many bioinformatics tools can only be used through a command line interface, or 
have extra capabilities in the command line version that are not available in the GUI.
This is true, for example, of BLAST, which offers many advanced functions only accessible
to users who know how to use a shell.  
* The shell makes your work less boring. In bioinformatics you often need to do
the same set of tasks with a large number of files. Learning the shell will allow you to
automate those repetitive tasks and leave you free to do more exciting things.  
* The shell makes your work less error-prone. When humans do the same thing a hundred different times
(or even ten times), they're likely to make a mistake. Your computer can do the same thing a thousand times
with no mistakes.  
* The shell makes your work more reproducible. When you carry out your work in the command-line 
(rather than a GUI), your computer keeps a record of every step that you've carried out, which you can use 
to re-do your work when you need to. It also gives you a way to communicate unambiguously what you've done, 
so that others can check your work or apply your process to new data.  
* Many bioinformatic tasks require large amounts of computing power and can't realistically be run on your
own machine. These tasks are best performed using remote computers or cloud computing, which can only be accessed
through a shell.

In this lesson you will learn how to use the command line interface to move around in your file system. 

## How to access the shell

On a Mac or Linux machine, you can access a shell through a program called Terminal, which is already available
on your computer. If you're using Windows, you'll need to download a separate program to access the shell (see installation instructions [here](https://carpentries-incubator.github.io/metagenomics-workshop/setup.html)).

In this workshop, we will use the campus HPC, so we can invest most of our time learning the basics of shell by manipulating some experimental data, instead of dealing with installations. The bioinformatics packages we will use for this class have already been installed on the HPC. Also, we will work with large datasets that can only be run on a server with a significant amount of CPU and memory  (not your laptop!)

> ## Shell alternatives
> 
> You can also access the campus HPC from the [UA HPC Online portal](https://ood.hpc.arizona.edu/pun/sys/dashboard). To get to the shell, go to the top menu bar select "Clusters" and "Shell access" from the pull-down list. 
> This resource also provides you with access to other interactive resources like Jupyter notebook and R studio servers that we will use in this class.
> 

## HPC Sponsorship
Ask Dr. Hurwitz for sponsorship to the HPC if you have not already received an email with a notice that you have received sponsorship.

## Logging in to the shell from your laptop
If you are accessing the shell from your laptop, you need to log in using the `ssh` command (ssh stands for Secure Shell), your username and the address of the machine you are logging into.
~~~
$ ssh bhurwitz@hpc.arizona.edu
~~~

When you are prompted to type the password, and use duo mobile for 2-factor authentication. Take into account that while you are typing a password no characters will appear on the screen, trust that they are being typed and press enter. 

After logging in, you will see a screen showing something like this: 

~~~
Last login: Fri Aug 18 13:36:52 2023 from c-71-226-40-183.hsd1.az.comcast.net
This is a bastion host used to access the rest of the RT/HPC environment.

Type "shell" to access the job submission hosts for all environments
-----------------------------------------

[bhurwitz@gatekeeper ~]$ 

~~~

This provides a lot of information about the remote server that you're logging in to. In this case, we are logging into the gatekeeper node. This is the node that is the "gateway" to the clusters at UA. In our case, we want to go the the ocelote cluster (our teaching cluster). To do this, we use the "shell" command and then "ocelote" to go to the ocelote cluster.

~~~
[bhurwitz@gatekeeper ~]$ shell
Last login: Fri Aug 18 11:24:47 2023 from ood.hpc.arizona.edu
***
The default cluster for job submission is Puma
***
Shortcut commands change the target cluster
-----------------------------------------
Puma:
$ puma
(puma) $
Ocelote:
$ ocelote
(ocelote) $
ElGato:
$ elgato
(elgato) $
-----------------------------------------

(puma) [junonia@/home/u20/bhurwitz]$ ocelote
~~~

Once you login, the system will send you to your home directory. In my case, this is "/home/u20/bhurwitz". 

## Navigating your file system

The part of the operating system responsible for managing files and directoriesis called the **file system**.
It organizes our data into files, which hold information,
and directories (also called "folders"), which hold files or other directories.

Several commands are frequently used to create, inspect, rename, and delete files and directories.

> ## Preparation Magic
>
> If you type the command:
> `PS1='\W\$ '`
> into your shell, followed by pressing the <kbd>Enter</kbd> key,
> your window should look like this:    
> `~\$ `   
> That only shows the ultimate directory where you ar standing. In this case
> it is the home directory. The symbol `~` is an abbreviation of the home directory. 
> This isn't necessary to follow along (in fact, your prompt may have
> other helpful information you want to know about).  This is up to you!  

The dollar sign is a **prompt**, which shows us that the shell is waiting for input;
your shell may use a different character as a prompt and may add information before the prompt. When typing commands, either from these lessons or from other sources, do not type the prompt, only the commands that follow it. In this lesson we will use the dollar sign to indicate the prompt. 

~~~
$
~~~

Let's find out where we are by running a command called `pwd`
(which stands for "print working directory").
At any moment, our **current working directory**
is our current default directory,
i.e.,
the directory that the computer assumes we want to run commands in
unless we explicitly specify something else.
Here, the computer's response is `/home/u20/bhurwitz`,
which is my home directory.

~~~
$ pwd
~~~

~~~
/home/u20/bhurwitz
~~~

Let's look at how our file system is organized. We can see what files and subdirectories are in this directory by running `ls`,
which stands for "listing":

~~~
$ ls
~~~

~~~
ondemand  scripts  teaching
~~~

`ls` prints the names of the files and directories in the current directory in alphabetical order, arranged neatly into columns. 

But, because our metagenomics files are large, we are going to be working in the `/xdisk/bhurwitz/bh_class/<your_netid>` directory. We'll be working within the `bh_class` subdirectory, and creating new subdirectories, throughout this class.  

The command to change locations in our file system is `cd` followed by a directory name to change our working directory.
`cd` stands for "change directory".

Let's say we want to navigate to the `bh_class/<your_netid>` directory we saw above (where you swap out <your_netid> with your own netid).  We can use the following command to get there:

~~~
$ cd /xdisk/bhurwitz/bh_class/<your_netid>
~~~

Let's look at what is in this directory:

~~~
$ ls
~~~

~~~
assignments exercises
~~~

We can make the `ls` output more comprehensible by using the **flag** `-F`,
which tells `ls` to add a trailing `/` to the names of directories, or other symbols to identify the type of elements in the directory:

~~~
$ ls -F
~~~

~~~
assignments/  exercises/
~~~

Anything with a "/" after it is a directory. Things with a "*" after them are programs. If there are no decorations, it's a file.

To understand a little better how to move between folders, let's see the following image:

<a href="../fig/directory_structure.png">
  <img src="../fig/directory_structure.png" width="870" height="631" alt="Folder organization diagram showing a parent directory called dc_workshop, with tree subdirectories called data, mags, and taxonomy. Insida data there is another one called untrimmed_fastq, and inside taxonomy there is another one called mags_taxonomy."/>
</a>

Here we can see a diagram of how the folders are arranged one inside another. In this way, if we think about moving,
from your directory to the assignments folder, the path must go as they are ordered: `cd <your_net_id>/assignments`

`ls` has lots of other options. To find out what they are, we can type:

~~~
$ man ls
~~~

Some manual files are very long. You can scroll through the file using
your keyboard's down arrow or use the <kbd>Space</kbd> key to go forward one page and the <kbd>b</kbd> key to go backwards one page. When you are done reading, hit <kbd>q</kbd> to quit.

> ## Excercise 1: Extra information with `ls -l`
> Use the `-l` option for the `ls` command to display more information for each item 
> in the directory. What is one piece of additional information this long format
> gives you that you don't see with the bare `ls` command?
>
<details>
  <summary markdown="span">Solution</summary>
  <ul>
~~~
$ ls -l
~~~

~~~
total 12
drwxr-xr-x 3 dcuser dcuser 4096 Jun  3 17:59 data
drwxrwxr-x 2 dcuser dcuser 4096 Jun  3 18:02 mags
drwxrwxr-x 3 dcuser dcuser 4096 Jun  3 18:25 taxonomy
~~~

The additional information given includes the name of the owner of the file, when the file was last modified, and whether the current user has permission to read and write to the file.

</details>

No one can possibly learn all of these arguments, that's why the manual page
is for. You can (and should) refer to the manual page or other help files
as needed.

Let's go into the `data/untrimmed_fastq` directory and see what is in there.

~~~
$ cd data/untrimmed_fastq
$ ls
~~~
{: .bash}

~~~
JC1A_R1.fastq.gz  JC1A_R2.fastq.gz  JP4D_R1.fastq.gz  JP4D_R2.fastq.gz  TruSeq3-PE.fa
~~~
{: .output}

This directory contains a file `TruSeq3-PE.fa`, that we will use in a later lesson and four files with `.fastq.gz` extensions. FASTQ is a format
for storing information about sequencing reads and their quality. GZ is an archive file compressed.
We will be learning more about FASTQ files in a later lesson. These data comes in a compressed format, 
which is why there is a `.gz` at the end of the files. 
This makes it faster to transfer, and allows it to take up less space on our computer. 
Let's use `gunzip` to decompress the files so that we can look at the FASTQ format.
~~~
$ gunzip JC1A_R1.fastq.gz  JC1A_R2.fastq.gz  JP4D_R1.fastq.gz  JP4D_R2.fastq.gz
$ ls
~~~
{: .bash}

~~~
JC1A_R1.fastq  JC1A_R2.fastq  JP4D_R1.fastq  JP4D_R2.fastq  TruSeq3-PE.fa
~~~
{: .output}

### Shortcut: Tab Completion

Usually the key Tab is located on the left side of the keyboard just above the "Shift" key or "Caps lock" key. 

Typing out file or directory names can waste a
lot of time and it's easy to make typing mistakes. Instead we can use tab complete 
as a shortcut. When you start typing out the name of a directory or file, then
hit the <kbd>Tab</kbd> key, the shell will try to fill in the rest of the
directory or file name.

Return to your home directory:

~~~
$ cd
~~~
{: .bash}

then enter:

~~~
$ cd dc<tab>
~~~
{: .bash}

The shell will fill in the rest of the directory name for
`dc_workshop`.

Now change directories to `dc_workshop`

~~~
$ cd dc_workshop
~~~
{: .bash}

Using tab complete can be very helpful. However, it will only autocomplete
a file or directory name if you've typed enough characters to provide
a unique identifier for the file or directory you are trying to access.

If we navigate to our `data` directory and try to access one of our sample files:

~~~
$ cd data/untrimmed_fastq
$ ls JC<tab>
~~~
{: .bash}

The shell auto-completes your command to `JC1A_R`, because there is another file name in 
the directory begin with this prefix. When you hit
<kbd>Tab</kbd> again, the shell will list the possible choices.

~~~
$ ls JC1A_R<tab><tab>
~~~
{: .bash}

~~~
JC1A_R1.fastq  JC1A_R2.fastq
~~~
{: .output}

Tab completion can also fill in the names of programs, which can be useful if you
remember the beginning of a program name.

~~~
$ pw<tab><tab>
~~~
{: .bash}

~~~
pwd   pwdx
~~~
{: .output}

Displays the name of every program that starts with `pw`. 

## Summary

We now know how to move around our file system using the command line.
This gives us an advantage over interacting with the file system through
a Graphical User Interface (GUI) as it allows us to work on a remote server, carry out the same set of operations 
on a large number of files quickly, and opens up many opportunities for using 
bioinformatics software that is only available in command line versions. 

In the next few episodes, we'll be expanding on these skills and seeing how 
using the command line shell enables us to make our workflow more efficient and reproducible.

## Key Points
- "The shell gives you the ability to work more efficiently by using keyboard commands rather than a GUI."
- "Useful commands for navigating your file system include: `ls`, `pwd`, and `cd`."
- "Most commands take options (flags) which begin with a `-`."
- "Tab completion can reduce errors from mistyping and make work more efficient in the shell."
