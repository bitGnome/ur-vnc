# Install and run VNC server on a UR Robot
In order to remove the pendant from a UR robot you need a way to see the Polyscope view. This can be achieved by installing a VNC server on the robot and connecting to it via VNC client. This process does not use magic files are requires the use of SSH and SCP. If you would like to use magic files you that infomation can be found in the following thread [Remote desktop to the controller UI](https://forum.universal-robots.com/t/remote-desktop-to-the-controller-ui/3826/6). This thead was the inspiration for this repo.

## Install and Configure the VNC Server
The following process outlines the steps needed to configure and install the VNC server on the robot. You will need to robot IP and the root password before proceeding.

1. Copy the packages and the init.d script to the robot.
    * `scp vnc-pkgs.tgz root@robot-ip:~/`
    * `scp x11vnc root@robot-ip:~/etc/init.d`
2. SSH into the robot `ssh robot@robot-ip` 
3. Move the pakages into a new `vnc-server` directory.
    * `mkdir vnc-server`
    * `mv vnc-pkgs.tgz vnc-server`
3. From the `vnc-server` directory install the packages.
    * `cd vnc-server`
    * `tar xvf vnc-pkgs.tgz`
4. Install the packages.
    * `dpkg -i *.deb`
5. Create a directory to store the password for logging into the VNC server.
    * `mkdir /root/.vnc`
6. Generate a password for VNC connections. 
    * `x11vnc -storepassword <set-password> /root/.vnc/passwd`
7. Ensure the init.d script has the correct permissions.
    * `chmod u+x /etc/init.d/x11vnc`
8. Register the x11vnc service
    * `update-rc.d x11vnc defaults`
9. Start up the VNC service
    * `/etc/init.d/x11vnc start`

## Install VNC client
In order to connect to the VNC server you will need to use a VNC client on the computer that will be viewing the polyscope console. The information here is for installing the `tigervnc-viewer` on ubuntu. Other installs can be found on the (TigerVnc)[https://tigervnc.org/] page.

1. On the system that will be connecting to the VNC server install the packages.
    * sudo apt install tigervnc-viewer
2. Once installed you can just run the VNC Viewer binary as you would any other application.
3. Connect to the VNC server by entering the IP address and the password you set up.
4. Once connected you should be able to see the Polyscope console on your machine.
