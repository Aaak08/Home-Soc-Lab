# Home-Soc-Lab
Building a home SOC lab for hands on defensive cybersecurity practice

-Started off with installing a ubuntu server iso file from: https://ubuntu.com/download/server#manual-install-tab 
-Then chose Oracle's Virtualbox for the virutalization process as its free
-Then set up the first virutal machine with 4096 MB of ram and 40gb storage and allocated it with 2 processors. 
-While allocating storage it had no "dynamic allocation" maybe because i was using a older version.
-Also while choosing the username it had to be in all lowercase.
- Wazuh install script kept failing to download properly:
-First attempt used version 4.9 in the URL, but that version is outdated — 
-Wazuh's current version is 4.14. Old URL returned a tiny "Access Denied" file -instead of the real script.
-Also had a typo: saved/ran the file as "wazuh.install.sh" (dot) instead of "wazuh-install.sh" (hyphen) — looks identical at a glance but breaks the command  completely.
- First install attempt on 4GB RAM threw a hardware requirement warning (Wazuh recommends more RAM, mainly for the indexer component).
- Had 16GB total on host, so bumped the VM's Base Memory from 4096MB to 6144MB (6GB) instead of just ignoring the warning with -i.
- Shut the VM down properly first (sudo shutdown now) before changing VM settings as in VirtualBox — can't resize RAM while it's running.
- Ran the installer successfully: sudo bash ./wazuh-install.sh -a
- Took several minutes, installs manager + indexer + dashboard all on one VM
- Credentials (admin password etc.) get saved in wazuh-install-files.tar extracted with: sudo tar -O -xvf wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt..while doing i think i choose to just close the entire tab on accident so i saved the credentials it gave out as a screenshot instead.
 - Getting to the dashboard from the host browser wasn't easy and i couldnt just go https://localhost:(iphere) it had to be forwarded so..
 - VM's network adapter is on NAT, which by default blocks the host from reaching the VM directly by its internal IP.
- Fixed with port forwarding: VirtualBox → VM Settings → Network → Adapter 1 → Advanced → Port Forwarding added a rule with a host port and guest port
-after that i tested if the dashboard was working by running it on my VM before try on my host browser using curl -k https;//local...
-while entering the dashboard it gave out a connection not private warning which is when i learnt that wazuh uses a self signed certificate   
-after a break i came back and switched the network settings from NAT to NAT Network so that the VM's can talk to each other which normal NAT cannot. 
-it also turns out the port forwarding rule dident carry over on its own so i had to manually add it by going to the network managed and then NAT networks-- Port forwarding but even after that i could nott get to the wazuh dashboard.
-After a while it turns out the real issue was the Guest Ip being in a default 10.2.0.4 instead of the one i got from running the command "ip a"
-Since this is dynamically applied it will change back on reboot and would need to be changed again..
