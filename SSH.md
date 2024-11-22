# SSH
ssh (secure shell) it is a network protocol allows users to  establish connection between remote servers and user's computer so that he can perform operations on it over an encrypted connection.\
SSH includes\
* Private key
* Public key
* SSH Agent

## Public Key
it is an encrypted key which is stored in server and it can be shared openly

## Private Key
it has the description algorithm of public key so that it can access the server and it is stored in local machine and it should not be shared

## SSH Agent
it is used to store the private keys in a memory so that while connecting to a server it will manage the private keys and establish connection between local and server machine\
command to activate ssh agent is `eval "$(ssh-agent -s)"`\
to add private keys to ssh agent the command is `ssh-add "private key path"`


## how SSH works
* the ssh makes a connection between remote server and local client. this connection will be active until the ssh sessions gets expires.
* when we run some commands in local machine then the commands are sent to server machine in an encrypted ssh tunnel 
* to connect with server the server should have ssh daemon and the local machine or clint should have ssh client
* ssh daemon will listen on network port and it will establish an environment for client if the client has the correct credentials or the authentication keys are valid 
* ssh client  will establish a secure connection between ssh server and ssh client if the authentication is successful.

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
* what is port forwarding 
* what is ssh tunnel
* 
# What is SSH
SSH (Secure shell) it is a network protocol works on port 22 which allows user to access and run operations on remote server from local machine.\
when user tries to connect to remote server it will ask for authentication and the authentication is of 2 types 
* Password based authentication
* Key based authentication
## Password based authentication
in password based authentication the remote server will ask for username password this is not a secure way to access the server.
## Key based authentication
in this type user will generate a pair of keys (private & public) public key will be with server and it can be shared with anyone it has no restriction where as private key it should be with the user and should not share it with anyone when user tries to connect with server then server will authenticate with keys and allow access to the user
# How to generate keys
In general the keys are generated with a command `ssh-keygen`.\
this command will generate both public & private keys. This keys are generated with RSA algorithm by default and this keys are stored in `~/.ssh` by default.\
the key names are `id_rsa` (private key) & `id_rsa.pub` (public key)\ 
Once keys are generated the public key should be stored in the server\
to store the public key in the server there are 2 ways
1. use this command `ssh-copy-id -i ~/.ssh/id_rsa.pub username@server_ip` here `ssh-copy-id` command used to copy the public key and it will append the key file instead of overwrite.\
`-i` identity\
`~/.ssh/id_rsa.pub` the path for the public key which should be copied\
`username@server_ip` username of the server and the ip address of the server/
2. manually copy the public key which is in `~/.ssh/id_rsa.pub` and connect to server via password based authentication and paste the public key in authorized_key file which will be at `~/.ssh/authorized_keys`.
# Where the public key should be in the server
the public key should be at `~/.ssh/authorized_keys` 
# How ssh works
When user from local machine (clint) wants to access remote server and do some operations on it then user will initiate the request to server.\
Then the server will send a cipher text to clint which means encrypted message. This encrypted message is generated with the help of public key algorithm.\ 
When the local machine receives the cipher text it will decode it because it has the private key.\
It will send the decrypted text to server. Then the server gets to know that the clint has the private key and it will allow user to access the server and perform operations on it.
# SSH Agent 
It is a background process which is used to store and manage your private keys when you add your private keys to ssh-agent then the agent interacts with the SSH client to authenticate you to servers without exposing your private key.\
to activate the ssh-agent the command is `eval "$(ssh-agent -s)"`\
here `eval` it is a command which will evaluate and execute strings as a single command. for example:\
```
cmd = "echo "hello world""
eval $cmd
```
Output
```
hello world
```
here it has evaluated the string `"echo"hello world""` and executed `echo"hello world"`
in the same way when we run `ssh-agent -s` it should set up environment variables instead it will give output like
```
SSH_AUTH_SOCK=path_of_file; export SSH_AUTH_SOCK;
SSH_AGENT_PID=1234; export SSH_AGENT_PID;
```
when we execute this with eval then the agent will get activated

## Add keys to ssh-agent
to add keys to ssh-agent the command is 
```
ssh-add "key_path"
```
by this command you can add keys to agent\
to list the keys added to ssh-agent the command is
```
ssh-add -l
```

## Agent Forwarding
agent forwarding is used to copy your private keys from local to the remote server and it will be in the remote server until the ssh session expires.\
Agent will be used when you need to switch from one server to another server and you need to use the same private key for the another server as well then we will use agent forwarding.\
For Example:\
there are 4 remote servers and all the 4 remote servers have the same public key and the private key is only in local machine and you need to connect with remote server and then you need to connect to another server without switching back to local machine then you will use agent forwarding.\
the command to do this is.
```
ssh -A username@server_ip
```
# SSH port
ssh by default runs on network port number 22.\
User can change the port number to make the ssh connection more secure.\
To change the ssh port number there are few steps.
* Chang the ssh configuration file in server at `/etc/ssh/sshd_config` in the file you will find a line `Port 22` and you can comment it or change it to the port number you required and restart the ssh in server by `sudo systemctl restart sshd`
* At local machine you need to update ssh client configuration file which will be at `~/.ssh/ssh_config` you need to add few lines
* ``` 
    Host myserver
         HostName server_ip
         User username
         Port (port_number) 
* With this process you can access the remote server with the port number you defined.

# SSH Configuration files at client and remote server
The configuration files are stored in different locations in client and server
* Client `~/.ssh/ssh_config`
* Server `/etc/ssh/sshd_config`

# How to run commands without generating ssh session
To run commands in server without connecting to server then the command is
* `ssh username@server_ip command`
* `ssh username@server_ip command1; command2; ... command;`
when you have a script locally and you want to run the script in server then the command is 
* `ssh username@server_ip 'bash -s' < script.sh` 
* Here `bash -s` means 
    * `bash` it will call or start the bash shell in the remote server.
    * `-s` in general when you run bash shell it will expect the script file location but in this case the script is not copied to the server so `-s` will convert the script into standard input (stdin) so that the bash will run the commands from the stdin instead of file.

# Jump Host
* jump host act as a bridge between clients and the remote servers which increase the level of security and reduces the risk.
* It also used as gateway to access servers in a restricted environment.
* jump host are used to monitor all the activities happening in the server
* we can also monitor the servers but each server gives different formate of logs based on the application when we use jump server we will get all information who accessed what server
* if there are 10 servers and 3 clients and each of them has access to only few servers in this case if we try to monitor each server it is difficult so if we use jump server (jump host) everyone will connect to server only when they are connected to jump server.

# Known Host
* when we connect to a new server with ssh then it will ask for authenticate so that it will connect to server 
* it will ask for authentication because it is the new server and will you trust this server when you say yes then ssh will store the details of the server in `~/.ssh/known_hosts` and it will not ask for authentication again.

