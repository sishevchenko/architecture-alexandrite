# Задание 1

## Разделы

- [Что нужно сделать](#что-нужно-сделать)
- [Анализ схемы и описания системы](#анализ-схемы-и-описания-системы)
- [Разработка инициатив](#разработка-инициатив)

## Что нужно сделать

1. Проанализируйте схему и описание системы. Идентифицируйте существующие и потенциальные проблемные места. Напишите их список.
2. Разработайте инициативы, которые необходимы для устранения нежелательных ситуаций. Запишите их в список.
3. Расставьте инициативы в порядке приоритета. Опишите ход своих рассуждений и ответьте на вопросы:
    - Какой вы видите целевую архитектуру через полгода?
    - Если бы у вас была возможность выполнить только три пункта из списка инициатив в ближайшие полгода, что бы вы выбрали и почему? Необязательно добавлять в список только эпики. Вы можете включить в план как крупные изменения, так и локальные задачи.

## Анализ схемы и описания системы

Следует применить итеративный подход к решению проблемы и сначала взяться за наиболее остые вопросы, которые приводят к снижению прибыли и решить конкретные проблемы бизнеса. Далее постепенно внедрять технологии, которые в последствии помогут справиться с высокими нагрузками.

Лучшим вариантом, на мой взгляд, будет составление плана разделенного на несколько крупных этапов, где в первую очередь будут внедрятся технологии и практики, которые помогут здесь и сейчас улучшить пользовательский опыт и повысить производительность системы.

Можно выделить несколько основных проблем с которыми сталкивается бизнес:

- Жалобы клиентов на то, что они не получают заказанные изделия в срок. Клиенты API тоже сталкиваются с аналогичной проблемой.
- Замечания операторов из-за подвисания системы с отображением заказов.
- Сложности с деплоем новых версий приложения на прод.

Очевидным уязвимым местом является интеграция CRM и MES через RabbitMQ т.к. кролик работает по push модели и может задудосить потребителя. Доп. фактором нагрузки являются потребители API.

`Симптомы`:

- Заказы "теряются" (клиенты не получают заказы)
- Задержки в обработке (работают месяцами вместо 3 недель)

`Возможные причины`:

- Нет proper error handling при обработке сообщений
- Сообщения могут теряться/дублироваться
- Нет механизма retry для неудачных обработок
- Нет end-to-end трейсинга заказов
- Возможно, очередь перегружена и сообщения обрабатываются с большой задержкой

## Разработка инициатив

- Выделить под расчет моделей машину с самым мощным CPU.
- Отслеживать самые популярные изделия за некий период времени и заранее прогревать ими кэш.
- Воспользоваться услугами облачной архитектуры и перенести часть расчетов туда.
- Заранее просчитывать все шаблонные и наиболее популярные модели и помещать результаты рассчета в БД.
- Расчеты стоимости моделей должны запускаться только после проверки кэша и БД на наличия аналогичного рассчета.
- Сделать сервис API Gateway для пользователей API. Это предотвратит доступ пользователей напряму в API нашей внутренней системы и за счет дополнительного слоя абстракции мы сможем более гибко управлять функциональными возможностями публичного API.
- Разработать лимиты и тарифы для пользователей API, т.к. они могут недобросовестно его использовать и перегружать нам систему.
- Ввести систему мониторинга

[<- На главную страницу](../ReadMe.md)
