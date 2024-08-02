# Where to find the project

Find the project here: https://hub.docker.com/r/pnuema/gns3_juiceshop_container 

## Overview

The `gns3_juiceshop` container provides a vulnerable OWASP Juice Shop application with the option to be secured by web application firewalls (WAFs). This setup allows for learning and practicing web security techniques. The container includes the following tools and WAFs:

- `apt-utils`
- `curl`
- `nginx`
- **`libnginx-mod-http-modsecurity`**
- `apache2`
- **`libapache2-mod-security2`**
- `git`
- `supervisor`
- `iproute2`
- `nano`
- `net-tools`
- `iputils-ping`

The WAFs being used are:

- **Nginx with ModSecurity** (`libnginx-mod-http-modsecurity`)
- **Apache with ModSecurity** (`libapache2-mod-security2`)

You can research these WAFs further to learn how to configure and secure the Juice Shop application effectively.

The container is intended to serve as a vulnerable server within a GNS3 topology, allowing for pentesting through a DMZ protected by a firewall and practicing machine security with the firewall.

## Installation

These instructions are specifically for a user running a local instance of GNS3.

### Step 1: Pull the Docker Image

Pull the container onto either your local machine or remote server depending on how you run your environment:

```sh
sudo docker pull pnuema/gns3_juiceshop_container:latest
```

### Step 2: Add the Docker Image in GNS3

1. Open your local GNS3 instance.
2. Navigate to **Edit > Preferences > Docker > Docker Containers > New**.
3. Add the Docker image:
   - **Existing Image**: Select `pnuema/gns3_juiceshop_container:latest`.
   - Click **Next**.
   - Rename the container as desired.
   - Click **Next**.
   - Set **Adapter** to 1.
   - Click **Next**.
   - Leave the start command blank.
   - Click **Next**.
   - Set **Console Type** to Telnet.
   - Click **Next**.
   - No environment variables are necessary.
   - Click **Finish**.

4. Click **Apply** and then **OK** to exit.

### Step 3: Configure Template Usage

To ensure you remember how to use the container, configure the template usage:

1. Click on the newly added device in GNS3.
2. Open the **Configure Template** dialog.
3. Add the following usage instructions to the **Usage** tab:

   The container runs Nginx on port 80, proxy-passing traffic to Juice Shop on port 3000. Follow these steps to configure the network interface and start Nginx and Juice Shop:

   #### Configure Network Interface

   1. Edit the network interface configuration file:

      ```sh
      nano /etc/network/interfaces
      ```

   2. Example configuration for `/etc/network/interfaces`:

      ```sh
      # This is a sample network config

      # Uncomment this line to load custom interface files
      # source /etc/network/interfaces.d/*

      # Static config for eth0. Change to the desired IP to reach the web application
      auto eth0
      iface eth0 inet static
             address 10.0.10.10
             netmask 255.255.255.0
             gateway 10.0.10.1
             up echo nameserver 10.0.10.1 > /etc/resolv.conf

      # DHCP config for eth0
      #auto eth0
      #iface eth0 inet dhcp
      #       hostname juiceshop_Server-1
      ```

      **Note:** Change the IP addresses (address, netmask, gateway, and nameserver) to fit your IP schema.

   #### Restart the Machine and Run the Script

   3. Restart the machine, then run the script located at `/root`:

      ```sh
      /root/./start_juiceshop_app.sh
      ```

   #### One-Step Configuration and Startup

   4. To configure the network interface and start the services in one step, execute:

      ```sh
      echo '# This is a sample network config

      # Uncomment this line to load custom interface files
      # source /etc/network/interfaces.d/*

      # Static config for eth0. Change to the desired IP to reach the web application
      auto eth0
      iface eth0 inet static
             address 10.0.10.10
             netmask 255.255.255.0
             gateway 10.0.10.1
             up echo nameserver 10.0.10.1 > /etc/resolv.conf

      # DHCP config for eth0
      #auto eth0
      #iface eth0 inet dhcp
      #       hostname juiceshop_Server-1' > /etc/network/interfaces

      # Manually restart the machine, then run the script
      /root/./start_juiceshop_app.sh
      ```

## Acknowledgments

A special thanks to the creators of the OWASP Juice Shop application for providing an excellent platform for learning and practicing web security. The OWASP Juice Shop is an intentionally insecure web application for security training and awareness. It encompasses vulnerabilities from the entire OWASP Top Ten along with many other security flaws found in real-world applications.

For more information on OWASP Juice Shop and other OWASP projects, visit [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) and [OWASP](https://owasp.org/).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

```plaintext
MIT License

Original work Copyright (c) by Bjoern Kimminich & the OWASP Juice Shop contributors 2014-2023

Modifications Copyright (c) 2024 Aaron Carroll


Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
