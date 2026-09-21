# From Search Sets to Reusable Architectures

## A Provenance-Preserving Ontology for AI-Assisted Patent Intelligence in USPTO Patent Public Search

**Almighty Sonoxo**  
Independent Researcher  
Preprint / Methods and Technical Framework - Version 0.1.0 - 2026-09-21

> **Publication status:** Public preprint/technical paper. This version has not undergone external peer review.

## Abstract

Patent databases combine technical disclosures, structured bibliographic metadata, family and priority lineage, classification codes, citations, legal events, and long-form claims, but they are difficult to convert into reusable machine-actionable knowledge without losing provenance. This methods paper presents an ontology-first, provenance-preserving framework for AI-assisted patent intelligence built around the United States Patent and Trademark Office (USPTO) Patent Public Search (PPUBS) and scalable USPTO open-data sources. The framework separates search relevance from technical validation and models queries, L-set history, databases, patent families, document sections, evidence states, and abstracted architecture patterns as explicit entities. A staged pipeline is proposed: bounded discovery, provenance capture, normalization, family deduplication, section extraction, architecture abstraction, evidence-state labeling, independent verification, ontology registration, and monitoring. Three 2026 U.S. patent application publications are used as illustrative case studies: machine-learning clinical outcome prediction, distributed state orchestration, and virtual-metrology-based semiconductor process transfer. The third case yields a reusable pattern of domain knowledge plus designed experiments plus telemetry, feature construction, low-variance and collinearity filtering, cross-validated model comparison, feature attribution, minimal feature selection, bounded trials, source-to-target transfer, target-side verification, and re-selection when validation fails. The framework is designed to scale through the USPTO Open Data Portal and PatentsView rather than through indefinite interactive-interface scraping. It does not treat a patent publication or grant as independent scientific validation, and it does not claim that millions of search hits were individually analyzed. The contribution is a reproducible methodology and ontology for transforming patent-search sessions and bulk records into traceable technical knowledge that can support research, engineering design, and future empirical evaluation.

**Keywords:** patent intelligence; USPTO; Patent Public Search; ontology; provenance; knowledge graph; text mining; large language models; technology intelligence; virtual metrology

## 1. Introduction

Patent literature is simultaneously a technical, legal, bibliographic, and historical record. A single patent publication can contain a title, abstract, detailed description, claims, drawings, classifications, citations, inventors, applicants or assignees, priority relationships, and continuity data. At corpus scale, these records can support technology landscaping, prior-art retrieval, trend analysis, engineering design, competitive intelligence, and scientific-history studies. Prior work has established text mining as a major approach to patent analysis and has examined keyword processing, citation networks, topic clustering, knowledge graphs, and semantic retrieval [1-8]. More recently, transformer and large-language-model approaches have been used for patent retrieval, design knowledge extraction, and innovation analysis [6,9,10].

The central difficulty is not simply obtaining more text. The difficult part is retaining the chain of evidence that explains where a machine-generated concept came from, what kind of source asserted it, which family member or document section contained it, and whether the concept was independently validated. Without these distinctions, an AI system can collapse a keyword hit into relevance, an applicant statement into an established fact, or a patent grant into proof that a technology works. Those collapses are especially problematic when patents are used outside their original legal context, including in scientific or medical research.

This paper introduces a provenance-preserving ontology and workflow for AI-assisted learning from USPTO Patent Public Search (PPUBS) and related USPTO open-data products. The goal is not to claim that an AI system has manually 'read the whole patent library.' Instead, the goal is to define how a very large corpus can be ingested, normalized, queried, abstracted, verified, versioned, and audited. The framework treats broad interactive-search counts as scale signals and uses official bulk/API resources for genuine corpus-scale work.

The contributions are fourfold. First, we define a patent-search ontology that links query provenance to document, family, classification, section, and evidence-state objects. Second, we define an architecture-extraction layer that converts source-specific embodiments into generalized technical patterns while preserving the source. Third, we introduce a validation boundary that keeps patent assertions separate from independent replication, peer-reviewed evidence, regulatory evidence, and established fact. Fourth, we show through three illustrative 2026 patent applications how substantially different source domains can yield reusable computational patterns without claiming ownership or validation of the patented subject matter.

