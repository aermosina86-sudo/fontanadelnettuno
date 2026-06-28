layout: default
Fontana del Nettuno
Enriching Cultural Heritage Knowledge with ArCo and Large Language Models

View on GitHub

🏠 Home | 🏛️ Topic | 🛠️ Methodology | 📊 SPARQL & Results | 🔍 Identifying Gaps | 💬 LLM Prompts | 🔗 RDF Triples | ⚠️ Challenges | ✅ Conclusion

Identifying Gaps

After exploring the ArCo Knowledge Graph through SPARQL queries, several possible information gaps were identified in the representation of the Fontana del Nettuno in Bologna.

The purpose of this section is to explain which information is already present in the dataset and which relationships could be made more explicit through RDF enrichment.

Gap 1 — The selected resource is not directly connected to Giambologna

One important gap concerns the relationship between the Fontana del Nettuno in Bologna and Giambologna, also known as Jean de Boulogne or De Boulogne.

Earlier SPARQL queries showed that Giambologna appears in some subject and photographic records related to the Fontana del Nettuno. For example, one subject resource contains the label:

“De Boulogne J. (detto Giambologna)/ Fontana Del Nettuno/ Bologna”

However, this does not necessarily mean that the main Bologna resource is directly connected to Giambologna through a structured RDF property.

To verify this, an ASK query was used.

ASK Query
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
Result

The query returned:

false




Interpretation

The result false means that the selected Bologna resource “Fontana del Nettuno, detta del Gigante” is not directly connected to Giambologna through the RDF properties searched in this query.

This does not mean that Giambologna is unrelated to the monument. Instead, it shows that the information exists in a less direct way. Giambologna appears in related subject and photographic records, but not as a clear structured relation attached to the main Fontana del Nettuno resource.

This is an important enrichment gap. A possible RDF enrichment could explicitly connect the main Fontana del Nettuno resource to Giambologna after human verification.
