---
inclusion: auto
---

# Buy vs. Build Decision Framework

For every component in the target architecture, evaluate buy vs. build explicitly. Document the decision and rationale.

## Decision Criteria

| Factor | Favors Buy | Favors Build |
|---|---|---|
| Core business differentiator? | No — commodity function | Yes — competitive advantage |
| Maintenance burden | High (security patches, upgrades, compatibility) | Low (stable, rarely changes) |
| Customization needed | Standard behavior is sufficient | Highly specific to this business |
| Team expertise | Nobody wants to maintain this | Team has deep expertise here |
| Compliance requirements | Vendor provides certifications (SOC 2, etc.) | Custom controls needed |
| Integration complexity | Clean APIs, standard protocols | Deep integration with custom logic |
| Cost | Licensing < build + maintain cost | Build cost < multi-year licensing |
| Vendor risk | Established vendor, multiple alternatives | Single vendor, no alternatives |

## Common Buy Candidates

Always evaluate these as buy-first:

- **Authentication/Identity**: Keycloak (OSS), Auth0, Okta, Azure AD B2C, AWS Cognito
- **CMS**: Strapi (OSS), Directus (OSS), Contentful, Sanity
- **PDF generation**: Gotenberg (OSS), wkhtmltopdf, commercial libraries
- **Email delivery**: SES, SendGrid, Postmark, Mailgun
- **Monitoring/observability**: Grafana (OSS), Datadog, New Relic, Dynatrace
- **Analytics/BI**: Metabase (OSS), Superset (OSS), Redash (OSS), Looker, Power BI
- **Search**: Typesense (OSS), Meilisearch (OSS), Elasticsearch, Algolia
- **Message queue**: RabbitMQ (OSS), Redis Streams, SQS, Kafka
- **Object storage**: MinIO (OSS), S3, Azure Blob, GCS
- **Log aggregation**: Loki (OSS), ELK (OSS), Splunk
- **Secrets management**: Vault (OSS), AWS Secrets Manager, Azure Key Vault
- **API gateway**: Kong (OSS), Traefik (OSS), AWS API Gateway, Azure APIM

## Open Source Sensitivity

Ask early: "What is your organization's policy on open-source software?"

Possible answers and implications:
- **"We love open source"** → Full range of OSS options available
- **"Open source is fine if it has commercial support"** → Prefer OSS with enterprise tiers (Keycloak/Red Hat SSO, Grafana Enterprise, Elastic, Confluent)
- **"We need vendor support contracts and SLAs"** → Lean toward commercial products or OSS with enterprise support
- **"Our security team requires SOC 2 / ISO 27001 from all vendors"** → Commercial products or self-hosted OSS (you provide your own security controls)
- **"No open source allowed"** → Commercial only. This is rare but real in some regulated industries.
