Calendar for WordPress by CasePress
=====================

# Todo
Этот раздел описывает требования к плагину
## Общие требования
1. Тип поста event_cp, слаг events
2. Дата начала = дата поста. Чтобы иметь возможность выборки событий по дате старта через WP_Date_Query (https://wpmag.ru/2013/rabota-s-datami-v-wordpress-wp_date_query/)
3. На странице архива по этому посту выводить справа календарь. Слева сайдбар. Обычно.
4. Таксономия "Категория событий". Позволят указывать типы. Примеры: Срок, Напоминание, Начало, Звонок, Встреча, Веха, Контроль, Заметка.
5. Дата окончания события хранится в поле order таблицы posts, и является количеством секунд от даты начала. Затем чтобы прибавляя order к дате начала, иметь возможность получить длительность события. Обычно оно равно часу. Но может быть больше.


## API
1. add_event_cp($object_type, $object_id, $event_key, $event_value, $event_duration) - добавляет события с привзякой. аналог http://wp-kama.ru/function/add_metadata
2. update_event_cp($object_type, $object_id, $event_key, $event_value, $event_duration) - обновляет событие. если оно отличается.
3. delete_event_cp
4. get_event_cp


##Форма добавляени события
1. Делаем шорткодом
2. Можем заполнить поля: Наименование, Описание, Ссылка (то куда перейдем при переходе на событие), Основание (может быть ID поста), Участники (также как в Делах)
3. Поля также можем заполнить через GET.
4. Можем в опциях выбрать страницу, которая будет основой формы (для того чтобы затем иметь возможность плучить ID этой страницы и интегрировать генерацию УРЛ в другие части системы)


## Шаблон архива / списка
1. Календарь выводим на базе http://arshaw.com/fullcalendar/
2. Встает вопрос как этому календарю отдать его данные в формате JSON? Для этого подойдет механизм по аналогии с feed, только JSON. Передаем в URL параметр типа ?view_cp=json-fullcalendar и получаем вывод постов не в виде обычной таблицы, а в виде JSON


## Переход на страницу события
1. Сами события как посты не публичные. Переходить на них нельзя.
2. У каждого события есть либо родитель в виде другого поста, либо ID комментария в мете "parrent_comment". Чтобы иметь возможность получить родительский пост или родительский коммент.


##Описание функций
###event_cp_init
Регистрация типа поста
###add_event_taxonomies
Регистрация таксономии
###add_extra_fields_cp
Добавление на странице редактирования события необходимых полей
###cp_calendar_enqueue
Подключение скриптов и стилей
###extra_fields_update_cp(int $post_id)
Сохранение дополнительных полей события в пост с ID $post_id
###validate_date($date)
Проверка формата даты (string $date) должен соответствовать Y-m-d H:i:s
###add_event_cp(string|int $start, string|int $end, string $title, string $description, string|int|array $event_key, string $url)
Добавляет пост типа "Событие" 
string|int $start Дата и время начала события. Строка формата Y-m-d H:i:s или UNIX timestamp.
string|int $end Дата и время окончания события. Строка формата Y-m-d H:i:s или UNIX timestamp.
string $title Заголовок события
string $description Описание события
string|int|array $event_key Термины таксономии "Категория события". Строка содержашая название термина/слаг или целочисленное значение ID термина или массив целочисленых значений ID терминов.
string $url URL события
###delete_event_cp( int $event_id, bool $force_delete = false)
Удаление события с ID $event_id. $force_delete Определяет помещать событие в корзину или удалять полностью. По умолчанию false т.е. помещение в корзину.
###get_event_cp( int $event_id, string $output = 'OBJECT', string $filter = 'raw')
Получение события с ID $event_id
$output Определяет формат возвращаемого значения
Варианты:
- OBJECT - Объект WP_Post с дополнительными свойствами duration и url содержащими длительность события в секундах и URL. По умолчанию.
- ARRAY_A - Ассоциативный массив
- ARRAY_N - Нумерованый массив
$filter Фильтрация поста. Смотрите http://codex.wordpress.org/Function_Reference/sanitize_post_field для полного списка значений. По умолчанию 'raw'