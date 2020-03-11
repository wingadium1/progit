Getting Started
===============

This chapter will be about getting started with Git. We will begin at the beginning by explaining some background on version control tools, then move on to how to get Git running on your system and ﬁnally how to get it setup to start working with. At the end of this chapter you should understand why Git is around, why you should use it and you should be all setup to do so.

About Version Control
---------------------

What is version control, and why should you care? Version control is a system that records changes to a ﬁle or set of ﬁles overtime so that you can recall speciﬁc versions later. For the examples in this book you will use software source code as the ﬁles being version controlled, though in reality you can do this with nearly any type of ﬁle on a computer.

If you are a graphic or web designer and want to keep every version of an image or layout (which you would most certainly want to), a Version Control System (VCS) is a very wise thing to use. It allows you to revert ﬁles back to a previous state, revert the entire project back to a previous state, compare changes over time, see who last modiﬁed something that might be causing a problem, who introduced an issue and when, and more. Using a VCS also generally means that if you screw things up or lose ﬁles, you can easily recover. In addition, you get all this for very little overhead.

### Local Version Control Systems

Many people's version-control method of choice is to copy ﬁles into another directory (perhaps a time-stamped directory, if they're clever). This approach is very common because it is so simple, but it is also incredibly error prone. It is easy to forget which directory you're in and accidentally write to the wrong ﬁle or copy over ﬁles you don't mean to.

To deal with this issue, programmers long ago developed local VCSs that had a simple database that kept all the changes to ﬁles under revision control (see Figure 1.1).

![](../media/26ac315eb6f6088f3911d1197ba19b66.png)

Figure 1.1 Local version control diagram

One of the more popular VCS tools was a system called rcs, which is still distributed with many computers today. Even the popular Mac OS X operating system includes the rcs command when you install the Developer Tools. This tool basically works by keeping patch sets (that is, the differences between ﬁles) from one change to another in a special format on disk; it can then re-create what any ﬁle looked like at any point in time by adding up all the patches.
### Centralized Version Control Systems

The next major issue that people encounter is that they need to collaborate with developers on other systems. To deal with this problem, Centralized Version Control Systems (CVCSs) were developed. These systems, such as CVS, Subversion and Perforce, have a single server that contains all the versioned ﬁles, and a number of clients that check out ﬁles from that central place. For many years, this has been the standard for version control (see Figure 1.2).

![](../media/ed26bfcec6e6f9b250092a54df3cc34d.png)

Figure 1.2 Centralized version control diagram

This setup offers many advantages, especially over local VCSs. For example, everyone knows to a certain degree what everyone else on the project is doing. Administrators have ﬁne-grained control over who can do what; and it's far easier to administer a CVCS than it is to deal with local databases on every client.

However, this setup also has some serious downsides. The most obvious is the single point of failure that the centralized server represents. If that server goes down for an hour, then during that hour nobody can collaborate at all or save versioned changes to anything they're working on. If the hard disk the central database is on becomes corrupted, and proper backups haven't been kept, you lose absolutely everything—the entire history of the project except what ever single snapshots people happen to have on their local machines. Local VCS systems suffer from this same problem—whenever you have the entire history of the project in a single place, you risk losing everything.

### Distributed Version Control Systems

This is where Distributed Version Control Systems (DVCSs) step in. In a DVCS     (such as Git, Mercurial, Bazaar or Darcs), clients don't just check out the     latest snapshot of the ﬁles: they fully mirror the repository. Thus if any     server dies, and these systems were collaborating via it, any of the client     repositories can be copied back up to the server to restore it. Every     checkout is really a full backup of all the data (see Figure 1.3).

![](../media/f1f1805c0eb1cf6841653a547c6151c1.png)

Figure 1.3 Distributed version control diagram

Furthermore, many of these systems deal pretty well with having several remote repositories they can work with, so you can collaborate with different groups of people in different ways simultaneously with in the same project. This allows you to setup several types of workﬂows that aren't possible in centralized systems, such as hierarchical models.

A Short History of Git
----------------------

As with many great things in life, Git began with a bit of creative destruction and ﬁery controversy. The Linux kernel is an open source software project of fairly large scope. For most of the lifetime of the Linux kernel maintenance (1991-2002), changes to the software were passed around as patches and archived ﬁles.

In 2002, the Linux kernel project began using a proprietary DVCS system called
BitKeeper.

