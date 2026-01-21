## Area-Based Retrofit Potential in Camden

### Context
The UK has established the ambitious target of reducing greenhouse gas emissions by at least 80% by 2050. Across the country - and especially in London - there is a substantial stock of older, energy-inefficient housing, which generates high levels of carbon emissions. Although recent years have seen the rollout of subsidized loan programs, direct grants for vulnerable social groups, and new obligations for energy companies to participate in retrofit schemes, the pace of housing retrofit remains low. To address this issue, some local areas have adopted mass retrofit strategies where groups of similar buildings are upgraded together, optimizing purchasing power, logistics, and attractiveness to suppliers (for example, this practice has been piloted in London’s Hammersmith & Fulham and Lewisham). In this research, I investigate the potential for area-based retrofit in Camden by clustering nearby buildings using energy- and fabric-related characteristics to highlight candidate groups for coordinated upgrades. Such an approach could support targeted retrofit delivery, helping to reduce carbon emissions while also lowering households’ energy bills.

ССЫЛКИ ГДЕ?

### Methodology 

А ОТКУДА ВОЗЬМЕМ ФАКТ ТОГО ЧТО РЕТРОФИТ УЖЕ БЫЛ?

### Data
##### LBSMv2
Описание: Данные о ключевых характеристиках зданий жилого назначения в Лондоне [Greater London Authority: LBSMv2]

Coordinate Reference System (CRS) [London Datastore: LBSMv2 data dictionary]: 
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
    - Часть отсутвующих данных синтетически воссоздана на основе снепшота данных 2024 года. Для этого использовалась decision tree boosting algorithm как для задач регрессии (численные переменные), так и классификации (для категориальных переменных). Среди смоделированных переменных [London Datastore: LBSMv2 description]:
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

Общая оценка: 
НАПИСАТЬ? 

ПЕРЕВЕСТИ КРАСИВО!

##### EPC
НАПИСАТЬ, ЧТО ИСПОЛЬЗУЮ, ПОТОМУ ЧТО ХОЧУ САМЫЕ СВЕЖИЕ ДАННЫЕ, ГДЕ ОНИ ЕСТЬ 

##### Camden Open Data
НАПИСАТЬ, ЧТО ИСПОЛЬЗУЮ ЭТИ ДАННЫЕ ДЛЯ ВЫЯВЛЕНИЕ ГЕОМЕТРИИ И ВИЗУАЛИЗАЦИИ

##### Какие-то еще данные для basemap
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
[Greater London Authority: LBSMv2]: https://www.gov.uk/algorithmic-transparency-records/greater-london-authority-london-building-stock-model-2?utm_source=chatgpt.com
[London Datastore: LBSMv2 description]: https://data.london.gov.uk/blog/london-building-stock-model-2/?utm_source=chatgpt.com
[London Datastore: LBSMv2 data dictionary]: https://data.london.gov.uk/dataset/london-building-stock-model-2-lbsm-2-2k55d