```bash
#####Politique 1
sudo passwd -n 1 test1
#ou
sudo chage -m 1 test1
#####Politique 2
sudo passwd -n 3 -x 30 test2
#ou
sudo chage -m 3 -M 30 test2
#####Politique 3
sudo passwd -n 0 -x 90 -w 3 test3
#ou
sudo chage -m 0 -M 90 -W 3 test3
#####Politique 4
sudo passwd -n 0 -x 1 -w 1 test4
#ou
sudo chage -m 0 -M 1 -W 1 test4
#####Politique 5
sudo chage -m 7 -M 10 -W 2 -E $(date -d +180days +%Y-%m-%d) test5
```
# SCRIPT 
```bash
#!/bin/bash

user=$1

getent passwd $user > /dev/null 2&>1

if [ $? -eq 0 ]; then

    echo "User exists"

else

    echo "User $user doesn't existe"

    exit 1

fi

date_change="$(passwd $user -S | cut -d ' ' -f3)"

date_change="${date_change//-/\/}"

date_end="$(passwd $user -S | cut -d ' ' -f5)"

param="$date_change +$date_end days"

output="$(date -d "$param" '+%Y/%m/%d')"

echo "The password of the user $user expires at : $output"

exit 0
```

## Correction Script
```bash
#!/bin/bash


```