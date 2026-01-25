# Area-Based Retrofit Potential in Camden

## Context
The UK’s Climate Change Act initially set a legally binding target to reduce greenhouse gas emissions by at least 80% by 2050, which has since been strengthened to a net zero target for 2050. *(Sources: [UK CCA (historical target)][UK_CCA80], [UK CCC - Net Zero][UK_CCC_NETZERO]).*

Across the country - and especially in London - there is a substantial stock of older, energy-inefficient housing. While a range of support mechanisms exist (including grants and supplier obligations such as the Energy Company Obligation), the pace of domestic retrofit remains well below what is required to meet long-term climate goals. *(Sources: [UK Parliament - Retrofitting homes for net zero][UKP_RETROFIT], [Ofgem - ECO][OFGEM_ECO]).*

To address this, some local areas have adopted place-based or programme-led approaches to retrofit, coordinating upgrades across groups of homes. This has been explored in the literature through area-based whole-house retrofit mapping and has been reflected in borough retrofit strategies (e.g., Hammersmith & Fulham and Lewisham). *(Sources: [Gupta & Gregg, 2020][GUPTA_GREGG_2020], [H&F strategy][HF_RETROFIT_STRATEGY], [Lewisham strategy][LEWISHAM_RETROFIT_STRATEGY]).*

In this project, I identify candidate area-based retrofit opportunities in Camden by first selecting low-performing homes (EPC Band D or below) and clustering them by spatial proximity to delineate compact “delivery areas”. I then profile each cluster using fabric and energy indicators to determine the dominant retrofit need (i.e., the measure affecting ≥50% of homes in the cluster) and to highlight where clusters are mixed and may require further segmentation. This workflow helps reveal where groups of inefficient homes share similar upgrade priorities - supporting more targeted engagement and potentially enabling neighbourhood- or programme-led delivery that can reduce costs and disruption through coordinated planning and procurement. Ultimately, scaling such targeted retrofit delivery can help lower household energy bills and contribute to reducing carbon emissions. *(Sources: [UK Parliament - Retrofitting homes for net zero][UKP_RETROFIT], [IET - Scaling Up Retrofit 2050][IET_RETROFIT_2050]).*

## Study Area: Camden (Rationale)
Camden is selected as the study area for several reasons:
- It has a large stock of older homes (e.g., more than half of flats were built over 100 years ago), which is often associated with higher retrofit need. *(Source: [Camden CAP summary][CAMDEN_CAP_SUMMARY]).*
- It faces fuel poverty challenges (government statistics estimate 13.7% of households were in fuel poverty in 2019), making retrofit a social as well as an environmental priority. *(Source: [Camden fuel poverty stats][CAMDEN_FUEL_POVERTY_STATS]).*
- The borough has committed to becoming net zero carbon by 2030 and is implementing retrofit works to improve the energy efficiency of council homes. *(Sources: [Camden CAP summary][CAMDEN_CAP_SUMMARY], [Camden Retrofit][CAMDEN_RETROFIT]).*

## Method

> 🔴 **TODO / Notes**
> - Definitions of the main concepts
> - Overview of workflow
> - Where to download **LBSM2**, **EPC**, and **conservation areas** datasets? (next step)
> - How should the data be preprocessed? (about EPC)
> - Clustering methodology (and how do we select the parameters?) + Feature set (again?)

---

### Defining retrofit-need (candidate buildings)

In this project, retrofit need is operationalised as an EPC-based screening step that defines the candidate set for subsequent spatial clustering. Additional variables are used to profile candidate clusters and to prioritise interventions, rather than to define technical similarity directly.

**Primary screening criterion (EPC).** A dwelling is included in the retrofit candidate set if its current EPC rating is Band D or below. This aligns with widely cited UK retrofit pathways that prioritise upgrading the lowest-performing stock first and aim to raise homes towards at least Band C. *(Sources: [UKGBC - Home Retrofit][UKGBC_HOME_RETROFIT], [UK Parliament - Retrofitting homes for net zero][UKP_RETROFIT]).*

