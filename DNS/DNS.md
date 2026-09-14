DNS is Domain Name System , and is used to map an IP to a name , since it's hard to use , it translates the name into an ip which is essentially like an address on how to reach it.

So when we enter google.com and goes to the DNS server and looks up for the name google.com and what does it translate to as an ip and that's where it is forwarded to.

Now how does this one system handle all the information despite the load and traffic of the world , this is fixed with local cache, this will fix the point of going to the server to find the domain , we will instead store that in cache of the browser, there are multiple levels , from os to browser to router to isp and so on.

And how does this handle the load and the single point of failure , this is done by de-centralizing it , 

Root Name Servers => there are 13 root name servers they are responsible for the DNS query and they internally many have multiple servers , 

then with the help of top level domain , .dev, .com , .in and they get filtered based on this and then goes to the registrar and returns the ip and is resolved.

you can use nslookup to get the ip 

| Record    | Purpose                                    | Example                               |
| --------- | ------------------------------------------ | ------------------------------------- |
| **A**     | Domain → IPv4 address                      | `example.com → 10.0.0.5`              |
| **AAAA**  | Domain → IPv6 address                      | `example.com → 2001:db8::1`           |
| **CNAME** | Domain → another domain name               | `www.example.com → example.com`       |
| **MX**    | Specifies mail servers                     | `example.com → mail.example.com`      |
| **NS**    | Specifies authoritative DNS servers        | `example.com → ns1.example.com`       |
| **TXT**   | Stores arbitrary text/verification data    | SPF, DKIM, domain verification        |
| **SRV**   | Specifies service location/port            | `_sip._tcp.example.com → server:5060` |
| **PTR**   | IP address → domain name                   | `10.0.0.5 → server.example.com`       |
| **SOA**   | Information about the DNS zone             | Primary NS, serial, refresh, etc.     |
| **CAA**   | Specifies which CAs can issue certificates | `example.com → Let's Encrypt`         |

NS is basically used saying that let abc.test.com should be looked at the name server of ns.something.com
this won't redirect to the page , but will only ask for the ip which will be found in the server mentioned and then on port 53 then the website will be routed here.

And this is used for self hosted domain name system



www.dev.example.com.
                  ↑
                root

www.dev.example.com
                  ↑
                 TLD
www.dev.example.com
            ↑
           SLD
www.dev.example.com
        ↑
       third-level

How to issue certificates for my domain only    

You
 │
 │ "Give me a certificate for api.example.com"
 ↓
Certificate Authority
 │
 │ "Prove you control api.example.com"
 ↓
You create a DNS record
 │
 │ _acme-challenge.api.example.com
 │      TXT = random-value
 ↓
CA checks DNS
 │
 │ Value matches?
 ↓
Certificate issued


Important stuff:

/etc/hosts => this will show the local browser dns, how localhost is resolved.
/etc/resolve.conf => this will show who resolves the dns for me , usually either cloudflare (1.1.1.1) or google (8.8.8.8)



