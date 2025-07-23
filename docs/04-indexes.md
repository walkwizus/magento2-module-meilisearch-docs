---
sidebar_label: Indexes settings
title: Indexes settings
---

# Indexes settings

It is possible to configure the indexes present in Meilisearch directly from your back office.

To do so, go to **System → Meilisearch → Index Settings**.

Select the index you want to configure from the list. Several sections are available:

## Ranking Rules

![Magento Admin Index Settings Ranking Rules](/img/indexes/ranking-rules.png)

Meilisearch contains six built-in ranking rules in the following order:

- words
- typo
- proximity
- attribute
- sort
- exactness

### Words

Results are sorted by decreasing number of matched query terms. Returns documents that contain all query terms first.

:::note
The **words** rule works from right to left. Therefore, the order of the query string impacts the order of results.
For example, if someone were to search **batman dark knight**, the **words** rule would rank documents containing all three terms first, 
documents containing only **batman** and **dark** second, and documents containing only batman third.
:::

### Typo

Results are sorted by **increasing number of typos**. Returns documents that match query terms with fewer typos first.

### Proximity

Results are sorted by increasing distance between matched query terms. 
Returns documents where query terms occur close together and in the same order as the query string first.
It is possible to lower the precision of this ranking rule. This may significantly improve indexing performance. 
In a minority of use cases, lowering precision may also lead to lower search relevancy for queries using multiple search terms.

### Attribute

Results are sorted according to the attribute ranking order. Returns documents that contain query terms in more important attributes first.
Also, note the documents with attributes containing the query words at the beginning of the attribute will be considered 
more relevant than documents containing the query words at the end of the attributes.

### Sort

Results are sorted **according to parameters decided at query time**. 
When the **sort** ranking rule is in a higher position, sorting is exhaustive: results will be less relevant but 
follow the user-defined sorting order more closely. 
When **sort** is in a lower position, sorting is relevant: results will be very relevant but might not always follow the order defined by the user.

:::note
Differently from other ranking rules, sort is only active for queries containing the sort search parameter. 
If a search request does not contain sort, or if its value is invalid, this rule will be ignored.
:::

### Exactness

Results are sorted by **the similarity of the matched words with the query words**. Returns documents that contain exactly the same terms as the ones queried first.

[Learn more about ranking rules.](https://www.meilisearch.com/docs/learn/relevancy/relevancy)

## Stop Words

![Magento Admin Index Settings Stop Words](/img/indexes/stop-words.png)

Your dataset may contain words you want to ignore during search because, for example, they don’t add semantic 
value or occur too frequently (for instance, the or of in English). 
You can add these words to the stop words list and Meilisearch will ignore them during search.

[Learn more about stop words.](https://www.meilisearch.com/docs/reference/api/settings#stop-words)

## Synonyms

![Magento Admin Index Settings Synonyms](/img/indexes/synonyms.png)

Your dataset may contain words with similar meanings. 
For these, you can define a list of synonyms: words that will be treated as the same or similar for search purposes. 
Words set as synonyms won’t always return the same results due to factors like typos and splitting the query.

[Learn more about synonyms.](https://www.meilisearch.com/docs/learn/relevancy/synonyms)


## Typo Tolerance

![Magento Admin Index Settings Typo Tolerance](/img/indexes/typo-tolerance.png)

Typo tolerance is a built-in feature that helps you find relevant results even when your search queries contain 
spelling mistakes or typos, for example, typing chickne instead of chicken. 
This setting allows you to do the following for your index:

- Enable or disable typo tolerance
- Configure the minimum word size for typos
- Disable typos on specific words
- Disable typos on specific document attributes



[https://www.meilisearch.com/docs/learn/relevancy/typo_tolerance_settings](https://www.meilisearch.com/docs/learn/relevancy/typo_tolerance_settings)

