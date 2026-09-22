# DNS control planes

Systems that can directly inspect or modify authoritative DNS state.

| Target | Integration surface | Relevant capabilities | Seamless status | Notes |
| --- | --- | --- | --- | --- |
| Cloudflare DNS | API / OAuth | DNS CRUD, Domain Connect, DNSSEC, discovery | Interested | Existing project relationship |
| Amazon Route 53 | AWS API | DNS CRUD, automation | Candidate | Cloud-native adapter target |
| Google Cloud DNS | Google Cloud API | DNS CRUD, automation | Candidate | Cloud-native adapter target |
| Azure DNS | Azure API | DNS CRUD, automation | Candidate | Cloud-native adapter target |
| deSEC | API / token | DNS CRUD, DNSSEC, CDS/CDNSKEY | Pilot | DNSSEC pilot target |
| DNSimple | API / OAuth | DNS CRUD, DNSSEC | Pilot | DNSSEC pilot counterpart |
| Akamai Edge DNS | API | DNS CRUD, automation | Candidate | Enterprise DNS target |
| IBM NS1 Connect | API | DNS CRUD, automation | Candidate | Enterprise DNS target |
| IONOS DNS | API / Domain Connect | Domain Connect, DNS CRUD | Candidate | Existing Domain Connect surface |
| Name.com DNS | API / Domain Connect | Domain Connect, DNS CRUD | Pilot | Pilot participant |
| GoDaddy DNS | API / Domain Connect | Domain Connect, DNS CRUD | Candidate | Existing Domain Connect surface |
| NameSilo DNS | API / Domain Connect | Domain Connect, DNS CRUD | Candidate | Existing Domain Connect surface |
| Vercel DNS | API / Domain Connect | Domain Connect, DNS changes | Candidate | Application-platform DNS |
| WordPress.com DNS | API / Domain Connect | Domain Connect, DNS changes | Candidate | Hosting-associated DNS |
