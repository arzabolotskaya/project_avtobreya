# Проект: "Автоматическая обработка языка"
_Алина Заболотская, 3 курс_

## Файлы
"final" - итоговая оптимизированная версия

"version 1" - первая версия (была слишком медленной, но тоже работает)

"documents/avrobreya_project" - сохраненный после парсинга словарь

## Описание работы
__Шаг 1)__ Сбор данных. Создание парсера. 

__Шаг 2)__ Создание корпуса. Деление на предложения. Корпус состоит из стихотворений А. Блока, отвечающих регламенту проекта (более 100 слов).

__Шаг 3)__ Добавление уровня морфологической разметки Mystem в корпус.

__Шаг 4)__ Реализация поискового функционала. Использован морфоанализатор pymorphy.

__Шаг 5)__ Поиск. Сама строка запроса, в которую читатель может вбить любой текст и проверить работу поисковой системы. Здесь же приведены примеры запросов.

## Оптимизация
Изначально поисковой функционал был реализован через итерацию по питоновским словарям в несколько (до трех) этапов. Но это показалось мне слишком медленно, поэтому я переписала функционал с использованием модуля pandas и с объединением трех этапов.

## Отчет
__Как собирался корпус:__ Был создан парсер для веб-ресурса slova.org.ru с использованием модулей requests и BeautifulSoup. Собирались только стихотворения А. Блока. На выходе словарь с названием и текстом.

__Как предобработан корпус:__ С помощью nltk удалены нетекстовые элементы: пунктуация, символы переноса строки и т д. Все стихотворения поделены на предложения и записаны на новый уровень словаря.

__Как корпус размечен:__ Каждое предложение пронализировано с помощью морфоанализатора Mystem. Добавлен новый уровень словаря, где каждому слову соответствуют теги части речи, словоформы и леммы. Словарь сохраняется на компьютер. 

__Как обрабатываются запросы:__ Словарь записывается в датафрейм pandas для ускорения работы поиска. Поиск делит запрос на слова (1-3 слова). Сначала итерируется по колонке слов, проверяя вхождения для первого слова, затем проверяет последующие за ним слова. Проверка "подходящести" осуществляется через категоризацию слов запроса: запрос в кавычках, запрос без кавычек, запрос с указанием части речи, запрос в формате "словоформа+часть речи" - для каждой категории проверяется соответствующее ей условие в колонке датафрейма. Когда найдены вхождения для всего словосочетания, выбирается соответствующая датафрейма и приводится в человекочитаемый вид. В запросах в формате "словоформа+часть речи" ищется точное вхождение словоформы, а не все вхождения лексемы. В запросах без кавычек ищутся все словоформы с помощью pymorphy.

__Как проводилось тестирование, какие сложные / пограничные случаи проверялись:__ Проверялась работа всех форматов запросов в разных комбинациях. Особое внимани уделялось омонимии: например, при внутреннем морфоразборе поискового запроса "пил" должны были учитываться все возможные разборы (пила - сущетсвительное, пить - глагол). При этом, в самом корпусе было важно разрешить омонимическую неоднозначность. 
