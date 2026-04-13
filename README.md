# dns-local-repo
about DNS all configuration in that repository 
{# DNS Configuration (Forward & Reverse Lookup)

This project demonstrates complete DNS setup using BIND server with forward and reverse lookup configuration.

---

## 1. Install BIND DNS Server

### For RHEL/CentOS:
```bash
sudo yum install bind bind-utils -y
```

### For Ubuntu:
```bash
sudo apt install bind9 -y
```

---

## 2. Main Configuration File

File: /etc/named.conf

```bash
options {
        directory "/var/named";
        allow-query { any; };
        recursion yes;
};

zone "example.com" IN {
        type master;
        file "forward.zone";
};

zone "1.168.192.in-addr.arpa" IN {
        type master;
        file "reverse.zone";
};
```

---

## 3. Forward Lookup Zone File

File: /var/named/forward.zone

```bash
$TTL 86400
@   IN  SOA  ns1.example.com. admin.example.com. (
        2026041301
        3600
        1800
        604800
        86400 )

    IN  NS   ns1.example.com.

ns1         IN  A   192.168.1.10
server1     IN  A   192.168.1.10
server2     IN  A   192.168.1.20
www         IN  A   192.168.1.10
```

---

## 4. Reverse Lookup Zone File

File: /var/named/reverse.zone

```bash
$TTL 86400
@   IN  SOA  ns1.example.com. admin.example.com. (
        2026041301
        3600
        1800
        604800
        86400 )

    IN  NS   ns1.example.com.

10  IN  PTR  server1.example.com.
20  IN  PTR  server2.example.com.
```

---

## 5. Set Permissions

```bash
sudo chown root:named /var/named/forward.zone
sudo chown root:named /var/named/reverse.zone
```

---

## 6. Start and Enable DNS Service

```bash
sudo systemctl start named
sudo systemctl enable named
```

---

## 7. Restart Service

```bash
sudo systemctl restart named
```

---

## 8. Configure Firewall

```bash
sudo firewall-cmd --permanent --add-service=dns
sudo firewall-cmd --reload
```

---

## 9. Test DNS

### Forward Lookup:
```bash
nslookup server1.example.com
```

### Reverse Lookup:
```bash
nslookup 192.168.1.10
```

---

## Explanation

- Forward Lookup: Domain name → IP address (A record)
- Reverse Lookup: IP address → Domain name (PTR record)
- BIND is used as DNS server to manage these records

---

## Summary }

This project shows how to configure DNS server with forward and reverse lookup using BIND on Linux system.
