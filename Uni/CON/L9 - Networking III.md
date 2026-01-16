## The Application Layer
### Domain Name System
- UDP port 53
- Transforms host names into IP addresses
	- `online.tugraz.at` ---> `129.27.2.210`
- Hierarchical structure
	- `.` ---> root name servers (typically hardcoded)
	- `at.` ---> ask `81.91.161.98 (d.ns.at)`
	- `tugraz.at` ---> ask `129.27.2.3 (ns1.tu-graz.ac.at)`
	- `online.tugraz.at` ---> it's at `129.27.2.210`

### DNS Stub Resolver
#### What is it?
- Is a part of the operating system running on a computer, cell phone, or another device
- It talks to a recursive DNS to convert DNS names into IP addresses for all the applications running on the device
#### How it works
1. A user types a DNS name such as `www.google.com` into an application like a web browser.
2. The application makes a standard operating system call to request a DNS lookup for the name.
3. The **DNS stub resolver** on the device creates a DNS message for the request and sends it to a **DNS recursive resolver**.
4. The DNS recursive resolver consults its cache and various authoritative DNS servers to find the address for the name. 
5. The IP address for the name is sent back to the stub resolver. 
	- The stub resolver caches the IP address and returns it to the application.
6. The web browser can now connect to the IP address and display the web page.