## Area-Based Retrofit Potential in Camden

### Context
The UK has established the ambitious target of reducing greenhouse gas emissions by at least 80% by 2050. Across the country - and especially in London - there is a substantial stock of older, energy-inefficient housing, which generates high levels of carbon emissions. Although recent years have seen the rollout of subsidized loan programs, direct grants for vulnerable social groups, and new obligations for energy companies to participate in retrofit schemes, the pace of housing retrofit remains low. To address this issue, some local areas have adopted mass retrofit strategies where groups of similar buildings are upgraded together, optimizing purchasing power, logistics, and attractiveness to suppliers (for example, this practice has been piloted in London’s Hammersmith & Fulham and Lewisham).

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
- а policy-таргетинг обычно добавляет депривацию/уязвимость отдельным слоем (а не как меру технической похожести) - 

А ОТКУДА ВОЗЬМЕМ ФАКТ ТОГО ЧТО РЕТРОФИТ УЖЕ БЫЛ?

- откуда скачать LBSM2, EPC, OS, Historic England,
- какие поля/даты использованы,
- шаги подготовки