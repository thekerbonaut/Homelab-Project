Building a HomeLab with Dell PowerEdge Server and VMware ESXi
=============================================================

In the ever-evolving landscape of technology, having a HomeLab can be an invaluable asset for enthusiasts and IT professionals alike. It provides a playground for testing, learning, and experimenting with various services and applications in a controlled environment. In this guide, we'll explore setting up a HomeLab using a Dell PowerEdge server running VMware ESXi, and we'll delve into the installation and configuration of essential services.

Hardware Setup
--------------

Before diving into the software configurations, let's ensure that your hardware is ready for the HomeLab. The Dell PowerEdge series is a robust choice for a server, providing reliability and performance. Make sure the server is properly powered, connected to the network, and has adequate storage.

Virtualization with VMware ESXi
-------------------------------

### Installing VMware ESXi

1.  Download the VMware ESXi ISO from the official VMware website.
2.  Create a bootable USB drive using tools like Rufus or Etcher.
3.  Install VMware ESXi on your Dell PowerEdge server by booting from the USB drive.

### Configuring ESXi

1.  Access the ESXi web interface by navigating to the server's IP address.
2.  Create a virtual switch to manage network traffic.
3.  Upload necessary ISOs to the datastore for virtual machine installations.

Essential Services in HomeLab
-----------------------------

1\. pfSense - Open Source Firewall and Router
---------------------------------------------

Ensuring the security of your HomeLab is paramount, and a robust firewall solution is a fundamental component. pfSense, an open-source firewall and router platform, is an excellent choice for this purpose. Here's a step-by-step guide on how to install pfSense on VMware ESXi and use it to protect other virtual machines in your environment.

### Step 1: Create a New Virtual Machine

1.  Open the VMware vSphere Client and connect to your ESXi host.
2.  Navigate to the "Virtual Machines" tab and click "Create / Register VM."
3.  Select "Create a new virtual machine" and click "Next."
4.  Provide a name for your virtual machine, select a storage location, and choose the compatibility. Click "Next."
5.  Choose the guest OS family as "Other" and version as "FreeBSD (64-bit)." Click "Next."
6.  Specify the amount of memory (RAM) for pfSense. A recommended minimum is 512MB, but adjust based on your environment's requirements. Click "Next."
7.  Create a new network adapter for WAN connectivity. Connect it to the appropriate network or port group that connects to your physical WAN connection. Click "Next."
8.  Add a second network adapter for LAN connectivity. Connect it to the internal network where your other virtual machines reside. Click "Next."
9.  Review the settings and click "Finish" to create the virtual machine.

### Step 2: Install pfSense