**Profiling indicators (retrofit potential and technical drivers).** EPC alone is not a complete descriptor of retrofit need, and current EPC metrics have recognised limitations for net-zero delivery. Candidate buildings and resulting spatial clusters are therefore profiled using:
  - **EPC “efficiency gap”** (potential minus current EPC score, where available) to indicate unrealised improvement potential. *(Source: [CCC-EPC metrics reform annex][UK_CCC_NETZERO]).*
  - **Fabric and heating descriptors** to reflect that effective retrofit packages typically combine envelope measures with heating upgrades. Variables used:
    - Fabric: `wall_type`, `wall_insulation`, `roof_type`, `roof_insulation`, `glazing_type`
    - Heating: `main_heat_type`, `main_fuel_type`
  *(Sources: [CCC-EPC metrics reform annex][UK_CCC_NETZERO], [Gupta & Gregg, 2020][GUPTA_GREGG_2020], [UK Parliament - Retrofitting homes for net zero][UKP_RETROFIT]).*

**Equity lens (prioritisation overlay).** Socio-economic vulnerability (e.g., deprivation or fuel poverty proxies) is applied as a prioritisation overlay to support fair targeting of limited resources and to reflect the role of energy efficiency improvements in reducing heating costs for fuel-poor households. *(Source: [Fuel Poverty Strategy-Technical annex][FP_TECH_ANNEX]).*

### Handling conservation areas

In England, a conservation area is defined as “an area of special architectural or historic interest the character or appearance of which it is desirable to preserve or enhance.” *(Source: [PLBCA Act 1990, s.69][PLBCA_S69]).*

In this study, properties located within conservation areas are excluded from the main clustering workflow. This scope decision is motivated by the fact that conservation-area designation typically introduces additional planning constraints on external alterations, which can materially change both the set of feasible retrofit measures and the associated delivery costs. Empirical evidence suggests that conservation-area regulations act as a barrier to energy-efficiency upgrades and are associated with lower retrofit investment. *(Source: [Fetzer, 2023][FETZER_2023]).* Complementary research on residential heritage retrofit further highlights that “standard” retrofit packages-especially those affecting the building exterior-may be unacceptable or infeasible in protected contexts, implying the need for a distinct pathway and measure set. *(Source: [Wise et al., 2021][WISE_2021]).*

The overarching aim of this project is to identify groups of homes that could plausibly receive a similar retrofit package at a comparable cost, improving attractiveness for delivery partners and potentially reducing costs for residents through coordinated implementation. Because buildings inside and outside conservation areas are likely to require different measure packages, permissions, and cost structures, they are treated separately in the present analysis. A natural extension of this work would be to model retrofit opportunities within conservation areas explicitly, using a constraint-aware approach (e.g., conservation-area-specific measure sets and planning pathways).

### Accounting for existing retrofit interventions (limitation and future work)

Some homes in Camden have already undergone retrofit, or are currently part of active retrofit programmes. Evidence of completed or planned interventions can be derived from (i) official borough documentation and (ii) EPC histories, although both sources have important limitations.

First, Camden publishes programme-level evidence through council reports and procurement documentation. For example, the borough’s Climate Action Plan annual reviews and climate budget materials describe estate-scale and street-property retrofit projects (e.g., 5–7 Belsize Grove, Brookes Court, and batches of street properties). In addition, procurement notices provide site-level details (including the number of dwellings and indicative measures) for specific retrofit schemes. *(Sources: [Camden CAP Annual Review 2022–23][CAMDEN_CAP_REVIEW_22_23], [Camden Climate Budget 2024–25][CAMDEN_CLIMATE_BUDGET_24_25], [Contracts Finder-Belsize Grove retrofit works][CAMDEN_CONTRACTS_BELSIZE_GROVE]).*

