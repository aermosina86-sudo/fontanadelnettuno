---

## layout: default

# Fontana del Nettuno

## Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

[View on GitHub](https://github.com/aermosina86-sudo/fontanadelnettuno)

[🏠 Home](index.html) | [🏛️ Topic](topic.html) | [🛠️ Methodology](methodology.html) | [📊 SPARQL & Results](sparql.html) | [🔍 Identifying Gaps](gaps.html) | [💬 LLM Prompts](prompts.html) | 🔗 RDF Triples | [⚠️ Challenges](challenges.html) | [✅ Conclusion](conclusion.html)

---

# Generating New RDF Triples

## Aim of this section

After identifying the main information gaps in the ArCo representation of the **Fontana del Nettuno in Bologna**, we asked Large Language Models to help us generate possible RDF triples.

The aim was not to modify ArCo directly. Instead, the goal was to create **candidate enrichment triples** that show how the missing or unclear information could be represented in RDF.

The three gaps addressed in this section are:

1. **Location ambiguity**: the label **“Fontana del Nettuno”** can refer to fountains in different cities.
2. **Keyword ambiguity**: the word **“fontana”** can refer to a fountain, but also to a surname, artist name, family name, or company name.
3. **Missing cultural description**: the explored ArCo resources contain factual and documentary information, but not a clear cultural interpretation of the monument’s symbolic, civic, and urban meaning.

The RDF triples below were generated with the support of ChatGPT and Gemini, then manually checked and simplified.

---

# Prompt used for the RDF triples

The following prompt was used to ask the LLMs to generate RDF triples:

```text
I am working on a Knowledge Graph enrichment project about the Fontana del Nettuno in Bologna using the ArCo Knowledge Graph.

My project identified three information gaps:

1. Location ambiguity:
The label “Fontana del Nettuno” is not unique. It can refer to fountains in different cities. The resource selected for my project is the Fontana del Nettuno in Bologna.

Main resource:
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante

Site resource:
http://dati.beniculturali.it/iccd/schede/resource/Site/Sito_di_S001886_Fontana_del_Nettuno,_detta_del_Gigante

Site label:
BOLOGNA

2. Keyword ambiguity:
A broad SPARQL search for “fontana” returned mixed results. Some results referred to actual fountains, while others referred to people, artists, families, or companies named Fontana.

3. Missing cultural description:
The explored ArCo resources contain factual and documentary data, such as labels, site information, titles, and image-related records. However, they do not clearly provide a cultural description of the Fontana del Nettuno’s symbolic, civic, artistic, and urban significance.

Please propose RDF triples in Turtle syntax for these three gaps.

Requirements:
1. Separate confirmed information from proposed enrichment.
2. Use ArCo or standard RDF vocabularies where possible.
3. Use valid Turtle syntax.
4. Do not invent unsupported facts.
5. Clearly explain each triple.
6. Also create SPARQL CONSTRUCT queries for the proposed triples.
```

---

# 1. Triple set for Gap 1: Location Ambiguity

## Missing information

The first gap concerns **location ambiguity**.

The expression **“Fontana del Nettuno”** is not unique. It can refer to different Neptune fountains in different cities. For this project, the selected resource is the Fontana del Nettuno in Bologna:

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

The SPARQL exploration confirmed that this resource is connected to a site resource labelled **BOLOGNA**.

However, because the name **“Fontana del Nettuno”** is shared by several monuments, we decided to propose enrichment triples that make the Bologna location more explicit.

## Proposed RDF triples

```turtle
@prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
@prefix skos:    <http://www.w3.org/2004/02/skos/core#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix schema:  <https://schema.org/> .
@prefix ex:      <http://example.org/fontana-nettuno-bologna/enrichment/> .

<http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
    skos:altLabel "Fontana del Nettuno di Bologna"@it ;
    skos:altLabel "Fontana del Nettuno, Bologna"@it ;
    skos:altLabel "Fountain of Neptune in Bologna"@en ;
    dcterms:spatial <http://dati.beniculturali.it/iccd/schede/resource/Site/Sito_di_S001886_Fontana_del_Nettuno,_detta_del_Gigante> ;
    schema:location <http://dati.beniculturali.it/iccd/schede/resource/Site/Sito_di_S001886_Fontana_del_Nettuno,_detta_del_Gigante> ;
    ex:hasDisambiguationNote ex:location-ambiguity-note .

ex:location-ambiguity-note
    a skos:Concept ;
    rdfs:label "Location disambiguation note for the Fontana del Nettuno in Bologna"@en ;
    skos:scopeNote "The label 'Fontana del Nettuno' is ambiguous because it may refer to Neptune fountains in different cities. This enrichment identifies the selected resource as the Fontana del Nettuno located in Bologna."@en .
```

## Explanation

The `skos:altLabel` triples add clearer labels that include **Bologna**. This helps distinguish the Bologna fountain from other fountains with similar names.

The `dcterms:spatial` and `schema:location` triples connect the main resource to the Bologna site resource using standard vocabularies.

The local property `ex:hasDisambiguationNote` connects the resource to a note explaining why the enrichment is necessary.

This enrichment does not invent a new location. It only makes the already identified Bologna location more explicit.

## SPARQL CONSTRUCT query

```sparql
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos:    <http://www.w3.org/2004/02/skos/core#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX schema:  <https://schema.org/>
PREFIX ex:      <http://example.org/fontana-nettuno-bologna/enrichment/>

CONSTRUCT {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
      skos:altLabel "Fontana del Nettuno di Bologna"@it ;
      skos:altLabel "Fontana del Nettuno, Bologna"@it ;
      skos:altLabel "Fountain of Neptune in Bologna"@en ;
      dcterms:spatial <http://dati.beniculturali.it/iccd/schede/resource/Site/Sito_di_S001886_Fontana_del_Nettuno,_detta_del_Gigante> ;
      schema:location <http://dati.beniculturali.it/iccd/schede/resource/Site/Sito_di_S001886_Fontana_del_Nettuno,_detta_del_Gigante> ;
      ex:hasDisambiguationNote ex:location-ambiguity-note .

  ex:location-ambiguity-note
      a skos:Concept ;
      rdfs:label "Location disambiguation note for the Fontana del Nettuno in Bologna"@en ;
      skos:scopeNote "The label 'Fontana del Nettuno' is ambiguous because it may refer to Neptune fountains in different cities. This enrichment identifies the selected resource as the Fontana del Nettuno located in Bologna."@en .
}
WHERE {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
      ?p ?o .
}
LIMIT 1
```

## Result

![Location ambiguity CONSTRUCT result](assets/construct-location-result.png)

The result generated **9 RDF statements**. This confirms that the `CONSTRUCT` query successfully produced a small RDF graph for the proposed location-disambiguation enrichment.

---

# 2. Triple set for Gap 2: Keyword Ambiguity

## Missing information

The second gap concerns the ambiguity of the word **“fontana.”**

A broad search for **“fontana”** in ArCo retrieved mixed results. Some of them referred to actual fountains, while others referred to people, artists, families, or companies named **Fontana**.

Examples included:

* **Fontana del Nettuno**
* **Sorelle Fontana**
* **Fontana Roberto**
* **Fontana Prospero**
* **Fontana Luigi**
* **Maurizio Nicolò Fontana**
* **Fontana Pietro**
* **Ditta Fontanarte**

This shows that the keyword **“fontana”** is not precise by itself. It needs semantic disambiguation.

## Proposed RDF triples

```turtle
@prefix skos:    <http://www.w3.org/2004/02/skos/core#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix ex:      <http://example.org/fontana-nettuno-bologna/enrichment/> .

<http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
    dcterms:subject ex:fontana-as-water-feature ;
    dcterms:type ex:fountain-cultural-site .

ex:fontana-as-water-feature
    a skos:Concept ;
    skos:prefLabel "fontana as water feature"@en ;
    skos:prefLabel "fontana come struttura d'acqua"@it ;
    skos:scopeNote "Use this concept when the word 'fontana' refers to a fountain or water-related monument, not when Fontana is a surname, family name, artist name, or company name."@en .

ex:fountain-cultural-site
    a skos:Concept ;
    skos:prefLabel "fountain cultural site"@en ;
    skos:prefLabel "sito culturale fontana"@it ;
    skos:broader ex:fontana-as-water-feature .
```

## Explanation

This triple set is a **methodological enrichment**. It does not add a new historical fact about the monument. Instead, it records the semantic distinction between **fontana as a fountain** and **Fontana as a proper name**.

The concept `ex:fontana-as-water-feature` explains the intended meaning of the word **fontana** in this project.

The concept `ex:fountain-cultural-site` classifies the selected resource more precisely as a fountain-related cultural site.

This helps explain why the project moved from a broad search for **“fontana”** to more specific searches such as **“Fontana del Nettuno”** and **“Fontana del Nettuno” + “Bologna.”**

## SPARQL CONSTRUCT query

```sparql
PREFIX skos:    <http://www.w3.org/2004/02/skos/core#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX ex:      <http://example.org/fontana-nettuno-bologna/enrichment/>

CONSTRUCT {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
      dcterms:subject ex:fontana-as-water-feature ;
      dcterms:type ex:fountain-cultural-site .

  ex:fontana-as-water-feature
      a skos:Concept ;
      skos:prefLabel "fontana as water feature"@en ;
      skos:prefLabel "fontana come struttura d'acqua"@it ;
      skos:scopeNote "Use this concept when the word 'fontana' refers to a fountain or water-related monument, not when Fontana is a surname, family name, artist name, or company name."@en .

  ex:fountain-cultural-site
      a skos:Concept ;
      skos:prefLabel "fountain cultural site"@en ;
      skos:prefLabel "sito culturale fontana"@it ;
      skos:broader ex:fontana-as-water-feature .
}
WHERE {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
      ?p ?o .
}
LIMIT 1
```

## Result

![Keyword ambiguity CONSTRUCT result](assets/construct-keyword-result.png)

The result generated **10 RDF statements**. The output shows the main resource connected to the concept **fontana as water feature**, while the concept itself explains that this meaning should not be confused with **Fontana** as a surname, family name, artist name, or company name.

---

# 3. Triple set for Gap 3: Missing Cultural Description

## Missing information

The third gap concerns the absence of a clear cultural description.

The SPARQL exploration showed that ArCo contains several types of information about the Fontana del Nettuno, such as labels, site information, title resources, image-related records, and subject resources.

However, the explored resources did not provide a clear cultural explanation of the fountain’s symbolic, civic, artistic, and urban significance.

For this reason, we used LLMs to generate possible cultural descriptions. The final description was manually shortened and checked before being transformed into RDF triples.

## Proposed RDF triples

```turtle
@prefix rdfs:    <http://www.w3.org/2000/01/rdf-schema#> .
@prefix dcterms: <http://purl.org/dc/terms/> .
@prefix prov:    <http://www.w3.org/ns/prov#> .
@prefix a-cd:    <https://w3id.org/arco/ontology/context-description/> .
@prefix ex:      <http://example.org/fontana-nettuno-bologna/enrichment/> .

<http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
    dcterms:description "The Fontana del Nettuno in Bologna is a monumental marble and bronze fountain whose cultural significance depends not only on its material form, but also on its civic, symbolic, and urban role. Located in the historic centre near Piazza Maggiore, it represents Neptune as an image of authority and control over water, while its sculptural programme connects the monument to Renaissance ideas of power, urban order, and the public display of civic identity."@en ;
    a-cd:isSubjectOfInterpretation ex:cultural-interpretation-fontana-nettuno-bologna .

ex:cultural-interpretation-fontana-nettuno-bologna
    a a-cd:Interpretation ;
    rdfs:label "Cultural interpretation of the Fontana del Nettuno in Bologna"@en ;
    a-cd:hasInterpretationSubject <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante> ;
    dcterms:description "This interpretation describes the fountain as a civic and symbolic monument. Neptune is understood as a figure of authority over water and, by extension, political power. The monument's central urban position in Bologna connects its artistic programme to Renaissance urban space and to the public representation of civic and papal authority."@en ;
    dcterms:source <https://www.bolognawelcome.com/en/places/squares-streets-monuments/fontana-del-nettuno-2> ;
    dcterms:source <https://www.italia.it/en/emilia-romagna/bologna/fountain-neptune> ;
    prov:wasDerivedFrom <https://www.bolognawelcome.com/en/places/squares-streets-monuments/fontana-del-nettuno-2> ;
    prov:wasDerivedFrom <https://www.italia.it/en/emilia-romagna/bologna/fountain-neptune> .
```

## Explanation

The `dcterms:description` triple adds a cultural description to the main Fontana resource.

The property `a-cd:isSubjectOfInterpretation` links the fountain to a separate interpretation node. This is useful because the cultural meaning of a monument is not the same as a simple factual label.

The interpretation node contains a short explanation of the monument’s civic and symbolic meaning.

The `dcterms:source` and `prov:wasDerivedFrom` triples make the enrichment more transparent because they show that the cultural description was based on external sources and human verification, not only on the LLM output.

## SPARQL CONSTRUCT query

```sparql
PREFIX rdfs:    <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dcterms: <http://purl.org/dc/terms/>
PREFIX prov:    <http://www.w3.org/ns/prov#>
PREFIX a-cd:    <https://w3id.org/arco/ontology/context-description/>
PREFIX ex:      <http://example.org/fontana-nettuno-bologna/enrichment/>

CONSTRUCT {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
      dcterms:description "The Fontana del Nettuno in Bologna is a monumental marble and bronze fountain whose cultural significance depends not only on its material form, but also on its civic, symbolic, and urban role. Located in the historic centre near Piazza Maggiore, it represents Neptune as an image of authority and control over water, while its sculptural programme connects the monument to Renaissance ideas of power, urban order, and the public display of civic identity."@en ;
      a-cd:isSubjectOfInterpretation ex:cultural-interpretation-fontana-nettuno-bologna .

  ex:cultural-interpretation-fontana-nettuno-bologna
      a a-cd:Interpretation ;
      rdfs:label "Cultural interpretation of the Fontana del Nettuno in Bologna"@en ;
      a-cd:hasInterpretationSubject <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante> ;
      dcterms:description "This interpretation describes the fountain as a civic and symbolic monument. Neptune is understood as a figure of authority over water and, by extension, political power. The monument's central urban position in Bologna connects its artistic programme to Renaissance urban space and to the public representation of civic and papal authority."@en ;
      dcterms:source <https://www.bolognawelcome.com/en/places/squares-streets-monuments/fontana-del-nettuno-2> ;
      dcterms:source <https://www.italia.it/en/emilia-romagna/bologna/fountain-neptune> ;
      prov:wasDerivedFrom <https://www.bolognawelcome.com/en/places/squares-streets-monuments/fontana-del-nettuno-2> ;
      prov:wasDerivedFrom <https://www.italia.it/en/emilia-romagna/bologna/fountain-neptune> .
}
WHERE {
  <http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante>
      ?p ?o .
}
LIMIT 1
```

## Result

![Cultural description CONSTRUCT result](assets/construct-cultural-description-result.png)

The result generated **10 RDF statements**. This shows that the `CONSTRUCT` query successfully produced a proposed RDF graph containing a cultural description and a structured interpretation node.

---

# Failed or incomplete query attempts

During the construction of the RDF triples, not all query attempts worked correctly.

Some attempts returned an **empty HTML Microdata document**, meaning that the query produced no RDF statements.

![Empty CONSTRUCT result](assets/construct-failed-empty.png)

Another attempt produced a timeout error:

![Timeout result](assets/construct-timeout.png)

These results were not used as final enrichment triples. However, they were useful because they showed that SPARQL `CONSTRUCT` queries need to be carefully written. If the `WHERE` clause is too restrictive, the query may return no results. If the query is too broad, it may exceed the endpoint time limit.

For this reason, the final queries were simplified and limited to the selected Fontana del Nettuno resource.

---

# Analysis of ChatGPT and Gemini RDF outputs

Both ChatGPT and Gemini were useful for generating RDF triples, but their answers needed human control.

ChatGPT produced a safer and more cautious proposal. It separated confirmed baseline information from proposed enrichment and used standard vocabularies such as `skos`, `dcterms`, `schema`, and `prov`.

Gemini proposed a more ambitious solution. It suggested external links to Wikidata and DBpedia, such as `owl:sameAs`, and also proposed stronger semantic typing using external ontology classes. This was useful, but some of these links would require extra verification before being included in the final project.

For this reason, the final triples used in this project are closer to the cautious version. They do not claim that the Fontana resource is identical to an external Wikidata or DBpedia resource. Instead, they focus on:

* clearer labels for disambiguation;
* a project-level concept for the meaning of **fontana**;
* a cultural description supported by LLM analysis and human verification.

---

# Triples Summary

| Gap                  | Subject                           | Predicate                        | Object                               |
| -------------------- | --------------------------------- | -------------------------------- | ------------------------------------ |
| Location ambiguity   | Fontana del Nettuno main resource | `skos:altLabel`                  | “Fontana del Nettuno di Bologna”     |
| Location ambiguity   | Fontana del Nettuno main resource | `dcterms:spatial`                | Bologna site resource                |
| Location ambiguity   | Fontana del Nettuno main resource | `schema:location`                | Bologna site resource                |
| Location ambiguity   | Fontana del Nettuno main resource | `ex:hasDisambiguationNote`       | Location ambiguity note              |
| Keyword ambiguity    | Fontana del Nettuno main resource | `dcterms:subject`                | `ex:fontana-as-water-feature`        |
| Keyword ambiguity    | Fontana del Nettuno main resource | `dcterms:type`                   | `ex:fountain-cultural-site`          |
| Keyword ambiguity    | `ex:fontana-as-water-feature`     | `skos:scopeNote`                 | Explanation of the word “fontana”    |
| Cultural description | Fontana del Nettuno main resource | `dcterms:description`            | Cultural description of the fountain |
| Cultural description | Fontana del Nettuno main resource | `a-cd:isSubjectOfInterpretation` | Cultural interpretation node         |
| Cultural description | Cultural interpretation node      | `dcterms:source`                 | Bologna Welcome and Italia.it        |

---

# Final Consideration

The proposed RDF triples show how the representation of the **Fontana del Nettuno in Bologna** could be enriched.

The first set of triples helps solve the problem of location ambiguity. The second set explains why the keyword **fontana** can be ambiguous in ArCo searches. The third set adds a cultural interpretation that is not clearly visible in the explored ArCo results.

The LLMs were useful for suggesting possible RDF structures, but their outputs were not accepted automatically. The final triples were selected manually, simplified, and checked against the project’s SPARQL results.
