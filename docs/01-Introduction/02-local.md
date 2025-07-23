---
sidebar_label: Install Meilisearch
title: Install Meilisearch
---

## Installing Meilisearch (Local Setup)

There are multiple ways to install Meilisearch. In this documentation, we’ll focus on a **local installation** suited for development environments.

✅ If you're looking for a production-ready hosted solution or just want to quickly test Meilisearch, consider:
- **Meilisearch Cloud**: [Cloud Quick Start Guide](https://www.meilisearch.com/docs/learn/getting_started/cloud_quick_start)  
  _The easiest way to quickly test Meilisearch. Includes a 14-day free trial._
- **Self-hosted Meilisearch**: [Self-hosted Installation Guide](https://www.meilisearch.com/docs/learn/self_hosted/getting_started_with_self_hosted_meilisearch)
---

### Docker Installation

For a quick local setup using Docker, we recommend Mark Shust’s excellent Magento environment:  
👉 https://github.com/markshust/docker-magento

If you're using this Docker setup, simply add the following service definition to your `composer.yaml`:

```yaml
meilisearch:
  image: 'getmeili/meilisearch:latest'
  ports:
    - '${FORWARD_MEILISEARCH_PORT:-7700}:7700'
  volumes:
    - 'meilidata:/meili_data'
    # - './meilisearch-key.pem:/root/meilisearch-key.pem'
    # - './meilisearch.pem:/root/meilisearch.pem'
  # environment:
    # - MEILI_SSL_CERT_PATH=/root/meilisearch.pem
    # - MEILI_SSL_KEY_PATH=/root/meilisearch-key.pem
  healthcheck:
    test: ["CMD", "wget", "--no-verbose", "--spider", "http://localhost:7700/health"]
    retries: 3
    timeout: 5s
```

You can access the Meilisearch dashboard at http://localhost:7700

More information on local installation: [Local installation](https://www.meilisearch.com/docs/learn/self_hosted/install_meilisearch_locally#local-installation) 