#############################################################################

# v-server-setup - Nikolas -

## IP-Adresse: 159.69.20.89

## GitHub-Repository: https://github.com/Zeebuhh/v-server-setup

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

##############################################################################

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

Now i want to install nginx to create a webservice for the server

For this i first log into the server via ssh.
Then i need to update our current repositories from our server
to make sure i use the latest versions:

    ->sudo apt update

After this i can install nginx with this command:

    ->sudo apt install nginx -y

To check if the installation was successful:

    -> systemctl status nginx.service

returns:

    ....Active: active(running)...

indicating success.

When using a webclient to the server IP-adress we can see the NGINX default UI.
Now we want to change the default UI from the webserver.

For this we go to '/var/www on our sever with root permissions (sudo)
and create a 'alternatives' directory and a file named alternate-index-html

    ->sudo mkdir /var/www/alternatives                          (directory creation)
    ->sudo touch /var/www/alternatives/alternate-index.html     (file creation)

To check if it worked (list) i can try:

    -> ls /var/www/
    -> ls /var/www/alternatives

To let nginx knwo what configuirations we want for the webserver,
we create a file 'alternatives' under 'sites-enables'.

    ->sudo nano /etc/nginx/sites-enabled/alternatives

In nano we can now insert as follows:

    server {
            listen: 8081;
            listen [..]:8081;

            root /var/www/alternatives;
            index alternate-index.html;

            location / {
                    try_files $uri $uri/ =404;
            }
    }

Here we have our requests listen on Port 8081, define our 'alternatives' path as resource for root
and our index-page which will be my individual html file (alternate-index-html).
All other locations on the page will be handled by the location configuration.

The last thing we need is the alternate-index.html file itself:
We exit and try:

    ->sudo nano /var/www/alternatives/alternate-index-html

now add custom html to the file, save and exit.

To make changes effective we restart nginx and check if active state of the service is updated:

    ->sudo service nginx restart
    ->systemctl status nginx.service

To check we go to a webclient at {hostipadress}:8081 or {hostipadress}:8081/{custom}

##############################################################################

Now we just need to add git to our server:

    -> sudo apt-get install git -y

Then we create a new SSH key for the Server to connect to github to directly
work with the repository from the server

    ->  ssh-keygen -t ed25519 -C "github key"

To show and copy we use:

    -> cat ~/.ssh/id_ed25519.pub

Then add the Key to our GitHub SSH Keys (GitHub/Settings/SSH Keys GPG Keys)
On Our server we can add our username and email:

    ->git config --global user.name "username"
    ->git config --global user.email "email@example.com"

We can now clone our repository from Github and work with it !

    -> git clone git@github.com:YourName/vServer_setup.git

#############################################################################

SETUP DONE