## 2. USPTO data source and search model

USPTO Patent Public Search provides Basic and Advanced interfaces for searching U.S. patents and published patent applications [11]. USPTO documents searchable field indexes for applicant, assignee, inventor, application, publication, title, abstract, claims, classification, priority, continuity, government-interest, and other fields [12]. The Advanced interface supports Boolean operators and proximity operators such as ADJ, NEAR, WITH, and SAME [13]. Search history is represented through L-number result sets that can be recombined into additional queries [14].

The official FAQ makes an important methodological distinction: Patent Public Search is based on query matching and supports relevance sorting using TF-IDF; it is not a semantic-search system [14]. This means that an ontology built from PPUBS must distinguish retrieval semantics from later semantic interpretation. A returned document is evidence that the search condition matched. It is not, by itself, evidence that the document is technically central to the user's question.

A supplied PPUBS 4.3.0 capture illustrates the scale problem. The broad term 'patent' returned 13,816,773 results across US-PGPUB, USPAT, and USOCR. Other raw keyword searches included 'cancer' at 825,436 results and 'moe' at 1,411,963 results. These counts motivate partitioned searching, classification filters, family deduplication, and bulk-data workflows. They are not the sample size for this paper and are not represented as records individually analyzed.

For corpus-scale work, interactive search is best treated as a discovery, debugging, and document-inspection surface. USPTO now directs open-data extraction to the Open Data Portal, and its research-dataset program identifies PatentsView as a research-grade resource with API, query, bulk-download, and visualization support [15]. The proposed framework therefore separates an interactive discovery plane from a bulk ingestion plane.

## 3. Ontology design

| Layer | Core objects | Purpose |
| --- | --- | --- |
| Search provenance | Query, L-set, databases, UI version, date, result count | Reproduce retrieval context |
| Patent identity | Document, application, family, kind code, priority, continuity | Keep lineage and versions distinct |
| Content | Title, abstract, description, claims, drawings, citations | Ground extraction in source sections |
| Evidence | Patent claim, preprint, peer review, replication, regulatory evidence | Prevent unsupported evidence promotion |
| Architecture | Pattern, components, assumptions, source, adoption status | Reuse ideas while preserving provenance |

The ontology uses five linked layers: search provenance, patent identity and lineage, document content, evidence state, and architecture abstraction. Search provenance stores the exact query, database selection, retrieval date, PPUBS version, result count, L-set identifier where available, and sort/filter state. Patent identity stores document number, kind code, application number, family identifier, filing and publication dates, inventors, applicants or assignees, classifications, priority claims, and continuity relationships. Document content is sectioned into title, abstract, background, summary, detailed description, claims, drawings, and citations.

Evidence state is explicit. A PATENT_QUERY_HIT means only that a retrieval condition matched. PATENT_APPLICATION_CLAIM and PATENT_GRANT_CLAIM represent assertions in patent records. PATENT_EXAMPLE_OR_EXPERIMENT identifies experimental or example material contained within a patent specification. Separate states represent preprints, peer-reviewed evidence, independent technical replication, clinical-trial evidence, regulatory evidence, established facts, and contradicted or retracted material. This model prevents automatic promotion from legal/technical disclosure to scientific validation.

Architecture abstraction is deliberately separate from source text. Each ArchitecturePattern stores a source document, source section references, generalized components, assumptions, dependencies, failure modes, adoption status, and a caution field. The abstraction process is intended to preserve functional ideas without copying proprietary expression. A source-derived pattern can be marked PATENT_DERIVED_ARCHITECTURE_INSPIRATION even when it is useful; this status does not imply license, ownership, freedom-to-operate, standards conformance, or independent technical validation.

## 4. Corpus-scale learning workflow

