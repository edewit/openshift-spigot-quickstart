openshift-Spigot-quickstart
================================

A quickstart Minecraft (Spigot) server that will automatically download latest craftbukkit development build 
and start it.

Create the Openshift DIY Application
===

1. Create a DIY application using this git repo as source code:

   ```bash
   $ rhc app-create spigot diy --from-code=git://github.com/restedcow/openshift-spigot-quickstart.git
   ```

2. Accept the EULA:

   1. ssh into your gear:
   ```bash
   $ rhc ssh spigot
   ```
   2. Find the Eula.txt file and edit it:
   ```bash
   $ cd app-root
   $ cd data
   ```
   3. Use any text-editor you want, vi worked well for me:
   ```bash
   $ vi eula.txt
   ```
   4. Now accept the eula:
   ```
   eula = true
   ```
   5. Close your text-editor and exit the ssh-connection.
   6. Stop the gear:
   ```bash
   $ rhc app-force-stop spigot
   RESULT:
   spigot force stopped
   ```
   7. And start it:
   ```bash
   $ rhc app-start spigot
   RESULT:
   spigot started
   ```
   
   
3. Create a port-forward from your local machine to your remote server:


   ```bash
   $ rhc port-forward spigot
   Checking available ports ... done
   Forwarding ports ...

   To connect to a service running on OpenShift, use the Local address

   Service Local                OpenShift
   ------- --------------- ---- ---------
   java    127.0.0.1:25565  =>  *:25565

   Press CTRL-C to terminate port forwarding
   ```
3. The server will be accessible from `minecraft` using **Direct Connect** on 127.0.0.1:25565  


Setting Administrative Player (Op)
---

You will need at least one player to act as the administrator. In order to do that, you must add them to the *ops.txt* file.

1. SSH to the application:

   ```bash 
   $ rhc ssh
   ```
2. Go to the `$OPENSHIFT_DATA_DIR` directory:
   
   ```bash
   $ cd $OPENSHIFT_DATA_DIR
   ```
3. Add the `minecraft` user names to the *ops.txt* file. I use `nano`, but you can use `vi` if you wish.
   
   ```bash
   $ nano
   $ cat ops.txt
   syeary
   ```
4. stop and start the gear. I have found restart does not work very well.
   
   ```bash
   $ gear stop
   Stopping gear...
   Stopping DIY cartridge
   $ gear start
   Starting gear...
   Starting DIY cartridge
   + cd /var/lib/openshift/52bc4398e0b8cded36000038/app-root/data/
   + nohup java -jar craftbukkit-dev.jar -h 127.8.212.1 --noconsole
   ```

Reference
---
The OpenShift `diy` cartridge documentation can be found at:

https://github.com/openshift/origin-server/tree/master/cartridges/openshift-origin-cartridge-diy/README.md