1.  Download the latest pfSense ISO from the official website ([https://www.pfsense.org/download/](https://www.pfsense.org/download/)).
2.  Upload the pfSense ISO to your ESXi datastore using the vSphere Client.
3.  Power on the pfSense virtual machine.
4.  In the console window, follow the on-screen instructions to install pfSense.
5.  During the installation, assign the WAN and LAN interfaces based on the network adapters you created earlier.
6.  Once the installation is complete, remove the installation media (ISO) and reboot the virtual machine.

### Step 3: Initial pfSense Configuration

1.  After the reboot, pfSense will prompt you to configure the WAN and LAN interfaces. Assign IP addresses and configure the LAN interface as the default gateway for your internal network.
2.  Access the pfSense web interface by navigating to the assigned LAN IP address using a web browser.
3.  Complete the initial setup wizard, configuring the time zone, admin password, and optional services.

### Step 4: Configure Firewall Rules

1.  In the pfSense web interface, go to "Firewall" > "Rules."
2.  Create rules to control traffic between the WAN and LAN interfaces. By default, all traffic from LAN to WAN is blocked. Define rules to allow necessary traffic.

### Step 5: Protecting Other Virtual Machines

1.  Assign the LAN IP of pfSense as the gateway for other virtual machines on your internal network.
2.  Configure NAT (Network Address Translation) rules in pfSense to allow internal machines to access the internet.
3.  Utilize pfSense's firewall rules to filter and control traffic between virtual machines on your internal network.

By following these steps, you've successfully installed pfSense on VMware ESXi and configured it to protect other virtual machines in your HomeLab. Keep in mind that security is an ongoing process, and regularly update pfSense and review your firewall rules to adapt to changing needs. Your HomeLab is now equipped with a powerful firewall solution, ensuring a secure and controlled network environment.

2\. AdGuard - DNS Filtering and Ad Blocking
-------------------------------------------

AdGuard, a robust DNS filtering and ad-blocking solution, can significantly enhance the security and privacy of your entire HomeLab network. By filtering out unwanted content and potential threats at the DNS level, AdGuard ensures a cleaner and safer browsing experience. Here's a guide on deploying AdGuard and integrating it into your network to protect all connected devices.

### Step 1: Set Up a Virtual Machine

1.  In the VMware vSphere Client, create a new virtual machine for AdGuard.
2.  Choose a Linux distribution as the guest OS. Ubuntu Server is a popular and user-friendly choice for this purpose.
3.  Assign sufficient resources, including CPU, RAM, and storage, based on your network size and anticipated traffic.
4.  Create a new network adapter and connect it to the internal network where your other virtual machines reside.
5.  Follow the installation prompts to install the selected Linux distribution.

### Step 2: Install AdGuard Home

1.  Once the virtual machine is installed and running, log in to the Linux distribution.

2.  Open a terminal and download the AdGuard Home installation script from the official GitHub repository: `curl -sSL https://install.adguard.com | sh`

3.  Follow the on-screen prompts to complete the AdGuard Home installation process.

### Step 3: Configure AdGuard Home

1.  Access the AdGuard Home web interface by navigating to the server's IP address on a web browser.
2.  Complete the initial setup wizard, which includes setting up the admin password, selecting a filtering DNS server, and configuring basic network settings.
3.  Customize AdGuard Home settings based on your preferences and requirements. This may include setting up custom DNS filtering rules, configuring DHCP settings, and enabling or disabling specific features.

### Step 4: Integrate AdGuard into Your Network

1.  Set AdGuard Home as the DNS server for your entire network. This can usually be configured at the router level.
2.  Log in to your router's web interface and navigate to the DHCP settings.
3.  Set the DNS server IP address to the LAN IP address of the AdGuard virtual machine.

### Step 5: Test and Monitor

1.  Ensure that all devices on your network are receiving the AdGuard DNS server address through DHCP.
2.  Test the effectiveness of AdGuard by accessing websites known for ads or malicious content. AdGuard should block unwanted content and provide a more secure browsing experience.
3.  Regularly monitor the AdGuard Home dashboard for statistics on blocked requests and potential threats.

By following these steps, you've successfully deployed AdGuard for network-wide protection in your HomeLab. AdGuard will now filter out unwanted content, including ads and potential threats, at the DNS level, providing a safer and more enjoyable online experience for all devices connected to your network. Keep AdGuard updated and periodically review and adjust filtering rules to adapt to changing online landscapes.

3\. Mysterium - Decentralized VPN Node
--------------------------------------

Mysterium is a decentralized VPN (Virtual Private Network) solution that allows users to run nodes, contributing to a global network of secure and private internet access. Deploying a Mysterium node in your HomeLab not only enhances your own online privacy but also supports the broader Mysterium network. Here's a step-by-step guide on how to deploy a Mysterium node using a virtual machine in your VMware ESXi environment.

### Step 1: Create a Virtual Machine

1\. In the VMware vSphere Client, create a new virtual machine for the Mysterium node.

2\. Choose a Linux distribution as the guest OS. Ubuntu Server is a commonly used and well-supported choice.

3\. Assign adequate resources, including CPU, RAM, and storage, based on your expected usage and network demands.

4\. Create a new network adapter and connect it to the internal network where your other virtual machines reside.

5\. Follow the installation prompts to install the chosen Linux distribution.

### Step 2: Install Mysterium Node Software

1\. Open a terminal on the virtual machine.

2\. Add the Mysterium repository and install the Mysterium node software:

`sudo apt-get update`

`sudo apt-get install -y software-properties-common`

`sudo add-apt-repository ppa:mysteriumnetwork/node`

`sudo apt-get update`

`sudo apt-get install mysterium-node`

3\. During the installation, you will be prompted to set up a wallet for the node. Follow the on-screen instructions to create a new wallet or use an existing one.

### Step 3: Configure the Mysterium Node

1\. Open the Mysterium configuration file in a text editor:

`sudo nano /etc/mysterium-node/mysterium-node.toml`

2\. Configure the node settings, including your wallet address, node name, and other parameters. Refer to the Mysterium documentation for detailed configuration options.

3\. Save the configuration file and exit the text editor.

### Step 4: Start the Mysterium Node

1\. Start the Mysterium node service:

`sudo systemctl start mysterium-node`

2\. Enable the service to start on boot:

`sudo systemctl enable mysterium-node`

### Step 5: Monitor and Maintain the Node

1\. Check the status of your Mysterium node:

`sudo systemctl status mysterium-node`

2\. Monitor the Mysterium Node dashboard for information on earnings, traffic, and overall node health. You can access the dashboard through a web browser by navigating to [http://localhost:4449](http://localhost:4449/) (replace "localhost" with the actual IP address if accessing remotely).

3\. Regularly update the Mysterium node software to benefit from the latest features and security enhancements:

`sudo apt-get update`

`sudo apt-get upgrade mysterium-node`

By following these steps, you've successfully deployed a Mysterium node in your HomeLab. Your node is now part of the Mysterium network, contributing to decentralized VPN services. Keep an eye on the node's performance, ensure regular updates, and consider exploring additional features and configurations provided by Mysterium to optimize your contribution to the network.

4\. Nextcloud - Self-Hosted Cloud Storage
-----------------------------------------

Nextcloud is a powerful and open-source solution for creating a self-hosted cloud storage platform. By deploying a Nextcloud server in your HomeLab, you gain control over your data, file synchronization, and collaboration tools. Follow these steps to set up a Nextcloud server using a virtual machine in your VMware ESXi environment.

**Step 1: Create a Virtual Machine**

1.  Open the VMware vSphere Client and create a new virtual machine for Nextcloud.
2.  Choose a Linux distribution as the guest OS. Ubuntu Server is commonly used for Nextcloud deployments.
3.  Assign resources, including CPU, RAM, and storage, based on the expected usage and data storage needs.
4.  Create a new network adapter and connect it to the internal network where your other virtual machines reside.
5.  Follow the installation prompts to install the chosen Linux distribution.

**Step 2: Install LAMP Stack**

Nextcloud requires a LAMP (Linux, Apache, MySQL, PHP) stack. Install the necessary components on your virtual machine:

`sudo apt-get update`

`sudo apt-get install apache2 mysql-server php php-mysql libapache2-mod-php php-gd php-json php-curl php-mbstring php-intl php-imagick php-xml php-zip`

During the MySQL installation, set a secure root password when prompted.

**Step 3: Create a MySQL Database and User**

1.  Log in to the MySQL server:

`sudo mysql -u root -p`

2.  Create a new database for Nextcloud:

`CREATE DATABASE nextcloud;;`

3.  Create a MySQL user and grant privileges:

`CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'your_password';`

`GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';`

`FLUSH PRIVILEGES;;`

Exit the MySQL prompt:

`EXIT;`

**Step 4: Download and Configure Nextcloud**

1.  Download the latest Nextcloud release:

`wget https://download.nextcloud.com/server/releases/latest.zip`

2.  Extract the downloaded file:

`unzip latest.zip`

3.  Move the Nextcloud files to the Apache web directory:

`sudo mv nextcloud /var/www/html//`

4.  Set the correct permissions:

`sudo chown -R www-data:www-data /var/www/html/nextcloudd`

**Step 5: Configure Apache for Nextcloud**

1.  Create a new Apache configuration file:

`sudo nano /etc/apache2/sites-available/nextcloud.conf`

2.  Add the following configuration:

`<VirtualHost *:80> DocumentRoot /var/www/html/nextcloud/ ServerName your_domain_or_ip <Directory /var/www/html/nextcloud/> Options +FollowSymlinks AllowOverride All Require all granted </Directory> ErrorLog ${APACHE_LOG_DIR}/error.log CustomLog ${APACHE_LOG_DIR}/access.log combined </VirtualHost>`

Replace **your\_domain\_or\_ip** with your server's domain or IP address.

3.  Enable the new configuration:

`sudo a2ensite nextcloud.conf`

4.  Enable necessary Apache modules:

`sudo a2enmod rewrite sudo a2enmod headers sudo a2enmod env sudo a2enmod dir sudo a2enmod mime`

5.  Restart Apache:

`sudo systemctl restart apache2`

**Step 6: Complete the Nextcloud Installation**

1.  Open a web browser and navigate to your Nextcloud server (http://your\_domain\_or\_ip/nextcloud).
2.  Complete the installation by providing the MySQL database details, creating an admin account, and configuring data storage.
3.  Follow the on-screen instructions to finish the setup.

Congratulations! You've successfully deployed a Nextcloud server in your HomeLab. Now, you have a self-hosted cloud storage solution where you can store, share, and collaborate on your files securely. Regularly update Nextcloud and your server to benefit from new features and security enhancements.

5\. Plex - Media Server
-----------------------

Plex is a popular media server that allows you to organize and stream your media collection to various devices. By deploying a Plex server in your HomeLab, you can centralize your movies, TV shows, music, and photos, making them easily accessible across your network. Follow these steps to set up a Plex Media Server using a virtual machine in your VMware ESXi environment.

Step 1: Create a Virtual Machine
--------------------------------

1\. Open the VMware vSphere Client and create a new virtual machine for Plex.

2\. Choose a Linux distribution as the guest OS. Ubuntu Server is commonly used for Plex deployments.

3\. Assign resources, including CPU, RAM, and storage, based on the anticipated usage and media library size.

4\. Create a new network adapter and connect it to the internal network where your other virtual machines reside.

5\. Follow the installation prompts to install the chosen Linux distribution.

Step 2: Install Plex Media Server
---------------------------------

1\. Open a terminal on the virtual machine.

2\. Download the Plex Media Server package:

`wget https://downloads.plex.tv/plex-media-server-new/VERSION/plexmediaserver_VERSION_amd64.deb`

Replace "VERSION" with the latest version number available on the Plex website.

3\. Install the Plex Media Server package:

`sudo dpkg -i plexmediaserver_VERSION_amd64.deb`

4\. Start the Plex Media Server:

`sudo systemctl start plexmediaserver`

5\. Enable Plex to start on boot:

`sudo systemctl enable plexmediaserver`

Step 3: Configure Plex Media Server

1\. Open a web browser and navigate to http://your\_plex\_server\_ip:32400/web.

2\. Sign in or create a Plex account.

3\. Complete the initial setup by adding libraries for your media types (Movies, TV Shows, Music, etc.).

4\. Configure remote access if you plan to access your Plex server outside your home network.

Step 4: Add Media Libraries

1\. In the Plex web interface, click on the "+" icon next to "Libraries" in the left sidebar.

2\. Choose the type of media library you want to add (Movies, TV Shows, Music, Photos, etc.).

3\. Browse and select the folder where your media files are stored.

4\. Configure library settings, such as language preferences and metadata agents.

5\. Click "Add Library" to complete the process.

Step 5: Access Plex on Devices

1\. Download the Plex app on devices you want to stream media to (smartphones, tablets, smart TVs, etc.).

2\. Open the app and sign in with your Plex account.

3\. Your Plex server should automatically appear. Click on it to start browsing and streaming your media.

Congratulations! You've successfully deployed a Plex Media Server in your HomeLab. Your media library is now centralized and accessible from various devices on your network. Consider optimizing your Plex settings, such as transcoding options and remote access security, based on your specific requirements. Periodically update Plex to access new features and improvements. Enjoy streaming your favorite media content hassle-free!

6\. Twingate - Zero Trust Network Access
----------------------------------------

I use Twingate to provide authorized secure access to my network. Twingate is a modern and innovative solution for secure remote access and zero-trust network architecture. Unlike traditional VPNs, which often provide broad access to an entire network, Twingate takes a more granular and security-focused approach. Twingate follows the zero-trust security model, which means it does not automatically trust any user or device, even if they are part of the network. Instead, it verifies and authenticates every user and device attempting to access resources, regardless of their location. I have documented elsewhere how I use Twingate to bypass CGNAT and create secure tunnels to remote services.

7\. JumpCloud - Directory-as-a-Service
--------------------------------------

JumpCloud is a cloud-based directory service that redefines identity and access management. Unlike traditional on-premises solutions, JumpCloud operates as a comprehensive platform for securely managing and connecting users to their systems, applications, and networks, regardless of their location or device. With support for a wide range of operating systems, including Windows, macOS, and Linux, JumpCloud enables organizations to implement a unified identity infrastructure. It streamlines the administration of user access, automates device management, and enhances security through multi-factor authentication. JumpCloud's versatility makes it an ideal solution for businesses of all sizes seeking a modern and scalable approach to identity and access management in today's dynamic and distributed computing environments. I go into more depth with jumpcloud in another project.

Conclusion
----------

Setting up a HomeLab with a Dell PowerEdge server running VMware ESXi opens a world of possibilities for learning and experimenting with various services. Whether you're interested in network security, self-hosted applications, or media streaming, this guide provides a solid foundation for building a versatile HomeLab environment. Remember to continuously explore new technologies and keep your HomeLab dynamic and relevant to your interests and professional development.
