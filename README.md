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
    
Server user accounts must also be screwed down very tightly by various techniques like turning off 'Other' rights in directories and disabling login for the Sol user or whatever user you have assigned to running the 'solana-validator' binary software. 

Go to the root directory for your 'solana-validator' user like /home/sol and apply the command any time that you add files to this set of directories as follows:

    chmod -R a=,ug=rw /home/sol
    
Shut down login rights for the 'solana-validator' user. If for example this username is 'sol' then you would turn off this user's login by the command as follows:

    usermod --lock sol

SOFTWARE SECURITY

There are many good reasons that the Solana Foundation does not want validators to run externally produced binaries on their validator's nodes. A recent example of this was a 'special binary' that was sold to many validators for only 1 SOL. Apparently this was an attacker of the network and some nodes were then hacked. Never ever run any binary from anyone or any company on your validator node that is not open source or you can get a trojan software that will steal your secret Solana validator keys and send them over the Internet to the attacker's server. Once this happens you have lost your node permanently and any wallet balance or SOL balance on these node keys.

Open source can still be a liablility to install on your server. Never install unneeded software on your node.

PENATRATION TESTING

Kali Linux is the undisputed number one place to start learning PENATRATION TESTING. Download the latest kali image and learn it well. Test your server weekly for vulnerabilites.
