Объединила датасеты, выбрала переменные, почистила от недопустимых значений в аре

### СОЗДАНИЕ И ПРЕОБРАЗОВАНИЕ ПЕРЕМЕННЫХ

#### Семейное положение (1 - проживает с партнером, 0 - проживает без партнера)
gen marst = 1 if (w_marst == 3 | w_marst == 2 | wj324 == 1 | wj324 == 2)

replace marst = 0 if marst == .

#### Образование (1 - начальное, 2 - среднее, 3 - высшее)
gen educ = 1 if (w_diplom == 1 | w_diplom == 2 | w_diplom == 3)

replace educ = 2 if (w_diplom == 4 | w_diplom == 5)

replace educ = 3 if w_diplom == 6

#### Высшее образование (1 - есть, 0 - нет)
gen high_educ = 1 if educ == 3

replace high_educ = 0 if high_educ == .

#### Регион (1 - Мск и СПб, 0 - другие)
gen rg = 1 if (region == 138 | region == 140 | region == 141)

replace rg = 0 if rg == .

#### Тип местности (1 - городская, 2 - сельская)
gen st = 1 if (status == 1 | status == 2)

replace st = 2 if (status == 3 | status == 4)

#### Удельный доход = семейный доход за месяц/кол-во членов семьи
gen income = wf14/sqrt(w_nfm)

#### Логарифм удельного дохода
gen log_income = log(income+1)

#### Логарифм индивидуального дохода
gen log_wj60 = log(wj60+1)

#### Показатель нездоровья = 100 - EQ-VAS
gen unhealth = 100 - wm188

#### Пенсионный возраст (1 - достиг, 0 - не достиг, мужчины - 60 лет, женщины - 55 лет)
gen pension = 1 if ((wh5 == 1 & w_age >= 60) | (wh5 == 2 & w_age >= 55))

replace pension = 0 if pension == .

#### Возрастные группы
gen age_group = 1 if (w_age >= 18 & w_age <= 25)

replace age_group = 2 if (w_age >= 26 & w_age <= 35)

replace age_group = 3 if (w_age >= 36 & w_age <= 45)

replace age_group = 4 if (w_age >= 46 & w_age <= 55)

replace age_group = 5 if (w_age >= 56 & w_age <= 65)

replace age_group = 6 if (w_age >= 66 & w_age <= 75)

replace age_group =7 if w_age >= 76

### ОПИСАТЕЛЬНЫЕ СТАТИСТИКИ
#### Социально-экономические характеристики
sum w_age log_income log_wj60, detail

sum w_age log_income log_wj60 if wh5 == 1, detail

sum w_age log_income log_wj60 if wh5 == 2, detail

tab age_group

tab age_group if wh5 == 1

tab age_group if wh5 == 2

tab pension

tab pension if wh5 == 1

tab pension if wh5 == 2

tab marst

tab marst if wh5 == 1

tab marst if wh5 == 2

tab rg

tab rg if wh5 == 1

tab rg if wh5 == 2

tab st

tab st if wh5 == 1

tab st if wh5 == 2

#### Характеристики КЖСЗ
tab wm183

tab wm183 if wh5 == 1

tab wm183 if wh5 == 2

tab wm184

tab wm184 if wh5 == 1

tab wm184 if wh5 == 2

tab wm185

tab wm185 if wh5 == 1

tab wm185 if wh5 == 2

tab wm186

tab wm186 if wh5 == 1

tab wm186 if wh5 == 2

tab wm187

tab wm187 if wh5 == 1

tab wm187 if wh5 == 2

sum wm188

sum wm188 if wh5 == 1, detail

sum wm188 if wh5 == 2, detail

### МЕЖГРУППОВЫЕ РАЗЛИЧИЯ

#### Семейное положение
ttest unhealth, by (marst)

ttest unhealth if wh5 == 1, by (marst)

ttest unhealth if wh5 == 2, by (marst)

ksmirnov unhealth, by(marst) exact

ksmirnov unhealth if wh5 == 1, by(marst) exact

ksmirnov unhealth if wh5 == 2, by(marst) exact

#### Высшее образование
ttest unhealth, by (high_educ)

ttest unhealth if wh5 == 1, by (high_educ)

ttest unhealth if wh5 == 2, by (high_educ)

ksmirnov unhealth, by(high_educ) exact

ksmirnov unhealth if wh5 == 1, by(high_educ) exact

ksmirnov unhealth if wh5 == 2, by(high_educ) exact

#### Пенсионный возраст
ttest unhealth, by (pension)

ttest unhealth if wh5 == 1, by (pension) 

ttest unhealth if wh5 == 2, by (pension)

ksmirnov unhealth, by(pension) exact

ksmirnov unhealth if wh5 == 1, by(pension) exact

ksmirnov unhealth if wh5 == 2, by(pension) exact

#### Регион
ttest unhealth, by (rg)

ttest unhealth if wh5 == 1, by (rg)

ttest unhealth if wh5 == 2, by (rg)

ksmirnov unhealth, by(rg) exact

ksmirnov unhealth if wh5 == 1, by(rg) exact

ksmirnov unhealth if wh5 == 2, by(rg) exact

#### Тип местности 
ttest unhealth, by (st)

ttest unhealth if wh5 == 1, by (st)

ttest unhealth if wh5 == 2, by (st)

ksmirnov unhealth, by(st) exact

ksmirnov unhealth if wh5 == 1, by(st) exact

ksmirnov unhealth if wh5 == 2, by(st) exact

#### Возрастные группы
oneway unhealth age_group, sidak bonferroni scheffe

oneway unhealth age_group if wh5 == 1, sidak bonferroni scheffe

oneway unhealth age_group if wh5 == 2, sidak bonferroni scheffe

kwallis unhealth, by(age_group)

