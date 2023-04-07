# Проект 3.  Прогнозирование рейтинга отеля на Booking

## Оглавление
[1. Описание проекта](https://github.com/murattumov/project3/blob/master/README.md#Описание-проекта)

[2. Какой кейс решаем?](https://github.com/murattumov/project3/blob/master/README.md#Какой-кейс-решаем)

[3. Краткая информация о данных](https://github.com/murattumov/project1/blob/master/README.md#Краткая-информация-о-данных)

[4. Этапы работы над проектом](https://github.com/murattumov/project3/blob/master/README.md#Этапы-работы-над-проектом)

[5. Результат](https://github.com/murattumov/project3/blob/master/README.md#Результат)

[6. Краткая информация](https://github.com/murattumov/project3/blob/master/README.md#Краткая-информация)

### **Описание проекта**

Одна из проблем компании — это нечестные отели, которые накручивают себе рейтинг. Одним из способов нахождения таких отелей является построение модели, которая предсказывает рейтинг отеля. Если предсказания модели сильно отличаются от фактического результата, то, возможно, отель играет нечестно, и его стоит проверить.

### **Какой кейс решаем?**

Необходимо изучить данные. Очистить данные от пропущенных значений и дупликатов. Провести разведывательный анализ данных: получение и проектирование новых данных, отбор признаков, кодирование признаков  и проверка на мультиколлинеарность признаков. Обучаем модель ML, который будет предсказывать рейтинг отеля.

### **Краткая информация о данных**
Данные представляют собой базу рейтингов оттелей Booking. Состоит из 386803 строк и 16 признаков: кроме целевого, 8 числового типа и 8 типа object. 
Признаки:
- hotel_address - адрес отеля
- review_date - дата, когда рецензент разместил соответствующий отзыв.
- average_score - средний балл отеля, рассчитанный на основе последнего комментария за последний год
- hotel_name - название отеля
- reviewer_nationality - национальность рецензента
- negative_review - отрицательный отзыв, который рецензент дал отелю.
- review_total_negative_word_counts - общее количество слов в отрицательном отзыв
- positive_review - положительный отзыв, который рецензент дал отелю
- review_total_positive_word_counts - общее количество слов в положительном отзыве
- reviewer_score - оценка, которую рецензент поставил отелю на основе своего опыта
- total_number_of_reviews_reviewer_has_given - количество отзывов, которые рецензенты дали в прошлом
- total_number_of_reviews - общее количество действительных отзывов об отеле
- tags - теги, которые рецензент дал отелю.
- days_since_review - продолжительность между датой проверки и датой очистки
- additional_number_of_scoring - есть также некоторые гости, которые просто поставили оценку сервису, а не оставили отзыв. Это число указывает, сколько там действительных оценок без проверки.
- lat - широта отеля
- lng - долгота отеля


### **Этапы работы над проектом**

- Анализ структуры данных
  1. Пропущенные значения в признаках lat и lng.
  2. Число найденных дубликатов: 307. Сразу же убираем дубликаты


- Преобразование и получение новых признаков

  1. преобразовываем дату в datetime в признаке review_date.
  2. убираем пробелы в начале и в конце строки в reviewer_nationality.
  3. вычисляем страну отеля hotel_country по адресу hotel_address.
  4. определяем является ли рецензент гостем страны quest сравнивая hotel_country и reviewer_nationality.
  5. строим график распределения оценки рецензента reviewer_score в разрезе признака гостя quest по странам отелей hotel_country.
  6. строим график зависимости оценки рецензента reviewer_score от признаков "hotel_country"-страна отеля и "month"- месяц года оценки рецензентом.
  7. вычисляем день недели dayofweek оценки рецензента reviewer_score. И строим график зависимости оценки reviewer_score от признаков "hotel_country"-страна отеля и "dayofweek"-день недели.
  8. перевод строки days_since_review в целое число.
  9. просмотрев отзывы отбираем которые не соответсвуют признакам положительности в положительных отзывах positive_review и признакам отрицательности в негативных отзывах negative_review (вручную, в данном моменте дошли до 10 повторов отзывов). Создаем список не позитивных отзывов lst_no_positiv и список не негативных отзывов lst_no_negative.
  10. с помощью списка lst_no_positiv выявляем реально позитивные отзывы positiv_review_key в признаке positiv_review. Строим график соотношения реально позитивных и не позитивных отзывов. Более 7% из признака positiv_review являются не позитивным отзывом.
  11. Также выявляем реально негативные отзывы, с помощью lst_no_negative, в признаке negative_review. И строим график соотношения позитивных и реально негативных отзывов из признака negative_review. Треть отзывов записанных как негативные в negative_review позитивные.
  12. Обнуляем количество слов в признаках review_total_negative_word_counts и review_total_positive_word_counts, с помощью ключей negative_review_key и positiv_review_key соответсвенно.
  13. Создаем ключи для наиболее популярных слов в отзывах как для позитивных так и для негативных отзывов.
  14. оставляем признак negative_review_key как other, в дополнение слов из списка ключей.
  15. оставляем признак positiv_review_key как other, в дополнение слов из списка ключей.
  16. создаем коллекцию тэгов из признака tags. Отбираем из коллекции тэгов 15 популярных и создаем ключи.
  17. убираем ненужные признаки и строковые данные.


- Мультиколлинеарность признаков

  1. Разделяем на числовые непрерывные признаки и категориальные признаки.
  2. строим диаграмму для непрерывных признаков.
  3. удаляем признак из пар имеющих модуль корреляции больше 0.8. В данном случае total_number_of_reviews.
  4. строим диаграмму для категориальных признаков
  5. удаляем признак из пар имеющих модуль корреляции больше 0.7. В данном случаеBusiness trip.


- Обучение модели
  
  1. Разбиваем датафрейм на части, необходимые для обучения и тестирования модели.
  2. С помощью train_test_split разбиваем датасет на тренировочный и проверочный.
  3. Обучаем модель RandomForestRegressor на тренировочном наборе данных и предсказываем значения рейтинга на проверочном.
  4. сравниваем предсказанные значения (y_pred) с реальными (y_test), метрика Mean Absolute Percentage Error (MAPE) показывает среднюю абсолютную процентную ошибку предсказанных значений от фактических.
  5. в RandomForestRegressor есть возможность вывести самые важные признаки для модели.
  6. Предсказываем значения рейтинга на тестовом датасете test_data и сохраняем в submission.csv.


### **Результат**

Из 18 исходных признаков для построения модели было применено пять. Остальные - это признаки, сконструированные в процессе исследования данных. Критерий MAPE равно значению 12,91. В ряду важнейших признаков есть: общее количество слов в положительном отзыве, общее количество слов в отрицательном отзыве, дополнительные оценки без отзывов, средняя оценка отеля, продолжительность между датой проверки и датой очистки и количество отзывов, которые рецензенты дали в прошлом. В основном сконструированные признаки пошли на подгонку, корректировку, значений оценок. В дальнейшем можно сконструировать признаки из широты отеля и долготы отеля. Или обучить модель, использовав продолжительность между датой проверки и датой очистки, как временную последовательность. Также можно использовать продвинутые методы обработки текста для признаков 'negative_review', и 'positive_review'.

### **Краткая информация**

[Ссылка на Kaggle](https://www.kaggle.com/code/murattiroshevtumov/project3-hotels)
****


:arrow_up:[к оглавлению](https://github.com/murattumov/project1/blob/master/README.md#Оглавление)
