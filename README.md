# TCP-ip-push
Using C Language Push message to TCP/IP 
https://tecadmin.net/setup-autorun-python-script-using-systemd/
## Example:
    Open triminal and compile the C file
     
    gcc -o push push.c
    
    Run :
      ./push 
## How to autorun a Python script using systemd. 
## How to create own systemd service using Python script. 
## How to configure Python script to start as systemd. 
## How to manage Python service with systemctl?

#### Step 1 – Dummy Python Application

First of all, I have used a dummy Python script which listens on a specified port. Edit a Python file as following

    sudo vi /usr/bin/dummy_service.py

and add following content for dummy, You can use your own Python script as per requirements.
	
    #!/usr/bin/python3

    import socket
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind(("localhost", 9988))
    s.listen(1)

    while True:
        conn, addr = s.accept()
        data = conn.recv(1024)
        conn.close()
        my_function_that_handles_data(data)

#### Step 2 – Create Service File

Now, create a service file for the systemd as following. The file must have .service extension under /lib/systemd/system/ directory

    sudo vi /lib/systemd/system/dummy.service

and add the following content in it. Change Python script filename ad location. Also update the Description.

    [Unit]
    Description=Dummy Service
    After=multi-user.target
    Conflicts=getty@tty1.service

    [Service]
    Type=simple
    ExecStart=/usr/bin/python3 /usr/bin/dummy_service.py
    StandardInput=tty-force

    [Install]
    WantedBy=multi-user.target

#### Step 3 – Enable Newly Added Service
    sudo chmod 644 /lib/systemd/system/dummy.service

Your system service has been added to your service. Let’s reload the systemctl daemon to read new file. You need to reload this deamon each time after making any changes in in .service file.

    sudo systemctl daemon-reload

Now enable the service to start on system boot, also start the service using the following commands.

    sudo systemctl enable dummy.service
    sudo systemctl start dummy.service

#### Step 4 – Start/Start/Status new Service

Finally check the status of your service as following command.

    sudo systemctl status dummy.service
    sudo systemctl stop dummy.service          #To stop running service 
    sudo systemctl start dummy.service         #To start running service 
    sudo systemctl restart dummy.service       #To restart running service 

## Refernce
https://aticleworld.com/socket-programming-in-c-using-tcpip/

https://tecadmin.net/setup-autorun-python-script-using-systemd/

http://devopspy.com/linux/python-script-linux-systemd-service/
