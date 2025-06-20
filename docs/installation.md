---
sidebar_position: 3
sidebar_label: Module installation
---

# 📦 Installation

## ✅ Prerequisites

* Magento >= 2.4.4
* Meilisearch >= v1.9.0
* PHP >= 8.1

## 🧩 Magento 2 Module Installation

```bash
composer require walkwizus/magento2-module-meilisearch
bin/magento module:enable Walkwizus_MeilisearchBase Walkwizus_MeilisearchCatalog Walkwizus_MeilisearchFrontend Walkwizus_MeilisearchMerchandising
bin/magento setup:upgrade
```

## ⚙️ Configuration

You can configure the connection to your Meilisearch instance either via CLI or directly from the Magento Admin Panel.

### 🖥️ Option 1 – CLI Configuration

```bash
bin/magento config:set meilisearch_server/settings/address meilisearch:7700
bin/magento config:set meilisearch_server/settings/api_key "YOUR_API_KEY"
bin/magento config:set meilisearch_server/settings/client_address localhost:7700
bin/magento config:set meilisearch_server/settings/client_api_key "YOUR_CLIENT_API_KEY"
bin/magento config:set catalog/search/engine meilisearch
```

### 🧑‍💼 Option 2 – Admin Panel Configuration

Go to **Stores > Configuration > Meilisearch > Server Settings**  
From there, you can fill in the server URL, public/private API keys, and prefix settings.

![Magento Admin Configuration Screenshot](/img/installation/store-configuration.png)

## 🔁 Indexing

```bash
bin/magento indexer:reindex catalogsearch_fulltext
```
