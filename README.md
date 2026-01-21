## Area-Based Retrofit Potential in Camden

### Context
### Context
The UK’s Climate Change Act initially set a legally binding target to reduce greenhouse gas emissions by at least 80% by 2050, which has since been strengthened to a net zero target for 2050. *(Sources: [UK CCA (historical target)][UK_CCA80], [UK CCC — Net Zero][UK_CCC_NETZERO]).*

Across the country—and especially in London—there is a substantial stock of older, energy-inefficient housing. While a range of support mechanisms exist (including grants and supplier obligations such as the Energy Company Obligation), the pace of domestic retrofit remains well below what is required to meet long-term climate goals. *(Sources: [UK Parliament report][PARLIAMENT_RETROFIT], [Ofgem — ECO][OFGEM_ECO]).*

To address this, some local areas have adopted place-based or programme-led approaches to retrofit, coordinating upgrades across groups of homes. This has been explored in the literature through area-based whole-house retrofit mapping and has been reflected in borough retrofit strategies (e.g., Hammersmith & Fulham and Lewisham). *(Sources: [Gupta & Gregg, 2020][GUPTA_GREGG_2020], [H&F strategy][HF_RETROFIT_STRATEGY], [Lewisham strategy][LEWISHAM_RETROFIT_STRATEGY]).*

In this research, I investigate the potential for area-based retrofit in Camden by clustering nearby buildings using energy- and fabric-related characteristics to highlight candidate groups for coordinated upgrades. Such an approach could support targeted retrofit delivery, helping to reduce carbon emissions while also lowering households’ energy bills. *(Sources: [UK Parliament report][PARLIAMENT_RETROFIT], [IET — Scaling Up Retrofit 2050][IET_RETROFIT_2050]).*

### Study Area: Camden (Rationale)
Camden is selected as the study area for several reasons:
- It has a large stock of older homes (e.g., more than half of flats were built over 100 years ago), which is often associated with higher retrofit need. *(Source: [Camden CAP summary][CAMDEN_CAP_SUMMARY]).*
- It faces fuel poverty challenges (government statistics estimate 13.7% of households were in fuel poverty in 2019), making retrofit a social as well as an environmental priority. *(Source: [Camden fuel poverty stats][CAMDEN_FUEL_POVERTY_STATS]).*
- The borough has committed to becoming net zero carbon by 2030 and is implementing retrofit works to improve the energy efficiency of council homes. *(Sources: [Camden CAP summary][CAMDEN_CAP_SUMMARY], [Camden Retrofit][CAMDEN_RETROFIT]).*

### Methodology 

А ОТКУДА ВОЗЬМЕМ ФАКТ ТОГО ЧТО РЕТРОФИТ УЖЕ БЫЛ? ОБЯЗАТЕЛЬНО ЭТО УЧЕСТЬ В КАМДЕНЕ

### Data
#### LBSMv2
Описание: Данные о ключевых характеристиках зданий жилого назначения в Лондоне (source: [Greater London Authority: LBSMv2]). Они содеражат рассматриваемый район, включают в себя разлчиные необходимые для кластеризации признаки и обладают полнотой, так как используются как оригинальные так и смоделированные данные. Тем не менее, резальтаты аналитики необходимо проверить натурно в случае использования работы для policy, так как в ~10% синтетических данных по энергоэффективности зданий содержатся ошибки. 

Coordinate Reference System (CRS) (source: [London Datastore: LBSMv2 data dictionary]): 
    - Datum (геодезическая основа): OSGB36 (Ordnance Survey of Great Britain 1936)
    - Projection: British National Grid (EPSG:27700)

CRS хорошо подходит под задачу, и не требует перепроецирования, так как:
    - Проекция уже метрическая, что удобно, так как позволяет задавать легко интерпретируемые параметры для кластеризации
    - British National Grid спроектирована специально для GB, поэтому искажения масштаба небольшие 
    - Лондон находится в центре области, для которой сделана проекция, поэтому 

