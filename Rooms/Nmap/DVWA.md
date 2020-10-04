Nmap

- How to access Help Menu -

> nmap -h

- Switch for 'Stealth Scan' TCP Scan -

> -Ss

- UDP Scan -

> -Su

- OS Detection -

	> -O

- Service Verision Detection -

	> -sV
- Verbose Flag -

	> -v
	> -vv (for even more)

- Save Output in xml -

	> -oX
	- the o(OUTPUT)X(file-type)

- Aggressive Scan -

	> -A (Enables pretty much everything)

- Max Timing ('INSANE') -

	> -T<0-5>
	usage -T5

- Scan Specific Port -

	> -p

- Scan Every Port -

	> -p-

- Enable Script to Run -

	> --script-<ARGS>=<scripts>
	> --script-FILE=<FILE-NAME>

- Run all scripts in VULN category -

	> --scripts vuln

- Dont ping the host -

	> -Pn

--------- Part 2 --------

How to scan without an IP
	nmap -sS

> USAGE of Scan with IP
	- nmap -sS 10.10.96.63

Service Scan on Port 22

	- sudo nmap -sV -p22 10.10.96.63
	version is 6.6.1p1

Finding Vulnerabilities

	- Scan Aggressively
	- nmap -A <ip>
	- 'httponly' flag not set on port 80
	- nmap --script vuln 10.10.96.63
	http-slowloris-check:
|   VULNERABLE:
|   Slowloris DOS attack
|     State: LIKELY VULNERABLE
|     IDs:  CVE:CVE-2007-6750
|       Slowloris tries to keep many connections to the target web server open and hold
|       them open as long as possible.  It accomplishes this by opening connections to
|       the target web server and sending a partial request. By doing so, it starves
|       the http server's resources causing Denial Of Service
