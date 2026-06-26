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

### IRIs Found

The query returned several relevant IRIs. The most important ones are listed below.

1. **Fontana del Nettuno, detta del Gigante**
   `http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante`

2. **Fontana del Nettuno**
   `http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S015658_Fontana_del_Nettuno`

3. **Unit of description for Fontana del Nettuno**
   `http://dati.beniculturali.it/iccd/schede/resource/uod/S015658`

4. **Address resource for Fontana del Nettuno**
   `http://dati.beniculturali.it/iccd/schede/resource/Address/Indirizzo_della_sede_di_S015658_Fontana_del_Nettuno`

5. **Title related to Fontana del Nettuno in Bologna**
   `https://w3id.org/arco/resource/Title/1400042198-bologna-la-piazza-maggiore-con-la-fontana-del-nettuno`

6. **Historic or Artistic Property related to Fontana del Nettuno in Bologna**
   `https://w3id.org/arco/resource/HistoricOrArtisticProperty/0300639021`

### Interpretation of the Results

The results show that ArCo contains more than one resource related to **Fontana del Nettuno**. However, these resources do not all represent the same kind of entity. Some refer to the cultural site itself, while others refer to titles, addresses, units of description, photographs, drawings, or artistic representations of the fountain.

For this reason, the resource selected as the main IRI for the project is:

`http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S015658_Fontana_del_Nettuno`

This IRI was selected because its label directly corresponds to the topic of the project: **“Fontana del Nettuno.”**

The next step is to explore this selected IRI in more detail in order to understand what information ArCo already provides and where possible information gaps may exist.
