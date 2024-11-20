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
* ssh deamon will listen on network port and it will establish an environment for client if the client has the correct credentials