The end-to-end learning workflow is DISCOVER -> CAPTURE -> NORMALIZE -> DEDUPLICATE -> PROVENANCE -> EXTRACT -> VERIFY -> SYNTHESIZE -> REGISTER -> MONITOR. In interactive mode, DISCOVER uses bounded PPUBS queries, field indexes, classifications, dates, and L-set combinations. CAPTURE stores the complete retrieval context together with selected documents. NORMALIZE standardizes identifiers, names, dates, classification symbols, and continuity relationships. DEDUPLICATE separates document-level identity from family-level identity so multiple publications do not masquerade as independent inventions.

For bulk learning, the analogous pipeline is ACQUIRE -> HASH/SNAPSHOT -> PARSE -> NORMALIZE -> FAMILY-DEDUPE -> SECTIONIZE -> ENTITY/RELATION EXTRACT -> PATTERN ABSTRACT -> EVIDENCE LABEL -> VERIFY -> INDEX -> VERSION -> MONITOR. Each bulk snapshot should record source dataset, release date, date coverage, number of input records, number parsed successfully, exclusions, duplicate handling, schema version, and content hash. Broad corpora should be partitioned by publication or filing date, CPC/IPC/USPC classification, document kind, family, assignee/applicant, or technology domain.

The framework does not equate 'deep learning' with unbounded memorization. Model training, embeddings, topic models, or LLM-assisted extraction are optional downstream modules. They must operate on versioned source records, and their outputs must be traceable back to the document spans or metadata that support them. If a semantic model discovers a cluster or relationship, that output remains a model inference until confirmed against the source records and, where relevant, external evidence.

Evaluation is essential. A future full-corpus implementation should measure at least retrieval precision/recall on labeled queries, family deduplication accuracy, entity and relation extraction F1, section parsing completeness, citation-link accuracy, architecture-pattern inter-rater agreement, source-span traceability, hallucination rate, and evidence-state classification accuracy. Reporting only corpus size or search-hit counts would not establish system quality.

## 5. Illustrative architecture extractions

| Case | Source-specific domain | Generalized pattern |
| --- | --- | --- |
| US 20260279577 A1 | Clinical-outcome prediction | features -> KPI -> training -> normalized inference -> monitoring -> recalibration |
| US 20260277625 A1 | Multi-control-unit state management | request -> target resolution -> local validation/execution -> acknowledgements -> verified global state |
| US 20260277114 A1 | Plasma-process recipe transfer | candidate signals -> compact validated model -> bounded trial -> target transfer -> verify -> reselect if failure |

Three patent applications from the supplied PPUBS sessions illustrate how the ontology separates source-specific inventions from generalized patterns. These examples are not presented as independent validations of the patent applicants' claims. They are worked examples of the abstraction method.

Case 1 - Predictive outcome feedback loop. US 20260279577 A1 describes clinical outcome predictive analysis involving provider-feature extraction, an outcome/performance metric, training data linking, model training, prediction, normalization to an index, and ongoing monitoring [16]. The generalized computational pattern is SIGNAL EXTRACTION -> GROUND-TRUTH/KPI DEFINITION -> FEATURE-LABEL LINKAGE -> VALIDATED MODEL FIT -> INFERENCE -> NORMALIZED INDEX -> POST-DEPLOYMENT MONITORING -> MEASURED RECALIBRATION. In the ontology, the clinical claims remain patent assertions unless independently verified; the reusable contribution is the feedback-loop architecture.

Case 2 - Distributed state orchestration. US 20260277625 A1 describes management of function-group state transitions across multiple control units using a general configuration, a state-management center, node-level state management, local execution management, and reliability-aware behavior [17]. The generalized pattern is REQUEST -> AUTHORIZE -> RESOLVE TARGET NODES -> FAN OUT -> LOCAL VALIDATE -> EXECUTE BOUNDED TRANSITION -> ACKNOWLEDGE -> AGGREGATE -> VERIFY OBSERVED STATE -> LOG -> UPDATE SHARED STATE. This abstraction emphasizes that global state should be derived from acknowledgements and observed state rather than assumed from command delivery.

