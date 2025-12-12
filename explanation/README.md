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
-Ports 80, 82, 554, and 20000 open. web interface, media streaming, and device-specific services.
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
Device is a WiFi range extender. Open SSH/HTTP/HTTPS ports. It can have administrative access. Could be targeted if default credentials are used.
        
  
4.192.168.1.5 – shreesha       
Ports & Services:
6100/tcp – synchronet-db
OS Info:
No exact match
Network Distance: 1 hop
Explanation:
Unknown device (likely a PC or custom setup). One open service port. Not critical, but monitor.

D.Safe Mode
1. 192.168.1.1 – Router / Gateway
Ports & Services:
23/tcp – telnet (open, dangerous)
80/tcp – http
Vulnerabilities Found:
No CSRF, XSS, or DOM-based XSS detected.
Analysis:
Telnet is still open causing high-risk for brute-force attacks.
HTTP is relatively safe from XSS/CSRF according to Nmap, but unsecured web login could still be risky.

2. 192.168.1.2 – Uniview Device (CCTV / IoT)
Ports & Services:
80/tcp – http
82/tcp – xfer
554/tcp – rtsp
20000/tcp – dnp
Vulnerabilities Found:
CVE-2011-3192 – Apache Byterange DoS. It can crash the web server by sending overlapping byte ranges.
http-fileupload-exploiter-- no file-type field found (safe).
No XSS or CSRF detected.
/error.html folder exposed (potential information disclosure).
Analysis:
Device is vulnerable to a denial-of-service attack on HTTP (Apache bug from 2011).
IoT/CCTV devices often lag in updates so it needs firmware review.

3.192.168.1.3 – TP-Link Extender (tl-wa850re)
Ports & Services:
22/tcp – ssh
80/tcp – http
443/tcp – https
Vulnerabilities Found:
Slowloris DoS (CVE-2007-6750) → likely vulnerable on HTTP/HTTPS.
/error.html folder exposed.
No CSRF or XSS found.
Some script errors during scanning (like CVE2014-3704) → couldn’t complete.
Analysis:
Device is likely vulnerable to Slowloris DoS attacks (partial connections can freeze the web admin interface).
SSH access must be secured (strong passwords or keys).

4. 192.168.1.5 – shreesha (your device?)
Ports & Services:
6100/tcp – synchronet-db
Vulnerabilities Found:
No NSE vulnerabilities detected.
Analysis:
Only a database service exposed locally. Check that it’s password-protected and firewall-restricted.

5. 192.168.1.32 & 192.168.1.33 – Other IoT / CCTV Devices
Ports & Services:
80/tcp – http
443/tcp – https
554/tcp – rtsp
Vulnerabilities Found:
Slowloris DoS (CVE-2007-6750) → likely vulnerable.
No XSS or CSRF found.
Analysis:
Multiple devices have the same HTTP exposure → monitor and update firmware.
Slowloris vulnerability → web interface can be DOS’ed remotely.

E. Full Scan

1. Host: 192.168.1.1
Device Info: Likely a ZTE ZXV10 W300 ADSL router.
Open Ports:
23/tcp (Telnet) – MPR-L8 3G mobile router telnetd
Risk: Telnet is unencrypted; anyone on your network could sniff credentials. Brute force attacks are possible.
80/tcp (HTTP) – Mini web server 1.0
Vulnerabilities found:
Slowloris DoS (CVE-2007-6750) – The web server may be overwhelmed by partially opened connections, causing Denial of Service.
Other HTTP scripts (XSS, CSRF) returned no issues.
OS Info: Linux kernel 2.4.17. Router/WAP device.
This router has telnet open (high-risk), and the HTTP service is likely vulnerable to DoS. No direct remote code execution is detected, but local network attackers could exploit telnet or Slowloris.

2. Host: 192.168.1.2
Device Info: Neato Botvac (smart vacuum).
Open Ports:
80/tcp (HTTP) – Custom web interface.
Vulnerabilities:
Apache byterange DoS (CVE-2011-3192) – Denial of Service via overlapping byte-range requests.
/error.html folder exists – may reveal sensitive info if accessed.
82/tcp – Unknown (xfer?) – returns SOAP HTTP responses.
554/tcp (RTSP) – RTSP streaming service.
20000/tcp – Unknown (dnp?) – could be a device-specific protocol.
The smart device runs HTTP, SOAP, and RTSP services. HTTP is vulnerable to DoS. RTSP is responding but does not show exploits. SOAP on port 82 may be an entry point for more device-specific attacks if misconfigured.

3. Host: 192.168.1.3 (tl-wa850re)
Device Info: TP-Link Wi-Fi range extender.
Open Ports:
22/tcp (SSH) – OpenSSH 6.6.0
Vulnerabilities: Multiple critical CVEs and known exploits (CVE-2023-38408, CVE-2016-1908, CVE-2020-15778, etc.).
Risk: High. Older SSH version may allow remote code execution, user enumeration, or authentication bypass.
80/tcp (HTTP) – Admin interface
/error.html detected; basic security headers missing.
Likely vulnerable to Slowloris DoS on HTTP/HTTPS.
443/tcp (HTTPS) – SSL web interface
Also likely vulnerable to Slowloris.
This extender is running outdated SSH with known exploits and a web interface vulnerable to DoS. High-priority target if hardening or penetration testing.
