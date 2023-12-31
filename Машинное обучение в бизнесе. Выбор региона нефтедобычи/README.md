# Машинное обучение в бизнесе

## Определение наиболее выгодного региона нефтедобычи

**Стек:** bootstrap / scikit-learn / scipy / pandas

**Задача:** Построить модель машшиного обучения, которая подскажет регион, где добыча принесет наибольшую прибыль из информации о 10 000 нефтяных месторождений. Проанализировать прибыль и риски техникой Bootstrap. Нужно оставить лишь те регионы, в которых вероятность убытков меньше 2.5%.

**План реализации проекта:**
* загрузить и подготовить данные
* обучить и проверить модели машинного обучения
* подготовить данные для расчета прибыли и рисков
* рассчитать прибыль и риски
* на основе полученных данных предложить регион для разработки месторождений

## Реализация проекта

Данные для исследования представляют собой 3 файла:
 - `geo_data_0.csv`
 - `geo_data_1.csv`
 - `geo_data_2.csv`

Каждый файл - это таблица с такими полями:
 - **id** - уникальный идентификатор месторождения
 - **f0, f1, f2** - нераскрываемые технические характеристики месторождения
 - **product** - предполагаемый запас сырья в скважине (тыс. баррелей)

### Предобработка

В датасетах нет пропусков и аномалий, за исключением данных второго региона: значение f2 и значение целевого признака(product) здесь распределены дискретно. В данных 1 и 3 регионов эти значения распределены нормально, с разницей лишь в количестве мод(пиков). Возможно, была допущена ошибка при сборе данных. А возможно, это искуственные данные. Нужно будет уточнить этот момент у заказчика при сдаче работы.  
Также стандартизировали признаки - среднее каждого признака стало 0, а стандартное отклонение 1

### Обучение моделей

В условии сказано, что оптимальной моделью является модель линейной регрессии. Ее и будем обучать.

***Результаты обучения модели по данным первого региона***

**RMSE = 37.58** <br>
Средний предсказанный запас сырья в регионе = 92.59

***Результаты обучения модели по данным второго региона***

**RMSE = 0.89**<br>
Средний предсказанный запас сырья в регионе = 68.73

***Результаты обучения модели по данным третьего региона***

**RMSE = 40.03**<br>
Средний предсказанный запас сырья в регионе = 94.97

**Вывод:**
Точнее всего оказалась модель для второго региона, наверное, повлияли отличающиеся данные. Метрика RMSE в нашем случае показывает, на сколько единиц(тысяч баррелей) модель в среднем ошибается. Для наших регионов целевой признак варьируется от 0 до значений в 140-180. Получается, для 1 и 3 регионов разброс предсказаных ответов составляет в среднем ~ 30-40%, что достаточно много и означает большие риски. Для 2 региона разброс в среднем меньше 1%, что означает меньшие риски


### Расчёт прибыли и рисков

Выполним процедуру **Bootstrap** и найдем 95% доверительный интервал чистой прибыли по каждой из выборок, среднюю прибыль и величину риска.

***Первый регион***  

95% доверительный интервал (-111.2 : 909.8)  
Средняя прибыль = 396.2 млн. руб.  
Величина риска = 6.9%  

***Второй регион***  

95% доверительный интервал (33.8 : 852.3)  
Средняя прибыль = 456.0 млн. руб.  
Величина риска = 1.5%  

***Третий регион***  

95% доверительный интервал (-163.4 : 950.4)  
Средняя прибыль = 404.4 млн. руб.  
Величина риска = 7.6%  

### Общий вывод

По результатам проведенной работы выяснилось, что подходящим под требования (нужно оставить лишь те регионы, в которых вероятность убытков меньше 2.5%.) является второй регион. Другие, имеют несоответствующую величину риска. Так же, второй регион имеет более высокий показатель средней прибыли и меньший размах доверительного интервала. Плюс ко всему, RMSE данных этого региона меньше 1, что говорит о высокой точности прогнозов.
Таким образом, регион для разработки - это второй регион **geo_data_1.csv**.