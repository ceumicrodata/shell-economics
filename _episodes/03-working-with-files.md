---
title: "Working with Files and Directories"
teaching: 30
exercises: 15
questions:
- "How can I view and search file contents?"
- "How can I create, copy and delete files and directories?"
- "How can I control who has permission to modify a file?"
- "How can I repeat recently used commands?"
objectives:
- View, search within, copy, move, and rename files. Create new directories.
- Use wild cards (`*`) to perform operations on multiple files.
- Make a file read only
- Use the `history` command to view and repeat recently used commands.
keypoints:
- "You can view file contents using `less`, `cat`, `head` or `tail`."
- "The commands `cp`, `mv`, and `mkdir` are useful for manipulating existing files and creating new directories."
- "You can view file permissions using `ls -l` and change permissions using `chmod`."
- "The `history` command and the up arrow on your keyboard can be used to repeat recently used commands."
---

## Working with Files

### Our data set: CSV files

Now that we know how to navigate around our directory structure, let's
start working with our data sets. We donwloaded data on procurement contracts from the World Bank to the `WorldBank` directory. 

### Wild cards

Navigate to your `WorldBank` directory.

~~~
$ cd ~/data/WorldBank
~~~
{: .bash}

We are interested in looking at the CSV file in this directory. We can list
all files with the .csv extension using the command:

~~~
$ ls *.csv
~~~
{: .bash}

~~~
Corporate_Procurement_Contract_Awards.csv
~~~
{: .output}

The `*` character is a special type of character called a wildcard, which can be used to represent any number of any type of character. 
Thus, `*.csv` matches every file that ends with `.csv`. 

This command: 

~~~
$ ls *Awards.csv
~~~
{: .bash}

~~~
Corporate_Procurement_Contract_Awards.csv
~~~
{: .output}

lists only the file that ends with `Awards.csv`.

We can use the command `echo` to see how the wildcard character is intepreted by the
shell.

~~~
$ echo *.csv
~~~
{: .bash}

~~~
Corporate_Procurement_Contract_Awards.csv
~~~
{: .output}

The `*` is expanded to include any file that ends with `.csv`.

This command:

~~~
$ ls /usr/local/bin/b*
~~~
{: .bash}

~~~
/usr/local/bin/bead	/usr/local/bin/bs	/usr/local/bin/bundler
/usr/local/bin/brew	/usr/local/bin/bundle
~~~
{: .output}

Lists every file in `/usr/local/bin` that starts with the character `b`.

> ## Home vs. Root
> 
> The `/` character is another navigational shortcut and refers to your root directory.
> The root directory is the highest level directory in your file system and contains
> files that are important for your computer to perform its daily work, but which you usually won't
> have to interact with directly. In our case,
> the root directory is two levels above our home directory, so `cd` or `cd ~` will take you to `/home/dcuser`
> and `cd /` will take you to `/`, which is equivalent to `~/../../`. Try not to worry if this is confusing,
> it will all become clearer with practice.
> 
> While you will be using the root at the beginning of your absolute paths, it is important that you avoid 
> working with data in these higher-level directories, as your commands can permanently alter files that the 
> operating system needs to function. In many cases, trying to run commands in root directories will require 
> special permissions which are not discussed here, so it's best to avoid it and work within your home directory.
{: .callout}

> ## Exercise
> Do each of the following tasks from your current directory using a single
> `ls` command for each.
> 
> 1.  List all of the files in `/usr/bin` that start with the letter 'c'.
> 2.  List all of the files in `/usr/bin` that contain the letter 'a'. 
> 3.  List all of the files in `/usr/bin` that end with the letter 'o'.
>
> Bonus: List all of the files in `/usr/bin` that contain the letter 'a' or the
> letter 'c'.
> 
> Hint: The bonus question requires a Unix wildcard that we haven't talked about
> yet. Trying searching the internet for information about Unix wildcards to find
> what you need to solve the bonus problem.
> 
> > ## Solution
> > 1. `ls /usr/bin/c*`
> > 2. `ls /usr/bin/*a*`
> > 3. `ls /usr/bin/*o`  
> > Bonus: `ls /usr/bin/*[ac]*`
> > 
> {: .solution}
{: .challenge}


