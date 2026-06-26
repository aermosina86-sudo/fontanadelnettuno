---

## layout: default

# Fontana del Nettuno

## Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

[View on GitHub](https://github.com/aermosina86-sudo/fontanadelnettuno)

[🏠 Home](index.html) | [🏛️ Topic](topic.html) | 🛠️ Methodology | [📊 SPARQL & Results](sparql.html) | [🔍 Identifying Gaps](gaps.html) | [💬 LLM Prompts](prompts.html) | [🔗 RDF Triples](triples.html) | [⚠️ Challenges](challenges.html) | [✅ Conclusion](conclusion.html)

---

# Methodology

This project follows a multi-step methodology combining Knowledge Graph exploration, SPARQL querying, Large Language Model prompting, and human verification. The general aim is to investigate how the Fontana del Nettuno is represented in the ArCo Knowledge Graph and to propose possible RDF enrichment triples based on identified information gaps.

## 1. Selecting the Topic

The first step was to select a cultural heritage object that could be meaningfully explored through semantic web technologies. The Fontana del Nettuno in Bologna was chosen because it is a well-known monument with strong artistic, historical, and civic relevance. Its importance makes it a suitable case for testing whether a cultural heritage knowledge graph provides a rich structured representation.

## 2. Exploring the ArCo Knowledge Graph

The project uses ArCo as the main knowledge graph. ArCo is an ontology network and RDF dataset dedicated to Italian cultural heritage. The ArCo SPARQL endpoint was used to search for resources related to the Fontana del Nettuno and to understand how the object is represented in RDF.

At this stage, the goal was not yet to create new knowledge, but to explore existing information. We looked for labels, types, predicates, depictions, subjects, and other available properties connected to the selected cultural property.

## 3. Building SPARQL Queries

Several SPARQL queries were created to retrieve information from ArCo. The queries were designed to answer basic questions such as:

* Does ArCo contain a resource related to the Fontana del Nettuno?
* Which IRI best represents the selected object?
* What information is already available about this resource?
* Are subject, creator, denomination, or contextual data already present?
* Which predicates are used for similar cultural properties?

The queries use the mandatory SPARQL keywords required for the project: OPTIONAL, DISTINCT, UNION, FILTER, REGEX, LIMIT, and ORDER BY.

## 4. Analyzing Query Results

After running the SPARQL queries, the results were examined manually. This step was necessary to understand whether the returned resources were directly relevant to the Fontana del Nettuno or only loosely connected to the topic.

The analysis focused on selecting the most appropriate IRI and identifying which parts of the RDF description were already complete and which parts appeared limited or missing.

## 5. Identifying Information Gaps

The information gaps were identified by comparing what is known about the Fontana del Nettuno with what is explicitly represented in ArCo. Particular attention was given to possible gaps related to:

* subject or iconographic meaning;
* alternative names or denominations;
* creators or related agents;
* location and contextual information;
* missing links between the monument and relevant cultural concepts.

Only gaps that could be reasonably supported by SPARQL results and external verification were considered for RDF enrichment.

## 6. Prompt Engineering with LLMs

Large Language Models were then used to support the enrichment process. The project compares the behaviour of two LLMs: ChatGPT and Gemini.

Three prompting techniques were applied:

1. Zero-shot prompting;
2. Zero-shot Chain-of-Thought prompting;
3. Few-shot Chain-of-Thought prompting.

The purpose was to test whether different prompt designs influence the quality, accuracy, and structure of the models’ answers.

## 7. Comparing LLM Outputs

The outputs from ChatGPT and Gemini were compared according to several criteria:

* accuracy;
* clarity;
* consistency with ArCo data;
* ability to produce RDF-style output;
* hallucination risk;
* usefulness for knowledge graph enrichment.

This comparison helped determine which model and which prompting technique produced the most reliable candidate knowledge.

## 8. Human Verification

The LLM outputs were not accepted automatically. Since LLMs can produce plausible but incorrect information, each answer was checked manually. Outputs were accepted only if they were consistent with the SPARQL results and could be supported by reliable cultural heritage information.

This step was essential to distinguish between useful enrichment proposals and unsupported or inaccurate claims.

## 9. Creating RDF Triples

After verification, selected pieces of information were transformed into RDF triples. These triples were created as proposed enrichment data and were not inserted into the official ArCo Knowledge Graph.

The triples use ArCo-style vocabulary where possible. When no suitable ArCo predicate was available, the project explains the modelling choice and proposes a cautious solution.

## 10. Evaluation

The final step was to evaluate the process. The evaluation considers how useful SPARQL and LLMs were for identifying and addressing knowledge gaps. It also discusses the strengths and limitations of the models, especially their ability to generate structured RDF-compatible knowledge.

The project therefore combines technical exploration, LLM-assisted generation, and human judgement to propose possible enrichment of cultural heritage data.
