---
layout: default
---

# Fontana del Nettuno

## Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

[View on GitHub](https://github.com/aermosina86-sudo/fontanadelnettuno)

[🏠 Home](index.html) | [🏛️ Topic](topic.html) | [🛠️ Methodology](methodology.html) | [📊 SPARQL & Results](sparql.html) | [🔍 Identifying Gaps](gaps.html) | [💬 LLM Prompts](prompts.html) | [🔗 RDF Triples](triples.html) | ⚠️ Challenges | [✅ Conclusion](conclusion.html)

---

# Challenges Faced During the Project

During the development of this KE4H project on the **Fontana del Nettuno in Bologna**, several challenges appeared. These challenges were related to SPARQL querying, resource identification, ambiguity in labels, LLM prompting, RDF triple generation, and GitHub Pages.

Below, each challenge is described together with the solution adopted during the project.

---

## Understanding how SPARQL works

At the beginning of the project, one of the main difficulties was understanding how to write correct SPARQL queries.

SPARQL requires precise syntax and logical structure. Even a small mistake in a prefix, punctuation mark, or query pattern can change the result or make the query fail.

This was especially challenging because the project required the use of several SPARQL keywords, such as `SELECT`, `DISTINCT`, `FILTER`, `REGEX`, `OPTIONAL`, `UNION`, `ORDER BY`, `LIMIT`, `ASK`, and `CONSTRUCT`.

### Solution

The solution was to start with simple queries and then gradually make them more complex.

First, we used broad label searches with `rdfs:label`, `FILTER`, and `REGEX`. Then we added more specific conditions, such as searching for both **Fontana del Nettuno** and **Bologna**.

This helped us understand how to move from general exploration to more precise resource identification.

---

## Identifying the correct Fontana del Nettuno resource

A major challenge was identifying the correct ArCo resource for the **Fontana del Nettuno in Bologna**.

The expression **Fontana del Nettuno** is not unique. It can refer to Neptune fountains in several cities, including Bologna, Florence, Rome, Naples, and Messina.

In ArCo, a search for **Fontana del Nettuno** returned several resources. These resources did not all represent the same object. Some were title resources, some were image-related records, some were subject resources, and some were cultural site resources.

### Solution

To solve this problem, we refined the queries step by step.

First, we searched broadly for **Fontana del Nettuno**.

Then we searched for **Fontana del Nettuno** together with **Bologna**.

Finally, we used the site information to identify the most relevant Bologna resource:

`http://dati.beniculturali.it/iccd/schede/resource/CulturalInstituteOrSite/S001886_Fontana_del_Nettuno,_detta_del_Gigante`

This showed that location information was necessary to disambiguate the resource.

---

## Dealing with location ambiguity

The project showed that **location ambiguity** was one of the main gaps.

The label **Fontana del Nettuno** alone is not enough to identify the Bologna fountain. The same or similar name can refer to fountains in other cities.

This was also confirmed through LLM prompting. When we asked whether there are other Fontana del Nettuno monuments in Italy, both ChatGPT and Gemini recognized that several cities have Neptune fountains.

### Solution

The solution was to treat location as a key part of the enrichment.

In the RDF triples, we proposed clearer labels such as **Fontana del Nettuno di Bologna** and **Fountain of Neptune in Bologna**.

We also proposed connecting the main resource more explicitly to the Bologna site resource through properties such as `dcterms:spatial` and `schema:location`.

This helps distinguish the Bologna fountain from other fountains with the same name.

---

## The word fontana was too broad

Another challenge appeared when we searched for the word **fontana**.

At first, this seemed like a useful keyword because the project was about a fountain. However, the results showed that **fontana** does not always mean a fountain.

The search also returned names such as **Sorelle Fontana**, **Fontana Roberto**, **Fontana Prospero**, **Fontana Luigi**, **Maurizio Nicolo Fontana**, **Fontana Pietro**, and **Ditta Fontanarte**.

This showed that **Fontana** can be a surname, artist name, family name, or company name.

### Solution

The solution was to avoid relying only on the keyword **fontana**.

Instead, we used more precise search patterns, such as **Fontana del Nettuno** and **Fontana del Nettuno** together with **Bologna**.

We also used LLMs to classify whether the results referred to fountains, people, or companies. This helped us understand that keyword-based search in a knowledge graph requires manual filtering and semantic interpretation.

---

## Engineering effective LLM prompts

Another challenge was creating prompts that produced useful answers.

Simple prompts sometimes gave very general answers. For example, asking only **Where is Fontana del Nettuno?** usually produced a short answer about Bologna, but did not fully explain the ambiguity of the name.

For this reason, different prompting techniques were necessary.

### Solution

We used three types of prompting: **zero-shot prompting**, **few-shot prompting**, and **chain-of-thought prompting**.

Zero-shot prompting was useful for testing the model's first answer.

Few-shot prompting was useful for classification tasks, such as distinguishing **fontana** as a fountain from **Fontana** as a surname or company name.

Chain-of-thought prompting was the most useful for cultural interpretation because it guided the model through a structured analysis of the monument.

---

## Testing RDF triples with CONSTRUCT queries

Another challenge was testing the generated RDF triples.

Both ChatGPT and Gemini generated RDF triples and SPARQL `CONSTRUCT` queries, but their results were different.

The ChatGPT-based `CONSTRUCT` queries worked successfully and generated RDF statements.

The Gemini-based queries were more ambitious, but some returned empty results or caused a timeout.

### Solution

We compared the two models carefully.

ChatGPT's queries were simpler because they used the selected Fontana del Nettuno resource directly. This made them easier to test in the SPARQL endpoint.

Gemini's queries often used broader searches, exact labels, external vocabularies, or external authority links. These ideas were interesting, but they were harder to verify.

For this reason, the final RDF triples were mainly based on the working ChatGPT-style queries, while Gemini's output was discussed as an alternative proposal.

---

## Technical problems with GitHub Pages

There were also practical challenges while building the GitHub Pages website.

Some images did not load because the filename in the Markdown page did not exactly match the uploaded image filename. In other cases, the image was not in the correct `assets` folder.

GitHub Pages is strict with filenames. For example, `query6.png`, `Query6.png`, `query 6.png`, and `query6.PNG` are treated as different filenames.

### Solution

The solution was to rename image files clearly and upload them to the `assets` folder.

This made the website more stable and easier to manage.

---

# Final consideration

The challenges faced during this project show that Knowledge Graph enrichment requires both technical and interpretive work.

SPARQL helped us find existing data and identify gaps, but the results needed careful interpretation. LLMs helped generate candidate cultural descriptions and RDF triples, but their outputs required human checking. GitHub Pages helped publish the project, but also required attention to formatting, filenames, and image paths.

The most important lesson is that LLMs can support Knowledge Graph enrichment, but they cannot replace human verification. The final RDF triples were selected only after comparing model outputs, testing SPARQL `CONSTRUCT` queries, and checking that the proposed enrichment was consistent with the project evidence.