## Command History

If you want to repeat a command that you've run recently, you can access previous
commands using the up arrow on your keyboard to go back to the most recent
command. Likewise, the down arrow takes you forward in the command history.

A few more useful shortcuts: 

- <kbd>Ctrl</kbd>+<kbd>C</kbd> will cancel the command you are writing, and give you a 
fresh prompt.
- <kbd>Ctrl</kbd>+<kbd>R</kbd> will do a reverse-search through your command history.  This
is very useful.
- <kbd>Ctrl</kbd>+<kbd>L</kbd> or the `clear` command will clear your screen.

You can also review your recent commands with the `history` command, by entering:

~~~
$ history
~~~
{: .bash}

to see a numbered list of recent commands. You can reuse one of these commands
directly by referring to the number of that command.

For example, if your history looked like this:

~~~
259  ls *
260  ls /usr/local/bin/b*
261  ls *.csv
~~~
{: .output}

then you could repeat command #260 by entering:

~~~
$ !260
~~~
{: .bash}

Type `!` (exclamation point) and then the number of the command from your history.
You will be glad you learned this when you need to re-run very complicated commands.

> ## Exercise
> Find the line number in your history for the command that listed all the 
> files starting with `b` in `/usr/local/bin`. Rerun that command.
>
> > ## Solution
> > First type `history`. Then use `!` followed by the line number to rerun that command.
> {: .solution}
{: .challenge}


## Examining Files

We now know how to switch directories, run programs, and look at the
contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all of the
contents using the program `cat`. 

Enter the following command from within the `WorldBank` directory: 

~~~
$ cat Corporate_Procurement_Contract_Awards.csv
~~~
{: .bash}

(Remember, you can use TAB completion.) This will print out all of the contents of the `Corporate_Procurement_Contract_Awards.csv` to the screen.


> ## Exercise
> 
> 1. Print out the contents of the `~/data/WorldBank/Corporate_Procurement_Contract_Awards.csv` file. What is the last line of the file? 
> 2.  From your home directory, and without changing directories,
> use one short command to print the contents of all of the files in
> the `~/data/WorldBank` directory.
> 
> > ## Solution
> > 1. The last line of the file is `06/29/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,Mobile Partners Career Program,IBRD,18-0449,Net Expat Inc.,USA,US,,WBG,WBG HR Vice Presidency`.
> > 2. `cat ~/data/WorldBank`
> {: .solution}
{: .challenge}

`cat` is a terrific program, but when the file is really big, it can
be annoying to use. The program, `less`, is useful for this
case. `less` opens the file as read only, and lets you navigate through it. The navigation commands
are identical to the `man` program.

Enter the following command:

~~~
$ less Corporate_Procurement_Contract_Awards.csv
~~~
{: .bash}

Some navigation commands in `less`

| key     | action |
| ------- | ---------- |
| <kbd>Space</kbd> | to go forward |
|  <kbd>b</kbd>    | to go backward |
|  <kbd>g</kbd>    | to go to the beginning |
|  <kbd>G</kbd>    | to go to the end |
|  <kbd>q</kbd>    | to quit |

`less` also gives you a way of searching through files. Use the
"/" key to begin a search. Enter the word you would like
to search for and press `enter`. The screen will jump to the next location where
that word is found. 

**Shortcut:** If you hit "/" then "enter", `less` will  repeat
the previous search. `less` searches from the current location and
works its way forward. Note, if you are at the end of the file and search
for the string "SOFTWARE", `less` will not find it. You either need to go to the
beginning of the file (by typing `g`) and search again using `/` or you
can use `?` to search backwards in the same way you used `/` previously.

For instance, let's search forward for the string `SOFTWARE` in our file. 
You can see that we go right to that string, what it looks like,
and where it is in the file. If you continue to type `/` and hit return, you will move 
forward to the next instance of this string. If you instead type `?` and hit 
return, you will search backwards and move up the file to previous examples of this string.

> ## Exercise
>
> What is the next word after the first instance of the string "`AUDIO`"?
> 
> > ## Solution
> > `VISUAL`
> {: .solution}
{: .challenge}

Remember, the `man` program actually uses `less` internally and
therefore uses the same commands, so you can search documentation
using "/" as well!

