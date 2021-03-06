#SERVER-SECURITY

Many different talents are needed to protect a Solana validator node from external threats. This document will help new validators to begin to understand the complexities of software and hardware security.


HARDWARE SECURITY


Hardware must be locked if possible in a cage in the datacenter and nobody should be allowed near your server but you. You must not type in the password to your server at the console unless you put a coat over your head and keyboard as most DC's have video cameras inside the server rooms.

A bank alarm system should be installed inside the cage so that if your lock is picked, which is real easy, your alarm will tell you.

You should use an encrypted SSD drive for your OS and home directory for Solana and the validator keys as this will prevent a DC personell from shutting off your server, removing the SSD and reading your secret validator keys.

Particularly critical is to never remotely log into your server console without using HTTPS to connect. Do not use remote software that only has HTTP connection ever. 

Best practice to remote access your Linux server is with a SSH key and with SSH set on the server to not allow the ROOT user to login with a password. You must set this in the server's /etc/ssh/sshd_config file by adding the line at the bottom of the file as follows:

    PermitRootLogin prohibit-password
    
You must then restart the sshd daemon by running the command as follows:

    systemctl restart ssh
    
However an even better way to help secure your node is to hide the SSH login port. Modern SSH keys are so long that it would take a Japanese supercomputer 1000's of years to crack the key but you can hide the port if you like.

WARNING: IT IS DANGEROUS TO CHANGE SSH PORTS. You may inadvertantly move the SSH port to a port in use or even worse, a port which is intermittantly in use, in which case you may be jammed by the other port from logging into your server. If this happens there is no way to recover except to remote login to your server console and if you have no HTTPS remote login to the console, then you will have to drive to the datacenter and hand login to the console to fix this issue. But if you are confident as well as paranoid then just add one more line in the same /etc/ssh/sshd_config file as follows:

    port 31234    
    
Its best to use a port above 1000 and be sure its also above the Solana portrange too. Use 30000 or higher ports. DO NOT FORGET TO OPEN THE FIREWALL PORT AT SAME PORT NUMBER AS THE SSH PORT LIKE SO:

    ufw allow 31234
    
Server user accounts must also be screwed down very tightly by various techniques like turning off 'Other' rights in directories and disabling login for the Sol user or whatever user you have assigned to running the 'solana-validator' binary software. 

Go to the root directory for your 'solana-validator' user like /home/sol and apply the command any time that you add files to this set of directories as follows:

    chmod -R a=,ug=rw /home/sol
    
Shut down login rights for the 'solana-validator' user. If for example this username is 'sol' then you would turn off this user's login by the command as follows:

    usermod --lock sol
    

The UFW Firewall must be installed and used at all times. The minute that you have your server online issue these commands immediatly at the console:

    ufw allow ssh
    ufw enable
       
Port Security is vital to defense against attackers. Add the other ports needed by Solana to be allowed into the node. Pay close attention to the port 8899 for example and see that it is blocked by the firewall if necessary. See the latest firewall setting recommendations from the solana docs.


SOFTWARE SECURITY & VALIDATOR SECRET KEY SECURITY

There are many good reasons that the Solana Foundation does not want validators to run externally produced binaries on their validator's nodes. A recent example of this was a 'special binary' that was sold to many validators for only 1 SOL. Apparently this was an attacker of the network and some nodes were then hacked. Never ever run any binary from anyone or any company on your validator node that is not open source or you can get a trojan software that will steal your secret Solana validator keys and send them over the Internet to the attacker's server. Once this happens you have lost your node permanently and any wallet balance or SOL balance on these node keys.

Open source can still be a liablility to install on your server. Never install unneeded software on your node.

Basically the only reason that you really need extra security in a Solana validator is to keep your validator and voting secret keys from being stolen. All the security procedures around hardware and software that are recommended basically are to prevent this secret key theft. Once these keys are copied by an attacker you can no longer be a solana validator.

The most important thing to do daily is to keep an eye on the balance of your Voting Key as this will accumulate profits. You must therefore withdraw all profits as often as possible or if your server is hacked you will loose the balance left in the voting account. However if you change the withdraw key to a third offserver key, then you are protected from this theft. However you will then not be able to ever be a validator using the validator keys as the hacker will have a copy of them now too. You can regenerate new validator and vote keys only if you have changed your withdraw key to an offserver location supposedly but that is going to cause some serious problems with your investors who staked to your node. Your reputation will be damaged and badly.

PENATRATION TESTING

Kali Linux is the undisputed number one place to start learning PENATRATION TESTING. Download the latest kali image and learn it well. Test your server weekly for vulnerabilites.
