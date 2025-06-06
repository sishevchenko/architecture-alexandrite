# Задание 2

## Разделы

- [Введение](#введение)
- [Что нужно сделать](#что-нужно-сделать)
- [Анализ системы компании в контексте мониторинга](#анализ-системы-компании-в-контексте-мониторинга)
- [Зачем нужен мониторинг](#зачем-нужен-мониторинг)
- [Выбор подхода к мониторингу](#выбор-подхода-к-мониторингу)
- [Метрики подлежащие отслеживанию](#метрики-подлежащие-отслеживанию)

## Введение

Сайт «Александрит» подключён к Яндекс Метрике. Но с тех пор как бизнес начал предоставлять оформление заказов через API, данные Яндекс Метрики уже не дают полной картины. Чтобы начать улучшать систему, вам нужно от чего-то отталкиваться. В этом задании вы запланируете внедрение мониторинга.

Вам нужно определить, что вы хотите измерять и как вы будете это делать. А затем —  постараться обосновать свои решения для бизнеса. Не забывайте: бизнесу не всегда очевидно, что мониторинг стоит того, чтобы выделять на него ресурсы команды.

## Что нужно сделать

1. Проанализируйте систему компании и C4-диаграмму в контексте планирования мониторинга.
2. Добавьте в файл раздел «Мотивация». Напишите здесь, почему в систему нужно добавить мониторинг и что это даст компании.
3. Добавьте раздел «Выбор подхода к мониторингу». Выберите, какой подход к мониторингу вы будете использовать: RED, USE или «Четыре золотых сигнала». Для разных частей системы можно использовать разные подходы.
4. Опишите, какие метрики и в каких частях системы вы будете отслеживать. Перед вами список метрик. Выберите метрики, которые вы считаете нужным отслеживать. Для выбранных метрик напишите:
    - Зачем нужна эта метрика.
    - Нужны ли ярлыки для этой метрики. Если ярлыки нужны, опишите, какие именно вы планируете добавить. Вы можете не ограничивать себя только этим списком. Если вы видите, что стоит добавить какие-то ещё метрики, — добавьте и их тоже.

## Анализ системы компании в контексте мониторинга

На данный момент в системе нет централизованного сбора метрик и мониторинга системы

## Зачем нужен мониторинг

Мониторинг поможет понять что и когда пошло не так после чего можно будет оперативно принять меры для предотвращения проблем

## Выбор подхода к мониторингу

Для обеспечения всестороннего контроля над различными компонентами системы предлагается использовать два подхода к мониторингу, адаптированных к их специфике:

- `USE` - Рекомендован для мониторинга MES (Manufacturing Execution System) сервиса, характеризующегося интенсивными вычислительными процессами. Данный подход позволяет эффективно выявлять проблемы, связанные с неоптимальным использованием ресурсов CPU, памяти и дискового пространства.

- `RED` - Предлагается использовать для мониторинга остальных сервисов (CRM API, Shop API и др.), поскольку он является универсальным и подходит для мониторинга веб-сервисов, запросов к базам данных, очередей сообщений и других компонентов. Данный подход позволяет отслеживать ключевые показатели: количество запросов, количество ошибок и время обработки запросов.

## Метрики подлежащие отслеживанию

Первостепенно следует внедрить технические метрики для наблюдаемости системы и составления аналитики по частям системы

- `RPS` — одна из стандартных метрик, которая отражает нагруженность системы и позволит адаптировать стэк к нагрузке
- `Latency` — для отслеживания задержек при запросах и составления аналитики насколько система справляется с такой нагрузкой необходимо отслеживать задержка, связанную с запросами,
- `CPU stat` — на сервере с MES обязательно отслеживать загрузку процессора, т.к. рассчтеты моделей это CPU bound нагрузка и возможно часть проблем связанна с железом.
- `Загруженность сети` — для выявления потенциальных бутылочных горлышек
- `RAM stat` — если на сервере запущено много процессов, то они могут занять все ресурсы оперативной памяти и запустить механизм своппинга, которых сильно замедляет отклик системы

Метрики производительности:

- `Response time` — для определения момента когда в системе возникают задержки стоит отслеживать время отклика системы,
- `Error rate` — количество ошибок.

Метрики очереди:

- `Queue size` — Текущее количество сообщений/задач в очереди.Помогает обнаруживать накопление задач (например, если потребители не успевают обрабатывать сообщения).Пороговые значения: Если размер очереди растет — возможны проблемы с обработчиками или нехватка ресурсов.
- `Queue growth rate` — Изменение размера очереди за единицу времени (например, сообщений/секунду).Резкий рост может указывать на сбои в потребителях или аномальный входящий трафик.
- `Message age` — Время, которое сообщение находится в очереди до обработки.Высокий возраст — признак медленной обработки или "зависших" задач.
- `Consumer lag` — Разница между последним отправленным и последним обработанным сообщением (актуально для Kafka, RabbitMQ).Большой lag — сигнал о проблемах с потребителями (например, нехватка CPU/RAM).
- `Processing time` — Сколько времени занимает обработка одного сообщения.Резкое увеличение может указывать на проблемы в алгоритмах или БД.
- `Rejected messages` — Количество сообщений, которые не были приняты в очередь (например, из-за переполнения). Важно для настройки политик retry и dead-letter queues.

Бизнесс метрики:

- Процент зависших заказов
- Число новых заказов за сутки.
- Среднее время прохождения заказа от оформления до готовности.

Стандартным и хорошо зарекомендовавшим себя инструментом мониторинга с большим количеством настраиваемых метрик является Grafana.

[<- На главную страницу](../ReadMe.md)