There's another way that we can look at files, and in this case, just
look at part of them. This can be particularly useful if we just want
to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at
the beginning and end of a file, respectively.

~~~
$ head Corporate_Procurement_Contract_Awards.csv
~~~
{: .bash}

~~~
Award Date,Quarter and Fiscal Year,Commodity Category,Contract Description,WBG Organization,Selection Number,Supplier,Supplier Country,Supplier Country Code,Contract Award Amount,Fund Source,VPU description
09/30/2013 12:00:00 AM,Q1 - FY14,CONTRACT CONSULTANTS,Scientific Coordination for Impact Evaluation Baseline Survey in Burkina Faso,IBRD,1115541,University Hospital Heidelberg,Germany,DE,325000.00,TRUST FUND,Africa
09/29/2013 12:00:00 AM,Q1 - FY14,SOFTWARE,Software development to support ICT-enabled Investment Climate reform in Bangladesh,IFC,14-0010,DataSoft Systems Bangladesh Li,Bangladesh,BD,,WBG,IFC VP Asia Pacific
09/27/2013 12:00:00 AM,Q1 - FY14,CONTRACT CONSULTANTS,Consortium-Approach to the Development of Gas to Power and the Gas Ring in the Energy Community,IBRD,1090280,"Economic Consulting Associates, Ltd.",United Kingdom,GB,1290836.64,TRUST FUND,Europe and Central Asia
09/27/2013 12:00:00 AM,Q1 - FY14,CONTRACT CONSULTANTS,Updating the Regional Balkans Infrastructure Study (REBIS),IBRD,1096179,Systema Consulting,Greece,GR,556297.09,TRUST FUND,Europe and Central Asia
09/26/2013 12:00:00 AM,Q1 - FY14,SOFTWARE,Software development to support ICT-enabled Investment Climate reform in Bangladesh,IFC,14-0010,Technohaven Company Ltd,Bangladesh,BD,,WBG,IFC VP Asia Pacific
09/26/2013 12:00:00 AM,Q1 - FY14,CONTRACT CONSULTANTS,Retrospective Evaluation of the Global Facility for Disaster Reduction and Recovery (GFDRR) Program in a Sample of Disaster-prone Countries,IBRD,1114772,Fundacion Dara Internacional,Spain,ES,293273.00,TRUST FUND,Sustainable Development Network
09/26/2013 12:00:00 AM,Q1 - FY14,SOFTWARE,Software development to support ICT-enabled Investment Climate reform in Bangladesh,IFC,14-0010,ATI Limited,Bangladesh,BD,,WBG,IFC VP Asia Pacific
09/26/2013 12:00:00 AM,Q1 - FY14,SOFTWARE,Software development to support ICT-enabled Investment Climate reform in Bangladesh,IFC,14-0010,Synesis IT Ltd.,Bangladesh,BD,,WBG,IFC VP Asia Pacific
09/25/2013 12:00:00 AM,Q1 - FY14,CONTRACT CONSULTANTS,Investment Prioritization for Resilient Livelihoods and Ecosystems in Coastal Zones of Tanzania,IBRD,1091861,DHI,Denmark,DK,489945.00,TRUST FUND,Africa
~~~
{: .output}

~~~
$ tail Corporate_Procurement_Contract_Awards.csv
~~~
{: .bash}

