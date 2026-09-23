# Agent registry and discovery control planes

Systems and emerging patterns that control agent or machine-accessible service registration, publication, discovery, resolution, or bootstrap. This area is cataloged by capability while concrete control planes emerge.

| Target | Integration surface | Relevant capabilities | Seamless status | Notes |
| --- | --- | --- | --- | --- |
| Agent registries | Registry API | Create, update, delete, and resolve agent registrations | Research | Concrete protocols and targets to identify |
| Agent directories | Directory API | Publish and query endpoints, capabilities, and metadata | Research | Discovery-oriented surface |
| Service registries | Registry API | Register machine-accessible services | Research | Evaluate fit for agent discovery |
| Agent gateways | Gateway API | Publish reachable endpoints and routing metadata | Research | May combine registry and routing roles |
| Domain-based discovery systems | Domain resolution | Resolve a domain to agent or service bootstrap data | Research | Domain-centric discovery pattern |
| `.well-known` discovery systems | HTTPS metadata | Publish bootstrap metadata | Research | Web-native bootstrap pattern |
| DNS-based bootstrap systems | DNS records | Publish agent or service bootstrap data | Research | DNS-native discovery pattern |
| Cloud agent registries | Cloud APIs | Register agents deployed in cloud agent runtimes | Research | Concrete cloud targets to identify |