Преимущества датасета: 
- Авторитетность и экспертиза создателей датасета. Данные созданы государственной организацией Greater London Authority в партнерстве с UCL Energy Institute and the Centre for Advanced Spatial Analysis. 
- Применимость в контексте задачи. Датасет создан специально для "housing improvement programmes", и для retrofit purposes, в частности (следует из описания)
- Историческая актуальность. Данные были обновлены до второй версии в октябре 2024 года после первой версии от 2017 года
- Авторитетность композитных источников. Данные были созданы на основании ряда авторитетных источников и объединены по UPRN (unique property address):
    - Energy Performance Certificates (EPC) for new buildings, when properties are sold or when rented properties are let.
    - Ordnance Survey data to infer building type (semi-detached, terrace, blocks of flats, etc) - along with identifying nearest neighbours for each building
    - Census data 2021 for local constraints for modelled data
    - Land Registry data for type of organisation that owns the land for each property 
    - Colouring London is data about building age и др. 
- Широта вопросов, которые покрыты в датасете: 
    - EPC
    - Building outlines
    - Addresses within buildings
    - Main heating system
    - Roof type и др. 
- Полнота данных. Текущие источники покрывают примерно половину информации о зданиях, поэтому в LBSMv2:
    - Используется сразу ряд готовых авторитетных датасетов (чтобы отсутствующая информация в одном источнике компенсировалась другим)
    - Часть отсутвующих данных синтетически воссоздана на основе снепшота данных 2024 года. Для этого использовалась decision tree boosting algorithm как для задач регрессии (численные переменные), так и классификации (для категориальных переменных). Среди смоделированных переменных (source: [London Datastore: LBSMv2 description]):
        - Property type & built form
        - Floor area & number of habitable rooms 
        - Property tenure 
        - Primary fuel type & heating system 
        - Wall type & insulation 
        - Roof type & insulation
        - Construction age 
        - Glazing type 
        - Energy consumption 
        - Current & potential EPC rating 
- Предобработка данных. Для повышения качества данных уже было:
    - Для нескольких доступных опций для одного поля, данные усреднялись / брались самые свежие / самые частые значения
    - Для пропущенных данных были произведены оценки: например, energy consumption was estimated based on fuel bill data
- Точность синтететических данных. Достаточно высокая точность модели 87%
- Внешняя экспертная валидация. The outputs of the model have been independently reviewed by Buro Happold at an area level with a focus on attributes most important for the likely uses, with the conclusion ‘that the model performed well’

Недостатки:
- "The individual properties within these ‘search areas’ will then need to be surveyed as part of the detailed design of any retrofit measures. This is needed because EPCs do not contain sufficient detail for this installation design and although modelled EPC values have high accuracy (80 - 90%) they are not 100% accurate."

#### EPC
НАПИСАТЬ, ЧТО ИСПОЛЬЗУЮ, ПОТОМУ ЧТО ХОЧУ САМЫЕ СВЕЖИЕ ДАННЫЕ, ГДЕ ОНИ ЕСТЬ 

#### Camden Open Data
НАПИСАТЬ, ЧТО ИСПОЛЬЗУЮ ЭТИ ДАННЫЕ ДЛЯ ВЫЯВЛЕНИЕ ГЕОМЕТРИИ И ВИЗУАЛИЗАЦИИ

#### Какие-то еще данные для basemap
НАПИСАТЬ, КАКИЕ ДАННЫЕ ДЛЯ BASEMAP

### Feature Selection
Для группировки зданий сначала мне потребовалось выявить основания деления. Они определяются характером работ подрядчика, который заинтересован осуществлять схожие меры для retrofit на расположенных близко друг к другу территориях. Поэтому, кроме локационных, как важные были выделены признаки, связанные с теплопотерями оболочки и масштабом работ, потому что они обуславливают схожие меры для retrofit:
- built_form
- total_floor_area / estimated_floor_count
- wall_type
- roof_type
- glazing_type
- main_heat_type (или комбинированный heating system)
- main_fuel_type
- epc_score (или производный “efficiency gap”)

