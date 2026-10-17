# Nmap assessment workflow — commands and interpretation

The following sequence records commands used in the **authorised, deliberately isolated personal VMware lab** during 7–16 October 2026. Run these only against systems within your authorised scope. The terminal captures in the PDF and text outputs in `../scans/` provide the evidence.

## Stage A — Confirm network and reachability

**Kali Linux**

```bash
ip addr
nmap --version
ping -c 4 192.168.249.131
sudo nmap -sn 192.168.249.0/24 -oN ~/nmap-network-scanning-lab/scans/host-discovery.txt
```

The host discovery step found four responding hosts. Further service testing was limited to the Ubuntu target `192.168.249.131`.

## Stage B — Enumerate TCP ports and services

**Kali Linux**

```bash
sudo nmap 192.168.249.131
sudo nmap -p- 192.168.249.131 -oN ~/nmap-network-scanning-lab/scans/full-tcp-scan.txt
sudo nmap -sV -p 22,3306 192.168.249.131 -oN ~/nmap-network-scanning-lab/scans/service-version-detection.txt
sudo nmap -O 192.168.249.131 -oN ~/nmap-network-scanning-lab/scans/os-detection.txt
```

The full TCP scan found only 22/tcp and 3306/tcp open. The OS fingerprint was ambiguous; the actual Ubuntu release was checked on the target. Note that the default Nmap scan was documented by a screenshot rather than its own saved `.txt` file.

## Stage C — Targeted protocol enumeration

**Kali Linux**

```bash
sudo nmap -p 22 --script ssh2-enum-algos 192.168.249.131 -oN ~/nmap-network-scanning-lab/scans/ssh-enumeration.txt
sudo nmap -p 3306 --script mysql-info 192.168.249.131 -oN ~/nmap-network-scanning-lab/scans/mysql-info.txt
sudo nmap -p 3306 --script mysql-empty-password 192.168.249.131 -oN ~/nmap-network-scanning-lab/scans/mysql-empty-password.txt
```

The empty-password script reported no positive finding; this is not proof that authentication is secure. The public mysql-info output omits the transient session salt.

## Stage D — Verify target-side configuration and privileges

**Ubuntu target**

```bash
lsb_release -a
dpkg-query -W openssh-server
sudo ss -lntp | grep ':22'
sudo ss -lntp | grep ':3306'
sudo ufw status verbose
sudo mysql -e "SELECT user, host, plugin FROM mysql.user;"
sudo mysql -e "SHOW GRANTS FOR 'dbadmin'@'%';"
```

These checks confirmed Ubuntu 22.04.5 LTS; OpenSSH `1:8.9p1-3ubuntu0.16`; MySQL listening on `0.0.0.0:3306`; UFW inactive; intentionally broad-host `dbadmin@'%'`; and extensive privileges only on two named lab databases, **not global server-wide ALL PRIVILEGES**.

## Stage E — Evaluate patch applicability and limitations

Vendor references were consulted on the host machine rather than changing VMware isolation. The installed OpenSSH Ubuntu package revision is later than the vendor fix for CVE-2024-6387. Offline `apt update` could not refresh package indexes, so the assessment does not assert the latest package state.

## Evidence handling

All seven public Nmap scan output files appear in `../scans/`. Fifteen cropped figures are in `../screenshots/`. The final chronological narrative and risk assessment are in `../report/Nmap_Vulnerability_Assessment.pdf`.