```bash
#Politique 1
sudo passwd -n 1 test1
#Politique 2
sudo passwd -n 3 -x 30 test2
#Politique 3
sudo passwd -n 0 -x 90 -w 3 test3
#Politique 4
sudo passwd -x 1 -w 1 test4
#Politique 5
sudo chage 
```