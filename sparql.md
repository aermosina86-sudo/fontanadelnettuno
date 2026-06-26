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

1. [De Boulogne J. (detto Giambologna)/ Fontana Del Nettuno/ Bologna](https://w3id.org/arco/resource/Subject/740679cbe96f6c4c7d6c9da040c4d242)

2. [Italia - Emilia Romagna - Bologna - Piazza Del Nettuno - Palazzo Re Enzo - Fontana Del Nettuno](https://w3id.org/arco/resource/Subject/1c81f0efabe53549630809ab6fd79047)

3. [Fontana Del Nettuno A Bologna](https://w3id.org/arco/resource/Subject/21c4ddfe628b306443993418d67a87e8)

4. [La Fontana Del Nettuno A Bologna Con Passanti E Piccioni](https://w3id.org/arco/resource/Subject/7d3d7dcb35ed1b46b2e2fd34390c3499)

### Interpretation of the Results

The results show that ArCo does contain subject resources related to the Fontana del Nettuno. This means that the fountain is not only present through cultural property records and image-related resources, but also appears as a subject in the knowledge graph.

However, the subject labels are not completely uniform. Some labels refer directly to the fountain, while others include additional contextual information, such as Bologna, Piazza del Nettuno, Palazzo Re Enzo, Giambologna, or specific visual scenes. This suggests that the topic is represented in ArCo, but its subject representation is distributed across several differently named resources.

## Query 4 — Using UNION to retrieve multiple naming patterns

In this query, we explored whether the **Fontana del Nettuno** appears in ArCo through different naming patterns. In particular, we wanted to compare the standard name **“Fontana del Nettuno”** with the alternative expression **“Gigante”**, which is sometimes associated with the fountain.

The aim of the query was to retrieve resources that contain either one of these naming patterns. This helps us understand whether the monument is represented through different labels, alternative names, or related records in the dataset.

### Explanation of keywords used

**UNION**: combines two alternative search patterns. In this query, it allows us to retrieve resources whose labels contain either “Fontana del Nettuno” or “Gigante.”

**FILTER** and **REGEX**: search for specific words inside the labels.

**DISTINCT**: removes duplicate results.

**LIMIT**: limits the number of results to 50.

### SPARQL Query

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?resource ?label
WHERE {
  {
    ?resource rdfs:label ?label .
    FILTER(REGEX(?label, "Fontana del Nettuno", "i"))
  }
  UNION
  {
    ?resource rdfs:label ?label .
    FILTER(REGEX(?label, "Gigante", "i"))
  }
}
LIMIT 50
```

### Results

The query returned several resources that match either the standard name **“Fontana del Nettuno”** or the alternative naming pattern connected to **“Gigante.”**

![Query 4 results](assets/query4.png)

### Examples of IRIs Found

1. [Fontana del Nettuno, detta del Gigante](http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante)
   Label: “Fontana del Nettuno, detta del Gigante”

2. [Fontana del Nettuno](http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S015658_Fontana_del_Nettuno)
   Label: “Fontana del Nettuno”

3. [Unit of description: Fontana del Nettuno, detta del Gigante](http://dati.beniculturali.it/iccd/schede/resource/uod/S001886)
   Label: “Fontana del Nettuno, detta del Gigante”

4. [Unit of description: Fontana del Nettuno](http://dati.beniculturali.it/iccd/schede/resource/uod/S015658)
   Label: “Fontana del Nettuno”

5. [Address resource for Fontana del Nettuno, detta del Gigante](http://dati.beniculturali.it/iccd/schede/resource/Address/Indirizzo_della_sede_di_S001886_Fontana_del_Nettuno,_detta_del_Gigante)
   Label: “Indirizzo della Sede di Fontana del Nettuno, detta del Gigante”

6. [Bologna. La Piazza Maggiore con la fontana del Nettuno](https://w3id.org/arco/resource/Title/1400042198-bologna-la-piazza-maggiore-con-la-fontana-del-nettuno)
   Label: “Bologna. La Piazza Maggiore con la fontana del Nettuno”

7. [Giambologna. La Fontana del Nettuno. Bologna](https://w3id.org/arco/resource/Title/0800365640-giambologna-la-fontana-del-nettuno-bologna)
   Label: “Giambologna. La Fontana del Nettuno. Bologna”

### Interpretation of the Results

The results show that ArCo contains resources using different naming patterns related to the Fontana del Nettuno. Some resources use the simple label **“Fontana del Nettuno,”** while others use the longer expression **“Fontana del Nettuno, detta del Gigante.”**

This is important because it suggests that the same monument, or closely related resources, may appear in the dataset under different names. The query therefore helps us identify possible alternative denominations and related records.

At the same time, the results also show that the expression **“Fontana del Nettuno”** is not unique in the dataset. The query retrieved resources related to fountains in other Italian cities, such as Trento, Florence, and Rome. It also retrieved different types of resources, including cultural sites, units of description, addresses, photographic records, titles, and historic or artistic properties.

For this reason, the query required manual filtering. The most relevant results for this project are the resources connected to Bologna, especially the resources labelled **“Fontana del Nettuno”** and **“Fontana del Nettuno, detta del Gigante.”**

This query shows that the representation of the Fontana del Nettuno is distributed across several labels and resource types. It also suggests a possible information gap: the relationship between the standard name **“Fontana del Nettuno”** and the alternative denomination **“detta del Gigante”** could be made more explicit in RDF.

## Query 5 — General search for “fontana” entities

After gathering specific data about the **Fontana del Nettuno**, we broadened the research by exploring other entities in ArCo whose labels contain the word **“fontana.”**

The aim of this query was to compare our selected topic with other fountain-related resources in the ArCo ontology. This broader search can help identify how fountains are generally represented in the dataset and whether there are properties, patterns, or types of information that might be missing from the description of the Fontana del Nettuno.

Since the word **“fontana”** can appear in many different contexts, we used `LIMIT 50` to avoid producing an excessively large result table.

### Explanation of keywords used

**FILTER** and **REGEX**: search for labels that contain the word “fontana,” regardless of uppercase or lowercase letters.

**arco:HistoricOrArtisticProperty**: restricts the search to resources classified as historic or artistic cultural properties in ArCo.

**DISTINCT**: removes duplicate results.

**LIMIT**: restricts the number of results to 50 for readability and efficiency.

### SPARQL Query

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/> 
PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/> 

SELECT DISTINCT ?cp ?label
WHERE {  
  ?cp a arco:HistoricOrArtisticProperty ;  
      rdfs:label ?label .  

  FILTER(REGEX(STR(?label), "fontana", "i"))  
}
LIMIT 50
```

### Results

The query returned a broad set of ArCo resources whose labels contain the word **“fontana.”**

![Query 5 results](assets/query5.png)

### Examples of IRIs Found

Some examples of resources retrieved by the query include:

1. [Abito di gala by Sorelle Fontana](https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201412758)
   Label: “abito di gala, cavalleresco o civile, da sera, femminile by Sorelle Fontana”

2. [Fontana, decorative element](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0300010509-1)
   Label: “fontana (decorazione plastico-pittorica, elemento d'insieme)”

3. [Sibilla by Fontana Annibale](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0300029809-2)
   Label: “Sibilla (statua, elemento d'insieme) by Fontana Annibale”

4. [Fontana della vita](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0300136952A-11)
   Label: “fontana della vita”

5. [Simboli mariani: fontana](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0300174171-7)
   Label: “simboli mariani: fontana”

6. [Giardino con fontana](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0500013802-2)
   Label: “Giardino con fontana”

### Interpretation of the Results

The results show that the word **“fontana”** appears in many different ArCo records. Some of these resources are actually related to fountains as architectural or artistic objects. For example, the query retrieved labels such as **“fontana della vita,” “simboli mariani: fontana,”** and **“Giardino con fontana.”**

However, the query also returned results where **“Fontana”** is not used as the name of a fountain, but as a surname or part of an artist, family, or company name. For example, the results include records connected to **Sorelle Fontana**, **Fontana Annibale**, and **Ditta Fontanarte**. These results are not directly useful for the study of fountains as cultural heritage objects, but they are important because they show the limits of a broad keyword search.

This query therefore helped us understand that a general search using only the word **“fontana”** is too broad. It retrieves both relevant and irrelevant results. Manual filtering is necessary to distinguish between actual fountain-related cultural properties and records where “Fontana” refers to a person, family name, or company.

This shows why more specific queries are needed when working with SPARQL and cultural heritage data. It also confirms that the Fontana del Nettuno should be studied with more precise label patterns, such as **“Fontana del Nettuno,” “Bologna,”** or related subject resources.

## Query 6 — Investigating used properties for “fontana”

The aim of this query was to understand which properties and values are commonly used to describe entities whose labels contain the word **“fontana”** in the ArCo dataset.

After exploring resources directly related to the **Fontana del Nettuno**, this query broadened the search. The purpose was to compare the selected topic with other fountain-related entities and to identify which RDF properties are commonly used for similar resources. This can help us decide which vocabulary may be useful when proposing enrichment triples for the Fontana del Nettuno.

### Explanation of keywords used

**?property ?value**: retrieves the predicate-object pairs connected to each resource. This allows us to inspect how the resource is described in RDF.

**FILTER** and **REGEX**: restrict the results to labels containing the word “fontana.”

**ORDER BY DESC(?property)**: organizes the results by property name in descending order.

**LIMIT**: limits the number of results so that the output remains readable.

### SPARQL Query

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> 
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX arco: <https://w3id.org/arco/ontology/arco/> 
PREFIX a-cd: <https://w3id.org/arco/ontology/context-description/>
PREFIX cis: <http://dati.beniculturali.it/cis/> 

SELECT DISTINCT ?cp ?label ?property ?value
WHERE {  
  ?cp a arco:HistoricOrArtisticProperty ; 
      rdfs:label ?label ; 
      ?property ?value .  

  FILTER(REGEX(STR(?label), "fontana", "i"))  
}  
ORDER BY DESC(?property)
LIMIT 100
```

### Results

The query returned several ArCo resources whose labels contain the word **“fontana.”** The visible results include both actual fountain-related objects and records where “Fontana” appears as a surname or part of an artist/company name.

![Query 6 results](assets/query6.png)

### Examples of results found

Some examples of resources retrieved by the query include:

1. [Abito di gala by Sorelle Fontana](https://w3id.org/arco/resource/HistoricOrArtisticProperty/1201412758)
   Label: “abito di gala, cavalleresco o civile, da sera, femminile by Sorelle Fontana”

2. [Giardino con fontana](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0500013802-2)
   Label: “Giardino con fontana”

3. [Fontana della vita](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0500087470-2)
   Label: “fontana della vita”

4. [Fontana in un giardino](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0800067348-75)
   Label: “fontana in un giardino, veduta con fontana”

5. [Ritratto di donna by Fontana Roberto](https://w3id.org/arco/resource/HistoricOrArtisticProperty/0900335032-1)
   Label: “ritratto di donna by Fontana Roberto”

6. [Preghiera di Noè dopo il diluvio by Fontana Prospero](https://w3id.org/arco/resource/HistoricOrArtisticProperty/1200257975-1)
   Label: “preghiera di Noè dopo il diluvio by Fontana Prospero”

### Interpretation of the Results

The results show that a broad search for the word **“fontana”** retrieves many different kinds of resources. Some are directly relevant because they refer to fountains as artistic, architectural, or decorative objects. For example, resources such as **“Giardino con fontana,” “fontana della vita,”** and **“fontana in un giardino”** are connected to fountain-related subjects or representations.

However, many results are not directly related to fountains. In several cases, **“Fontana”** appears as a surname, such as **Sorelle Fontana**, **Fontana Annibale**, **Fontana Roberto**, or **Fontana Prospero**. These results are not useful for understanding fountains as cultural heritage objects, but they reveal an important limitation of simple keyword-based SPARQL searches.

This query was useful because it showed that the term **“fontana”** alone is too broad. It retrieves both relevant and irrelevant resources. For this reason, more precise filters are necessary when studying the Fontana del Nettuno. Terms such as **“Nettuno,” “Bologna,” “Piazza del Nettuno,”** or specific ArCo subject resources are more effective for identifying relevant data.

The query also helped us inspect the RDF structure of fountain-related resources. By looking at the `?property` and `?value` columns, it becomes possible to see which predicates are commonly used in ArCo descriptions. This is important for the enrichment phase, because proposed RDF triples should use vocabulary that is compatible with the existing structure of the knowledge graph.