kwallis unhealth if wh5 == 1, by(age_group)

kwallis unhealth if wh5 == 2, by(age_group)

#### Квинтильные группы индивидуального дохода
centile log_wj60, centile (20, 40, 60, 80)

scalar wj60_20 = r(c_1)

scalar wj60_40 = r(c_2)

scalar wj60_60 = r(c_3)

scalar wj60_80 = r(c_4)

gen quantile_wj60 = 1 if log_wj60 <= wj60_20

replace quantile_wj60 = 2 if (log_wj60 > wj60_20 & log_wj60 <= wj60_40)

replace quantile_wj60 = 3 if (log_wj60 > wj60_40 & log_wj60 <= wj60_60)

replace quantile_wj60 = 4 if (log_wj60 > wj60_60 & log_wj60 <= wj60_80)

replace quantile_wj60 = 5 if log_wj60 > wj60_80

oneway unhealth quantile_wj60, tab

kwallis unhealth, by(quantile_wj60)

#### Квинтильные группы удельного дохода
centile log_income, centile (20, 40, 60, 80)

scalar income_20 = r(c_1)

scalar income_40 = r(c_2)

scalar income_60 = r(c_3)

scalar income_80 = r(c_4)

gen quantile_income = 1 if log_income <= income_20

replace quantile_income = 2 if (log_income > income_20 & log_income <= income_40)

replace quantile_income = 3 if (log_income > income_40 & log_income <= income_60)

replace quantile_income = 4 if (log_income > income_60 & log_income <= income_80)

replace quantile_income = 5 if log_income > income_80

oneway unhealth quantile_income, tab

kwallis unhealth, by(quantile_income)

### ЛОГИСТИЧЕСКАЯ РЕГРЕССИЯ НА КОМПОНЕНТЫ КЖСЗ

replace wm183 = 0 if wm183 == 1

replace wm183 = 1 if (wm183 == 2 | wm183 == 3)

logistic wm183 i.wh5 w_age log_wj60 i.marst i.high_educ i.rg i.st, vce(robust)

est store reg1

replace wm184 = 0 if wm184 == 1

replace wm184 = 1 if (wm184 == 2 | wm184 == 3)

logistic wm184 i.wh5 w_age log_wj60 i.marst  i.high_educ i.rg i.st, vce(robust)

est store reg2

replace wm185 = 0 if wm185 == 1

replace wm185 = 1 if (wm185 == 2 | wm185 == 3)

logistic wm185 i.wh5 w_age log_wj60 i.marst  i.high_educ i.rg i.st, vce(robust)

est store reg3

replace wm186 = 0 if wm186 == 1

replace wm186 = 1 if (wm186 == 2 | wm186 == 3)

logistic wm186 i.wh5 w_age log_wj60 i.marst  i.high_educ i.rg i.st, vce(robust)

est store reg4

replace wm187 = 0 if wm187 == 1

replace wm187 = 1 if (wm187 == 2 | wm187 == 3)

logistic wm187 i.wh5 w_age log_wj60 i.marst  i.high_educ i.rg i.st, vce(robust)

est store reg5

esttab reg*

### КРИВЫЕ И ИНДЕКСЫ КОНЦЕНТРАЦИИ

#### Ранжирование по индивидулаьному доходу
glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_wj60) lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord "EQ-VAS"

twoway (line yord rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust

drop rank yord rank rank2

#### Разбивка по полу
glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_wj60) replace by (wh5) split lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord_1 "Мужчины"

lab var yord_2 "Женщины"

twoway (line yord_1 rank , sort)(line yord_2 rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust compare(wh5)

drop rank yord_1 yord_2 rank rank2

#### Разбивка по региону
glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_wj60) replace by (rg) split lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord_1 "Москва и Санкт-Петербург"

lab var yord_0 "Другие регионы"

twoway (line yord_0 rank , sort)(line yord_1 rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust compare(rg)

drop rank yord_0 yord_1 rank rank2

#### Разбивка по пенсионному возрасту
glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_wj60) replace by (pension) split lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord_1 "После пенсионного возраста"

lab var yord_0 "До пенсионного возраста"

twoway (line yord_0 rank , sort)(line yord_1 rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust compare(pension)

drop rank yord_0 yord_1 rank rank2
 
#### Ранжирование по удельному доходу
glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_income) lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord "EQ-VAS"

twoway (line yord rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust

drop rank yord rank rank2

#### Разбивка по полу
glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_income) replace by (wh5) split lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord_1 "Мужчины"

lab var yord_2 "Женщины"

twoway (line yord_1 rank , sort)(line yord_2 rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust compare(wh5)

drop rank yord_1 yord_2 rank rank2

#### Разбивка по региону
drop if log_wj60 == .

glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_income) replace by (rg) split lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord_1 "Москва и Санкт-Петербург"

lab var yord_0 "Другие регионы"

twoway (line yord_0 rank , sort)(line yord_1 rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust compare(rg)

drop rank yord_0 yord_1 rank rank2

#### Разбивка по пенсионному возрасту
drop if log_wj60 == .

glcurve unhealth, glvar(yord) pvar(rank) sortvar(log_income) replace by (pension) split lorenz

gen rank2 = rank

lab var rank "Кумулятивная доля населения"

lab var rank2 "Линия равенства"

lab var yord_1 "После пенсионного возраста"

lab var yord_0 "До пенсионного возраста"

twoway (line yord_0 rank , sort)(line yord_1 rank , sort)(line rank2 rank , sort), ytitle(Кумулятивная доля показателя нездоровья) plotregion(fcolor(white)) graphregion(fcolor(white))

conindex unhealth, rankvar(rank) bounded limits(0 100) robust compare(pension)

drop rank yord_0 yord_1 rank rank2 
