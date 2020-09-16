# Day2

---
### What is Amass?

- Amass is an open source network mapping and attack surface discovery tool that uses information gathering and other techniques such as active reconnaissance and external asset discovery to scrap all the available data. In order to accomplish this, it uses its own internal machinery and it also integrates smoothly with different external services to increase its results, efficiency and power.

- This tool maintains a strong focus on DNS, HTTP and SSL/TLS data discovering and scrapping. To do this, it has its own techniques and provides several integrations with different API services like (spoiler alert!) the SecurityTrails API.

- It also uses different web archiving engines to scrape the bottom of the internet’s forgotten data deposits.

* Amass also prioritizes the use of many different sources of input, whereas many tools only have a few. 
So when a new technique comes out
  *   such as certificate transparency
  *   Developers are quick to include it.
     Here’s a short list of all the different techniques for Information gathering looks at:

#### Information Gathering 
 Techniques Used:

- DNS: Basic enumeration, Brute forcing (upon request), Reverse DNS sweeping, Subdomain name alterations/permutations, Zone transfers (upon request)- 
- Scraping: Ask, Baidu, Bing, CommonCrawl, DNSDumpster, DNSTable, Dogpile, Exalead, FindSubdomains, Google, IPv4Info, Netcraft, PTRArchive, Riddler, SiteDossier, Yahoo
-  Certificates: Active pulls (upon request), Censys, CertDB, CertSpotter, Crtsh, Entrust
- APIs: AlienVault, BinaryEdge, BufferOver, CIRCL, DNSDB, HackerTarget, PassiveTotal, Robtex, SecurityTrails, Shodan, ThreatCrowd, Twitter, Umbrella, URLScan, VirusTotal
- Web Archives: ArchiveIt, ArchiveToday, Arquivo, LoCArchive, OpenUKArchive, UKGovArchive, Wayback

<img src="https://securitytrails.com/blog/owasp-amass/amass-github.png">

- It’s very important (especially when using security tools) that you check the integrity of those downloaded binaries to be sure there has been no tampering whatsoever between what you intended to download and what you actually ended up downloading onto your hard drive.

- To do that, you need to save the file amass_checksums.txt, which includes all the hash checksums corresponding to the binaries required to verify the authenticity of the OS binaries.

- For the Amass 3.5.2 version (the latest release available at the time of this writing) the checksum file has the following contents:

### Our Goal

In-depth DNS Enumeration, Attack Surface Mapping and External Asset Discovery!

The OWASP Amass Project has developed a tool to help information security professionals perform network mapping of attack surfaces and perform external asset discovery using open source information gathering and active reconnaissance techniques.

If you have any questions about the OWASP Amass Project, please email the project leader Jeff Foley, or contact us on the project’s Discord server (Discord is highly preferred).

#### Amass Checksum list

```bash
7106f14d0eb9260cebe90b51ab3a1dc308dd7335aaeb8aa1a1d4c693ae05a5a8  amass_v3.5.2_windows_amd64.zip
f2f0b1d04e9cb9ff411127bcf340c1f493c0803f30d8fa6f1ce3140a5234b3ba  amass_v3.5.2_linux_amd64.zip
b12f8b605703277086774f2055139e88bb598b78e549bdf0b09a4bc8919930ab  amass_v3.5.2_freebsd_i386.zip
219872396d3f4c8219f933bb16a3273590ccc7426e1d685bcd2bb1e7f660c09c  amass_v3.5.2_macos_amd64.zip
564ad2c8e049ab2ad809a53a1f51136bcb382c0115b994f4b59e38dd5191edfb  amass_v3.5.2_linux_i386.zip
b1f95d43bf50cb27e83a4986ceb52f9c3aba9e71b4a5c17d41a5b051ecdfefb9  amass_v3.5.2_freebsd_amd64.zip
944e168bac2ba002e52280b5016fca8caad40a10d269c4a8b01056a22eb85fe9  amass_v3.5.2_freebsd_arm64.zip
5eeb342fbc268c4a02b11f770d7d92b6ab7144d2ea4e388cef577eb9bf0bfdf3  amass_v3.5.2_linux_arm64.zip
d5a7dc850388d1ce1923ddb9b16c095ace04fb7f5bcc6b687835db8c8766abda  amass_v3.5.2_linux_arm.zip
```
- As in this analysis we’re using Linux in an amd64 CPU architecture, we are only verifying this hash (you can avoid this if you want, but the check will output several “No such file…” errors). To do so, we must first remove all non-corresponding hashes from the file, and invoke the following command:

```bash
$ shasum -c amass_checksums.txt
amass_v3.5.2_linux_amd64.zip: OK
```

- With that result, we can be assured that the binary is correct, and that there were no file modifications while it was downloading.

- Simply fetch the desired .zip file (in our case that would be amass_v3.5.3_linux_amd64.zip), uncompress it and enter the newly created folder (amass_v3.5.3_linux_amd64).

###### Source file to Download Amass;
```
https://github.com/OWASP/Amass.git
```

### Installation

Here are the best way to install amass.

You’ll need to make sure your Go pathing is set up correctly so you can run it. You might need a chicken to kill.



```
Docker
```bash
docker build -t amass https://github.com/OWASP/Amass.git
docker run -v ~/amass:/amass/
amass enum –list
```
##### Using Docker

- Build the Docker image:

  ```bash 
  sudo docker build -t amass https://github.com/OWASP/Amass.git
  ```

- Run the Docker image:
  
 ```bash 
sudo docker run amass –passive -d example.com 
```

- The wordlists maintained in the git repository are available in /wordlists/ within the docker container. 
  For example, to use all.txt:

 ```bash

