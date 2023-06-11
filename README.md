Welcome back, Reader. This article describing how to Build and Deploy ReactJS applications on Node.js Runtime.

TABLE OF CONTENTS
Terminology
Install Node.js
Node Build
Node Deploy
Node Run
Terminology
Node.js:  Node.js is the Javascript runtime environment to execute  Javascript files.

node: Node and Node.js both are referencing to the Javascript runtime environment to execute the Javascript files.

nvm: Node Version Manager to install/upgrade/manage Node.js Runtime packager/software.

npm: Node Package Manager to install third party libraries, supported plugins/packages to run the Javascript files on node.js platform.

Install Node.js
Below commands to install Node.js in /opt directory

Download Node.js Binary

# cd /opt/

# wget https://nodejs.org/dist/v16.18.0/node-v16.18.0-linux-x64.tar.xz

# tar -xvf node-v16.18.0-linux-x64.tar.xz

Define PATH variable as below in /etc/profile configuration file

# export PATH=$PATH:/opt/node-v16.18.0-linux-x64/bin

Verify the node version

# [root@ip-172-31-29-38 ~]# npm –version
8.19.2
# [root@ip-172-31-29-38 ~]# node –version
v16.18.0
# [root@ip-172-31-29-38 ~]#

NOTE: Installing node.js will also install NPM to manage the node packages.

Node Build
1. Download ReactJS source code from this Bitbucket repository

2. Install node modules if not already present by running the below command

# npm install

3. Build the source code to create deployment ready packages by running the below command, This commands creates build folder with deployment ready files.

# npm run build

Node Deploy
1. Copy the build folder content to deployment/dist folder

# cp -pr build/* deployment/dist/

Node Run
1. Start the ReactJS application to run in Node.js runtime environment in fore-ground

# npm start



Note: This command start the process in fore-ground with default 3000 port.  This command is used by the developer while developing the application and testing the bugs.

2. Install pm2 modules – Production Process Manager to manage the application to deploy into Production  Environment.

# npm install -g pm2

3. Start the ReactJS application to run in Node.js runtime environment as demonize process with default 3000 port.

# pm2 start



# pm2 status



4.  Browse the web application from web client (Internet Browser) on Port 3000 like http://www.edshopper.com:3000 assuming that server IP is already added to the DNS.

[Note: If Nginx proxy or Load Balancer configured then Port 80 will be forwarded to the application port 3000, so client not required to add port 3000 from the browser as below]


4. Enable application to auto start after the server reboot

# pm2 startup

[root@ip-172-31-29-38 deployment]# pm2 startup
[PM2] Init System found: systemd
Platform systemd
Template
[Unit] Description=PM2 process manager
Documentation=https://pm2.keymetrics.io/
After=network.target
[Service] Type=forking
User=root
LimitNOFILE=infinity
LimitNPROC=infinity
LimitCORE=infinity
Environment=PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/node-v16.18.0-linux-x64/bin:/root/bin
Environment=PM2_HOME=/root/.pm2
PIDFile=/root/.pm2/pm2.pid
Restart=on-failure
ExecStart=/opt/node-v16.18.0-linux-x64/lib/node_modules/pm2/bin/pm2 resurrect
ExecReload=/opt/node-v16.18.0-linux-x64/lib/node_modules/pm2/bin/pm2 reload all
ExecStop=/opt/node-v16.18.0-linux-x64/lib/node_modules/pm2/bin/pm2 kill

[Install] WantedBy=multi-user.target
Target path
/etc/systemd/system/pm2-root.service
Command list
[ ‘systemctl enable pm2-root’ ] [PM2] Writing init configuration in /etc/systemd/system/pm2-root.service
[PM2] Making script booting at startup…
[PM2] [-] Executing: systemctl enable pm2-root…
Created symlink from /etc/systemd/system/multi-user.target.wants/pm2-root.service to /etc/systemd/system/pm2-root.service.
[PM2] [v] Command successfully executed.
+—————————————+
[PM2] Freeze a process list on reboot via:
$ pm2 save

[PM2] Remove init script via:
$ pm2 unstartup systemd
5. Save the running RectJS applications for auto restarts

[root@ip-172-31-29-38 deployment]# pm2 save
[PM2] Saving current process list…
[PM2] Successfully saved in /root/.pm2/dump.pm2
[root@ip-172-31-29-38 deployment]#
Hope, You are now able to Build and Run ReactJS application on Node.js Runtime environment.
