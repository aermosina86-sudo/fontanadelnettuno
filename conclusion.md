---

## layout: default

# Fontana del Nettuno

## Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

[View on GitHub](https://github.com/aermosina86-sudo/fontanadelnettuno)

[🏠 Home](index.html) | [🏛️ Topic](topic.html) | [🛠️ Methodology](methodology.html) | [📊 SPARQL & Results](sparql.html) | [🔍 Identifying Gaps](gaps.html) | [💬 LLM Prompts](prompts.html) | [🔗 RDF Triples](triples.html) | [⚠️ Challenges](challenges.html) | ✅ Conclusion

---

# Conclusion

This project on the **Fontana del Nettuno in Bologna** allowed us to explore how Knowledge Graphs and Large Language Models can be used together to enrich cultural heritage data.

The main objective was to investigate how the Fontana del Nettuno is represented in the **ArCo Knowledge Graph**, identify possible information gaps, and use LLMs to propose candidate RDF triples for enrichment.

The project showed that ArCo already contains different kinds of information about the fountain, including labels, site information, title resources, subject resources, and image-related records. However, the exploration also revealed that this information is not always easy to retrieve, interpret, or connect to one clear cultural description of the monument.

---

## 🧠 SPARQL and ArCo Ontology

Working with SPARQL and ArCo was one of the most important parts of the project.

SPARQL made it possible to search the knowledge graph in a structured way and to move from broad exploration to more precise results. Through different queries, we learned how to use important SPARQL keywords such as:

```text
SELECT
DISTINCT
FILTER
REGEX
OPTIONAL
UNION
ORDER BY
LIMIT
ASK
CONSTRUCT
```

The project showed that keyword selection is very important. A broad search using only **“fontana”** returned mixed results, including actual fountains, artists, surnames, families, and companies. This demonstrated that a simple keyword search is not always enough in a knowledge graph.

More precise searches, such as **“Fontana del Nettuno”** and **“Fontana del Nettuno” + “Bologna,”** helped identify the resource that best represents the Bologna monument:

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

The SPARQL analysis also showed that location information is essential for disambiguation, because the name **“Fontana del Nettuno”** can refer to fountains in different cities.

---

## 🔍 Main Information Gaps Identified

The project identified three main gaps.

The first gap was **location ambiguity**. The label **“Fontana del Nettuno”** is not unique, so the Bologna resource needs to be clearly distinguished from other Neptune fountains in other cities.

The second gap was **keyword ambiguity**. The word **“fontana”** can mean a fountain, but it can also appear as a surname, family name, artist name, or company name. This made manual filtering necessary.

The third gap was the **lack of a clear cultural description**. The explored ArCo results contained factual and documentary information, but they did not clearly express the fountain’s symbolic, civic, artistic, and urban significance.

These gaps showed that the problem was not the complete absence of data. Rather, the issue was that some information was ambiguous, fragmented, or not expressed in a culturally descriptive way.

---

## 🤖 Large Language Models

The project compared the outputs of **ChatGPT** and **Gemini**.

The LLMs were tested through different prompting techniques:

```text
Zero-shot prompting
Few-shot prompting
Chain-of-thought prompting
```

Zero-shot prompts were useful for testing how the models responded to simple questions, such as whether there are Fontana del Nettuno monuments in other cities.

Few-shot prompting was useful for classification tasks. For example, the models were asked to distinguish between cases where **Fontana** referred to a fountain and cases where it referred to a person, family, or company.

Chain-of-thought prompting was the most useful technique for generating cultural interpretation. By asking the models to follow a structured sequence of steps, we obtained richer descriptions of the monument’s visual elements, symbolic meaning, historical context, and urban role.

The comparison showed that the two models had different strengths.

ChatGPT produced more structured and cautious answers. It was especially useful for generating RDF triples and SPARQL `CONSTRUCT` queries that worked successfully in the endpoint.

Gemini produced richer and more ambitious ideas, especially for external links, semantic typing, and cultural interpretation. However, some of its SPARQL `CONSTRUCT` queries returned empty results or caused a timeout. This showed that Gemini’s outputs were useful for inspiration, but required more verification before being used.

---

## 🔗 RDF Triple Generation

The final part of the project transformed the identified gaps into proposed RDF enrichment triples.

The RDF triples were created for three purposes:

1. To make the Bologna location clearer.
2. To clarify the meaning of **fontana** as a water-related cultural object.
3. To add a cultural interpretation of the Fontana del Nettuno.

The successful `CONSTRUCT` queries generated RDF statements for the proposed enrichments. These triples do not modify ArCo directly. They show how the resource could be enriched in a future version of the knowledge graph.

An important methodological point was the distinction between:

```text
confirmed baseline information
```

and:

```text
proposed enrichment triples
```

Confirmed information included the resource label and its connection to the Bologna site. Proposed enrichment included alternative labels, disambiguation notes, project-level concepts, and cultural descriptions.

This distinction is important because LLM-generated information should not be treated as automatically authoritative.

---

## 🌍 Future Developments

This project opens several possibilities for future development.

First, the proposed RDF triples could be improved by checking them against additional authoritative sources, such as museum records, academic publications, or official cultural heritage documentation.

Second, the cultural description could be expanded with more precise information about the fountain’s iconography, materials, patronage, and role in the urban history of Bologna.

Third, the visual resources found in ArCo could be connected more explicitly to the main Fontana del Nettuno resource. This would make it easier for users to move from the monument to related photographs, prints, and historical views.

Finally, the project could be extended by comparing the Fontana del Nettuno in Bologna with other Neptune fountains in Italy, such as those in Florence, Rome, Naples, and Messina. This would make the location ambiguity problem even more visible and could support richer semantic disambiguation.

---

## Final Reflection

Through this project, we gained practical experience with SPARQL, ArCo, RDF triples, LLM prompting, and GitHub Pages.

The project showed that semantic web technologies are powerful tools for exploring cultural heritage data, but they require careful interpretation. SPARQL can reveal useful patterns and gaps, but the results are not always immediately clear. LLMs can help generate descriptions and candidate RDF triples, but their outputs must always be checked by humans.

The most important conclusion is that Knowledge Graph enrichment is a collaborative process between structured data, language models, and human verification.

For the **Fontana del Nettuno in Bologna**, this process made it possible to identify ambiguity, propose clearer RDF representations, and add a cultural layer that helps explain why the monument is not only a fountain, but also an important symbol of Bologna’s civic and urban identity.
