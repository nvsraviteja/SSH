# SSH
ssh (secure shell) it is a network protocol allows users to  establish connection between remote servers and user's computer so that he can perform operations on it over an encrypted connection.\
SSH includes\
* Private key
* Public key
* SSH Agent

## Public Key
it is an encrypted key which is stored in server and it can be shared openly

## Private Key
it has the decription algorithm of public key so that it can access the server and it is stored in local machine and it should not be shared

## SSH Agent
it is used to store the private keys in a memory so that while connecting to a server it will manage the private keys and establish connection between local and server machine\
command to activate ssh agent is `eval "$(ssh-agent -s)"`\
to add private keys to ssh agent the command is `ssh-add "private key path"`


## how SSH works
* the ssh makes a connection between remote server and local client. this connection will be active untile the ssh sessions gets expires.
* when we run some commands in local machine then the commands are sent to server machine in an encrypted ssh tunnel 
* to connect with server the server should have ssh deamon and the local machine or clint should have ssh client
* ssh deamon will listen on network port and it will establish an environment for client if the client has the correct credentials or the authentication keys are valid 
* ssh client  will establish a secure connection between ssh server and ssh client if the authentication is sucessfull.

# SSH Key Concepts
* Private/Public key
* SSH agent
* how to add keys which are in different locations
* where to paste ssh pub keys in server
* which port is used and how to change port in ssh
* ssh configuration file client and server
* how scp works
* how to run commands without generating ssh session
* jump host
* known Host
* types of keys
* network monitoring
* what is port forwording 
* what is ssh tunnel
* 
# What is SSH
SSH (Secure shell) it is a network protocol works on port 22 which allows user to access and run operations on remort server from local machine.\
when user tries to connect to remote server it will ask for authentication and the authentication is of 2 types 
* Password based authentication
* Key based authentication
## Password based authentication
in password baesd authentication the remote server will ask for username password this is not a secure way to access the server.
## Key based authentication
in this type user will generate a pair of keys (private & public) public key will be with server and it can be shared with anyone it has no restricton where as private key it should be with the user and should not share it with anyone when user tries to connect with server then server will authenticate with keys and allow access to the user
# How to generate keys
In general the keys are generated with a command `ssh-keygen`.\
this command will generate both public & private keys and this keys are stored in `~/.ssh` by default.\
the key names are `id_rsa` (private key) & `id_rsa.pub` (public key)\ 
Once keys are generated the public key should be stored in the server\
to store the public key in the server there are 2 ways
1. use this command `ssh-copy-id -i ~/.ssh/id_rsa.pub username@server_ip` here `ssh-copy-id` command used to copy the public key and it will append the key file insted of overwrite.\
`-i` identity\
`~/.ssh/id_rsa.pub` the path for the public key which should be coppied\
`username@server_ip` username of the server and the ip address of the server/
2. manualy copy the public key which is in `~/.ssh/id_rsa.pub` and connect to server via password based authentication and paste the public key in authorized_key file which will be at `~/.ssh/authorized_keys`.
