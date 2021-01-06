# Convert Files to Read-Write-Execute numeric equivalent  

<b>This script adds the numeric file parameters to ls -lA</b>  
  
ls -lA  
-rwxr-xr-x 1 root root     106 Jan  1 09:34 temp.sh  
-rw------- 1 root root    2070 Jun 18  2017 .viminfo  
-rw-r--r-- 1 root root     215 Nov 16 16:46 .wget-hsts  
  
./CvtReadWriteExecute.sh  
755 -rwxr-xr-x 1 root root 106 Jan 1 09:34 temp.sh  
600 -rw------- 1 root root 2070 Jun 18 2017 .viminfo  
644 -rw-r--r-- 1 root root 215 Nov 16 16:46 .wget-hsts  
  
.bashrc alias:  
alias fo='/root/CvtReadWriteExecute.sh'  # list file parameters  

  
```
#!/bin/bash
#
# CvtReadWriteExecute.sh
# Convert current directory drwxr-xr-x and its files -rwxr-xr-x to numeric equivelent.
#

filePath=${0%/*}  # current file path

ls -lA > $filePath/tempCRWE1.txt
sed -i '1d' $filePath/tempCRWE1.txt            # delete line 1 in file

           #d123456789
bucket1=0  #-rwx------   ${line1:1:1}

#c=10 && c=$((c + 9)) && echo "$c"

while read line1; do
      if [[ ${line1:1:1} = 'r' ]]; then  # r owner
         bucket1=$((bucket1 + 400))
      fi

      if [[ ${line1:2:1} = 'w' ]]; then  # w owner
         bucket1=$((bucket1 + 200))
      fi

      if [[ ${line1:3:1} = 'x' ]]; then # x owner
         bucket1=$((bucket1 + 100))
      fi

      if [[ ${line1:4:1} = 'r' ]]; then  # r groups
         bucket1=$((bucket1 + 40))
      fi

      if [[ ${line1:5:1} = 'w' ]]; then  # w groups
         bucket1=$((bucket1 + 20))
      fi

      if [[ ${line1:6:1} = 'x' ]]; then # x groups
         bucket1=$((bucket1 + 10))
      fi

      if [[ ${line1:7:1} = 'r' ]]; then  # r others
         bucket1=$((bucket1 + 4))
      fi

      if [[ ${line1:8:1} = 'w' ]]; then  # w others
         bucket1=$((bucket1 + 2))
      fi

      if [[ ${line1:9:1} = 'x' ]]; then # x others
         bucket1=$((bucket1 + 1))
      fi

echo "$bucket1 $line1"
bucket1=0
done< $filePath/tempCRWE1.txt # read file line by line

rm $filePath/tempCRWE1.txt

exit 0

```