Second, previously retrofitted homes can sometimes be inferred from changes in EPC records over time (e.g., a substantial uplift in EPC score or changes in recorded fabric/heating descriptors). However, EPC presence and timing are not a reliable indicator of retrofit completion: EPCs are typically required when a dwelling is sold or let (and are valid for up to 10 years), so a “first” EPC may appear for reasons unrelated to retrofit. Moreover, research on EPC renewals shows that EPC ratings can change due to assessor practices, data-quality issues, or methodological factors, and therefore improvements in ratings do not always imply physical upgrades. *(Sources: [UK Gov-EPC technical notes][UK_EPC_TECH_NOTES], [UK Gov-EPC technical note (PDF)][UK_EPC_TECH_NOTE_PDF], [Yuan, 2023-EPC renewal analysis][YUAN_2023_EPC_RENEWAL]).*

In policy-oriented applications, these signals could be integrated into targeting and scheduling logic-for example, excluding properties that are confirmed to be in current retrofit programmes, or explicitly incorporating already planned measures alongside physical need (e.g., EPC-based screening) and equity overlays (e.g., deprivation/fuel poverty proxies). In the present study, however, prior or ongoing retrofit is not modelled explicitly when defining priority groups; this is treated as a limitation and a clear direction for further work.

<!-- ### Constraints and prioritisation (policy overlays)

**Planning constraints (conservation areas).** Retrofit feasibility varies across Camden: properties located within conservation areas may face additional planning constraints and may require a different retrofit pathway. These properties are therefore analysed separately using `conservation_area_flag`.

**Retrofit urgency (low-priority cases).** Some homes are unlikely to require urgent fabric retrofit (e.g., high EPC bands A–B). These are treated as lower-priority in the interpretation layer rather than driving “similarity” in clustering.

**Equity lens (IMD).** Socio-economic indicators (e.g., `imd19_income_decile`) are used for prioritisation and targeting (e.g., outreach or subsidy focus) rather than defining technical similarity between buildings. -->

## Data

### (1) LBSMv2