In 2005, the relationship between the community that developed the Linux kernel and the commercial company that developed BitKeeper broke down, and the tool's free-of-charge status was revoked. This prompted the Linux development community (and in particular Linus Torvalds, the creator of Linux) to develop their own tool based on some of the lessons they learned while using BitKeeper. Some of the goals of the new system were as follows:
-   Speed
-   Simple design
-   Strong support for non-linear development (thousands of parallel branches)
-   Fully distributed
-   Able to handle large projects like the Linux kernel efﬁciently (speed and data size)

Since its birth in 2005, Git has evolved and matured to be easy to use and yet
retain these initial qualities. It's incredibly fast, it's very efﬁcient with
large projects, and it has an incredible branching system for non-linear
development (See Chapter 3).

Git Basics
----------

So, what is Git in a nutshell? This is an important section to absorb, because if you understand what Git is and the fundamentals of how it works, then using Git effectively will probably be much easier for you. As you learn Git, try to clear your mind of the things you may know about other VCSs, such as Subversion and Perforce; doing so will help you avoid subtle confusion when using the tool. Git stores and thinks about information much differently than these other systems, even though the user interface is fairly similar; understanding those differences will help prevent you from becoming confused while using it.

### Snapshots, Not Differences

The major difference between Git and any other VCS (Subversion and friends included) is the way Git thinks about its data. Conceptually, most other systems store information as a list of ﬁle-based changes. These systems (CVS, Subversion, Perforce, Bazaar, and so on) think of the information they keep as a set of ﬁles and the changes made to each ﬁle over time, as illustrated in Figure 1.4.

![](../media/496641cb064ab3e7cfae5f7d1f5c6c78.png)

Figure 1.4 Other systems tend to store data as changes to a base version of each ﬁle.

Git doesn't think of or store its data this way. Instead, Git thinks of its data more like a set of snapshots of a mini ﬁlesystem. Every time you commit, or save the state of your project in Git, it basically takes a picture of what all your ﬁles look like at that moment and stores a reference to that snapshot. To bee fﬁcient, if ﬁles have not changed, Git doesn't store the ﬁle again—just a link to the previous identical ﬁle it has already stored. Git thinks about its data more like Figure 1.5.

![](../media/dfecf414bca6ad630fbafee9766a7b68.png)

Figure 1.5 Git stores data as snapshots of the project over time.

This is an important distinction between Git and nearly all other VCSs. It makes Git reconsider almost every aspect of version control that most other systems copied from the previous generation. This makes Git more like a mini ﬁlesystem with some incredibly powerful tools built on top of it, rather than simply a VCS. We'll explore some of the beneﬁts you gain by thinking of your data this way when we cover Git branching in Chapter 3.

### Nearly Every Operation Is Local

Most operations in Git only need local ﬁles and resources to operate generally no information is needed from another computer on your network. If you're used to a CVCS where most operations have that network latency overhead, this aspect of Git will make you think that the gods of speed have blessed Git with unworldly powers. Because you have the entire history of the project right there on your local disk, most operations seem almost instantaneous.

For example, to browse the history of the project, Git doesn't need to go out to the server to get the history and display it for you—it simply reads it directly from your local database. This means you see the project history almost instantly. If you want to see the changes introduced between the current version of a ﬁle and the ﬁle a month ago, Git can look up the ﬁle a month ago and do a local difference calculation, instead of having to either ask a remote server to do it or pull an older version of the ﬁle from the remote server to do it locally.

This also means that there is very little you can't do if you're ofﬂine or off VPN. If you get on an airplane or a train and want to do a little work, you can commit happily until you get to a network connection to upload. If you go home and can't get your VPN client working properly, you can still work. In many other systems, doing so is either impossible or painful. In Perforce, for example, you can't do much when you aren't connected to the server; and in Subversion and CVS, you can edit ﬁles, but you can't commit changes to your database (because your database is ofﬂine). This may not seem like a huge deal, but you may be surprised what a big difference it can make.

### Git Has Integrity

Everything in Git is check-summed before it is stored and is then referred to by that checksum. This means it's impossible to change the contents of any ﬁle or directory without Git knowing about it. This functionality is built into Git at the lowest levels and is integral to its philosophy. You can't lose information in transit or get ﬁle corruption without Git being able to detect it. The mechanism that Git uses for this check summing is called a SHA–1 hash. This is a 40-character string composed of hexadecimal characters (0-9 and a-f) and calculated based on the contents of a ﬁle or directory structure in Git. A SHA–1 hash looks something like this:

```
24b9da6552252987aa493b52f8696cd6d3b00373
```

You will see these hash values all over the place in Git because it uses them so much. In fact, Git stores everything not by ﬁle name but in the Git database addressable by the hash value of its contents.

