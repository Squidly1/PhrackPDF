# PhrackPDF
Snag the Phracks and mass convert them to PDFs.

# Grab all the .tar.gz files (issues 1 thru 70), each with the classic ASCII text files (71 was released as a nice PDF, so no need to convert it):
for i in $(seq 1 70); do wget -c http://phrack.org/archives/tgz/phrack$i.tar.gz; done

# List just the names of the .tar.gz files:
ls -1 *.tar.gz > list.txt

# Use the new list of files to create individual dirs for each and ungz/tar everything in each separate dir:
while IFS= read -r LINE; do nu=$(echo $LINE | awk -F. '{print $1}'); mkdir -p $nu; tar -xzf "$LINE" -C $nu; done < list.txt

# List the dirs, with full paths (since that was the 'easiest' way to get thie next while to work):
readlink -f * | grep phrack > dir.txt

# Add all the ASCII files in each separate dir into its own PhrackXX.pdf file, and show what files are done:
while IFS= read -r LINE; do cd $LINE; echo $LINE; nunam=$(echo $LINE | awk -F\/ '{print $6}'); enscript -p *.txt -o - | ps2pdf - $nunam.pdf; ls -al $nunam.pdf; cd .. ; done < /home/squidly1/Desktop/Phracks/dir.txt