~~~
04/10/2018 12:00:00 AM,Q4 - FY18,SOFTWARE,Fined Grained Authorization Framework Implementation Services,IFC,18-0383,PlainID,USA,US,986000.00,WBG,IFC VP Corporate Strategy & Resources
06/19/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,Colombia Enhancing the Right of Access to Information by Victims of the Armed Conflict,IBRD,1257877,UNESCO,France,FR,280000.00,TRUST FUND,"VP, EFI Practice Group"
05/15/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,Review Certain Components of MIGAs Financial Risk Management Framework,MIGA,1255929,"Oliver Wyman, Inc.",USA,US,499500.00,WBG,MIGA
04/26/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,Myanmar Technical Consultant Yangon Elevated Expressway Public Private Partnership (PPP) Project,IFC,1254471,"Kyong Dong Engineering Co.,Ltd",Korea,KR,727680.00,TRUST FUND,IFC Chief Operating Officer
04/01/2018 12:00:00 AM,Q4 - FY18,TELECOMMUNICATIONS,Global Multiprotocol Label Switching (MPLS) Wide Area Network (WAN) Services,IBRD,17-0375,SITA Information Networking,USA,US,9544230.00,WBG,Information and Technology Solutions
05/16/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,Pakistan Study of China-Pakistan Economic Corridor:  Status of Implementation and Future Prospects,IBRD,1253684,CHIP Training & Consulting (Pvt) Lt,Pakistan,PK,396635.00,TRUST FUND,South Asia
04/19/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,"Kenya, Somalia Children on the Move: Using Satellite Data Analysis in Conflict/ Famine-Affected Areas",IBRD,1256457,National Center for Civic Innovation,USA,US,250000.00,TRUST FUND,Development Economics and Chief Economist
04/05/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,West Africa Integrated Land and Water Management for Adaptation to Climate Variability and Change (ILWAC),IBRD,1254023,International Water Mgmt. Institute,Sri Lanka,LK,347589.00,TRUST FUND,"VP, SD Practice Group"
06/13/2018 12:00:00 AM,Q4 - FY18,SOFTWARE,Develop Online Learning Components,IBRD,18-0097,TATA Interactive Systems Ltd.,USA,US,,WBG,Development Economics and Chief Economist
06/29/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,Mobile Partners Career Program,IBRD,18-0449,Net Expat Inc.,USA,US,,WBG,WBG HR Vice Presidency
~~~
{: .output}

The `-n` option to either of these commands can be used to print the
first or last `n` lines of a file. 

~~~
$ head -n 1 Corporate_Procurement_Contract_Awards.csv
~~~
{: .bash}

~~~
Award Date,Quarter and Fiscal Year,Commodity Category,Contract Description,WBG Organization,Selection Number,Supplier,Supplier Country,Supplier Country Code,Contract Award Amount,Fund Source,VPU description
~~~
{: .output}

Note that this is just the header of the CSV file.

~~~
$ tail -n 1 Corporate_Procurement_Contract_Awards.csv
~~~
{: .bash}

~~~
06/29/2018 12:00:00 AM,Q4 - FY18,CONTRACT CONSULTANTS,Mobile Partners Career Program,IBRD,18-0449,Net Expat Inc.,USA,US,,WBG,WBG HR Vice Presidency
~~~
{: .output}

## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, and search files. But what if we want to copy files or move
them around or get rid of them? Most of the time, you can do these sorts of file manipulations without the command line,
but there will be some cases (like when you're working on a server) where it will be
impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all 
of those files. In cases like this, it's much faster to do these operations at the command line.

### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted. 
For this lesson, our raw data is our CSV file.  We don't want to accidentally change the original file, so we'll make a copy of it
and change the file permissions so that we can read from, but not write to, the files.

First, let's make a copy of one of our CSV file using the `cp` command. 

Navigate to the `data/WorldBank` directory and enter:

~~~
$ cp Corporate_Procurement_Contract_Awards.csv Corporate_Procurement_Contract_Awards-copy.csv
$ ls -F
~~~
{: .bash}

~~~
Corporate_Procurement_Contract_Awards-copy.csv	Corporate_Procurement_Contract_Awards.csv	download.sh*
~~~
{: .output}

We now have two copies of the `Corporate_Procurement_Contract_Awards.csv` file, one of them named `Corporate_Procurement_Contract_Awards-copy.csv`. We'll move this file to a new directory
called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir`
followed by a space, then the directory name you want to create.

~~~
$ mkdir backup
~~~
{: .bash}

### Moving / Renaming 

We can now move our backup file to this directory. We can
move files around using the command `mv`. 

~~~
$ mv Corporate_Procurement_Contract_Awards-copy.csv backup
$ ls backup
~~~
{: .bash}
 
~~~
Corporate_Procurement_Contract_Awards-copy.csv
~~~
{: .output}

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup.

~~~
$ cd backup
$ mv Corporate_Procurement_Contract_Awards-copy.csv Corporate_Procurement_Contract_Awards-backup.csv
$ ls
~~~
{: .bash}

~~~
Corporate_Procurement_Contract_Awards-backup.csv
~~~
{: .output}

### File Permissions

We've now made a backup copy of our file, but just because we have two copies doesn't make us safe. We can still accidentally delete or 
overwrite both copies. To make sure we can't accidentally mess up this backup file, we're going to change the permissions on the file so
that we're only allowed to read (i.e. view) the file, not write to it (i.e. make new changes).

View the current permissions on a file using the `-l` (long) flag for the `ls` command. 

~~~
$ ls -l
~~~
{: .bash}

~~~
-rw-r--r--@ 1 koren  staff  358585 Feb 18 16:58 Corporate_Procurement_Contract_Awards-backup.csv
~~~
{: .output}

The first part of the output for the `-l` flag gives you information about the file's current permissions. There are ten slots in the
permissions list. The first character in this list is related to file type, not permissions, so we'll ignore it for now. The next three
characters relate to the permissions that the file owner has, the next three relate to the permissions for group members, and the final
three characters specify what other users outside of your group can do with the file. We're going to concentrate on the three positions
that deal with your permissions (as the file owner). 

![Permissions breakdown](../fig/rwx_figure.svg)

Here the three positions that relate to the file owner are `rw-`. The `r` means that you have permission to read the file, the `w` 
indicates that you have permission to write to (i.e. make changes to) the file, and the third position is a `-`, indicating that you 
don't have permission to carry out the ability encoded by that space (this is the space where `x` or executable ability is stored, we'll 
talk more about this in [a later lesson](http://www.datacarpentry.org/shell-genomics/05-writing-scripts/)).

Our goal for now is to change permissions on this file so that you no longer have `w` or write permissions. We can do this using the `chmod` (change mode) command and subtracting (`-`) the write permission `-w`. 

~~~
$ chmod -w chmod -w Corporate_Procurement_Contract_Awards-backup.csv
$ ls -l 
~~~
{: .bash}

~~~
-r--r--r--@ 1 koren  staff  358585 Feb 18 16:58 Corporate_Procurement_Contract_Awards-backup.csv
~~~
{: .output}

### Removing

To prove to ourselves that you no longer have the ability to modify this file, try deleting it with the `rm` command.

~~~
$ rm Corporate_Procurement_Contract_Awards-backup.csv 
~~~
{: .bash}

You'll be asked if you want to override your file permissions.

~~~
override r--r--r--  koren/staff for Corporate_Procurement_Contract_Awards-backup.csv? 
~~~
{: .output}

If you enter `n` (for no), the file will not be deleted. If you enter `y`, you will delete the file. This gives us an extra 
measure of security, as there is one more step between us and deleting our data files.

**Important**: The `rm` command permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

By default, `rm` will not delete directories. You can tell `rm` to
delete a directory using the `-r` (recursive) option. Let's delete the backup directory
we just made. 

Enter the following command:

~~~
$ cd ..
$ rm -r backup
~~~
{: .bash}

This will delete not only the directory, but all files within the directory. If you have write-protected files in the directory, 
you will be asked whether you want to override your permission settings. 

> ## Exercise
>
> Starting in the `data/WorldBank/` directory, do the following:
> 1. Make sure that you have deleted your backup directory and all files it contains.  
> 2. Create a copy of each of your FASTQ files. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't 
> learned yet how to do this
> with a wild-card.)  
> 3. Use a wildcard to move all of your backup files to a new backup directory.   
> 4. Change the permissions on all of your backup files to be write-protected.  
>
> > ## Solution
> >
> > 1. `rm -r backup`  
> > 2. `cp SRR098026.fastq SRR098026-backup.fastq` and `cp SRR097977.fastq SRR097977-backup.fastq`  
> > 3. `mkdir backup` and `mv *-backup.fastq backup`
> > 4. `chmod -w backup/*-backup.fastq`   
> > It's always a good idea to check your work with `ls -l backup`. You should see something like: 
> > 
> > ~~~
> > -r--r--r-- 1 dcuser dcuser 47552 Nov 15 23:06 SRR097977-backup.fastq
> > -r--r--r-- 1 dcuser dcuser 43332 Nov 15 23:06 SRR098026-backup.fastq
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}
