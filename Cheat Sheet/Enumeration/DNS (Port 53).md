## Linux
**host** command to find the following (bolded shortform are the tags to be used when running host command):
- **NS**: Nameserver records contain the name of the authoritative servers hosting the DNS records for a domain.
- **A**: Also known as a host record, the "_a record_" contains the IPv4 address of a hostname 
- **AAAA**: Also known as a quad A host record, the "_aaaa record_" contains the IPv6 address of a hostname
- **MX**: Mail Exchange records contain the names of the servers responsible for handling email for the domain. A domain can contain multiple MX records.
- **PTR**: Pointer Records are used in reverse lookup zones and can find the records associated with an IP address.
- **CNAME**: Canonical Name Records are used to create aliases for other host records.
- **TXT**: Text records can contain any arbitrary data and be used for various purposes, such as domain ownership verification. 
Examples:
```
host {domain name} [To get the IP Address of the domain]
host -t mx {domain name} [To get the mail exchange records of the domain]
```

## Windows
Acts similar to Linux way of enumeration, command used is **nslookup**

Examples:
```
nslookup {domain name} [To get the IP Address of the domain]
nslookup -type=TXT {host} {IP of domain server} [To retrieve any txt records from the host]
```
