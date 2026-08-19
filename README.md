# Deploying an ICS Honeypot on My Home Network Using T-Pot

I was recently given the gift of a decrepit old tower PC, more specifically an HP Pavilion 500-424 boasting an AMD A8-6410 quad-core processor (2.0 GHz), integrated AMD Radeon R5 graphics, and 8 GB of DDR3 RAM. Thinking of ways I could put this retired piece of machinery to use, I thought, "Well, obviously, with an old tower PC I should try to build a homelab." Long story short, I successfully installed Proxmox and even got to the web UI, only to discover the CPU architecture was so old it couldn't run RHEL VMs. A little frustrated and demoralized, I took a step back and thought hard about where I could apply this old computer to do something productive with my summer, if not studying SELinux. Then I came back to the concepts I had just recently covered for my Security+ exam. I thought, "What if, instead of locking down a server, I unlock one?" And that's when the honeypot ball started rolling. First, I researched existing platforms like Conpot but didn't have much faith in myself to sort raw logs. So naturally, I landed on T-Pot, the all-in-one honeypot.

## What I Used to Set Up My Server (Prerequisites)

* HP Pavilion 500-424
* Raspberry Pi 3 Model B
* USB-to-Ethernet adapter (the Pi only has one onboard Ethernet port)
* 2 Ethernet cables
* Power cables for the Pi and the Pavilion

## Network Layout

```text
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
Honeypot
```

## Steps I Followed

1. Installed OpenWrt on the Pi (easily configurable firewall).
2. Installed Debian on the Pavilion, making sure it was headless (conserves resources and improves stability).
3. Updated both.
4. Found drivers for the USB-to-Ethernet adapter (turns out there were prerequisite drivers; this took me several hours).
5. Installed T-Pot on the server and configured the .yml file, i.e., `docker-compose-custom.yml`. Because of my resource constraints, I only enabled the Conpot and backup services.
6. Configured a subnet for the honeypot. My home network operates on 10.0.0.0/24, while the honeypot is on 10.10.10.0/24, keeping the two from ever being able to interact.
7. Started T-Pot and ensured it couldn't reach the home network.
8. Added an explicit rule to allow connections on port 64295 (T-Pot SSH) only from my home laptop's IP, so I SSH into the Pi, then into T-Pot.
9. Once everything was running and I'd verified the home network was unreachable from the honeypot, exposed the ports to the internet.
10. Sat back and watched the dashboard.
