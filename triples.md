---

## layout: default

# Fontana del Nettuno

## Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

[View on GitHub](https://github.com/aermosina86-sudo/fontanadelnettuno)

[🏠 Home](index.html) | [🏛️ Topic](topic.html) | [🛠️ Methodology](methodology.html) | [📊 SPARQL & Results](sparql.html) | [🔍 Identifying Gaps](gaps.html) | [💬 LLM Prompts](prompts.html) | 🔗 RDF Triples | [⚠️ Challenges](challenges.html) | [✅ Conclusion](conclusion.html)

---

# Generating New RDF Triples

## Aim of this section

After identifying the main information gaps in the ArCo representation of the **Fontana del Nettuno in Bologna**, we used Large Language Models to help us generate possible RDF enrichment triples.

The aim was not to modify ArCo directly. Instead, the aim was to create **candidate RDF triples** showing how the missing or unclear information could be represented in a more explicit way.

The triples proposed in this section should therefore be understood as **project-level enrichment proposals**, not as existing triples already present in ArCo.

The three gaps addressed are:

1. **Location ambiguity**: the label **“Fontana del Nettuno”** can refer to fountains in different cities.
2. **Keyword ambiguity**: the word **“fontana”** can refer to a fountain, but also to a surname, artist name, family name, or company name.
3. **Missing cultural description**: the explored ArCo resources contain factual and documentary information, but not a clear cultural interpretation of the monument’s symbolic, civic, and urban meaning.

Both **ChatGPT** and **Gemini** were asked to propose RDF triples and SPARQL `CONSTRUCT` queries. However, after testing the generated queries, the results were different. The queries based on ChatGPT’s proposal worked successfully, while the queries proposed by Gemini either returned empty results or caused a timeout. For this reason, the final triples are mainly based on the working ChatGPT-style queries.

---

# Prompt used for RDF triple generation

The following prompt was given to both ChatGPT and Gemini.

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

# ChatGPT and Gemini outputs

## ChatGPT result

ChatGPT proposed a cautious RDF model. It separated confirmed information from proposed enrichment and used the selected Fontana del Nettuno resource directly. Its `CONSTRUCT` queries were simpler and easier to test in the SPARQL endpoint.

## Gemini result


Gemini proposed a more ambitious model. It suggested external links to Wikidata and DBpedia, stronger semantic typing, and additional external vocabulary. This was useful as an idea, but some parts were more difficult to verify and did not work reliably in the SPARQL endpoint.

---

# 1. Triple set for Gap 1: Location Ambiguity

## Missing information

The first gap concerns **location ambiguity**.

The expression **“Fontana del Nettuno”** is not unique. It can refer to Neptune fountains in different Italian cities, such as Bologna, Florence, Rome, Naples, or Messina.

In ArCo, the resource selected for this project is:

```text
http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante
```

The SPARQL exploration showed that this resource is connected to a site resource labelled **BOLOGNA**.

However, because the name **“Fontana del Nettuno”** can refer to different monuments, the location information should be made more explicit. This would help users and automated systems distinguish the Bologna fountain from other Neptune fountains.

---

## ChatGPT proposal

ChatGPT proposed adding alternative labels and a disambiguation note. This was useful because the enrichment does not invent a new place. It only makes the already identified Bologna location clearer.

## Final selected RDF triples

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

The `skos:altLabel` triples add alternative labels that include **Bologna**. This makes the resource easier to identify.

The `dcterms:spatial` and `schema:location` triples connect the main resource to the Bologna site resource using standard vocabularies.

The local property `ex:hasDisambiguationNote` connects the resource to an explanatory note about the ambiguity.

---

## ChatGPT CONSTRUCT query

This was the working query used for Gap 1.

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

The query generated **9 RDF statements**. This means the `CONSTRUCT` query successfully produced a small RDF graph for the proposed location-disambiguation enrichment.

---

## Gemini proposal and result

Gemini proposed a different and more ambitious solution. It suggested connecting the site to a Wikidata entity for Bologna and linking the ArCo resource to external records through `owl:sameAs`.

Example of Gemini’s proposed query:

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX owl: <http://www.w3.org/2002/07/owl#>
PREFIX clv: <https://w3id.org/italia/onto/CLV/>
PREFIX arco-location: <https://w3id.org/arco/ontology/location/>
PREFIX wd: <http://www.wikidata.org/entity/>

CONSTRUCT {
  ?site clv:hasCity wd:Q10143 .
  ?culturalResource owl:sameAs wd:Q1418858 .
}
WHERE {
  ?culturalResource rdfs:label "Fontana del Nettuno, detta del Gigante"@it ;
                    arco-location:hasSite ?site .
  ?site rdfs:label "BOLOGNA"@it .
}
```

This query did not work successfully in the endpoint. One reason is that the site relation found in our SPARQL exploration used `cis:hasSite`, while the Gemini query used `arco-location:hasSite`. The query also depended on exact label matching, which can be too restrictive.

For this reason, Gemini’s query was not adopted as the final version.

---

# 2. Triple set for Gap 2: Keyword Ambiguity

## Missing information

The second gap concerns the ambiguity of the word **“fontana.”**

A broad SPARQL search for **“fontana”** returned mixed results. Some were relevant because they referred to fountains or fountain-related objects. Others were not directly relevant because **Fontana** appeared as a surname, family name, artist name, or company name.

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

---

## ChatGPT proposal

ChatGPT proposed creating a small project-level SKOS concept explaining the difference between **fontana as a water feature** and **Fontana as a proper name**.

This was useful because this gap is methodological. It does not add a new historical fact about the monument. Instead, it documents why manual filtering was necessary.

## Final selected RDF triples

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

The concept `ex:fontana-as-water-feature` clarifies the meaning of **fontana** in this project.

The concept `ex:fountain-cultural-site` classifies the selected resource as a fountain-related cultural site.

The `skos:scopeNote` explains that the word **fontana** should not be confused with **Fontana** as a surname, family name, artist name, or company name.

---

## ChatGPT CONSTRUCT query

This was the working query used for Gap 2.

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

The query generated **10 RDF statements**. The result shows the main resource connected to the concept **fontana as water feature**, while the concept explains that this meaning should not be confused with **Fontana** as a proper name.

---

## Gemini proposal and result

Gemini proposed a different solution based on stronger semantic typing with external resources.

Example of Gemini’s proposed query:

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX wd: <http://www.wikidata.org/entity/>

CONSTRUCT {
  ?fountainResource a dbo:Fountain, wd:Q483453 .
}
WHERE {
  ?fountainResource rdfs:label ?label .
  FILTER(CONTAINS(LCASE(?label), "fontana del nettuno"))
  FILTER(CONTAINS(STR(?fountainResource), "CulturalInstituteOrSite"))
}
```

This query was conceptually interesting because it tried to classify the resource as a fountain. However, it was less suitable for the final project because it introduced external classes and depended on broader endpoint searching.

One Gemini-based attempt returned an empty HTML Microdata document:

![Empty CONSTRUCT result](assets/construct-failed-empty.png)

Another attempt produced a timeout error:

![Timeout result](assets/construct-timeout.png)

For this reason, Gemini’s version was not used as the final RDF model for this gap.

---

# 3. Triple set for Gap 3: Missing Cultural Description

## Missing information

The third gap concerns the absence of a clear cultural description.

The SPARQL exploration showed that ArCo contains several types of information about the Fontana del Nettuno, such as labels, site information, title resources, image-related records, and subject resources.

However, the explored resources did not provide a clear cultural explanation of the fountain’s symbolic, civic, artistic, and urban significance.

For this reason, LLMs were used to generate possible cultural descriptions. The final description was then shortened and checked manually before being transformed into RDF triples.

---

## ChatGPT proposal

ChatGPT proposed adding a cultural description and connecting the resource to a separate interpretation node. This was useful because interpretation is not the same as a simple factual label.

The final version therefore represents cultural meaning as an interpretation linked to the main resource.

## Final selected RDF triples

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

The `dcterms:description` triple adds a short cultural description to the main resource.

The property `a-cd:isSubjectOfInterpretation` links the fountain to a separate interpretation node.

The interpretation node is typed as `a-cd:Interpretation` and contains a description of the fountain’s civic and symbolic meaning.

The `dcterms:source` and `prov:wasDerivedFrom` triples make the enrichment more transparent because they show that the description is based on external sources and human verification, not only on the LLM output.

---

## ChatGPT CONSTRUCT query

This was the working query used for Gap 3.

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

The query generated **10 RDF statements**. The result shows a proposed cultural description and a structured interpretation node connected to the main Fontana del Nettuno resource.

---

## Gemini proposal and result

Gemini proposed a more narrative and ambitious cultural enrichment. It used properties such as `skos:scopeNote`, `skos:historyNote`, and `skos:editorialNote`.

Example of Gemini’s proposed query:

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX skos: <http://www.w3.org/2004/02/skos/core#>

CONSTRUCT {
  ?res skos:scopeNote "Symbol of papal power and governance, commissioned by Cardinal Legate Charles Borromeo to celebrate the election of Pope Pius IV."@en ;
       skos:historyNote "Functions as a visual and physical anchor of the civic center, intentionally positioning the power of the church adjacent to Palazzo d'Accursio."@en ;
       skos:editorialNote "A premier example of Italian Mannerist sculpture, showcasing Giambologna's dynamic, multi-viewpoint composition (figura serpentinata)."@en .
}
WHERE {
  ?res rdfs:label "Fontana del Nettuno, detta del Gigante"@it .
}
```

This proposal was useful because it showed how LLMs can generate cultural interpretation. However, it was not adopted as the final query for two reasons.

First, it depended on exact label matching. Second, it included some specific historical and interpretive claims that require careful verification before being represented as RDF.

For this reason, the final project used the simpler ChatGPT-style query with a separate interpretation node and source information.

---

# Comparison of ChatGPT and Gemini RDF outputs

| Aspect                    | ChatGPT                             | Gemini                                            |
| ------------------------- | ----------------------------------- | ------------------------------------------------- |
| General approach          | Cautious and controlled             | Ambitious and broader                             |
| Query structure           | Used the selected resource directly | Often searched more broadly or used exact labels  |
| Use of external resources | Limited                             | Suggested Wikidata, DBpedia, and external classes |
| SPARQL result             | Worked and generated RDF statements | Some queries returned empty results or timeout    |
| Main strength             | Practical and easier to test        | Useful for generating enrichment ideas            |
| Main limitation           | Less semantically ambitious         | Needed more verification and correction           |

---

# Interpretation of the comparison

The comparison shows that both LLMs were useful, but in different ways.

ChatGPT was more useful for producing **working RDF triples and CONSTRUCT queries**. Its approach was simpler and more practical because it used the selected Fontana del Nettuno resource directly.

Gemini was useful for generating ideas about possible semantic enrichment, such as external links, stronger typing, and cultural interpretation. However, its proposed queries did not work reliably in the SPARQL endpoint. Some returned empty results, while another caused a timeout.

This confirms an important methodological point: LLMs can help generate RDF structures, but their outputs must always be checked by humans and tested in a SPARQL endpoint.

For this reason, the final triples used in this project are based mainly on the **working ChatGPT-style CONSTRUCT queries**, while Gemini’s answers are discussed as alternative proposals that were not directly adopted.

---

# Failed or incomplete query attempts

During the construction of the RDF triples, some query attempts did not work correctly.

One result returned an **empty HTML Microdata document**, meaning that the query produced no RDF statements.

![Empty CONSTRUCT result](assets/construct-failed-empty.png)

Another attempt produced a timeout error.

![Timeout result](assets/construct-timeout.png)

These failed results were not used as final enrichment triples. However, they were useful because they showed that SPARQL `CONSTRUCT` queries need to be carefully written.

If the `WHERE` clause is too restrictive, the query may return no results. If the query is too broad or complex, it may exceed the endpoint time limit.

---

# Triples Summary

| Gap                  | Subject                           | Predicate                        | Object                               |
| -------------------- | --------------------------------- | -------------------------------- | ------------------------------------ |
| Location ambiguity   | Fontana del Nettuno main resource | `skos:altLabel`                  | “Fontana del Nettuno di Bologna”     |
| Location ambiguity   | Fontana del Nettuno main resource | `skos:altLabel`                  | “Fountain of Neptune in Bologna”     |
| Location ambiguity   | Fontana del Nettuno main resource | `dcterms:spatial`                | Bologna site resource                |
| Location ambiguity   | Fontana del Nettuno main resource | `schema:location`                | Bologna site resource                |
| Location ambiguity   | Fontana del Nettuno main resource | `ex:hasDisambiguationNote`       | Location ambiguity note              |
| Keyword ambiguity    | Fontana del Nettuno main resource | `dcterms:subject`                | `ex:fontana-as-water-feature`        |
| Keyword ambiguity    | Fontana del Nettuno main resource | `dcterms:type`                   | `ex:fountain-cultural-site`          |
| Keyword ambiguity    | `ex:fontana-as-water-feature`     | `skos:scopeNote`                 | Explanation of the word “fontana”    |
| Cultural description | Fontana del Nettuno main resource | `dcterms:description`            | Cultural description of the fountain |
| Cultural description | Fontana del Nettuno main resource | `a-cd:isSubjectOfInterpretation` | Cultural interpretation node         |
| Cultural description | Cultural interpretation node      | `dcterms:source`                 | Bologna Welcome and Italia.it        |
| Cultural description | Cultural interpretation node      | `prov:wasDerivedFrom`            | Bologna Welcome and Italia.it        |

---

# Final consideration

The proposed RDF triples show how the representation of the **Fontana del Nettuno in Bologna** could be enriched.

The first triple set helps solve the problem of location ambiguity. The second triple set explains why the keyword **fontana** can be ambiguous in ArCo searches. The third triple set adds a cultural interpretation that was not clearly visible in the explored ArCo results.

The experiment also showed that LLMs are useful for generating candidate RDF structures, but their outputs cannot be accepted automatically. In this project, ChatGPT produced the queries that worked successfully in the SPARQL endpoint, while Gemini produced useful but less reliable enrichment proposals.

The final RDF triples were therefore selected manually, simplified, and checked against the project’s SPARQL results.
