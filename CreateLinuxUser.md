### These are quick notes to create a new linux user on the cluster. 

suppose the new user to be added is `ayush` and the cluster ip address is 10.110.1.1

Login to the cluster as root and follow the steps on cluster

Add the user using: 
  ```sudo useradd ayush```

Set password for new user using: 
  sudo passwd ayush 

if /home/ayush does not exist already create using
  sudo mkdir /home/ayush

Change permissions: 
  sudo chown ayush:ayush /home/ayush
  sudo chmod 755 /home/ayush

Open /etc/passwd  and search for the line with ayush, and change it to "ayush:x:1056:1057::/home/ayush:/bin/sh" 
  sudo emacs -nw  /etc/passwd

Open the sshd_config file and temporarily allow password authentication [it should look like: PasswordAuthentication yes]
  sudo emacs -nw  /etc/ssh/sshd_config

restart sshd using: 
  sudo systemctl restart sshd

On the client machine [laptop/desktop etc] create the ssh key pair [give an appropriate path and name for this file, and keep it password less]
    ssh-keygen

Copy this key to 10.110.1.1
  ssh-copy-id -i ayush.pub ayush@10.110.1.1

Confirm that you can login using new user: 
  ssh ayush@10.110.1.1


