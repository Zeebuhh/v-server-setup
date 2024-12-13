#############################################################################

# v-server-setup - Nikolas -

## Setting up a v-server with nginx

##############################################################################

First Steps:
-Create SSH keys for Server Setup
-Save SSH Keys

Next log into V-Server via bash shell with given IP,
password and username Developer Akademie and see if access is granted:

    -> ssh user@hostipadress
    -> password: userpassword

Connection successfull!

    -> logout

##############################################################################

The next Step is to deposit my public SSH Key onto the server to make
the connection more secure and password login unnecessary.
For this i locally make an ssh copy with:

    -> ssh-copy-id -id ~/{path of publickey}.pub user@hostipadress
    -> user password: {password}

    'Number of key(s) added: 1' indicates it worked.

Now we'll try to access the server as suggested:

    ###
    Now try logging into the machine, with: "ssh -i {path to key} 'user@hostipadress'"
    and check to make sure that only the key(s) you wanted were added.
    ###

The access has succeeded, i can now see the command field of the server.

To check if the key is authorized i do:
->ls -al ~/ seeing all hidden data with "." prefix inlcuding .shh directory
->ls -al ~/.ssh see the authorized_keys file
->cat ~/.ssh/authorized_keys finding my ssh public key

#######################################

Now i try deny access via password, so only ssh authorization is possible.
This is done by adjusting the sshd_config file on the server.

For this i use this command:

    ->sudo nano /etc/ssh/sshd_config

Now i am in sshd_config and find the property :

    `#PasswordAuthentication yes`

and change it to:

    `PasswordAuthentication  no`

without a `#` at the start.
then save the file with Ctrl+O and ENTER and Close with Ctrl+X.

Now i need to restart the ssh server to make the adjustment functionable.

    ->sudo systemctl restart ssh.service

Next i logout the server and try to connect with the server to check if it works.

    (server)
        ->logout

    (local machine)
        ->ssh -i {path to key} user@hostipadress

The ssh login works, thus i logout again.
With:
->ssh -o PubkeyAuthentication=no user@hostipadress

I check to see if further configurations can be made from local machine,
but the response is: [ Permission denied (publickey) ],
which shows that it worked.

##############################################################################
