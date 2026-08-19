---
marp: true
title: Knowledge graph approaches for linking datasets to uncover new biological insights
paginate: true
header: Knowledge graph approaches for linking datasets to uncover new biological insights
style: |
  section {
    --ink: #20281f;
    --canopy: #234d35;
    --canopy-light: #5d8a6c;
    --loam: #8c5a35;
    --loam-light: #c99a6c;
    --mist: #eef3ec;
    --paper: #fbfbf8;
    --rule: #d7e1d6;

    font-family: "Liberation Sans", Arial, sans-serif;
    color: var(--ink);
 
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    padding: 40px;
  }

  h1, h2, h3 {
    font-weight: normal;
    color: var(--canopy);
  }

  h1 {
    border-bottom: 3px solid var(--rule);
    padding-bottom: 0.3em;
  }

  h2 {
    border-bottom: 2px solid var(--rule);
  }

  ul > li::marker {
    color: var(--canopy);
  }

  strong {
    color: var(--canopy);
  }

  pre > code {
    font-size: 20px;
  }

  a {
    color: var(--canopy);
    text-decoration: none;
    border-bottom: 1px solid var(--canopy-light);
  }

  blockquote {
    background: var(--mist);
    border-left: 5px solid var(--canopy);
    margin: 0.6em 0;
    padding: 0.5em 1em;
    color: var(--ink);
  }

  blockquote > p {
    margin: 0;
  }

  table {
    border-collapse: collapse;
    font-size: 0.8em;
  }

  table th {
    background: var(--canopy);
    color: var(--paper);
    font-weight: normal;
  }

  table td {
    border-bottom: 1px solid var(--rule);
    padding: 0.4em 0.8em;
    font-size: 0.6em
  }

  table tr:nth-child(even) td {
    background: var(--mist);
  }

  p, li, td {
    font-size: 20px;
  }
  
  footer {
    font-family: "Bitstream Charter", Georgia, serif;
    font-style: italic;
    color: var(--canopy-light);
    font-size: 0.55em;
  }

  header {
    margin: 0;
    position: absolute;
    top: 4px;
    right: 30px;
    color: var(--canopy-light);
    font-size: 14px; 
    text-align: right;
  }

  section::after {
    color: var(--canopy-light);
  }

  /* Title slide */
  section.lead {
    background: var(--mist);
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  section.lead h1 {
    font-size: 2em;
    border-bottom: none;
    margin-bottom: 0.6em;
  }

  section.lead h1::after {
    content: "";
    display: block;
    width: 90px;
    height: 4px;
    background: var(--canopy);
    margin-top: 0.5em;
  }

  section {
    border-top: 10px solid var(--canopy);
  }

  /* Hands-on slides: warm soil accent marks the interactive thread */
  section.hands-on {
    border-top: 10px solid var(--loam);
  }

  section.hands-on h1,
  section.hands-on h2 {
    color: var(--loam);
    border-bottom-color: var(--loam-light);
  }

  section.hands-on strong {
    color: var(--loam);
  }

  .columns {
    display: grid;
    grid-template-columns: 1.3fr 0.7fr;
    gap: 1rem;

    /* Center align content vertically */
    align-items: center;
  }
  
  img[alt~="align-bottom-centered"] {
    position: absolute;
    bottom: 2px;
    left: 50%; /* centering */
    transform: translateX(-50%); /* centering */
    margin: 0; /* control space to bottom end of slide */
  }
  
  .aside {
    border-left: 4px solid #0069c1;
    background: #f0f6fc;
    padding: 3em;
    border-radius: 4px;
    font-size: 5px;
  }
  
---

<!-- _class: lead -->
<!-- _header: "" -->

# Knowledge graph approaches for linking datasets to uncover new biological insights

**ECCB 2026 — Tutorial 12: Resources for plant sciences**
Session 3 · 15:30–16:10
Dr. Carissa Bleker, National Institute of Biology, Slovenia

---

## What this session covers

- Why knowledge graphs matter for your work
- The fundamental ingredients: Graphs and Ontologies
- What kinds of questions become answerable that weren't before
- A quick tour of the plant KG landscape
- **Hands-on:** querying real plant knowledge graphs yourself

---

## The everyday problem

<div class="columns">

<div>

Plant biology data is scattered across resources:

- **Genomic/functional annotation**: TAIR (Arabidopsis), Gramene (comparative plant genomics), Ensembl Plants, ...
- **Traits**: Plant Ontology (PO), Trait Ontology (TO), Planteome, ...
- **Pathways**: AraCyc, KEGG, Gene Ontology (GO), MapMan, ...
- **Interactions:** STRING, Intact, PlantTFDB, miRTarBase, ...
- Published literature
- Your own omics data

Each resource may have it's own identifier systems, schema, granularity, ... 

Each is searchable on its own, but **cross-cutting questions** integrating multiple sources of data are difficult. 

How to easily recover indirect biological relationships?

</div>

<div>

![w:450](images/many-resources.png)
<!-- * (KEGG, STRING, a paper, a spreadsheet) showing data scattered across sources* -->

</div>

</div>

---

## What is a knowledge graph?

<style scoped>
.columns {
  grid-template-columns: 1.4fr 0.6fr;
}
</style>

<div class="columns">

<div>

  A knowledge graph (KG) represents information in a data model of:
  - **Nodes** — entities (a gene, a protein, a pathway, a phenotype, a paper)
  - **Edges** — relationships between entities (*encodes*, *regulates*, *interacts with*, *is part of*)
  - **Properties** — attributes attached to nodes or edges (confidence score, data source, interaction type)


<br>

<br>

<br>

<br>

<div class="aside">

This is the *property graph* definition. An alternative representation are *RDF* (Resource Description Framework) **triples**, in the form of Subject *predicate* Object. 

</div>

</div>

<div>

![w:250](images/eg-graph.svg)

Together: a traversable source of facts.

</div>

</div>

---


## A concrete example

Much of the important information in biology lives in the *relationships*, not just the entities. For example, the sentence:

> The **DREB2A** gene `encodes` a transcription factor that `regulates` drought-responsive genes

Can be represented as **triples**: Subject *predicate* Object

* DREB2A (`gene`) *encodes* Transcription factor (`protein`)
* Transcription factor (`protein`) *regulates* Drought response (`pathway`)

---

## Why this matters for research

<!-- _footer: "Adapted from the BioCypher Workshop slides (Heidelberg, June 2026), CC-BY 4.0" -->

Once relationships are explicit and typed, you can query *along* them.

Examples:
- Start at a stress phenotype → follow `associated with` → reach a candidate gene → follow `regulates` → get a list of **upstream regulators** to investigate
- Start at a significant locus (e.g. a GWAS hit) → follow `is part of` → reach the genes in that region → follow `interacts with` → narrow down to the most plausible **candidate genes**

These are single, easy-to-state graph queries, not a chain of separate database lookups stitched together.

---

## A step further: reasoning

<!-- _footer: "Adapted from the BioCypher Workshop slides (Heidelberg, June 2026), CC-BY 4.0" -->

Explicit relationships also let you **infer** new facts, not just retrieve stored ones:

- *Gene A* `confers resistance to` *Powdery mildew*
- *Powdery mildew* `is a` *Fungal disease*
- → Therefore: *Gene A* `confers resistance to` **some** Fungal disease(s)

This is what separates a knowledge graph from a static lookup table — connections not explicitly store can still be derived from the ones you did.

<div class="aside">

**A necessary caveat: not all edges are equal**

Not every relationship in a knowledge graph carries the same confidence:

- A curated, experimentally validated interaction ≠ a text-mined or computationally predicted one
- If reasoning chains through an uncertain edge, the conclusion inherits that uncertainty — silently, unless the graph tracks it
- Uncertainty should be modelled explicitly as **evidence**, **confidence**, or **provenance**.

</div>

---

## Foundations: What is an ontology?

<!-- _footer: "Adapted from the BioCypher Workshop slides (Heidelberg, June 2026), CC-BY 4.0" -->

**To connect data, graphs need a shared language.**

- A controlled vocabulary for a domain: defines classes, subclass relationships, and how entities relate
- Matters in biology because the same thing can be named, grouped, or interpreted differently across datasets
- Example: *"drought tolerance," "water-deficit tolerance,"* and *"drought resistance"* overlap in everyday speech, but aren't necessarily the same defined trait across phenotyping databases
- An ontology (e.g., the Plant Trait Ontology) defines what's meant and how it relates to other traits, this is what lets a knowledge graph connect entities from different sources consistently

---

## Where this lives: graph databases

<div class="columns">

<div>

- Knowledge graphs are typically stored in a **graph database** (e.g. Neo4j) or as **RDF triples**, rather than rows and columns in a relational table
- Relationships are stored as **first-class objects**, so multi-step connections (gene → pathway → phenotype → condition) can be traversed directly
- Queried with graph-native languages: **Cypher** or **SPARQL** (you don't always need to write these yourself!)

</div>

<div>

![w:500](images/skm-neo4j-screenshot.png)
*A Neo4j graph database showing labelled nodes and typed edges*

</div>

</div>

---

## What a knowledge graph adds:

<div class="columns">

<div>

A knowledge graph puts all of this in one place, connected, so you can ask questions that span sources:

- *What else is in the neighbourhood of my gene of interest?*
- *Is there existing evidence linking it to this stress response?*
- *Why might pathway A interfere with pathway B's effect?*

This is a shift from **lookup** to **traversal and reasoning**.

</div>

<div>

![w:800](images/example-aba.svg)
*Reactions and node properties from Stress Knowledge Map*
</div>

</div>

---

## Question types this unlocks

<div class="columns">

<div>


- **Guilt-by-association** — find candidate genes/proteins via their network neighbourhood
- **Mechanism hypotheses** — find paths connecting an observed phenotype to known signalling components
- **-Omics contextualisation** — place your experimental data (transcriptomics, proteomics) within prior knowledge
- **And on...** — gap-finding, conflict detection, cross-species comparison...

These are the questions a single database or a literature search struggles to answer directly.

</div>

<div>

![w:800](images/example-abaa.svg)
*Reactions and node properties from Stress Knowledge Map*
</div>

</div>

---

## The future: Beyond manual querying

<div class="columns">

<div>

Answering these questions often require either a developer to build a tool/interface, or *you* to write manual queries.

**Knowledge graphs are a strong foundation for retrieval-augmented generation (GraphRAG)** — letting an LLM do that work instead:

- Selective retrieval of structured content from an external resource
- Answers can be grounded in explicit graph facts, preserving provenance and reducing hallucination
- Increases accessibility by allowing natural language interaction

</div>

<div>

![w:250](images/graphrag_flow.svg)
*GraphRAG*

</div>

</div>

---

## A few plant knowledge graphs to know

<!-- | Resource | Built for | URL |
|---|---|---|
| **AgroLD** <br> ![h:50](images/agroLD-logo.png) | Broad-scale Semantic Web integration across many crops | [v2.agrold.org/agrold](https://v2.agrold.org/agrold) |
| **KnetMiner** <br> ![h:50](images/KnetMiner800_name.png) | Gene-centric candidate discovery, strong GWAS/QTL tie-in | [knetminer.com](https://knetminer.com) |
| **SKM** <br>  ![h:50](images/skm_logo_large.png) | Curated molecular interactions for plant stress signalling + hypothesis generation | [skm.nib.si](https://skm.nib.si) |
 -->


| Resource | Built for | URL |
|---|---|---|
| ![h:50](images/agroLD-logo.png) | Broad-scale Semantic Web integration across many crops | [v2.agrold.org/agrold](https://v2.agrold.org/agrold) |
| ![h:50](images/KnetMiner800_name.png) | Gene-centric candidate discovery, strong GWAS/QTL tie-in | [knetminer.com](https://knetminer.com) |
| ![h:50](images/skm_logo_large.png) **Stress Knowledge Map** | Curated molecular interactions for plant stress signalling + hypothesis generation | [skm.nib.si](https://skm.nib.si) |
| PlantMetWiki | RDF-based linked data for plant metabolic pathways, built from PlantCyc across 500+ species | [plantmetwiki.bioinformatics.nl](https://plantmetwiki.bioinformatics.nl/) |
| TomTom | Tomato-focused KG integrating databases for multi-stress gene regulatory network analysis | [github.com/Plant-Net/TomTom](https://github.com/Plant-Net/TomTom) |
| PlantConnectome | LLM-mined KG from 71,000+ plant biology abstracts, includes genes, molecules, stresses, ... | [plant.connectome.tools](https://plant.connectome.tools/) |
|PlantGraph | Large-scale Arabidopsis KG with natural-language querying | [plantgraph.se](https://plantgraph.se/) |
| Plant Reactome | Curated rice reference pathways projected to 130+ plant species via orthology (Gramene project) | [plantreactome.gramene.org](https://plantreactome.gramene.org/index.php?option=com_content&view=article&id=45&Itemid=290&lang=en) |


*Lost? FAIDARE (FAIR Data-finder for Agronomic REsearch) indexes AgroLD, KnetMiner, SKM and 30+ other plant databases (genotyping, phenotyping, germplasm) through a common interface ([urgi.versailles.inrae.fr/faidare](https://urgi.versailles.inrae.fr/faidare/))!*

<!-- _footer: "Not an exhaustive list, first three are ELIXIR services" -->

---

## AgroLD

<div class="columns">

<div>

- Semantic Web / SPARQL-based knowledge graph
- ~1 billion triples, 150+ datasets, 50+ plant species (broad crop coverage)
- Built to support hypothesis formulation and validation across diverse plant species
- Best fit when your question spans **many species or crops** and you're comfortable with SPARQL

<br>

<br>

<br>

[v2.agrold.org/agrold](https://v2.agrold.org/agrold)

</div>

<div>

![w:200](images/placeholder-image.svg)
*Placeholder: screenshot of the AgroLD homepage or Quick Search interface*

</div>

</div>

---

## KnetMiner

<style scoped>
.columns {
  grid-template-columns: .4fr .6fr;
}
</style>

<div class="columns">

<div>

- Gene-centric network mining tool
- Evidence-scored search: ranks candidate genes by network support
- Strong integration with GWAS/QTL data
- Best fit when you're starting from a **trait or locus** and need **candidate gene prioritisation**

<br>

<br>

<br>

[knetminer.com](https://knetminer.com)

</div>

<div>

![w:600](images/knetminer-screenshot.png)
*Screenshot of the KnetMiner Plants Lite network view*

</div>

</div>

---

## Stress Knowledge Map (SKM)

<style scoped>
.columns {
  grid-template-columns: .4fr .6fr;
}
</style>

<div class="columns">

<div>

- Two complementary graphs:
  - **PSS** — curated, mechanistic *P*lant *S*tress *S*ignalling model (800+ _reactions_)
  - **CKN** — *C*omprehensive molecular interaction *k*nowledge *n*etwork (480,000+ interactions)
- Built by domain experts through systematic literature/database curation
- Best fit for **plant stress biology**, mechanistic hypothesis generation, and quantitative modelling

<br>

<br>

<br> 

[skm.nib.si](https://skm.nib.si)

</div>

<div>

![w:500](images/skm-webapp-screenshot.png)
*Screenshot of the SKM home page*

</div>

</div>

---

## Let's try them

<!-- _class: hands-on -->

We've seen what each resource is built for — now let's actually use them.

---

## Hands-on: AgroLD

<!-- _class: hands-on -->
<!-- _header: "[v2.agrold.org/agrold](https://v2.agrold.org/agrold)" -->

<div class="columns">

<div>

Using the browser interface:

- **Quick Search** — keyword search across the whole knowledge base
- **Advanced Search** — pick an entity type and search within it
- **SPARQL Query Editor** — comes with ready-made example queries you can run as-is

[v2.agrold.org/agrold](https://v2.agrold.org/agrold)

</div>

<div>

![w:180](images/placeholder-image.svg)
</div>

</div>

---

## Hands-on: KnetMiner (Plants Lite)

<!-- _class: hands-on -->
<!-- _header: "[app.knetminer.com/plants-lite](https://app.knetminer.com/plants-lite)" -->

<div class="columns">

<div>

Using the browser interface. Select the (free) **Plants Lite** resource:

- **Keyword search** — e.g. a trait of interest (*"grain colour"*, *"drought tolerance"*)
- **Gene list / genome region search** — start from your own candidate genes or a genomic interval
- **Network view** — launch the knowledge network for a gene, explore connections, and see the evidence-ranked candidate list

<br>

<br>

<br>

<br>

[app.knetminer.com/plants-lite](https://app.knetminer.com/plants-lite)

</div>

<div>

![w:600](images/knetminer-screenshot.png)
*Screenshot of the KnetMiner Plants Lite network view*

</div>

</div>

---

## Worked Example: Grain Colour & Pre-Harvest Sprouting (PHS)

<!-- _class: hands-on -->
<!-- _header: "[app.knetminer.com/plants-lite](https://app.knetminer.com/plants-lite)" -->

<style scoped>
  section p, li, td {
    font-size:18px;
  }
</style>

Start from a **wheat gene list** and the traits *"grain colour and pre-harvest sprouting"*:
- **Trait dissection** — KnetMiner breaks the trait into sub-components (seed dormancy, testa pigmentation, ABA signalling) rather than treating it as one keyword
- **Semantic motif search** — runs 180+ pre-defined motif queries per gene, scoring each for evidence linking it to grain colour, dormancy, and sprouting resistance
- **Interactive gene–trait network** — builds a knowledge graph connecting genes, QTLs, publications, and ontology terms, ranked by evidence
- **Graph Chat** — ask follow-up questions in natural language (*"which of these genes also affect root architecture?"*); the agent reasons directly over the constructed KG, not just text

![h:360 align-bottom-centered](images/knetminer-worked-example.png)

---

## Hands-on: SKM

<!-- _class: hands-on -->
<!-- _header: "[skm.nib.si](https://skm.nib.si)" -->

<style scoped>
.columns {
  grid-template-columns: .4fr .6fr;
}
</style>

<div class="columns">

<div>

Using the browser interface:

- Search for a gene/protein of interest and explore its neighbourhood interactively
- Visualise and export subnetworks directly from the browser

[skm.nib.si/pss](https://skm.nib.si/pss) OR [skm.nib.si/ckn](https://skm.nib.si/ckn)

For more targeted, repeatable, or programmatic interrogation, we turn to **skm-tools** next.

</div>

<div>

![w:500](images/pss-screenshot.png)
</div>

</div>

---

## Hands-on: Querying SKM yourself

<!-- _class: hands-on -->

We'll now query SKM directly using [**skm-tools**](https://github.com/NIB-SI/skm-tools), a Python toolkit for interrogating PSS and CKN.

**Notebook:** `[link to notebook on GitHub]`

**Option A — Google Colab** (no install, runs in browser)
`[link]`

**Option B — Local Python**
```python
pip install "skm-tools @ git+[https://github.com/NIB-SI/skm-tools.git](https://github.com/NIB-SI/skm-tools.git)"
```

Note: Visualisation of network analyses results in Cytoscape requires a local Cytoscape install and won't work in Colab.

---

## What we'll do in the notebook

<!-- _class: hands-on -->

<div class="columns">

<div>

1. Load a SKM graph as a NetworkX object
2. Filter to the interactions relevant to our biological question
3. Extract neighbourhoods around genes/components of interest
4. Find paths connecting two parts of the network
5. Visualise the resulting subnetwork

</div>

<div>

![w:180](images/placeholder-image.svg)
<!-- *Placeholder: left-to-right flow diagram of the 5 steps above* -->

</div>

</div>

---

## Worked Example: [placeholder]

<!-- _class: hands-on -->

Applying what we have learned to a specific biological question:

*(case study)*

*(Walkthrough of the notebook results here)*

---

## Where this fits in the bigger picture

<div class="columns">

<div>

- Knowledge graphs integrate and contextualise genes, pathways, and phenotypes within existing knowledge
- They're one route to making sense of large-scale data, including pangenome and variant data (*Session 4**)

### Beyond traversal
- **Graph algorithms** such as shortest paths, centrality, community detection, ... for identifying hubs or clusters
- **Knowledge graph embeddings** to learn a vector representation of every nodes and/or edges, for node prioritisation or link prediction
- **Graph neural networks (GNNs)**: combine graph structure with node features (e.g. expression data) for tasks like gene-function prediction

---

## Thank you

<div class="columns">

<div>

**Slides available at** [github.com/NIB-SI/ECCB2026_T12](https://github.com/NIB-SI/ECCB2026_T12)

</div>

<div>

**Contributors**
- Keywan Hassani-Pak

- Several slides on knowledge graph basics and ontologies adapted from BioCypher Workshop materials (ssciwr/slides-biocypher), CC-BY 4.0

</div>

**Questions welcome!**

</div>