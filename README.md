# Lab-Unix

## Lesson 2: Basic Unix command and Introduction to WHOI's HPC

### Review
**Last time we:**
1. Opened the terminal on our local machine
2. Talked about file / folder structures
3. Tested out some  commands for navigating around our file structures via the command line:
        - `ls`: list files in current location
        - `pwd`: print working directory (where am I?)
        - `cd`: change directory (moving from place to place)


### Goals for today: 
**Today we will:**
1. Get an introduction to HPCs in general and the HPC we will be using for this class (`poseidon.whoi.edu`) 
2. Discuss remote machine access (`ssh`) and log on to the HPC
3. Review the commands we learned at the end of last class 
4. Review wildcards and regex
5. Review outputs & string commands together
6. Review scripts


## 2.1 Logging on to the HPC
For this class (and all subsequent classes) we will be using WHOI's HPC: `poseidon.whoi.edu`. This is a remote cluster of computers (it isn't really *that* remote; it is in the basement of Clark). Nearly all command line systems (BASH etc.) have Secure Shell (`ssh`) natively installed. `ssh` is a cryptographic network protocol that allows you to provides a secure channel over an unsecured network. `ssh` can be used to log on to any number of platforms such as: remote computers (like a lab computer), computer clusters or high performance computers (HPCs), computers running in the cloud (e.g. AWS), etc. 

Today we will be using `ssh` to logon to `poseidon.whoi.edu`. To do this you will need to know what your WHOI username is and be within WHOI's firewall either by connecting to the local network (e.g. `eduroam`) or by logging into WHOI's vpn. 

Once you are logged on to WHOI's network you should be ready to `ssh`. In your terminal prompt type the following: 

```bash
ssh username@poseidon.whoi.edu
``` 
If this is your first time logging onto the network you will see a prompt like the following: 

``` text 
	Host key not found from the list of known hosts.
	Are you sure you want to continue connecting (yes/no)? 
```
This is a safety protocol ensuring that you do indeed want to add this address to a list of known hosts. Type `yes`. 

You should now be prompted for your password (this will be your WHOI email password): 
``` text 
Host 'poseidon.whoi.edu' added to the list of known hosts.
      [usernames]'s password:
```
Once you type the correct password you should see something like the following:
```text
  ___             _    _             ___ _         _           
 | _ \___ ___ ___(_)__| |___ _ _    / __| |_  _ __| |_ ___ _ _ 
 |  _/ _ (_-</ -_) / _` / _ \ ' \  | (__| | || (_-<  _/ -_) '_|
 |_| \___/__/\___|_\__,_\___/_||_|  \___|_|\_,_/__/\__\___|_|  

Welcome to the Poseidon Cluster at WHOI!

Please do not run anything on the login nodes and submit jobs to SLURM. All running 
jobs/processes on the login nodes may be terminated without notice.

===================================================================================
$SCRATCH no longer provides any performance advantage.
===================================================================================

For more information, please visit https://hpc.whoi.edu.
For assistance, please contact the Help Desk at helpdesk@whoi.edu or x2439.
```
Congratulations! You have logged on to the HPC! This terminal now represents the environment of the remote computer that you just logged on to. 


>Look at your `prompt`. Has it changed? What information do you see now? What do the different parts mean? Hint: try using the command `whoami` and the command `hostname`. 

## 2.2 Navigating to our classroom directory and making folders
For this class we have set up a special workspace on `poseidon`. This is where you will do all your homework and projects. 

>Navigate to `/proj/omics/env-bio/2025`. What folders do you see?  Hint: if you are typing out the path provided rather than copying it, try hitting the `tab` key in the middle of the word or phrase, this should automatically complete the phrase for you *or* if there is more than one option repeatedly hitting `tab` will bring up a listing of all options. 

You should see a folder within `/proj/omics/env-bio/2025` called `users/`. This directory holds many subdirectories that we will be using in the class. 

Navigate to the `users` folder and create your personal directory. Use your poseidon user name, and command `mk` ("make directory"). Go to the folder you just made 

```bash
cd users
mkdir mpachiadaki
cd mpachiadaki
```
Get the class material from https://github.com/environmental-bioinformatics-master/unix-folders/archive/master.zip

```bash
wget  https://github.com/environmental-bioinformatics-master/unix-folders/archive/master.zip
```

And unzip it. Unlike last class where you had the option of unzipping `masters.zip` through your computer's GUI file navigator (`Finder` or the like), on the HPC you DO NOT have access to any sort of GUI and you must do everything via the `CLI`. 

```bash
unzip masters.zip
```

Now, navigate into the newly created folder `unix-folders-master/`

## Understanding Absolute vs. Relative Paths
In Unix-like operating systems, file paths can be expressed in two primary ways: absolute paths and relative paths. Let's practice navigating using absolute and relative paths.

First, check the 📂 Folder Structure by listing all it's contents (and including detailed information)

```bash
ls -lh
```

The class unix folder contains folders and files
```text
drwxrwsr-x 2 mpachiadaki sg-envbio-mgr 4.0K Sep 10  2019 data
drwxrwsr-x 2 mpachiadaki sg-envbio-mgr 4.0K Sep 10  2019 dictionary
drwxrwsr-x 2 mpachiadaki sg-envbio-mgr 4.0K Sep 10  2019 measurements
drwxrwsr-x 2 mpachiadaki sg-envbio-mgr 4.0K Sep 10  2019 notes
drwxrwsr-x 2 mpachiadaki sg-envbio-mgr 4.0K Sep 10  2019 scripts
-rw-rw-r-- 1 mpachiadaki sg-envbio-mgr  100 Sep 10  2019 stars
-rw-rw-r-- 1 mpachiadaki sg-envbio-mgr   77 Sep 10  2019 truth
drwxrwsr-x 2 mpachiadaki sg-envbio-mgr 4.0K Sep 10  2019 writing
```
to check what all folders contain you can use the wild card *

```bash
ls *
```


- 🛠️ Absolute Path

An **absolute path** gives the *full address* of a file or folder, starting from the root `/`.

For me, the absolute path to the file `lion.out` which is inside `data` is:

```bash
less /proj/omics/env-bio/2025/users/mpachiadaki/unix-folders-master/data/lion.out
```
*press q to leave the file*

Absolute paths work from anywhere in the filesystem, because they always point to the same place.

- 🛠️ Relative Path

A relative path starts from your current location (working directory).

If you are inside unix-folders-master:
```bash
less data/lion.out
```

If you have navigated to `data\`:
```bash
cd data
less lion.out
```

Relative paths are shorter, but they only make sense depending on where you are.

🔎 Special Relative Path Symbols

. → current directory

.. → parent directory (one level up)

~ → home directory

Example: since you are now inside the `data/` folder, you can read the file `words` (in `dictionary/`) like this:

```bash
less ../dictionary/words
```

>🧩 Challenge
Try these commands:
- From inside unix-folders-master/measurements/, open lipids.dat using a relative path.
- From anywhere on the system, print the contents of truth using an absolute path.


