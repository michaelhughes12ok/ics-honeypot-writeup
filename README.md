# Deploying an ICS honeypot on my home network using TPot
I was recently given the gift of a decrepit old tower pc. More specifically an HP Pavilion 500-424 boasting an AMD A8-6410 quad-coreprocessor(2.0 ghz), integrated AMD Radeon R5 graphics and 8 GB of DDR3 RAM. Thinking of ways I could put this retired piece of machinery to use I thought well obviously with an old tower pc I should try to build a homelab. Long story short I successfully installed proxmox and even got to the webui only to discover the cpu architecture was so old it couldn't run rhel vms. A little frustrated and demoralized I took a step back and thought hard about where I could apply this old computer to do something productive with my summer if it wasnt studying SELinux. Then I came back to the concepts I had just recently covered for my Security+ exam. I thought what if I were to instead of locking down a server unlock a server? And thats when the honeypot ball started rolling. First I researched existing platforms like conpot but didn't have much faith in myself to sort raw logs. So naturally, I landed on Tpot the all in one honeypot.
# What I used to setup my server (Prerequisites) 
* HP Pavilion 500-424
* Raspberry Pi 3 Model B
* Usb to Ethernet Adapter (Pi only has one ethernet port on the board)
* 2 Ethernet cables
* Power cables for Pi and Pavilion
# Network Layout
'''text
XB7-T
  │
  │ ethernet
  ▼
Pi onboard ethernet  [WAN zone]
  │
  │ OpenWrt routing/firewall
  ▼
Pi USB-ethernet adapter  [LAN/honeypot zone]
  │
  │ ethernet
  ▼
'''
Honeypot
# Steps I followed
1. Install OpenWrt on Pi (Easily configurable firewall)
1. Install Debian Making sure its headless on Pavilion (Conserves resources and stability)
1. Update Both.
1. Find Drivers for usb to ethernet adapter (turns out there were prereq drivers this took me several hours.)
1. Install TPot on server and configure .yml file aka docker-compose-custom.yml because of my resource constraint I only enabled conpot and backup services 
1. Configure subnet for honeypot. Home network operates on 10.0.0.0/24 while the honeypot is on 10.10.10.0/24 keeping the two from ever being able to interact.
1. Start TPot ensure it cant reach home network 
1. Explicit rule to allow connections on port 64295 (TPot SSH) but only from my home laptop ip ssh into pi then into TPot.
1. Once everything is running and verified you cant reach the home network from the honeypot expose ports to the internet.
1. Sit back and watch the dashboard.
