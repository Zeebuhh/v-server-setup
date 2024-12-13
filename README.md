######################################

# v-server-setup - Nikolas -

## Setting up a v-server with nginx

#######################################

First Steps:
-Create SSH keys for Server Setup
-Save SSH Keys

Next log into V-Server via bash shell with given IP, password and username Developer Akademie and see if access is granted:

    -> ssh user@hostipadress
    -> password: userpassword

Connection successfull!

    -> logout

#######################################

The next Step is to deposit my public SSH Key onto the server to make a the connection more secure and password login unnecessary.
For this i locally make an ssh copy with:

    -> ssh-copy-id -id ~/{path of publickey}.pub user@hostipadress
    -> user password: {password}

'Number of key(s) added: 1' indicates it worked.

Now we'll try to access the server as suggested:

    ###
    Now try logging into the machine, with: "ssh -i /c/Users/actio/.ssh/id_ed25519 'ngroth@159.69.20.89'"
    and check to make sure that only the key(s) you wanted were added.
    ###

The access has succeeded, i cannow see the command field of the server.

To check if the key is authorized i do:
->ls -al ~/ seeing all hidden data with "." prefix inlcuding .shh directory
->ls -al ~/.ssh see the authorized_keys file
->cat ~/.ssh/authorized_keys finding my ssh public key

#######################################