sudo docker run amass -w /wordlists/all.txt -d example.com
 ```

##### From Source

- If you prefer to build your own binary from the latest release of the source code, make sure you have a correctly configured Go >= 1.10 environment. More information about how to achieve this can be found on the golang website.Then, take the following steps:

######  Download OWASP :

```bash
go get -u github.com/OWASP/Amass/…
```

- If you wish to rebuild the binaries from the source code:
```bash
cd $GOPATH/src/github.com/OWASP/Amass
go install ./…
```

- At this point, the binaries should be in $GOPATH/bin.
 - Several wordlists can be found in the following directory:

```bash
ls $GOPATH/src/github.com/OWASP/Amass/wordlists/
```

##### Packages Maintained by the Amass Project

Homebrew (macOS)

- For Homebrew on Mac, the following two commands will install it into your macOS environment:
```bash
brew tap caffix/amass
brew install amass
```
###### Snapcraft

If your operating environment supports Snap, you can click here to install, or perform the following from the command-line:
```bash
sudo snap install amass
```
On Kali, follow these steps to install Snap and it + use AppArmor (for autoload):
```bash
sudo apt install snapd
sudo systemctl start snapd
sudo systemctl enable snapd
sudo systemctl start apparmor
sudo systemctl enable apparmor
```

- Add the Snap bin directory to your PATH:
```bash
export PATH=$PATH:/snap/bin
```
- Periodically, execute the following command to update all your snap packages:
```bash
sudo snap refresh
```
##### Packages Maintained by a Third-party

- FreeBSD
```bash 
cd /usr/ports/dns/amass/ && make install clean
pkg install amass
```

- Nix or NixOS
```bash

nix-env -f ‘<nixpkgs>’ -iA amass
```

- Pentoo Linux
```bash
sudo emerge net-analyzer/amass
```

- Periodically, execute the following command to update all packages:
```bash
sudo pentoo-updater
```

### First steps

- Let’s take a look at the subcommands so we can check out the power of this tool:

```bash
Subcommands:
  amass intel - Discover targets for enumerations
  amass enum - Perform enumerations and network mapping
  amass viz - Visualize enumeration results
  amass track - Track differences between enumerations
  amass db - Manipulate the Amass graph database
  amass dns - Resolve DNS names at high performance

```

#### Using the Tool:

The most basic use of the tool, which includes reverse DNS lookups and name alterations:

```bash
$ amass -d example.com
```
Result:

Add some additional domains to the enumeration:

$ amass -d example1.com,example2.com -d example3.com

You can also provide the initial domain names via an input file:

$ amass -df domains.txt

Get amass to provide the sources that discovered the subdomain names and print summary information:

$ amass -v -d example.com
[Google] www.example.com
[VirusTotal] ns.example.com
...
13242 names discovered - scrape: 211, dns: 4709, archive: 126, brute: 169, alt: 8027

Have amass print IP addresses with the discovered names:

$ amass -ip -d example.com

Have amass write the results to a text file:

$ amass -ip -o out.txt -d example.com

Have all the data collected written to a file as individual JSON objects:

$ amass -json out.txt -d example.com

Have amass send all the DNS and infrastructure enumerations to the Neo4j graph database:

$ amass -neo4j neo4j:DoNotUseThisPassword@localhost:7687 -d example.com

Specify your own DNS resolvers on the command-line or from a file:

$ amass -v -d example.com -r 8.8.8.8,1.1.1.1

The resolvers file can be provided using the following command-line switch:

$ amass -v -d example.com -rf data/resolvers.txt

If you would like to blacklist some subdomains:

$ amass -bl blah.example.com -d example.com

The blacklisted subdomains can be specified from a text file as well:

$ amass -blf data/blacklist.txt -d example.com

The amass feature that performs alterations on discovered names and attempt resolution can be disabled:

$ amass -noalts -d example.com

Use active information gathering techniques to attempt DNS zone transfers on all discovered authoritative name servers and obtain TLS/SSL certificates for discovered hosts on all specified ports:

$ amass -active -d example.com net -p 80,443,8080

Caution, this is an active technique that will reveal your IP address to the target organization.

Have amass perform brute force subdomain enumeration as well:

$ amass -brute -d example.com

By default, amass performs recursive brute forcing on new subdomains; this can be disabled:

$ amass -brute -norecursive -d example.com

If you would like to perform recursive brute forcing after enough discoveries have been made:

$ amass -brute -min-for-recursive 3 -d example.com

Change the wordlist used during the brute forcing phase of the enumeration:

$ amass -brute -w wordlist.txt -d example.com

Throttle the rate of DNS queries by number per minute:

$ amass -freq 120 -d example.com

Allow amass to include additional domains in the search using reverse whois information:

$ amass -whois -d example.com

You can have amass list all the domains discovered with reverse whois before performing the enumeration:

$ amass -whois -l -d example.com

Only the first domain provided is used while performing the reverse whois operation.

Network/Infrastructure Options
Caution: If you use these options without specifying root domain names, amass will attempt to reach out to every IP address within the identified infrastructure and obtain names from TLS certificates. This is “loud” and can reveal your reconnaissance activities to the organization being investigated.

If you do provide root domain names on the command-line, these options will simply serve as constraints to the amass output.

All the flags shown here require the ‘net’ subcommand to be specified first.

To discover all domains hosted within target ASNs, use the following option:

$ amass net -asn 13374,14618

To investigate within target CIDRs, use this option:

$ amass net -cidr 192.184.113.0/24,104.154.0.0/15

To limit your enumeration to specific IPs or address ranges, use this option:

$ amass net -addr 192.168.1.44,192.168.2.1-64

By default, port 443 will be checked for certificates, but the ports can be changed as follows:

$ amass net -cidr 192.168.1.0/24 -p 80,443,8080

You can use the tool and tell me your rating or difficulty when using amass. Thanks you.

