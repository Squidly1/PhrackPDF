# PhrackPDF
Snag the Phracks and mass convert them to PDFs.  The only requirement you need is to have **enscript** _already_ installed. Special note, this ran great on a couple of popular Linux distros but I ran into an issue attempting to run it in Cygwin, despite installing enscript in there as well )shrug(...

1. I created a dir named **Phracks**, changed into it and then grabbed all of the available .tar.gz issues (1 thru 70).  Each of the retrieved files holds the classic ASCII texts (71 was released as a nice PDF, so no need to convert it):

<code>cd Phracks </code>

<code>for i in $(seq 1 70); do wget -c http://phrack.org/archives/tgz/phrack$i.tar.gz; done </code>

2. List just the names of the **.tar.gz** files:

<code>ls -1 *.tar.gz > list.txt </code>

3. Use the new list of files to create individual dirs for each and unzip/tar'd everything, each in its own separate dir:

<code>while IFS= read -r LINE; do nu=$(echo $LINE | awk -F. '{print $1}'); mkdir -p $nu; tar -xzf "$LINE" -C $nu; done < list.txt </code>

4. Created a list of the files with their full paths (since that was the 'easiest' way to get thie next while loop to work, with all of the changing of directories):

<code>readlink -f * | grep phrack > dir.txt </code>

5. Add all the ASCII files in each separate dir into its own **phrackXX.pdf** file, and show what files are done:

<code>while IFS= read -r LINE; do cd $LINE; echo $LINE; nunam=$(echo $LINE | awk -F\/ '{print $6}'); enscript -p *.txt -o - | ps2pdf - $nunam.pdf; ls -al $nunam.pdf; cd .. ; done < /home/squidly1/Phracks/dir.txt </code>