Case 3 - Minimal-feature transfer and validation. US 20260277114 A1 describes semiconductor plasma-process development in which candidate predictors can be derived from domain knowledge, designed experiments, and manufacturing data; errors and low-variance variables are removed; interaction terms and transformations can be added; collinearity is reduced; compact regressions are cross-validated; multiple virtual-metrology models can be compared by prediction error; feature importance can be calculated with SHAP; and selected plasma parameters guide transfer from coupon-scale development to whole-wafer recipes [18]. If the whole-wafer result does not satisfy the required specification, feature selection can be re-executed [18].

The generalized pattern from Case 3 is DOMAIN KNOWLEDGE + DOE + TELEMETRY -> FEATURE CONSTRUCTION -> FILTER AND DECOLLINEARIZE -> CROSS-VALIDATED COMPACT PREDICTOR SET -> MULTIPLE MODEL COMPARISON -> FEATURE ATTRIBUTION -> MINIMAL SUFFICIENT FEATURE SET -> BOUNDED SMALL-SCALE TRIAL -> CONTROL-MODEL TRANSFER -> TARGET-SIDE SPECIFICATION CHECK -> RESELECT/RETRAIN IF FAILURE. The important design principle is not a particular plasma variable or wafer recipe. It is the disciplined cycle of reducing a candidate signal space, validating compact models, transferring from a bounded environment to a target environment, and returning to selection when transfer fails.

## 6. Relationship to prior patent-intelligence research

The framework extends rather than replaces established patent-analysis methods. Text-mining research has long addressed the difficulty of extracting useful information from patent documents [1]. Reviews of the field describe patent analysis as a broad combination of bibliometrics, text processing, visualization, and strategic interpretation [2]. Keyword-selection work shows that preprocessing choices can materially affect patent-mining results [3]. Application-specific studies have combined text mining with citation networks and clustering to identify technological hotspots and trends [4].

Knowledge graphs provide a natural representation for the provenance requirements described here. Patent-KG work has shown how knowledge facts can be extracted from patents for engineering design [5], while newer engineering research continues to explore patent-derived graph structures. Semantic retrieval has also advanced: SEARCHFORMER applies transformer-based embeddings to prior-art search [6]. The PatentSemTech workshop series demonstrates sustained research interest at the intersection of patent retrieval, text mining, machine learning, and semantic technologies [7,8].

Large language models add new capabilities but also increase the importance of provenance. Recent work has used LLMs to create design catalogs from patent documents [9] and to support bibliometric analysis linking patents and publications in medicine [10]. The ontology proposed here is intentionally model-agnostic: a transformer, retrieval model, classical classifier, or LLM may perform extraction or ranking, but every output should preserve the source document, section, retrieval context, model/version information, and evidence state.

## 7. Validation boundaries and research integrity

| Evidence state | Meaning | Permitted conclusion |
| --- | --- | --- |
| PATENT_QUERY_HIT | Document matched retrieval syntax | Relevant for screening only |
| PATENT_APPLICATION_CLAIM | Applicant disclosure in published application | Attributed technical/legal claim |
| PATENT_GRANT_CLAIM | Claim/disclosure in granted patent | Granted patent record, not independent validation |
| PREPRINT | Public manuscript without completed peer review | Preliminary scholarly evidence |
| PEER_REVIEWED_EVIDENCE | Published peer-reviewed study | Scholarly evidence within study limits |
| INDEPENDENT_TECHNICAL_REPLICATION | Independent reproduction/test | Independent technical support |
| REGULATORY_EVIDENCE | Regulator decision/record | Regulatory status for specified context |

A patent document is not a peer-reviewed scientific article. Patent systems evaluate legal requirements for patentability and disclosure; they do not certify that every experimental, engineering, or medical assertion has been independently replicated. Accordingly, this framework never upgrades a patent assertion to established fact merely because an application was published or a patent was granted.

