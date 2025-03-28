### Анализ проблем и рисков
1. Рост нагрузки на ins-product-aggregator. Увеличение количества страховых компаний с 5 до 10 приведет к увеличению времени выполнения запросов и нагрузки на сервис.
2. Синхронный запрос к ins-product-aggregator может стать узким местом.
3. Задержки и ошибки при получении данных от страховых компаний, могут привести к каскадному сбою core-app и ins-comp-settlement, если не получент ответ от ins-product-aggregator.
4. Локальные реплики данных в core-app-db  и ins-comp-settlement могут быть неактуальными из-за разной частоты обновления.
5. Потеря данных в случае ошибки получения ответа ins-comp-settlement от core-app.


Для взаимодействия между core-app и ins-comp-settlement (получение данных об оформленных страховках), необходимо использовать паттерн transactional outbox.
При оформлении страховки, данные записывают в таблицу outbox, читаются оттуда отдельным процессом и публикуются в Kafka.