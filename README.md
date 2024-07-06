# Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution

# DEVOPS TOOLING WEBSITE SOLUTION
![Capture](https://user-images.githubusercontent.com/74002629/183053774-9dddd124-bdb1-4e78-b077-e5877b85fb33.PNG)


### Step 1 - Prepare NFS server
1. To view all logical volumes created block devices are names **xvdf**, **xvdh**, **xvdg** respectively.
2. Use gdisk utility to create a single partition on each of the 3 disks 
6. Install lvm2 package
8. Create physical volume to be used by lvm.
![NFS](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/3115c820-b484-4340-a39c-62b74e6df7a0)


8. To check the PV have been created successfully
9. Next, Create the volume group and name it webdata-vg
11. Create 3 logical volumes using lvcreate utility.

![3 Partions](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/fc9a9f6c-490f-4ef9-a4f9-6a4563ef770b)

12. Verify Logical Volume has been created successfully.

![99](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/5be7dce2-76b8-4027-a451-2de1f241d84a)

16. Install NFS server, configure it to start on reboot and make sure it is up and running

![NFS 2](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/91d2d7da-ac4e-4539-9794-8e71cd29ec85)

17. Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, install all three Web Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet for higher level of security.
18. Set up permission that will allow our Web servers to read, write and execute files on NFS.

![pix9](https://user-images.githubusercontent.com/74002629/183051442-69ca2423-75d4-4b3c-9ac1-afe5ee373b0b.PNG)
19. In your choosen text editor, configure access to NFS for clients within the same subnet (my Subnet CIDR – 172.31.80.0/20 ):

20. Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)

![pixSG](https://user-images.githubusercontent.com/74002629/183053344-f40f0d65-5670-4613-835c-1da0137e0416.PNG)

### STEP 2 — CONFIGURE THE DATABASE SERVER
1. Install and configure a MySQL DBMS to work with remote Web Server
2. SSH in to the provisioned DB server and run an update on the server.
3. Install mysql-server.
4. Create a database.
![pix10](https://user-images.githubusercontent.com/74002629/183051459-c2a2c22e-44ec-453b-9d2b-44000ceccae1.PNG)

### Step 3 — Prepare the Web Servers

1. Install NFS client on the webserver1.
2.target the NFS server’s export for apps (Use the private IP of the NFS server)
3. Verify that NFS was mounted successfully.
4. Add the following line in the configuration file.
5. Install Remi’s repository, Apache and PHP.

8. Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №3 and №4 to make sure the mount point will persist after reboot.

9. Fork the tooling source code. 
10. Begin by installing git on the webserver.
11. Initialize Git.
12. Then run the git repsitory.
![0](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/274e9c9f-60dc-4575-bbf2-132290fed52e)

13. Deploy the tooling website’s code to the Webserver.
![tooling](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/e7e66006-144a-40bc-bc3f-a398d786efa2)

14. ensure port 80 in open to all traffic in the security groups.

16. Apply tooling-db.sql script to your database.
17. In the databse server update the bind address to 0.0.0.0.
18. Then create in MySQL a new admin user.
    
# LOAD BALANCER SOLUTION WITH APACHE
![Capture](https://user-images.githubusercontent.com/74002629/183334671-0641051c-31e2-44e9-950c-b2f7197b6343.PNG)


### Step 1 Configure Apache As A Load Balancer 
1. Create an Ubuntu Server 20.04 EC2 instance.
2. Open TCP port 80 and creating an Inbound Rule in Security Group.
3. Connect to the server through the SSh terminal and install Apache Load balancer then configure it to point traffic coming to LB to the Web Servers.
5. Ensure Apache2 is up and running.

![Proxy](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/7186fde3-7879-4822-877e-30aa2972a4ae)

7. Next, configure load balancing in the default config file.

![Proxy](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/7186fde3-7879-4822-877e-30aa2972a4ae)

8. Restart Apache.

9. The load balancer accepts the traffic and distributes it between the servers according to the method that was specified.
10. from the Web Servers to the NFS server, here I shall unmount them and give each Web Server has its own log directory.
11. Open two ssh/Putty consoles for both Web Servers.
12.  Refresh the browser page.
![0](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/bec04623-4fb6-48b4-8193-0cf45eafb29d)

### Step 2 – Configure Local DNS Names Resolution
1. Sometimes it may become tedious to remember and switch between IP addresses, especially when you have a lot of servers under your management.
We can solve this by configuring local domain name resolution. The easiest way is to use **/etc/hosts file**, although this approach is not very scalable, but it is very easy to configure and shows the concept well. 
2. Open this file on your LB server.
3. Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers

![Configfile](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/bf043161-c78f-4e8e-8387-af233ea7896d)

3. Now you can update LB config file with those names instead of IP addresses.

![Configfile](https://github.com/Hatem-sudo/Project-3-Devops-Tooling-Website-Solution-with-Load-Balancer-Solution/assets/113099054/bf043161-c78f-4e8e-8387-af233ea7896d)

4. can try to curl your Web Servers from LB locally .
![pix8](https://user-images.githubusercontent.com/74002629/183334784-ef5e63ba-78d2-4241-892b-b6669940d54c.PNG)
![pix9](https://user-images.githubusercontent.com/74002629/183334797-ad9753e0-d34b-47f8-9146-66ff4c9de5f0.PNG)