### Git Generally Only Adds Data

When you do actions in Git, nearly all of them only add data to the Git database. It is very difﬁcult to get the system to do anything that is not undoable or to make it erase data in any way. As in any VCS, you can lose or mess up changes you haven't committed yet; but after you commit a snapshot into Git, it is very difﬁcult to lose, especially if you regularly push your database to another repository.

This makes using Git a joy because we know we can experiment without the danger of severely screwing things up. For a more in-depth look at how Git stores its data and how you can recover data that seems lost, see "Under the Covers" in Chapter 9.

### The Three States

Now, pay attention. This is the main thing to remember about Git if you want the rest of your learning process to go smoothly. Git has three main states that your ﬁles can reside in: committed, modiﬁed, and staged. Committed means that the data is safely stored in your local database. Modiﬁed means that you have changed the ﬁle but have not committed it to your database yet. Staged means that you have marked a modiﬁed ﬁle in its current version to go into your next commit snapshot.

This leads us to the three main sections of a Git project: the Git directory, the working directory, and the staging area.

![](../media/6bad5a651da3822d21024eaa2392f9fc.png)

Figure 1.6 Working directory, staging area, and git directory

The Git directory is where Git stores the metadata and object database for your project. This is the most important part of Git, and it is what is copied when you clone a repository from another computer.

The working directory is a single checkout of one version of the project. These ﬁles are pulled out of the compressed database in the Git directory and placed on disk for you to use or modify.

The staging area is a simple ﬁle, generally contained in your Git directory, that stores information about what will go into your next commit. It's sometimes referred to as the index, but it's becoming standard to refer to it as the staging area.

The basic Git workﬂow goes something like this:
1.  You modify ﬁles in your working directory.
2.  You stage the ﬁles, adding snapshots of them to your staging area.
3.  You do a commit, which takes the ﬁles as they are in the staging area and stores that snapshot permanently to your Git directory.

If a particular version of a ﬁle is in the git directory, it's considered committed. If it's modiﬁed but has been added to the staging area, it is staged. And if it was changed since it was checked out but has not been staged, it is modiﬁed. In Chapter 2, you'll learn more about these states and how you can either take advantage of them or skip the staged part entirely.

Installing Git
--------------

Let's get into using some Git. First things ﬁrst—you have to install it. You can get it a number of ways; the two major ones are to install it from source or to install an existing package for your platform.

### Installing from Source

Some people may instead find it useful to install Git from source, because you'll get the most recent version. The binary installers tend to be a bit behind, though as Git has matured in recent years, this has made less of a difference.

If you do want to install Git from source, you need to have the following libraries that Git depends on: autotools, curl, zlib, openssl, expat, and libiconv. For example, if you're on a system that has dnf (such as Fedora) or apt-get (such as a Debian-based system), you can use one of these commands to install the minimal dependencies for compiling and installing the Git binaries:

```
$ sudo dnf install dh-autoreconf curl-devel expat-devel gettext-devel
openssl-devel perl-devel zlib-devel

$ sudo apt-get install dh-autoreconf libcurl4-gnutls-dev libexpat1-dev gettext
libz-dev libssl-dev

In order to be able to add the documentation in various formats (doc, html,
info), these additional dependencies are required:

$ sudo dnf install asciidoc xmlto docbook2X

$ sudo apt-get install asciidoc xmlto docbook2x
``` 