The same principle applies to AI explanations. Feature attribution, including SHAP, can help identify which variables influence a model, but explainability is not target validation. A model that performs on a source domain or small-scale trial must still be tested in the target environment. The virtual-metrology case study makes this boundary concrete: a transfer recipe is tested on a new whole wafer, and failure returns the workflow to feature selection rather than forcing deployment [18].

For high-stakes domains, external verification layers are mandatory. Medical claims should be checked against peer-reviewed biomedical literature, clinical-trial registries, regulatory decisions, and current clinical guidance as appropriate. Safety-critical engineering claims should be checked against applicable standards, independent testing, and authorized engineering review. The ontology records these as separate evidence objects rather than silently merging them with patent provenance.

## 8. Reproducibility and publication workflow

A reproducible patent-intelligence study should publish more than narrative conclusions. The recommended research package includes the ontology schema and version, exact queries, date windows, database selections, field aliases, inclusion/exclusion rules, family-deduplication method, document identifiers, code or pseudocode for parsing and extraction, model names and versions, prompts when LLMs are used, evaluation sets, metrics, error analysis, and a machine-readable reference list.

The present paper is released as a preprint/technical methods paper together with ontology version 1.2.0 and a BibTeX reference file. It should not be described as peer reviewed until an independent scholarly review process is completed. A later journal submission should preserve the version history and disclose AI assistance according to the receiving venue's policy.

For future corpus-scale work, the next empirical milestone is a benchmark dataset. A stratified sample across several CPC sections could be independently annotated for relevance, entity relations, family structure, and architecture patterns. Human annotations would then support quantitative comparison among rule-based extraction, embedding retrieval, conventional supervised models, and LLM-assisted extraction.

## 9. Limitations

This paper is a framework and methods contribution, not a completed full-corpus experiment. The supplied interactive captures expose large result sets, but the millions of returned records were not individually downloaded, parsed, or manually reviewed in this study. The case studies are illustrative and intentionally small. Consequently, this work does not report corpus-level precision, recall, extraction F1, latency, cost, or robustness metrics.

Patent-language generalization also introduces interpretation risk. Abstracting a source-specific embodiment into a reusable pattern necessarily removes domain detail. The provenance layer reduces this risk but cannot eliminate the need for expert review. Patent families, continuations, amended claims, assignments, and legal status can change over time, so a production system must refresh and version these relationships.

Finally, patent corpora are not a complete record of innovation. Some inventions are kept as trade secrets, some research is never patented, patenting behavior varies across sectors and jurisdictions, and PPUBS focuses on U.S. patent records rather than foreign patent databases [14]. Patent intelligence should therefore be combined with scientific literature, standards, regulatory records, products, and other evidence when the research question requires them.

## 10. Conclusion

AI can make a massive patent corpus more useful only if scale is paired with provenance. The ontology-first framework presented here keeps search behavior, family lineage, document content, evidence state, and generalized architecture patterns distinct but connected. That structure allows an AI system to learn from patents without pretending that keyword matches are relevance, that patents are scientific validation, or that an interactive search result count is a completed corpus analysis.

The three case studies demonstrate the intended abstraction process. A clinical prediction disclosure becomes a measured feedback-loop pattern; a multi-control-unit state-management disclosure becomes an acknowledgement-based orchestration pattern; and a virtual-metrology semiconductor disclosure becomes a minimal-feature transfer-and-validation pattern. Each remains traceable to its source. The next step is empirical validation on a bulk USPTO dataset with independently annotated benchmarks, quantified error rates, and public versioned artifacts.

## Data and artifact availability

The manuscript is accompanied by GPT-DOUG USPTO Patent Public Search Ontology v1.2.0 in JSON and Markdown form, plus a machine-readable BibTeX bibliography. The ontology records supplied PPUBS capture provenance and explicitly labels patent-derived architecture patterns as unverified inspiration until independently validated.

## Ethics statement

This methods paper analyzes public patent and scholarly records and does not report experiments involving human participants or animals.

## Funding and conflicts of interest