**Description.** The London Building Stock Model v2 (LBSM2) is a building-level dataset describing key characteristics of London’s domestic housing stock, designed to support housing improvement and retrofit programmes. It provides broad coverage across London by combining observed information (where available) with modelled values to fill gaps and resolve inconsistencies. This makes LBSM2 well-suited for exploratory analysis and neighbourhood-level targeting; however, results should be treated as indicative when used for policy decisions and should be complemented by on-site surveys for detailed retrofit design.  
*(Sources: [GLA Algorithmic Transparency Record][GLA_LBSM2], [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

#### Coordinate Reference System (CRS)

Building locations are provided as Easting/Northing in metres in OSGB36 / British National Grid (EPSG:27700).  
- **Datum:** OSGB36 (Ordnance Survey of Great Britain 1936)  
- **Projected CRS:** British National Grid (EPSG:27700)  
*(Sources: [LBSM2 Data Dictionary][LBSM2_DATA_DICTIONARY], [London Datastore dataset page][LDS_LBSM2_DATA]).*

CRS Suitability for Distance-Based Clustering: 
- It is metre-based, so distance thresholds for clustering are directly interpretable (e.g., `eps = 100` ≈ 100 m).
- British National Grid is a national projected CRS designed for Great Britain, so local-scale distortion is small for analyses within London.

#### Dataset advantages

- **Authoritative producer and expert partnerships.** LBSM2 is produced by the Greater London Authority as part of London-wide energy and housing policy support, and its development draws on specialist expertise (including UCL partners).  
  *(Sources: [GLA Algorithmic Transparency Record][GLA_LBSM2], [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

- **Fitness for purpose.** The dataset is explicitly framed as supporting retrofit programme design and delivery (e.g., identifying candidate areas and groups of similar homes).  
  *(Source: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

- **Recency / historical currency.** LBSM2 represents a snapshot as of October 2024 and supersedes the earlier LBSM version.  
  *(Source: [London Datastore - LBSM2 dataset page][LDS_LBSM2_DATA]).*

- **Reported model performance.** The Algorithmic Transparency Record reports an overall output accuracy of 87% (noting that performance varies by variable and is reported per attribute in the accompanying outputs).  
  *(Source: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

- **Authoritative composite inputs** LBSM2 integrates multiple sources into a consistent property-level model, with UPRN used as a key linkage identifier. Inputs referenced include EPC, Ordnance Survey address/building context, Census 2021 constraints, Land Registry data, and building-age information from sources such as Colouring London.  
  *(Sources: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC], [GLA Algorithmic Transparency Record][GLA_LBSM2]).*

- **Broad attribute coverage.** LBSM2 provides a rich set of building and energy attributes suitable for area-based analysis (e.g., EPC-related indicators, building form/size, heating/fuel, fabric characteristics, and related contextual layers).  
  *(Source: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

- **Improved completeness through modelling.** Because underlying sources are incomplete at property level, LBSM2 increases coverage by combining observed records with modelled values (to fill missing fields and reconcile inconsistencies). The modelling is based on a decision-tree boosting approach (LightGBM), used for both regression (numeric targets) and classification (categorical targets).
  *(Sources: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC], [GLA Algorithmic Transparency Record][GLA_LBSM2]).*

  **Examples of modelled variables** (as listed by the GLA):
  - Property type & built form  
  - Floor area & number of habitable rooms  
  - Property tenure  
  - Primary fuel type & heating system  
  - Wall type & insulation  
  - Roof type & insulation  
  - Construction age  
  - Glazing type  
  - Energy consumption  
  - Current & potential EPC rating  
  *(Source: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

- **Preprocessing and harmonisation.** Where multiple records exist for the same property (e.g., multiple EPC assessments), values are harmonised using rules such as selecting the most recent record, the most frequent category, or an average-depending on the attribute. Some missing quantities (e.g., energy consumption) are estimated using alternative information such as fuel bill data.  
  *(Sources: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC], [GLA Algorithmic Transparency Record][GLA_LBSM2]).*

- **External expert review.** Model outputs have been independently reviewed by Buro Happold at an area level, focusing on attributes most relevant to likely uses, with the conclusion that the model performed well.  
  *(Source: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

#### Data limitations (important for policy use)

- **Design-level limitations (EPC granularity + model uncertainty).** LBSM2 is well-suited for screening and area-level targeting; however, individual properties still require on-site surveys for detailed retrofit design. This is because EPC records alone do not contain sufficient installation-level detail, and modelled EPC values-while reported as high-accuracy (80–90%) - are not 100% accurate.  
  *(Sources: [GLA Algorithmic Transparency Record][GLA_LBSM2], [London Datastore - LBSM2 overview][LDS_LBSM2_DESC]).*

- **Temporal currency (snapshot date).** LBSM2 represents a snapshot of London’s housing stock as of October 2024; therefore, EPC-related attributes may not capture updates recorded after that date. For analyses conducted in 2026, EPC-linked fields should ideally be refreshed using the latest EPC register extracts (where feasible), or this should be clearly stated as a limitation.  
  *(Sources: [London Datastore - LBSM2 overview][LDS_LBSM2_DESC], [London Datastore - LBSM2 dataset page][LDS_LBSM2_DATA]).*

---

> 🔴 **TODO (Data sources sections to write)**
> - **(2) EPC:** Add a short note explaining that EPC data is used to obtain the **most up-to-date** information where available (to supplement the LBSM2 snapshot).
> - **(3) Camden Open Data:** Explain that these datasets are used to obtain **geometry layers and context for mapping/visualisation** (e.g., conservation areas, boundaries, planning constraints).
> - **(4) OS data for basemap:** Specify **which Ordnance Survey layers** are used for the basemap (e.g., background tiles, topographic features) and why.

---

<!-- Link references - check the articles -->
[UKGBC_HOME_RETROFIT]: https://ukgbc.org/our-work/home-retrofit/
[UKP_RETROFIT]: https://publications.parliament.uk/pa/cm5901/cmselect/cmesnz/453/report.html
[FP_TECH_ANNEX]: https://www.gov.uk/government/publications/fuel-poverty-strategy-for-england/fuel-poverty-strategy-for-england-technical-annex

[UK_CCA80]: https://www.gov.uk/guidance/climate-change 
[UK_CCC_NETZERO]: https://www.theccc.org.uk/climate-action/ 
[OFGEM_ECO]: https://www.ofgem.gov.uk/environmental-and-social-schemes/energy-company-obligation-eco 
[GUPTA_GREGG_2020]: https://radar.brookes.ac.uk/radar/file/7c6a1d16-757d-491b-909f-52cf8b0d5455/1/Domestic%20energy%20mapping%20for%20retrofit%20-%202020%20-%20Gupta%20Gregg.pdf 
[HF_RETROFIT_STRATEGY]: https://democracy.lbhf.gov.uk/documents/s131593/Appendix%201%20-%20HF%20Council%20Housing%20Retrofit%20Strategy.pdf 
[LEWISHAM_RETROFIT_STRATEGY]: https://lewisham.gov.uk/-/media/services/housing/housing-retrofit-strategy-empowering-communities-through-retrofitting.pdf 
[IET_RETROFIT_2050]: https://www.theiet.org/media/8758/retrofit.pdf 
[CAMDEN_CAP_SUMMARY]: https://www.camden.gov.uk/documents/20142/344816220/Climate%2BAction%2BPlan%2BCondensed%2BSummary_A5.pdf 
[CAMDEN_FUEL_POVERTY_STATS]: https://news.camden.gov.uk/keeping-warm-and-well-this-winter-in-camden/ 
[CAMDEN_RETROFIT]: https://www.camden.gov.uk/retrofit 
[GLA_LBSM2]: https://www.gov.uk/algorithmic-transparency-records/greater-london-authority-london-building-stock-model-2

[PLBCA_S69]: https://www.legislation.gov.uk/ukpga/1990/9/section/69
[FETZER_2023]: https://wrap.warwick.ac.uk/id/eprint/173618/1/WRAP-regulatory-barriers-climate-action-evidence-from-conservation-areas-England-Fetzer-2023.pdf
[WISE_2021]: https://journal-buildingscities.org/articles/10.5334/bc.94 

[CAMDEN_CAP_REVIEW_22_23]: https://camden.moderngov.co.uk/documents/s116400/Appendix%201%20-%20Climate%20Action%20Plan%20Annual%20Review%2022-23.pdf
[CAMDEN_CLIMATE_BUDGET_24_25]: https://www.camden.gov.uk/documents/d/guest/camden-climate-budget_24_25
[CAMDEN_CONTRACTS_BELSIZE_GROVE]: https://www.contractsfinder.service.gov.uk/notice/ee1174ea-a932-4bb4-b07c-f3a59271890d
[UK_EPC_TECH_NOTES]: https://www.gov.uk/government/publications/energy-performance-of-buildings-certificates-in-england-and-wales-technical-notes/energy-performance-of-buildings-certificates-in-england-and-wales-technical-notes
[UK_EPC_TECH_NOTE_PDF]: https://assets.publishing.service.gov.uk/media/60acc26d8fa8f520c8946a23/Energy_Performance_of_Buildings_Certificates_technical_note.pdf
[YUAN_2023_EPC_RENEWAL]: https://www.sciencedirect.com/science/article/pii/S0301421523001660

[LDS_LBSM2_DESC]: https://data.london.gov.uk/blog/london-building-stock-model-2
[LDS_LBSM2_DATA]: https://data.london.gov.uk/dataset/london-building-stock-model-2-lbsm-2-2k55d
[LBSM2_DATA_DICTIONARY]: https://data.london.gov.uk/download/2k55d/826853d0-0c2e-4550-b297-feafdd7e2afc/LBSMv2%20-%20Data%20Dictionary.xlsx