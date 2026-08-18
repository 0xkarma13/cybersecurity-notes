TryHackMe — Linux Fundamentals (Pt1)

I completed the Linux Fundamentals (Pt1) room on TryHackMe.

I already work with Linux and I’m pretty comfortable with it. Honestly, I enjoy using Linux much more than Windows.

So I mainly did this room for fun. I wanted to do a quick review of the basics, and at the same time share some of what I learned with others.
Linux is not only for hackers

The first part explains that Linux is not only used by hackers or programmers.

Linux is used in many different places, including:

    Websites and servers

    Car systems

    Point-of-sale systems

    Critical infrastructure

    Phones and small devices

So even if you don't directly use Linux every day, there is a good chance that you interact with Linux-powered systems regularly.
Terminal

The second part talks about the terminal — the black screen that hackers love 😄

The terminal is where we can execute commands on a Linux system.

Learning how to use the terminal is very important in cybersecurity because a large part of our work happens here, from running security tools to investigating attackers.
whoami

The whoami command tells you which user you are currently logged in as.

whoami

This is important because your current user determines what permissions you have on the system.

Don't confuse this with commands that provide system information. I used to make this mistake myself.

For system information, we can use commands such as:

uname -a
hostname

and many other commands.
echo

Working with the terminal is kind of like having a conversation.

You give the system a command, and it gives you an output.

The echo command simply prints the text you provide.

echo tryhackme

Output:

tryhackme

It is a very simple command, but it can be useful in many situations.

For example, writing text to a file:

echo "hello" > file.txt

Adding text to the end of a file:

echo "world" >> file.txt

It can also be useful when testing commands or scripts:

echo "Starting scan..."
echo "Checking ports..."
echo "Finished!"

Basic Linux Commands

The next section introduces four very basic Linux commands:
ls

Used to list files and directories in the current location.
pwd

Shows the full path of the current directory.
cat

Used to display the contents of a text file.
cd

Used to move to another directory.
find

find is used to search for files and directories.

For example:

find . -name "test.txt"

This searches from the current directory for a file named test.txt.

Another example:

find . -type f

This searches for files only.
grep

grep is used to search for specific text inside files.

For example:

grep "password123" password.txt

Or something I used in the room:

grep "THM" access.log

Combining Commands

The next section explains several operators that can be used to combine commands.
&

Runs a command in the background.

This can be useful when a command takes a long time to finish and you don't want the terminal to wait for it.
&&

Runs the second command only if the first command succeeds.

echo Hello && echo World

If the first command succeeds, the second command runs as well.
>

Redirects the output into a file.

echo "hey" > welcome

Instead of displaying hey on the screen, the output is written to the welcome file.

An important thing to remember is that if the file already exists, its previous contents will be overwritten.
>>

Appends output to the end of a file.

echo "thm" >> welcome

Unlike >, this does not replace the existing content. It adds the new text to the end of the file.

So:

>   = overwrite the file
>>  = append to the file

Overall, this room didn't teach me anything particularly new, but it was a nice quick review of Linux fundamentals.

For someone who is just starting with Linux, though, I think it is a good place to begin.
