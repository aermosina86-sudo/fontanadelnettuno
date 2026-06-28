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

## Query 8 — ASK: Is Giambologna directly connected to the selected Fontana del Nettuno resource?

After Query 7, we found that **Giambologna** appears in some subject and photographic records related to the Fontana del Nettuno. However, it was still unclear whether Giambologna is directly connected to the main Bologna resource **“Fontana del Nettuno, detta del Gigante”** through an explicit RDF property.

For this reason, we used an `ASK` query. The aim was to verify whether the selected Bologna resource has any direct value containing **“Giambologna,” “Boulogne,”** or **“De Boulogne.”**

## Explanation of the keyword

**ASK**: tests whether a query pattern has at least one solution. It does not return a table of results. It only returns `true` or `false`.

If the result is `true`, the pattern exists in the knowledge graph.
If the result is `false`, the pattern was not found.

## SPARQL Query

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

ASK
WHERE {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante> ?property ?value .

  OPTIONAL { ?value rdfs:label ?valueLabel . }

  BIND(
    CONCAT(
      STR(?value), " ",
      COALESCE(STR(?valueLabel), "")
    )
    AS ?searchText
  )

  FILTER(REGEX(?searchText, "Giambologna|Boulogne|De Boulogne", "i"))
}
```

## Result

The query returned:

```text
false
```

## Interpretation of the Result

The result `false` means that the selected Bologna resource **“Fontana del Nettuno, detta del Gigante”** is not directly connected to Giambologna through the RDF properties searched in this query.

This does not mean that Giambologna is unrelated to the monument. Previous queries showed that Giambologna appears in related subject and photographic resources, especially in labels such as **“De Boulogne J. (detto Giambologna)/ Fontana Del Nettuno/ Bologna.”** However, this information is not represented as a direct structured relation from the main Fontana del Nettuno resource.

This result identifies an important information gap. The relationship between the Fontana del Nettuno in Bologna and Giambologna exists in the broader dataset, but it is distributed across related records and labels rather than expressed as a clear RDF statement connected to the main resource.

This gap can be used later for RDF enrichment. A possible enrichment direction would be to propose a direct relation between the main Fontana del Nettuno resource and Giambologna, after human verification.

