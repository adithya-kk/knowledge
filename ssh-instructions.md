tags:#ssh #instructions  
https://stackoverflow.com/questions/6377009/adding-a-public-key-to-ssh-authorized-keys-does-not-log-me-in-automatically

Setting ssh authorized_keys seem to be simple, but it hides some traps I'm trying to figure.

-- SERVER --

In /etc/ssh/sshd_config, set passwordAuthentication yes to let the server temporarily accept password authentication

-- CLIENT --

    consider Cygwin as Linux emulation and install & run OpenSSH

1. Generate private and public keys (client side) # ssh-keygen 
Here pressing just Enter, you get default two files, "id_rsa" and "id_rsa.pub", in ~/.ssh/, but if you give a name_for_the_key, the generated files are saved in your current working directory.
2. Transfer the your_key.pub file to the target machine, ssh-copy-id user_name@host_name. If you didn't create a default key, this is the first step to go wrong ... you should use:  
ssh-copy-id -i path/to/key_name.pub user_name@host_name
3. Logging ssh user_name@host_name will work only for the default id_rsa file, so here is the second trap. You need to do ssh -i path/to/key_name user@host
(Use ssh -v ... option to see what is happening.)
If the server still asks for a password then you gave something. To Enter passphrase: when you've created keys (so it's normal).
If ssh is not listening on the default port 22, you must use ssh -p port_nr.
---- SERVER --  --  
. Modify file /etc/ssh/sshd_config to have
RSAAuthentication yes  
PubkeyAuthentication yes  
AuthorizedKeysFile %h/.ssh/authorized_keys   
(uncomment if case)  
This tells ssh to accept file authorized_keys and look in the user home directory for the key_name sting written in the .ssh/authorized_keys file.
4. Set permissions on the target machine
chmod 755 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
Also turn off pass authentication,
passwordAuthentication no
to close the gate to all ssh root/admin/....@your_domain attempts.
5. Ensure ownership and group ownership of all non-root home directories are appropriate.  
chown -R ~ usernamehere  
chgrp -R ~/.ssh/ user
6. 
- Ensure chmod 400 on private key to be readable only by you: chmod 400 ~/.ssh/id_rsa
- If Keys need to be read-writable by you: chmod 600 ~/.ssh/id_rsa

===============================================

7. Consider the excellent http://www.fail2ban.org

8. Extra SSH tunnel to access a MySQL (bind = 127.0.0.1) server



#Paste this in the config file in ~/.ssh/config
#Changing Port No is security through obfustication, though not much useful.
Host localhomepc
	HostName 192.168.something.something  
	User username  
	IdentityFile /home/username/.ssh/identityfilename  
	IdentitiesOnly yes  
	PubKeyAuthentication yes  
	Port portNo  

#In /etc/ssh/sshd_config:  
#<---------My settings--------->  
PermitRootLogin no  
PermitRootLogin no  
PasswordAuthentication no  
PermitEmptyPasswords no  
ChallengeResponseAuthentication no  
Port portNo  
UsePAM no  
PubkeyAuthentication yes  
AllowUsers username  
DenyUsers username root  
MaxAuthTries 3  
ClientAliveInterval 180  
#<---------My settings--------->  


#Useful only in those distros which use tcp wrappers, else  use fail2ban  
*** In /etc/host.deny  
sshd : ALL EXCEPT 192.168.something.something  
