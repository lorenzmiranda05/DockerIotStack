### **Docker IOT Stack**
#### Video by Andreas Spiess of Andreas Spiess YouTube Channel

[Youtube Video Link][#295 Raspberry Pi Server based on Docker, with VPN, Dropbox backup, Influx, Grafana, etc: IOTstack]

---

<br  />

**Raspberry Pi Desktop Setup**

1. Download Raspberry Pi Imager installer from this [link][Raspberry Pi Imager Download].

1. Install Raspberry Pi Imager.
1. Plug-in the SD Card to the machine.
1. Open Raspberry Pi Imager.
1. Select RASPBERRY PI OS (32-BIT) for the Operating System.
1. Select the SD Card for Storage.
1. Click the gear button to open the Advanced options for image customization.
1. Set the following:
    * Set hostname
    * Enable SSH
        * Use password authentication
    * Set username and password
        * Username
        
            ***IMPORTANT: Use pi as the Username to avoid errors in IOTstack installation***
        * Password        
    * Configure wireless LAN
        * SSID
        * Password
    * Set locale settings
        * Time zone
        * Keyboard layout

1. Click the save button.
1. Click the write button.
1. Wait for the SD Card imaging to end.
1. Eject the SD Card from the machine.
1. Insert the SD Card to the Raspberry Pi.
1. Turn on the Raspberry Pi.
1. Close Raspberry Pi Imager.

##### Reference: [Raspberry Pi Documentation][Raspberry Pi Documentation]
---

<br  />

**Raspberry Pi VNC Setup**

1. Open Windows PowerShell.

1. Run the command to connect to the Raspberry Pi using SSH.
    ```bash
    ssh pi@rapsberrypi.local
    # ssh [username]@[hostname].local
    ```
1. Enter the password.
1. Run the command to open the raspberry pi configuration.
    ```bash
    sudo raspi-config
    ```
1. In Interface Options, enable VNC Server.
1. Select \<Finish> and hit enter.
1. Close Windows PowerShell.

##### Reference: [Raspberry Pi Documentation][Raspberry Pi Documentation]
---

<br  />

**Connect to the Raspberry Pi via VNC**

1. Download VNC Viewer installer from this [link][VNC Viewer Download].

1. Install VNC Viewer.
1. Open VNC Viewer.
1. Enter *rapsberrypi.local* in the search bar.
1. Click continue.
1. Enter username and password.
1. Tick remember password.
1. Click OK.

---

<br  />

**Raspberry Pi Static IP Address Setup**

1. Open Windows PowerShell.

1. Run the command to connect to the Raspberry Pi using SSH.
    ```bash
    ssh pi@rapsberrypi.local
    # ssh [username]@[hostname].local
    ```
1. Enter the password.
1. Run the command to determine the ip address of the Raspberry Pi.
    ```bash
    hostname -I
    ```
1. Run the command to determine the ip address of the router.
    ```bash
    ip r
    ```
1. Run the command to determine the ip address of the domain name server (DNS).
    ```bash
    grep "nameserver" /etc/resolv.conf
    ```
1. Run the command to open /etc/dhcpcd.conf for editing in nano.
    ```bash
    nano /etc/dhcpcd.conf
    ```
1. Add the following lines at the bottom of the file.
    ```bash
    interface wlan0
    static_routers=192.168.1.1
    static domain_name_servers=192.168.1.1
    static ip_address=192.168.1.73/24
    # interface [INTERFACE]
    # static_routers=[ROUTER IP]
    # static domain_name_servers=[DNS IP]
    # static ip_address=[STATIC IP ADDRESS YOU WANT]/24
    ```
1. Save the file
    1. Press ctrl+x.

    1. Press y.
    1. Press enter.
1. Run the command to restart the Raspberry Pi and apply the new static IP.
    ```bash
    sudo reboot
    ```

##### Reference: [How to Set a Static IP Address on Raspberry Pi][How to Set a Static IP Address on Raspberry Pi] by Avram Piltch
---

<br  />

**IOT Stack Setup**

Requirements

IOTstack makes the following assumptions:
1. Your hardware is a Raspberry Pi (typically a 3B+ or 4B).

    * The Raspberry Pi Zero W2 has been tested with IOTstack. It works but the 512MB RAM means you should not try to run too many containers concurrently.
    * Users have also reported success on Orange Pi Win/Plus.

1. Your Raspberry Pi has a reasonably-recent version of 32-bit or 64-bit Raspberry Pi OS (aka "Raspbian") installed. You can download operating-system images:

    * Current release : "Raspberry Pi OS with desktop" is recommended.
    * Prior releases : This offers only "Raspberry Pi OS with desktop" images.
1. Your operating system has been updated:
    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

    **If the error "Cannot initiate the connection to mirror.rise.ph:80 (43.226.6.79). - connect (101: Network is unreachable)" was emcountered, see Troubleshooting section below before performing this step again.**

1. You are logged-in as the user "pi".
1. User "pi" has the user ID 1000.
    ```bash
    lslogins -u
    ```
1. The home directory for user "pi" is /home/pi/.
1. IOTstack is installed at /home/pi/IOTstack (with that exact spelling).

<br  />
New Installation (Automatic)

1. In the raspberry pi, install curl:
    ```bash
    sudo apt install -y curl
    ```

1. Run the command to begin installation:
    ```bash
    curl -fsSL https://raw.githubusercontent.com/SensorsIot/IOTstack/master/install.sh | bash
    ```
1. Install Python 3.6.9 or later and virtualenv.
1. Install Docker and docker-compose.
1. Restart the raspberry pi.
    ```bash
    sudo reboot
    ```
1. Run the command to open the menu.
    ```bash
    cd ~/IOTstack
    ./menu.sh
    ```


##### Reference: [IOT Stack Basic Setup][IOT Stack Basic Setup]
---

<br  />

**Troubleshooting**

1. "Cannot initiate the connection to mirror.rise.ph:80 (43.226.6.79). - connect (101: Network is unreachable)"
    1. Run the command to define a specific mirror to use
        ```bash
        sudo nano /etc/apt/sources.list
        ```

    1. Replace the mirror director with the closest mirror to your area.
        * From
            ```bash
            deb http://raspbian.raspberrypi.org/raspbian/ bullseye main contrib non-free rpi
            # Uncomment line below then 'apt-get update' to enable 'apt-get source'
            #deb-src http://raspbian.raspberrypi.org/raspbian/ bullseye main contrib non-free rpi
            ```
        * To
            ```bash
            #deb http://raspbian.raspberrypi.org/raspbian/ bullseye main contrib non-free rpi
            deb http://mirror.pregi.net/raspbian/raspbian/ bullseye main contrib non-free rpi
            # Uncomment line below then 'apt-get update' to enable 'apt-get source'
            #deb-src http://raspbian.raspberrypi.org/raspbian/ bullseye main contrib non-free rpi
            ```
    1. Save the file.
        1. Press ctrl+x.

        1. Press y.
        1. Press enter.

##### References:
* [Cannot connect to mirrordirector.raspbian.org][Cannot connect to mirrordirector.raspbian.org]
* [Raspbian Mirrors][Raspbian Mirrors]

---

<br  />


<!-- Reusable and Invisible URL Definitions  -->
[#295 Raspberry Pi Server based on Docker, with VPN, Dropbox backup, Influx, Grafana, etc: IOTstack]: https://www.youtube.com/watch?v=a6mjt8tWUws
[Raspberry Pi Imager Download]: https://www.raspberrypi.com/software/
[VNC Viewer Download]: https://www.realvnc.com/en/connect/download/viewer/
[How to Set a Static IP Address on Raspberry Pi]: https://www.tomshardware.com/how-to/static-ip-raspberry-pi
[Raspberry Pi Documentation]: https://www.raspberrypi.com/documentation/
[IOT Stack Basic Setup]: https://sensorsiot.github.io/IOTstack/Basic_setup/
[Cannot connect to mirrordirector.raspbian.org]: https://raspberrypi.stackexchange.com/questions/27479/cannot-connect-to-mirrordirector-raspbian-org
[Raspbian Mirrors]: http://www.raspbian.org/RaspbianMirrors