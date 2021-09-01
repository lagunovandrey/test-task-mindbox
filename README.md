# Тестовое задание

1. Решите задачу ниже.

Код должен открываться в браузере, без архивов, GitHub будет идеальным.

## Задача
Не ожидаем production-ready решения. Сделайте, как кажется правильным, опишите процесс поиска и принятые решения.
Опишите deployment для веб-приложения в kubernetes в виде yaml-манифеста. Оставляйте в коде комментарии по принятым решениям. Есть следующие вводные:

* у нас мультизональный кластер (три зоны), в котором пять нод
    * Весь кластер состоит из 5 worker node
    * мультизональный кластер (три зоны) - кластер расположен в трех зонах доступнсоти.
    * worker node в зонах доступности маркированы метками zone-NN (zone-1, zone-2, zone-3)
    * ingress на все зоны доступности
* приложение требует около 5-10 секунд для инициализации
    * проверить достаточно ли timeout для проверок у deployment
    * важно для масштабирования
* по результатам нагрузочного теста известно, что 4 пода справляются с пиковой нагрузкой
    * реплика сет можно поставить в 4
* на первые запросы приложению требуется значительно больше ресурсов CPU, в дальнейшем потребление ровное в районе 0.1 CPU. По памяти всегда “ровно” в районе 128M memory
    * cpu request 0,1 cpu limit 1 (что значит значительно больше?) 
    * по помяти используем запрос 128М
* приложение имеет дневной цикл по нагрузке – ночью запросов на порядки меньше, пик – днём
    * возможно авто масштабирование в обе стороны
* хотим максимально отказоустойчивый deployment
    * однозначно распределить по всем зонам доступности
    * всегда должно быть запущено 2 pod, для обеспечения гарантии ответа при падении одной ноды. В зависимости от приложения и определения доступности можно реализовать гарантированный ответ на балансере или ingress-controller. Например красивая страница-загушка при недостпности пода приложения.
* хотим минимального потребления ресурсов от этого deployment’а
    * это про литимы или про автомасштабирование?
