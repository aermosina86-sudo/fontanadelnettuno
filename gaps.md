---
## layout: default
---

# Fontana del Nettuno

## Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

[View on GitHub](https://github.com/aermosina86-sudo/fontanadelnettuno)

[🏠 Home](index.html) | [🏛️ Topic](topic.html) | [🛠️ Methodology](methodology.html) | [📊 SPARQL & Results](sparql.html) | 🔍 Identifying Gaps | [💬 LLM Prompts](prompts.html) | [🔗 RDF Triples](triples.html) | [⚠️ Challenges](challenges.html) | [✅ Conclusion](conclusion.html)

---

# Identifying Gaps

After exploring the ArCo Knowledge Graph through SPARQL queries, several possible information gaps were identified in the representation of the **Fontana del Nettuno in Bologna**.

The purpose of this section is to explain which information is already present in the dataset and which relationships could be made more explicit through RDF enrichment.

---
## Gap 1 — Location ambiguity and difficulty of identifying the correct resource

The first important gap identified in this project concerns **location ambiguity**. The expression **“Fontana del Nettuno”** is not unique in ArCo, and a search based only on the name of the monument does not automatically identify the correct resource for the **Fontana del Nettuno in Bologna**.

This issue first appeared in **Query 1**, where we searched broadly for resources whose labels contain **“Fontana del Nettuno.”** The query confirmed that ArCo contains resources related to the selected topic. However, the results also showed that there is more than one resource with this expression in its label. These resources do not all represent the same kind of entity. Some refer to cultural sites, while others refer to titles, addresses, units of description, photographs, drawings, or artistic representations.

This means that the name alone is not enough to identify the Bologna monument with certainty.

### Evidence from Query 1

Query 1 showed that ArCo contains several resources whose labels include **“Fontana del Nettuno.”** This was useful as a first exploratory step, but it also revealed that the topic is represented through multiple resources.

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?resource ?label
WHERE {
  ?resource rdfs:label ?label .
  FILTER(REGEX(STR(?label), "Fontana del Nettuno", "i"))
}
LIMIT 50
```
![Query 1 highlighted results](assets/query1highlighted.jpg)

The results showed that the expression **“Fontana del Nettuno”** appears in several different records. This already suggested that manual filtering and further disambiguation were necessary.

---

### Evidence from Query 2

To focus more specifically on the Bologna case, Query 2 searched for resources whose labels contain both **“Fontana del Nettuno”** and **“Bologna.”**

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?resource ?label
WHERE {
  ?resource rdfs:label ?label .

  FILTER(REGEX(STR(?label), "Fontana del Nettuno", "i"))
  FILTER(REGEX(STR(?label), "Bologna", "i"))
}
LIMIT 30
```

![Bologna title resources](assets/bologna-title-resources.png)

This query returned several resources related to the **Fontana del Nettuno in Bologna**, but the results were mainly **Title** resources. For example, the query retrieved labels such as:

* **“Giambologna. La Fontana del Nettuno. Bologna”**
* **“Fontana del Nettuno in Bologna / Ortografia”**
* **“Fontana del Nettuno in Bologna / Iconografia”**
* **“Bologna. La Piazza Maggiore con la fontana del Nettuno”**

This shows that even when the word **“Bologna”** is added, the results do not necessarily identify one central monument resource. Instead, they show that the Fontana del Nettuno in Bologna is represented through related titles and documentary records.

---

### Bologna-specific resource identification

To identify the most relevant resource for this project, we then searched for a resource whose label contains **“Fontana del Nettuno”** and whose site is explicitly connected to **BOLOGNA**.

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX cis: <http://dati.beniculturali.it/cis/>

SELECT DISTINCT ?resource ?label ?site ?siteLabel
WHERE {
  ?resource rdfs:label ?label ;
            cis:hasSite ?site .

  ?site rdfs:label ?siteLabel .

  FILTER(REGEX(STR(?label), "Fontana del Nettuno", "i"))
  FILTER(REGEX(STR(?siteLabel), "BOLOGNA", "i"))
}
LIMIT 20
```

![Bologna-specific Fontana query](assets/bologna-fontana-query.png)

This query returned the resource:

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

with the label:

```text
Fontana del Nettuno, detta del Gigante
```

and the site label:

```text
BOLOGNA
```

This confirms that **“Fontana del Nettuno, detta del Gigante”** is the most appropriate main resource for this project, because it is explicitly connected to Bologna.

---

### Confirmation with ASK query

To confirm that the label **“Fontana del Nettuno”** can also refer to a resource connected to another city, we used an `ASK` query.

The aim of this query was not to study Rome, but to prove that the label **“Fontana del Nettuno”** is ambiguous and can be connected to more than one place.

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX cis: <http://dati.beniculturali.it/cis/>

ASK
WHERE {
  ?resource rdfs:label ?label ;
            cis:hasSite ?site .

  ?site rdfs:label ?siteLabel .

  FILTER(REGEX(STR(?label), "Fontana del Nettuno", "i"))
  FILTER(REGEX(STR(?siteLabel), "ROMA", "i"))
}
```

The query returned:

![Location ASK result](assets/true.png)

The result `true` confirms that ArCo contains at least one resource labelled **“Fontana del Nettuno”** that is connected to **ROMA**. This supports the location ambiguity gap because it shows that the label alone does not automatically identify the Bologna monument.

---

### Interpretation

The gap is not that ArCo lacks information about location. The problem is that location information is necessary to correctly identify the resource. If a user searches only for **“Fontana del Nettuno,”** they may retrieve resources connected to different places or different types of records.

For this project, the correct main resource is:

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

This resource was selected because it is connected to **BOLOGNA**.

This gap is important for RDF enrichment because the connection between the main resource and its Bologna location should be easy to retrieve and interpret. A clearer location-based representation would reduce ambiguity and help users or automated systems distinguish the **Fontana del Nettuno in Bologna** from other resources with the same or similar name.

## Gap 2 — Ambiguity of the Word “Fontana”

On the basis of the previous SPARQL queries, we identified one main information gap related to the ambiguity of the word **“fontana.”**

At the beginning of the project, we searched for resources containing the word **“fontana”** in their labels. The aim was to understand how ArCo represents fountain-related resources and to see whether the **Fontana del Nettuno** could be found through a general keyword search.

However, the results showed that the word **“fontana”** does not always refer to a fountain. In ArCo, it can also appear as a surname, an artist name, a family name, or a company name.

For example, the query returned results such as:

* **Giardino con fontana**
* **fontana della vita**
* **Bologna. La Piazza Maggiore con la fontana del Nettuno**
* **abito di gala by Sorelle Fontana**
* **Sibilla by Fontana Annibale**
* **ritratto di donna by Fontana Roberto**
* **preghiera di Noè dopo il diluvio by Fontana Prospero**
* **Santo by Fontana Luigi**
* **Ditta Fontanarte**

These results show that a broad search using only **“fontana”** produces mixed results. Some of them are relevant because they refer to actual fountains or fountain-related objects. Others are not directly relevant to the monument, because **Fontana** is used as a name.


![General fontana results](assets/query6.png)

### Interpretation of the results

The results show that the keyword **“fontana”** is too broad. It does not identify only fountains. It also retrieves people, artists, fashion houses, and other resources where **Fontana** is part of a name.

This creates a problem for knowledge graph exploration because users or automated systems may retrieve irrelevant resources if they search only with the word **“fontana.”**

For this reason, more precise search patterns were necessary in the project. Instead of using only **“fontana,”** we searched for:

```text
Fontana del Nettuno
```

and later:

```text
Fontana del Nettuno + Bologna
```

This helped narrow the results and identify the resource related to the **Fontana del Nettuno in Bologna**.

### Final consideration

The gap is not that ArCo lacks information about fountains. The problem is that a general keyword search can return ambiguous and mixed results.

This gap is important because it shows why disambiguation is necessary in cultural heritage knowledge graphs. To identify the correct resource, it is not enough to search for a single word. The query must include more precise information, such as the full monument name and the location.

In this project, the ambiguity of the word **“fontana”** led us to refine the search strategy and focus on the Bologna resource:

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

## Gap 3 — Alternative Name Embedded in the Label

A second gap identified in the project concerns the alternative denomination of the **Fontana del Nettuno in Bologna**.

During the SPARQL exploration, the main Bologna resource was identified as:

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

The label of this resource is:

```text
Fontana del Nettuno, detta del Gigante
```

This is important because the expression **“detta del Gigante”** suggests that the monument is also known by another denomination connected to the word **“Gigante.”**

To investigate how this expression appears in the RDF description, we searched for property-value pairs of the main resource containing the word **“Gigante.”**

### Query

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?property ?value
WHERE {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
      ?property ?value .

  FILTER(REGEX(STR(?value), "Gigante", "i"))
}
ORDER BY ?property
LIMIT 50
```

### Explanation of the keywords used

`SELECT DISTINCT` was used to retrieve unique property-value pairs.

`?property` represents the RDF property connected to the main resource.

`?value` represents the value of that property.

`FILTER` was used to restrict the results.

`REGEX` was used to search for the word **“Gigante”** inside the values.

`STR(?value)` converts the value into a string so that the textual search can be applied.

`ORDER BY` was used to organize the results by property.

`LIMIT` was used to keep the output manageable.

### Result

![Gigante property query](assets/gigante-property-query.png)

The query returned two relevant results.

First, the resource has a `cis:hasSite` relation pointing to a site resource whose IRI contains the expression **“Fontana del Nettuno, detta del Gigante.”**

Second, the resource has an `rdfs:label` with the value:

```text
Fontana del Nettuno, detta del Gigante
```

This confirms that the expression **“detta del Gigante”** is present in the RDF description of the resource.

### Further inspection of the site resource

By clicking on the site IRI returned by `cis:hasSite`, we found that the site resource is labelled:

```text
BOLOGNA
```

The site page also shows that this site is connected back to the main resource through `cis:isSiteOf`.

![Bologna site resource](assets/bologna-site-resource.png)

This confirms that the resource **Fontana del Nettuno, detta del Gigante** is connected to Bologna through a site resource.

### Interpretation

The result shows that information about the alternative denomination is present, but mainly inside the full textual label:

```text
Fontana del Nettuno, detta del Gigante
```

In the explored results, **“detta del Gigante”** does not appear as a separate and independent alternative name. It is embedded inside the full label and inside resource IRIs.

The gap is therefore not a complete absence of information. The gap is that the alternative denomination is not clearly separated in a structured, machine-readable way.

### Final consideration

This gap is relevant for RDF enrichment because alternative names can support search and disambiguation. A user might search for **“Gigante”** or **“Fontana del Gigante”**, but this information is currently visible mainly as part of a longer label.






