Some Ubuntu commands
<a name="up" />

**[A](#a) [B](#b) [C](#c) [D](#d) [E](#e) [F](#f) [G](#g) [H](#h) [I](#i)
  [J](#j) [K](#k) [L](#l) [M](#m) [N](#n) [O](#o) [P](#p) [Q](#q) [R](#r)
  [S](#s) [T](#t) [U](#u) [V](#v) [W](#w) [X](#x) [Y](#y) [Z](#z)
  [&](#and) [|](#or) [!](#exclamation) [;](#semicolon) [>](#less)
  [.](#dot) [\\](#backslash) [$](#dollar)**

---
### <a name="a" />[A&ensp;:arrow_heading_up:](#up)

`addgroup` add new group. See `delgroup` delete group.

`adduser` add new user. See `deluser` delete user.

`apt` high-level commandline interface for the package management system.
For example, `sudo apt install packagename` installs new `packagename` from
repository. Commands `sudo apt update` and `sudo apt upgrade` are
necessary for keepering your Ubuntu up to date.

`apt-get` similar to `apt` older version.

---
### <a name="b" />[B&ensp;:arrow_heading_up:](#up)

`bash` **B**ourne-**A**gain **SH**ell,
sh-compatible command language interpreter.

`.bashrc` hidden file in the user accoutn directory,
that initializes an interactive shell session.

`bc` command line calculator. To exit type `quit`.

---
### <a name="c" />[C&ensp;:arrow_heading_up:](#up)

`cat` con**cat**enate files and print on the standard output.
Read files sequentially and write them to standard output.

`cd` change directory.

`chfn` change a user's finger information.
This information is stored in the file `/etc/passwd`,
and includes the user's real name, work room, work phone number,
and home phone number.

`chgrp` change group ownership

`chmod` **ch**ange **mod**ification,
change permissions of files or directories,
`chmod [ugoa][[+-=][rwxXstugo] file1][, file2, ...]`

`chown` change user owner.

`chsh` **ch**ange **sh**ell for the user.

`clear` clear console screen.

`cp` copy file or directory; `-r` recursively.
`cp file1 file2` makes a copy of *file1* and calls it *file2*.

*CUPTI* is a **CU**DA **P**rofiling **T**ools **I**nterface (CUPTI),
enables the creation of profiling and tracing tools that target
CUDA applications.

`curl` is a command line tool for transferring data with URL syntax.
**C**ommand **URL** (`curl`) transfers data from or to a server,
using one of the supported protocols (DICT, FILE, FTP, FTPS, GOPHER,
HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP,
SFTP, SMTP, SMTPS, TELNET and TFTP).
The command is designed to work without user interaction.
`curl` offers a busload of useful  tricks  like  proxy  support,
user authentication, FTP upload, HTTP post, SSL connections, cookies,
file transfer resume, Metalink, and more.

`<Ctrl>+<C>` interrupts whatever is currently running.
It can get you out of trouble at embarrassing moments.

`<Ctrl>+<Q>` Resumes current terminal.
Continue a long running program in the terminal.

`<Ctrl>+<S>` suspends current terminal.
Pause a long running program in the terminal. Stop the job.
Continue job `<Ctrl>+<Q>`.

`<Ctrl>+<Z>` puts a foreground process into the background.

---
### <a name="d" />[D&ensp;:arrow_heading_up:](#up)

`delgroup` delete group.

`deluser` delete user.

`df` **d**isk **f**ilesystem. Report file system disk space usage.

`dmidecode` DMI table decoder. Display hardware information.
For example, `sudo dmidecode -t memory` displays memory installed.

`docker` is a computer program that performs operating-system-level
virtualization, also known as "containerization". Docker is used to run
software packages called "containers". Containers are isolated from
each other and bundle their own application, tools, libraries and
configuration files; they can communicate with each other through
well-defined channels. All containers are run by a single operating
system kernel and are thus more lightweight than virtual machines.

`dpkg -i` similar to `apt install` (install package),
package manager for Debian.
`sudo dpkg -i package.deb`

`du` **d**isk **u**sage. Estimate file space usage.

---
### <a name="e" />[E&ensp;:arrow_heading_up:](#up)

`eb` - EasyBuild framework manages software
on High Performance Computing (HPC) systems in an efficient way.

`echo` display a line of text.
For `true ; echo $?` the output is `0`, for `false ; echo $?` the output is `1`.

`emacs` powerful Linux editor.

`env` list all environment variables or run a program in a modified environment.

---
### <a name="f" />[F&ensp;:arrow_heading_up:](#up)

`find dirname -iname file1` search directory *dirname* and
subdirectories for file *file1*.

`free` check memory usage, `-m` in MBs, `-g` in GBs.

---
### <a name="g" />[G&ensp;:arrow_heading_up:](#up)

`g++` GNU C++ compiler.
`g++` compiler builds the object code from source code,
and it does not generate any intermediate C version of the program.
`g++` is a complete compiler, but `gcc` requires the help of `g++`.

`gcc` GNU C compiler.

`getfacl` get file **a**ccess **c**ontrol **l**ists (ACL).
Displays the comment header, base ACL (access control list) entries,
and extended ACL entries, if there are any, for each file that is
specified. It also resolves symbolic links.

`git` *version-control system* for tracking changes in computer files and
coordinating work on those files among multiple people.
Use Git for savings on https://github.com web-site.

`grep` **g**lobally search a **r**egular **e**xpression and **p**rint,
search plain-text data sets for lines that match a regular expression.
Same as `ed` command doing a global search with the regular expression
and printing all matching lines.

`gsiscp` the same as `scp`, but for GSI protocol.

`gufw` is a GUI for `ufw`, **u**ncomplicated **f**ire**w**all.

`gunzip file.txt.gz` uncompress file *file.txt.gz*.
The uncompressed file will be called *file.txt*.

`gzip file.txt` compress *file.txt*.
The compressed file will be called *file.txt.gz*.

---
### <a name="h" />[H&ensp;:arrow_heading_up:](#up)

`halt` power off computer.
Without additional parameter `-p` user may safely hit the *Power* button
on his computer *manually*.

`head -30 filename` show the first 30 lines.

`history 15` list last 15 commands and number them.

`hostnamectl` query (show) and change the system hostname and related settings.

`htop` lightweight text-mode interactive process viewer.

---
### <a name="i" />[I&ensp;:arrow_heading_up:](#up)

`iptables` is a firewall,
installed by default on all official Ubuntu distributions
(Ubuntu, Kubuntu, Xubuntu). When you install Ubuntu,
`iptables` is there, but it allows all traffic by default.
Ubuntu comes with `ufw` --
a program for managing the `iptables` firewall easily.

`ipython` or **I**nteractive **P**ython is a command shell for interactive
computing in multiple programming languages, originally developed for the
Python programming language. Command `ipython` is used for old 2.x version
of Python.

`ipython3` new 3.x version of IPython.

---
### <a name="j" />[J&ensp;:arrow_heading_up:](#up)

`jupyter` spun-off from IPython in 2014 by Fernando Pérez, project Jupyter
supports execution environments in several dozen languages.
The name is a reference to the three core programming languages supported
by Jupyter, which are *Julia*, *Python* and *R*.

`jupyter notebook` starts Jupyter in the browser.

---
### <a name="k" />[K&ensp;:arrow_heading_up:](#up)

`kill pid` kill (stop) process with the given PID.

`killall` kill processes by name.

---
### <a name="l" />[L&ensp;:arrow_heading_up:](#up)

`ldconfig` configure dynamic linker run-time bindings.
Creates the necessary links and cache (for use by the run-time linker, `ld.so`)
to the most recent shared libraries found in the directories specified
on the command line, in the file `/etc/ld.so.conf`,
and in the trusted directories (`/usr/lib and /lib`).
Command `sudo ldconfig` restarts `/etc/ld.so.conf` file and
`/etc/ld.so.conf.d/` directory after update.

`less` similar to `more`, but allows both forward and backward navigation
through the file.

`ln -s target linkname` create hard link by default,
or symbolic ("soft") link if the `-s` (`--symbolic`) option is specified.
When creating hard links, each TARGET must exist.
Symbolic link mean *link to another link*, so if remove TARGET file
the data it contained will no longer be accessible with LINKNAME.

`link` creates *only* hard links, while `ln` can create symbolic link.

`locate` reads through the `mlocate.db` database file which contains
all file paths in your system.

`ls` list files and directories.

`lsb_release -cs` returns the name of your Ubuntu distribution,
such as `bionic`.
`lsb_release -a` check Ubuntu version, it gives LSB (Linux Standard Base) and
distribution-specific information on the CLI.

`lsmod` show which loadable kernel modules are currently loaded.

---
### <a name="m" />[M&ensp;:arrow_heading_up:](#up)

`make clean && make` clean previous compilation recompile automatically
large program.

`man` retrieve the information in the *man*ual and
display it as text output on the screen.
If unsure which *man*ual item to read, use a keyword search
`man -k keyword | more`.

`mc` call Midnight Commander file manager.

`mkdir -p dirname1/dirname2` make new directory.

`module` - command interface to the Modules package.

`more` view (but not modify) the contents of a text file one screen at a time.
`less` is a similar command with the extended capability of allowing both
forward and backward navigation through the file.

`mv file1 file2` move (rename) *file1* to *file2*.

---
### <a name="n" />[N&ensp;:arrow_heading_up:](#up)

`nano file.txt` small and friendly text editor.
When Midnight Commander is opened `<Ctrl>+<O>` keystoke doesn't save,
but switch to MC.

`npm` **n**ode.js javascript **p**ackage **m**anager.

`nvcc` **NV**IDIA's **C**UDA **C**ompiler,
it  hides the intricate details of CUDA compilation from developers.

`nvidia-smi` check NVIDIA driver version.

---
### <a name="o" />[O&ensp;:arrow_heading_up:](#up)



---
### <a name="p" />[P&ensp;:arrow_heading_up:](#up)

`passwd` change user password.

`pip` package manager
used to install and manage software packages written in Python 2.x.

`pip3` package manager for Python 3.x.

`pipenv` automatically creates and manages a `virtualenv`,
as well as package manager.
While `pip` can install Python packages, `pipenv` is recommended as
it’s a higher-level tool that simplifies dependency management for
common use cases.

`printenv` print all or part of environment.

`ps` list of user processes (programs) that are running,
along with their process ID (PID).
Report a snapshot of the current processes.

`pwd` **p**rint **w**orking **d**irectory, show current directory.

`pycharm-community &` is `python` IDE for professional developers
by JetBrains company.

`python` programming language, high-level and general-purpose.
Command `python` is used for old 2.x version, use `python3` instead
for new 3.x version.

`python3` new 3.x version of Python programming language.

---
### <a name="q" />[Q&ensp;:arrow_heading_up:](#up)



---
### <a name="r" />[R&ensp;:arrow_heading_up:](#up)

`reboot` reboot computer.

`rm` remove file or directory.

`rmdir dirname` remove directory.
Option `-r` recursively removes directory and subdirectories. 

`rsync` is a fast, versatile, remote (and local) file-copying tool.
Its basic command syntax is similar to `scp`.

---
### <a name="s" />[S&ensp;:arrow_heading_up:](#up)

`sbatch` - Submit a batch script to Slurm.

`setfacl` set file **a**ccess **c**ontrol **l**ists (ACL).
Sets (replaces), modifies, or removes the access control list (ACL).
It also updates and deletes ACL entries for each file and directory
that was specified by path.

`scp file user@machine:` secure copy *file* to the home directory
of *user* on *machine*. Note the colon!

`sftp machine` interactive secure FTP
(**f**ile **t**ransfer **p**rotocol) with *machine*.

`sh` standard command language interpreter.

`srun` - Run a parallel job on cluster managed by Slurm.
If necessary, srun will first create a resource allocation
in which to run the parallel job.

`ssh` connection via SSH protocol.
`ssh user@machine -p 22` login securely as *user* into *machine*
via port 22.

`shutdown -h now` power off computer immediately. The same as `halt -p`
and `poweroff`.
Additionally it send an ACPI command to signal the power supply unit to
disconnect the main power. This prevents you from having to physically
push the Power button on your computer.

`sleep` suspend program execution for a specified time.
For example, `sleep 3600 && systemctl suspend processname`
auto sleep after one hour.

`snap` tool to interact with *snaps*.
*Snaps* are packages that are mainly designed to be sandboxed and isolated
from other system software, secure, and easily installable, upgradeable,
degradable, and removable irrespective of its underlying system.

`snapd` is the service which runs on your machine and keeps track of your
installed *snaps*, interacts with the store and provides the `snap` command
for you to interact with it.

`sort filename` sort strings from file.
`ls | sort` sort files in directory.

`su username` login as another user. Check login with `whoami` command.
To run just a single command as another user
`sudo -u username command`

`sudo` from _"**su**peruser **do**"_ execute a command
as another user, superuser by default, similar to `su`

`systemctl` introspect and control the state of the `systemd` system
and *Service Manager*. Show status of the system `systemctl status`.

`systemd` system and service manager.

---
### <a name="t" />[T&ensp;:arrow_heading_up:](#up)

`tail -25 filename` show the last 25 lines.
`tail -f filename` show the last few lines and keep updating
as the file grows.

`tar xvzf file.tar.gz` un-gzip and un-tar `*.tar.gz` file.

`tee` read from standard input and write to standard output and files.

`top` show the *top* few processes sorted according to CPU usage.
Once `top` is running, type `<M>` to sort by memory usage,
and `<Q>` to quit.

`touch` change file timestamps and / or create file `touch file.txt`.

`tty` print the file name of the terminal connected to standard input.
`tty` command shows the device node of the terminal in which it is running
or prints "not a tty" if it is not running inside a terminal.

---
### <a name="u" />[U&ensp;:arrow_heading_up:](#up)

*Ubuntu* is a Linux operating system.

`ufw` **u**ncomplicated **f**ire**w**all.
The default firewall configuration tool for Ubuntu is `ufw`.
Developed to ease `iptables` firewall configuration,
`ufw` provides a user friendly way to create an IPv4 or IPv6
host-based firewall. By default `ufw` is disabled.
`gufw` is a GUI that is available as a frontend. 

`umask` return or set the value of the system's file mode creation mask.
For the *root* user default is `umask 022`, so any new files will,
by default, have the permissions 644 (666 - 022).
Likewise, any new directories will, by default,
be created with the permissions 755 (777 - 022).
`umask` is inverse of `chmod`.

`uname` print certain system information like kernel name, hostname,
operating system, etc.

`unzip file.zip -d destination_folder` un-zip `*.zip` file.

`usermod` modify a user account.
Modify or change any attributes of a already created user account
via command line.
`sudo usermod -a -G groupname username` add user `username`
to the group `groupname`.

---
### <a name="v" />[V&ensp;:arrow_heading_up:](#up)

`--version` option print information about program version and then exit.

`vim` clone of `vi` screen-oriented text editor.

`virtualenv` tool to create isolated Python environments.

`visudo` edits the *sudoers* file in a safe fashion, analogous to `vipw`.
`visudo` locks the *sudoers* file against multiple simultaneous edits,
provides basic sanity checks, and checks for parse errors.
If the *sudoers* file is currently being edited you will receive a message
to try again later.

`vmstat -s` show memory usage statistics.

---
### <a name="w" />[W&ensp;:arrow_heading_up:](#up)

`wc filename` counts lines, words and characters in a file. 

`which` identify the location of executables (`which command`).

`who` show who is logged in on the system.

`whoami` print current user ID.

`wget` retrieve content from web servers. Download via HTTP, FTP, etc.

---
### <a name="x" />[X&ensp;:arrow_heading_up:](#up)

`xargs` build and execute command lines from standard input.

---
### <a name="y" />[Y&ensp;:arrow_heading_up:](#up)



---
### <a name="z" />[Z&ensp;:arrow_heading_up:](#up)



---
### <a name="and" />[&&ensp;:arrow_heading_up:](#up)

`command &` run command in background.

`command &> /dev/null` run command in background without output on console.

`command &> ~/Documents/output.log` run command in background and
re-write, create new `output.log` file
which is in the `$HOME/Documents directory`.

`command &>> ~/Documents/output.log` run command in background and
write, append output to the end of the file `output.log`
which is in the `$HOME/Documents directory`.

`command1 && command2` logical _AND_, run *command2* only
if *command1* has no errors (zero exit code `echo $?`).

---
### <a name="or" />[|&ensp;:arrow_heading_up:](#up)

`command1 | command2` is a *pipeline*, which sends output of the
*command1* to the input of the *command2*.

`command1 || command2` logical _OR_, run *command2* only
if *command1* has error (non-zero exit code `echo $?`).

---
### <a name="semicolon" />[;&ensp;:arrow_heading_up:](#up)

`command1 ; command2` separator between two commands.

---
### <a name="exclamation" />[!&ensp;:arrow_heading_up:](#up)

`!!` repeat your last Unix command. See `history` command.

`!42` repeat command numbered 42.

`!wh` repeat the last command beginning with *"wh"*.

`!!addtext` appends *"addtext"* to previous command line.

`^string1^string2` substitute *string2* for *string1*
in previous command.

---
### <a name="less" />[>&ensp;:arrow_heading_up:](#up)

`command > output.log` create or re-write `output.log` file
with command output, previous info of the file will be lost.

`command >> output.log` create or append to the end of `output.log` file
command output, previous info of the file will remain.

`command < input.log` takes input for *command* from *input.log*.

---
### <a name="dot" />[.&ensp;:arrow_heading_up:](#up)

`.` current directory.
For example, `ls .` lists files in the current directory.

`..` parent directory.
For example, `ls ..` lists files in the parent directory.

`./command` run the command in the local directory.

---
### <a name="tilde" />[~&ensp;:arrow_heading_up:](#up)

`~` home directory, equivalent to `$HOME`.
For example, `ls ~` lists `$HOME` directory.

---
### <a name="backslash" />[\\&ensp;:arrow_heading_up:](#up)

`command1 ;\`<br/>
`command2` backslash ignores next symbol like new line or
executes special action sign like `\a` bell or `\t` tab.

---
### <a name="dollar" />[$&ensp;:arrow_heading_up:](#up)

`$HOME` similar to `~` contains path to your home directory.

`$LD_LIBRARY_PATH` environment variable tells the shell which directories
to search for general libraries.
*Since Ubuntu 9.04, LD_LIBRARY_PATH cannot be set in $HOME/.profile,
/etc/profile, nor /etc/environment files. You must use
`/etc/ld.so.conf.d/.conf` configuration files instead.*

`$PATH` environment variable is a colon-delimited list of directories
that your shell searches through when you enter a command.
`echo $PATH` to find out what your path is.
