A. Host discovery
1. 192.168.1.1 (Your Router/Gateway)
   This is the home router. It exposes Telnet (port 23) and HTTP (port 80), which is a security concern because Telnet is unencrypted and can allow password brute‑forcing.
2.192.168.1.2 (Uniview Device / Likely CCTV / IoT)
   This appears to be a Uniview IoT/CCTV device. Multiple web ports are open, and Nmap detected an old Apache vulnerability (CVE‑2011‑3192), making it potentially vulnerable to denial‑of‑service attacks.
3. 192.168.1.3 (TP‑Link Range Extender tl‑wa850re)
   This is a TP‑Link WiFi extender. Open SSH and HTTP/HTTPS ports show administrative interfaces. Nmap flags the device as likely vulnerable to Slowloris (DoS) on its web server.
4. 192.168.1.11 (Local Host)
   This is my own device. All ports are closed, indicating good host security and no unnecessary exposed services.

B.Service/Port Scan
1. 192.168.1.1 – Router / Gateway
-Telnet (port 23) is unsecured, could allow brute-force attacks.
-HTTP (port 80) is a basic web interface for the router.
-This host is your network gateway, critical to secure.

      
2. 192.168.1.2 – Uniview Device / IoT / CCTV
-This appears to be a CCTV or IoT device.
-Ports 80, 82, 554, and 20000 open → web interface, media streaming, and device-specific services.
-Could be vulnerable if outdated firmware, needs security review.

3. 192.168.1.3 – TP-Link Extender (tl-wa850re)
-Device is a WiFi range extender.
-Open SSH and HTTP/HTTPS → admin interfaces accessible over network.
-Could be targeted for DoS or weak SSH login if default credentials used.
      
4. 192.168.1.11 – Local Host
-All ports closed are closed.
-No unnecessary services exposed.
-Confirms your laptop is secure on local network.

C.OS Dectection

1.192.168.1.1 – Router / Gateway
-Ports & Services:
23/tcp – telnet
80/tcp – http
-OS Info:
Device type: general purpose
Running: Linux 3.2–4.9
Network Distance: 1 hop
Explanation:
Router is Linux-based. Telnet is insecure which can cause potential entry point for attackers.

2. 192.168.1.2 – Uniview Device / IoT / CCTV
Ports & Services:
80/tcp – http
82/tcp – xfer
554/tcp – rtsp
20000/tcp – dnp
-OS Info:
Device type: general purpose
Running: Linux 3.12–4.10
Network Distance: 1 hop
Explanation:
Likely an IoT or CCTV device. Open streaming and management ports. Needs security awareness.

4. 192.168.1.3 – TP-Link Extender (tl-wa850re)
        
Ports & Services:
22/tcp – ssh
80/tcp – http
443/tcp – https
OS Info:
OS scan unreliable (not enough info)
Aggressive guesses: network device/IoT device (printers, NAS, WAP, VoIP phones)
Network Distance: 1 hop
Explanation:
Device is a WiFi range extender. Open SSH/HTTP/HTTPS ports → administrative access. Could be targeted if default credentials are used.
        
  
4.192.168.1.5 – shreesha       
Ports & Services:
6100/tcp – synchronet-db
OS Info:
No exact match
Network Distance: 1 hop
Explanation:
Unknown device (likely a PC or custom setup). One open service port. Not critical, but monitor.