CONSERVATION AREA & RETROFIT ВМЕСТЕ УЖИВАЮТСЯ?
Важно, что не все здания нужно учитывать в равной степени - некоторые находятся внутри охраняемых территорий, то есть retrofit там либо затруднителен, либо невозможен. Такие здания должны быть рассматрены отдельно - для этого используется признак conservation_area_flag

КАКИМ ЗДАНИЯМ RETROFIT НУЖЕН В ПЕРВУЮ ОЧЕРЕДЬ?
Также для некоторых зданий ретрофит не нужен остро - они относятся к категориям A и B (в чатgpt продолжение...)

ЗАЧЕМ ИСПОЛЬЗУЕМ СОЦ-ДЕМ ПРИЗНАК?
- а policy-таргетинг обычно добавляет депривацию/уязвимость отдельным слоем (а не как меру технической похожести)

- откуда скачать LBSM2, EPC, OS, Historic England,
- какие поля/даты использованы,
- шаги подготовки

### References
[UK_CCA80]: https://www.gov.uk/guidance/climate-change "UK Government: Climate Change Act target (80% by 2050 - historical)"
[UK_CCC_NETZERO]: https://www.theccc.org.uk/climate-action/ "UK Climate Change Committee: Net Zero by 2050"
[PARLIAMENT_RETROFIT]: https://publications.parliament.uk/pa/cm5901/cmselect/cmesnz/453/report.html "UK Parliament (ESNZ Committee): Retrofitting homes for net zero (2025)"
[OFGEM_ECO]: https://www.ofgem.gov.uk/environmental-and-social-schemes/energy-company-obligation-eco "Ofgem: Energy Company Obligation (ECO)"
[GUPTA_GREGG_2020]: https://radar.brookes.ac.uk/radar/file/7c6a1d16-757d-491b-909f-52cf8b0d5455/1/Domestic%20energy%20mapping%20for%20retrofit%20-%202020%20-%20Gupta%20Gregg.pdf "Gupta & Gregg (2020): Domestic Energy Mapping to Enable Area-Based Whole House Retrofits"
[HF_RETROFIT_STRATEGY]: https://democracy.lbhf.gov.uk/documents/s131593/Appendix%201%20-%20HF%20Council%20Housing%20Retrofit%20Strategy.pdf "LB Hammersmith & Fulham: Council Housing Retrofit Strategy"
[LEWISHAM_RETROFIT_STRATEGY]: https://lewisham.gov.uk/-/media/services/housing/housing-retrofit-strategy-empowering-communities-through-retrofitting.pdf "LB Lewisham: Housing Retrofit Strategy"
[IET_RETROFIT_2050]: https://www.theiet.org/media/8758/retrofit.pdf "IET: Scaling Up Retrofit 2050"
[CAMDEN_CAP_SUMMARY]: https://www.camden.gov.uk/documents/20142/344816220/Climate%2BAction%2BPlan%2BCondensed%2BSummary_A5.pdf "Camden Climate Action Plan Summary"
[CAMDEN_FUEL_POVERTY_STATS]: https://news.camden.gov.uk/keeping-warm-and-well-this-winter-in-camden/ "Camden Council: fuel poverty stats (2019 estimate)"
[CAMDEN_RETROFIT]: https://www.camden.gov.uk/retrofit "Camden Council: Camden Retrofit programme"

[Greater London Authority: LBSMv2]: https://www.gov.uk/algorithmic-transparency-records/greater-london-authority-london-building-stock-model-2
[London Datastore: LBSMv2 description]: https://data.london.gov.uk/blog/london-building-stock-model-2
[London Datastore: LBSMv2 data dictionary]: https://data.london.gov.uk/dataset/london-building-stock-model-2-lbsm-2-2k55d