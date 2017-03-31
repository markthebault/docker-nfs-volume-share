# NFS Server and Client test with docker plugin
The purpose of this project is to try a plugin to mound docker volumes with nfs. This project setup two VMs, one server and one client with docker installed on it and the following plugin : ```https://github.com/ContainX/docker-volume-netshare```

##How to run
Just clone this project and tape the following command ```vagrant up```

##How to use it
You have to connect to the client ```vagrant ssh nfs-client``` you can check the following repertory mounted on nfs ```/nfs/home```.

Start the plugin ```sudo docker-volume-netshare nfs```.
Now you can run a new docker and mount the volume dirrectly to the nfs server ```docker run -i -t --volume-driver=nfs -v 192.168.100.100/var/nfs/home:/mount ubuntu /bin/bash```
