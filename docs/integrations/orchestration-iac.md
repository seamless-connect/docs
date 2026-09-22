# Orchestration and infrastructure as code

Systems that manage domain or DNS state across one or more underlying control planes.

| Target | Integration surface | Relevant capabilities | Seamless status | Notes |
| --- | --- | --- | --- | --- |
| Terraform | Provider / resources | Declarative Seamless Connect operations | Candidate | Could reduce per-provider configuration |
| OpenTofu | Provider / resources | Declarative Seamless Connect operations | Candidate | Terraform-compatible ecosystem |
| Pulumi | Provider / SDK | Programmatic Seamless Connect operations | Candidate | Multi-language IaC surface |
| Crossplane | Kubernetes provider | Kubernetes-native domain and DNS operations | Candidate | Reconciliation-based control plane |
| DNSControl | Provider adapter | Provider-independent DNS state | Candidate | Multi-provider DNS orchestration |
| OctoDNS | Provider plugin | Provider-independent DNS state | Candidate | Multi-provider DNS orchestration |
| ExternalDNS | Webhook / provider | Application-to-DNS lifecycle | Candidate | Kubernetes DNS controller |