No external funding is reported for this version. No conflict of interest is declared in this version.

## AI assistance disclosure

OpenAI GPT-5.6 Sol was used as a research and drafting assistant for source organization, literature retrieval, synthesis, ontology/file generation, and manuscript preparation under the author's direction. Patent claims were kept distinct from independent scientific evidence. The human author remains responsible for publication decisions and for verifying the final manuscript before scholarly submission.

## References

1. Tseng Y-H, Lin C-J, Lin Y-I. Text mining techniques for patent analysis. Information Processing & Management. 2007;43(5):1216-1247. doi:10.1016/j.ipm.2006.11.011.

2. Abbas A, Zhang L, Khan SU. A literature review on the state-of-the-art in patent analysis. World Patent Information. 2014;37:3-13. doi:10.1016/j.wpi.2013.12.006.

3. Noh H, Jo Y, Lee S. Keyword selection and processing strategy for applying text mining to patent analysis. Expert Systems with Applications. 2015;42(9):4348-4360. doi:10.1016/j.eswa.2015.01.050.

4. Pan X, Zhong B, Wang X, Xiang R. Text mining-based patent analysis of BIM application in construction. Journal of Civil Engineering and Management. 2021;27(5):303-315. doi:10.3846/jcem.2021.14907.

5. Zuo H, Yin Y, Childs P. Patent-KG: Patent Knowledge Graph Extraction for Engineering Design. Proceedings of the Design Society. 2022;2:821-830. doi:10.1017/pds.2022.84.

6. Vowinckel K, Hahnke V. SEARCHFORMER: Semantic patent embeddings by siamese transformers for prior art search. World Patent Information. 2023;73:102192. doi:10.1016/j.wpi.2023.102192.

7. Krestel R, Aras H, Andersson L, et al. 4th Workshop on Patent Text Mining and Semantic Technologies (PatentSemTech2023). 2023. doi:10.1145/3539618.3591929.

8. Krestel R, Aras H, Andersson L, et al. 5th Workshop on Patent Text Mining and Semantic Technologies (PatentSemTech2024). 2024. doi:10.1145/3626772.3657986.

9. Nomaguchi Y, Kuroishi K, Abd Syamimi ASB, et al. Creating design catalog from patent documents with large language model for design concept generation. Proceedings of the Design Society. 2025;5:1051-1060. doi:10.1017/pds.2025.10119.

10. Marcus A, Lockwood-Taylor G, Rueckert D, Bentley P. Quantifying Innovation in Stroke: Large Language Model Bibliometric Analysis. Journal of Medical Internet Research. 2026;28:e70754. doi:10.2196/70754.

11. United States Patent and Trademark Office. Patent Public Search. https://www.uspto.gov/patents/search/patent-public-search. Accessed 2026-09-21.

12. United States Patent and Trademark Office. Patent Public Search: Searchable indexes. https://www.uspto.gov/patents/search/patent-public-search/searchable-indexes. Accessed 2026-09-21.

13. United States Patent and Trademark Office. Patent Public Search: Operators. https://www.uspto.gov/patents/search/patent-public-search/operators. Accessed 2026-09-21.

14. United States Patent and Trademark Office. Patent Public Search FAQs. https://www.uspto.gov/patents/search/patent-public-search/faqs. Accessed 2026-09-21.

15. United States Patent and Trademark Office. Research datasets / PatentsView. https://www.uspto.gov/ip-policy/economic-research/research-datasets. Accessed 2026-09-21.

16. U.S. Patent Application Publication US 20260279577 A1. Technologies for Machine Learning Cloud Based Clinical Outcome Prediction. Published 2026-09-17.

17. U.S. Patent Application Publication US 20260277625 A1. State Management Method, Configuration File Generation Method, and Device. Published 2026-09-17.

18. U.S. Patent Application Publication US 20260277114 A1. Recipe Transfer of Plasma Process Using Plasma Parameters Determined by Virtual Metrology (VM) Feature Selection. Published 2026-09-17.
