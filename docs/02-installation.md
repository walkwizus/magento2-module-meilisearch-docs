---
sidebar_label: Module installation
title: Module installation
---

# Installation

## Prerequisites

* Magento >= 2.4.4
* Meilisearch >= v1.9.0
* PHP >= 8.1

## Magento 2 Module Installation

```bash
composer require walkwizus/magento2-module-meilisearch
bin/magento module:enable Walkwizus_MeilisearchBase Walkwizus_MeilisearchCatalog Walkwizus_MeilisearchFrontend Walkwizus_MeilisearchIndices Walkwizus_MeilisearchMerchandising
bin/magento setup:upgrade
```

:::info Breeze Compatibility
If you are using the **Breeze** frontend, a compatibility module is available to ensure seamless integration with Meilisearch.

👉 [Breeze Compatibility Module](https://github.com/walkwizus/magento2-module-breeze-meilisearch)

### Installation

```bash
composer require walkwizus/magento2-module-meilisearch-breeze
bin/magento module:enable Walkwizus_MeilisearchBreeze
bin/magento setup:upgrade
```
:::

## Configuration

You can configure the connection to your Meilisearch instance either via CLI or directly from the Magento Admin Panel.

## Generate a MASTER API Key

If you want a secure installation, you can generate a master key using the following command:

```bash
bin/magento meilisearch:generate:master-key
```
⚠️ This command only generates a Meilisearch key and can optionally store it in Magento, but it does **not** restart Meilisearch with the new master key.
To restart Meilisearch with this key, you need to add it to your `compose.yaml` if you're using Docker:

```yaml
meilisearch:
  image: 'getmeili/meilisearch:latest'
  ports:
    - '${FORWARD_MEILISEARCH_PORT:-7700}:7700'
  volumes:
    - 'meilidata:/meili_data'
    #- './meilisearch-key.pem:/root/meilisearch-key.pem'
    #- './meilisearch.pem:/root/meilisearch.pem'
  environment:
    #- MEILI_SSL_CERT_PATH=/root/meilisearch.pem
    #- MEILI_SSL_KEY_PATH=/root/meilisearch-key.pem
    - MEILI_MASTER_KEY=4ff477ac22ce5c3788cdc2799cef8f63
  healthcheck:
    test: ["CMD", "wget", "--no-verbose", "--spider", "http://localhost:7700/health"]
    retries: 3
    timeout: 5s
```

If you're using a standard installation (e.g. on a server), restart Meilisearch with the following command:

```bash
./meilisearch --master-key="MASTER_KEY"
```

When Meilisearch starts in secure mode with a MASTER KEY, it automatically generates three default API keys covering most use cases. You can display them with the following command:

```bash
bin/magento meilisearch:keys
```

You should see an output like this:

```
+------------------------+--------------------------------------+------------------------------------------------------------------+-------------------------+---------+------------+
| Name                   | UID                                  | Key                                                              | Actions                 | Indexes | Expires At |
+------------------------+--------------------------------------+------------------------------------------------------------------+-------------------------+---------+------------+
| Default Search API Key | 8c779f7b-5ea4-4ab5-bb09-82800cc097f2 | b7d005b29630e620ebb733549f989ac0be8225b0f8017b2cb08160c31ea3b49e | search                  | *       | never      |
| Default Admin API Key  | de1ee113-b2ce-4855-89e2-8c046c9c4a1f | e2bfca897f28a8576fd9c14c51dca38b02c70974600daba1a2bc06de86d61bcb | *                       | *       | never      |
| Default Chat API Key   | 07eed6fc-348d-4c02-9705-1c0b9ba30828 | 6f4c4cb918011d21ecfb6c82d85b1b4eb5e2fe3103daddfbe332fd51e1478baf | chatCompletions, search | *       | never      |
+------------------------+--------------------------------------+------------------------------------------------------------------+-------------------------+---------+------------+
```

- **Default Search API Key**: This key should be used on the Magento frontend (configured under *Meilisearch Server API Key (Client Side)*).
- **Default Admin API Key**: This key should be used on the server-side to index documents into Meilisearch. **It must never be exposed on the frontend**.

### Option 1 – CLI Configuration

```bash
bin/magento config:set meilisearch_server/settings/address meilisearch:7700
bin/magento config:set meilisearch_server/settings/master_key "YOUR_MASTER_KEY"
bin/magento config:set meilisearch_server/settings/api_key "YOUR_API_KEY"
bin/magento config:set meilisearch_server/settings/client_address localhost:7700
bin/magento config:set meilisearch_server/settings/client_api_key "YOUR_CLIENT_API_KEY"
bin/magento config:set catalog/search/engine meilisearch
```

### Option 2 – Admin Panel Configuration

Go to **Stores > Configuration > Meilisearch > Server Settings**  
From there, you can fill in the server URL, master/public/private API keys and prefix settings.

![Magento Admin Configuration Screenshot](/img/installation/store-configuration.png)

## Indexing

```bash
bin/magento indexer:reindex catalogsearch_fulltext
```
