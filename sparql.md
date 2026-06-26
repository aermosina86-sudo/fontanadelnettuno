---
## layout: default
---

# Fontana del Nettuno

## Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

[View on GitHub](https://github.com/aermosina86-sudo/fontanadelnettuno)

[🏠 Home](index.html) | [🏛️ Topic](topic.html) | [🛠️ Methodology](methodology.html) | 📊 SPARQL & Results | [🔍 Identifying Gaps](gaps.html) | [💬 LLM Prompts](prompts.html) | [🔗 RDF Triples](triples.html) | [⚠️ Challenges](challenges.html) | [✅ Conclusion](conclusion.html)

---

# SPARQL Queries & Results

## Query 1 — Does ArCo contain our topic?

The first step was to verify whether the ArCo Knowledge Graph already contains an entity representing our selected topic, **Fontana del Nettuno**.

Since the exact class of the monument was not known at the beginning, the first query searched broadly through labels instead of limiting the search only to one ArCo class. This allowed us to check whether any resource in ArCo contains the expression **“Fontana del Nettuno”** in its label.

### Explanation of keywords used

**DISTINCT**: eliminates duplicate results.

**FILTER** and **REGEX**: used to retrieve more specific information. `FILTER` restricts the results based on a condition, while `REGEX` searches for a textual pattern inside string values.

**resource**: this variable stands for any resource in the knowledge graph. It is used instead of `cp` because, at this exploratory stage, we do not yet know whether the Fontana del Nettuno is classified as a Cultural Property, a monument, an artistic object, or another type of ArCo resource.

### SPARQL Query

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?resource ?label
WHERE {
  ?resource rdfs:label ?label .
  FILTER(REGEX(STR(?label), "Fontana del Nettuno", "i"))
}
LIMIT 50
```

### Results

The query returned several resources whose labels contain the expression **“Fontana del Nettuno.”** This confirms that ArCo contains resources related to the selected topic.

![Query 1 results](assets/query1-results.png)


### Interpretation of the Results

The results show that ArCo contains more than one resource related to **Fontana del Nettuno**. However, these resources do not all represent the same kind of entity. Some refer to the cultural site itself, while others refer to titles, addresses, units of description, photographs, drawings, or artistic representations of the fountain.

## Query 2 — Finding image-related resources about the Fontana del Nettuno

In this query, the aim was to find different kinds of visual resources related to the **Fontana del Nettuno in Bologna**. Instead of looking only for direct `foaf:depiction` links, the query searches more broadly for image-related records, including photographs, postcards, prints, drawings, negatives, and other visual representations.

This was useful because visual resources in ArCo and ICCD are not always stored only through the `foaf:depiction` property. Sometimes the image-related nature of the resource appears directly in the label, for example through words such as *stampa*, *cartolina*, *positivo*, or *negativo*.

### Explanation of keywords used

**UNION**: combines two search strategies: one for ArCo historic/artistic properties and one for photographic records.

**FILTER** and **REGEX**: search for resources whose labels contain “Fontana,” “Nettuno,” and “Bologna.”

**OPTIONAL**: retrieves the type and depiction only if this information is available.

**DISTINCT**: removes duplicate results.

**LIMIT**: limits the number of results to 50.

### SPARQL Query

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT DISTINCT ?image ?label ?type ?depiction
WHERE {
  {
    ?image a arco:HistoricOrArtisticProperty ;
           rdfs:label ?label .
  }
  UNION
  {
    ?image rdfs:label ?label .
    FILTER(REGEX(STR(?image), "fotografico", "i"))
  }

  FILTER(REGEX(STR(?label), "Fontana", "i"))
  FILTER(REGEX(STR(?label), "Nettuno", "i"))
  FILTER(REGEX(STR(?label), "Bologna", "i"))

  OPTIONAL { ?image rdf:type ?type . }
  OPTIONAL { ?image foaf:depiction ?depiction . }
}
LIMIT 50
```
### Results

The query returned several image-related resources connected to the Fontana del Nettuno in Bologna. The results include postcards, prints, photographic positives, negatives, and views of the fountain in its urban context.
![Query 2 results](assets/query%202.png)
### Images

The following screenshots show examples of image-related resources retrieved by the query.
<div style="display: flex; gap: 15px; align-items: flex-start; flex-wrap: wrap;">

  <img src="assets/fontana%201.jpg" alt="Fontana image result 1" style="width: 30%; min-width: 180px;">

  <img src="assets/fontana%202.jpg" alt="Fontana image result 2" style="width: 30%; min-width: 180px;">

  <img src="assets/fontana%203.jpg" alt="Fontana image result 3" style="width: 30%; min-width: 180px;">

</div>

### Interpretation of the Results

The results show that ArCo and ICCD contain many visual records connected to the Fontana del Nettuno in Bologna. These resources do not all represent the fountain in the same way. Some are postcards, some are photographs, some are negatives, and others are prints or views of the surrounding urban space.

This shows that the Fontana del Nettuno is represented not only as a single cultural site, but also through a wider network of visual resources. However, not every result contains a direct `foaf:depiction` link, which suggests that image-related information exists in the dataset but is not always represented in a simple or uniform way.

## Query 3 — Identifying subjects related to the Fontana del Nettuno

This query was designed to find unique resources that are classified as **Subjects** in the ArCo ontology and whose labels include the words **“Fontana”** and **“Nettuno.”**

By doing this, we aimed to discover whether ArCo contains subject resources related to the Fontana del Nettuno. This is useful because subjects can reveal how the monument is semantically described or referenced in the knowledge graph.

### Explanation of keywords used

**a-cd:Subject**: searches for resources classified as subjects in the ArCo context-description ontology.

**rdfs:label**: retrieves the name or label of each subject.

**FILTER** and **REGEX**: restrict the results to labels containing “Fontana” and “Nettuno.”

**DISTINCT**: removes duplicate results.

### SPARQL Query

```sparql
PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?subject ?label
WHERE {
  ?subject a a-cd:Subject ;
           rdfs:label ?label .

  FILTER(REGEX(STR(?label), "Fontana", "i"))
  FILTER(REGEX(STR(?label), "Nettuno", "i"))
}
LIMIT 50
```

### Results

The query returned four subject resources related to the Fontana del Nettuno. Some of them refer directly to the fountain in Bologna, while others connect the fountain to people, places, or visual representations.

![Query 3 results](assets/query3.png)

### Subjects Found

1. `https://w3id.org/arco/resource/Subject/740679cbe96f6c4c7d6c9da040c4d242`
   Label: “De Boulogne J. (detto Giambologna)/ Fontana Del Nettuno/ Bologna”

2. `https://w3id.org/arco/resource/Subject/1c81f0efabe53549630809ab6fd79047`
   Label: “Italia - Emilia Romagna - Bologna - Piazza Del Nettuno - Palazzo Re Enzo - Fontana Del Nettuno”

3. `https://w3id.org/arco/resource/Subject/21c4ddfe628b306443993418d67a87e8`
   Label: “Fontana Del Nettuno A Bologna”

4. `https://w3id.org/arco/resource/Subject/7d3d7dcb35ed1b46b2e2fd34390c3499`
   Label: “La Fontana Del Nettuno A Bologna Con Passanti E Piccioni”

### Interpretation of the Results

The results show that ArCo does contain subject resources related to the Fontana del Nettuno. This means that the fountain is not only present through cultural property records and image-related resources, but also appears as a subject in the knowledge graph.

However, the subject labels are not completely uniform. Some labels refer directly to the fountain, while others include additional contextual information, such as Bologna, Piazza del Nettuno, Palazzo Re Enzo, Giambologna, or specific visual scenes. This suggests that the topic is represented in ArCo, but its subject representation is distributed across several differently named resources.

For the project, this is useful because it shows both the presence of subject information and a possible semantic gap: the Fontana del Nettuno is represented through multiple subject resources, but there may not be one clearly standardized subject resource that gathers all relevant information about the monument.
