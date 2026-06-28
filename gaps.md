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
## Gap 1 — Location ambiguity: “Fontana del Nettuno” is not unique

One important gap identified during the SPARQL exploration concerns **location ambiguity**. The label **“Fontana del Nettuno”** alone is not specific enough to identify the Bologna monument with certainty.

This issue already appeared in **Query 1**, where a broad label search was used to find resources containing the expression **“Fontana del Nettuno.”** The results showed that ArCo contains several resources with this expression, but they do not all represent the same monument. Some resources refer to cultural sites, while others refer to titles, addresses, subject records, photographs, prints, or other artistic representations.

This means that searching only by name can be misleading. A resource may contain the words **“Fontana del Nettuno”** in its label, but it may not refer to the specific monument in Bologna selected for this project.


### Evidence from the SPARQL queries

Query 1 showed that there are multiple resources containing the expression **“Fontana del Nettuno.”** This already suggested that the name is not unique in the dataset.

```text
Fontana del Nettuno, detta del Gigante → BOLOGNA
Fontana del Nettuno → ROMA
```
![Query 1 highlighted results](assets/query1highlighted.jpg)

### Interpretation

These results show that the expression **“Fontana del Nettuno”** is ambiguous in the ArCo Knowledge Graph. Without checking the place or site connected to the resource, it is possible to select the wrong entity.

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

This resource is relevant because it is connected to **BOLOGNA**. By contrast, the simpler resource labelled **“Fontana del Nettuno”** is connected to **ROMA**, so it should not be used as the main resource for a project about the Bologna monument.

This identifies an important information gap. The gap is not that ArCo lacks location information completely. Instead, the issue is that location information is necessary for disambiguation, because the label alone does not clearly distinguish between resources with the same or similar names.

For RDF enrichment, this suggests that the connection between the main Fontana del Nettuno resource and its Bologna location should be made explicit and easy to retrieve. This would reduce ambiguity and help users or automated systems identify the correct monument more reliably.