| Note | Users of RHEL and RHEL-derivatives like CentOS and Scientific Linux will have to [enable the EPEL repository](https://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F) to download the docbook2X package. |
|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|

### Installing on Linux

If you want to install Git on Linux via a binary installer, you can generally do so through the basic package-management tool that comes with your distribution. If you're on Fedora, you can use yum:

```
$ yum install git-core 
```


Or if you're on a Debian-based distribution like Ubuntu, try apt-get:

```
$ apt-get install git-core
```

### Installing on Mac

There are two easy ways to install Git on a Mac. The easiest is to use the graphical Git installer, which you can download from the SourceForge page (see Figure 1.7): [https://sourceforge.net/projects/git-osx-installer/](https://sourceforge.net/projects/git-osx-installer/files/)

The other major way is to install Git via Brew (<https://brew.sh/>).

You don't have to add all the extras, but you'll probably want to include +svn in case you ever have to use Git with Subversion repositories (see Chapter 8).

### Installing on Windows

There are also a few ways to install Git on Windows. The most official build is available for download on the Git website. Just go to <https://git-scm.com/download/win> and the download will start automatically. Note that this is a project called Git for Windows, which is separate from Git itself; for more information on it, go to [https://gitforwindows.org](https://gitforwindows.org/).

To get an automated installation you can use the [Git Chocolatey package](https://chocolatey.org/packages/git). Note that the Chocolatey package is community maintained.

Another easy way to get Git installed is by installing GitHub Desktop. The installer includes a command line version of Git as well as the GUI. It also works well with PowerShell, and sets up solid credential caching and sane CRLF settings. We'll learn more about those things a little later, but suffice it to say they're things you want. You can download this from the [GitHub Desktop website](https://desktop.github.com/).

First-Time Git Setup
--------------------

Now that you have Git on your system, you'll want to do a few things to customize your Git environment. You should have to do these things only once; they'll stick around between upgrades. You can also change them at any time by running through the commands again.

Git comes with a tool called git config that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

-   ```/etc/gitconfig```
file: Contains values for every user on the system and all their repositories. If you pass the option--system to git config, it reads and writes from this file specifically.

-   ```.gitconfig```
file: Specific to your user. You can make Git read and write to this file specifically by passing the ```--global```
option.

-   config file in the git directory (that is, ```.git/config```) of whatever repository you're currently using: Specific to that single repository. Each level overrides values in the previous level, so values in ```.git/config```
trump those in ```/etc/gitconfig```.

On Windows systems, Git looks for the .gitconfig file in the ```$HOME```
directory (```C:\Documents and Settings\$USER```
for most people). It also still looks for ```/etc/gitconfig```, although it's relative to the MSys root, which is wherever you decide to install Git on your Windows system when you run the installer.

### Your Identity

The first thing you should do when you install Git is to set your user name and e-mail address. This is important because every Git commit uses this information, and it's immutably baked into the commits you pass around:

```
$ git config --global user.name "John Doe"

$ git config --global user.email johndoe@example.com
```

Again, you need to do this only once if you pass the ```--global```
option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or e-mail address for specific projects, you can run the command without the ```--global```
option when you're in that project.

### Your Editor

Now that your identity is set up, you can configure the default text editor that will be used when Git needs you to type in a message. By default, Git uses your system's default editor, which is generally Vi or Vim. If you want to use a different text editor, such as Emacs, you can do the following:

```
$ git config --global core.editor emacs 
```

### Your Diff Tool

Another useful option you may want to configure is the default diff tool to use to resolve merge conﬂicts. Say you want to use vim diff:

```
$ git config --global merge.tool vimdiff 
```

Git accepts kdiff3, tkdiff, meld, xxdiff, emerge, vimdiff, gvimdiff, ecmerge, and opendiff as valid merge tools. You can also set up a custom tool; see Chapter 7 for more information about doing that.

### Checking Your Settings

If you want to check your settings, you can use the git config ```--list``` command to list all the settings Git can find at that point:

```
$ git config --list

user.name=Hoang Thanh Son

user.email=wingadium1@gmail.com

color.status=auto

color.branch=auto

color.interactive=auto

color.diff=auto ... 
```

You may see keys more than once, because Git reads the same key from different files (```/etc/gitconfig```
and ```.gitconfig```, for example). In this case, Git uses the last value for each unique key it sees. You can also check what Git thinks a specific key's value is by typing git config key:

```
$ git config user.name Hoang Thanh Son 
```

Getting Help
--------------------

If you ever need help while using Git, there are three ways to get the manual page (manpage) help for any of the Git commands:

```
$ git help <verb>
$ git <verb> --help
$ man git-<verb> 
```

For example, you can get the manpage help for the config command by running

```
$ git help config 
```

These commands are nice because you can access them anywhere, even ofﬂine. If the man pages and this book aren't enough and you need in-person help, you can try the ```#git```
or ```#github```
channel on the Freenode IRC server(irc.freenode.net). These channels are regularly filled with hundreds of people who are all very knowledgeable about Git and are often willing to help.

Summary
--------------------

You should have a basic understanding of what Git is and how it's different from the CVCS you may have been using. You should also now have a working version of Git on your system that's set up with your personal identity. It's now time to learn some Git basics.

If you can read only one chapter to get going with Git, this is it. This chapter covers every basic command you need to do the vast majority of the things you'll eventually spend your time doing with Git. By the end of the chapter, you should be able to configure and initialize a repository, begin and stop tracking files, and stage and commit changes. We 'll also show you how to setup Git to ignore certain files and file patterns, how to undo mistakes quickly and easily, how to browse the history of your project and view changes between commits, and how to push and pull from remote repositories.
