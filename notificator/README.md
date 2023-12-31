## Задание

Вам предстоит проверить на практике свои навыки по разбору требований и выработке структуры компонентов системы.

Результатом вашей работы должен стать документ (в формате PDF, Word или Markdown) в котором будет описан **Технический Проект** системы, которую вам предстоит разработать. Изложите описание структуры компонентов, схемы их взаимодействия в рамках модели C4 (вплоть до уровня C3). 

При необходимости ссылайтесь на различные справочные или исследовательские материалы. Для простоты мы представим, что заказчик описал данные почти идеально (т.е. ссылайтесь на внешние источники, как на фрагменты бизнес-требований)

Принятые архитектурные решения разместите в специальной форме ADR (о нём мы расскажем чуть ниже). К итоговому документу необходимо приложить архитектурные схемы: схему подсистемы на разных уровнях модели C4 и графическую схему взаимодействия конкретных компонентов.

## Задание

Вам необходимо описать технический проект подсистемы (обособленной части системы) отправки оповещений о событиях. От подсистемы требуется возможность рассылать оповещения по трём каналам связи данных типов: 

- e-mail;
- SMS;
- push-уведомления для мобильных устройств.

Так же от подсистемы требуется принимать от пользователя системы настройки по каждому каналу связи для рассылки уведомлений (вкл/выкл, адрес/номер/логин) и учитывать эти настройки при последующих рассылках. 

- **Подсказки**
    - Источником сообщений и их содержимого будут являться другие подсистемы, следовательно необходим подходящий способ взаимодействия с ними.
    - Для хранения пользовательских настроек необходимо предусмотреть хранилище, связанное с пользовательской информацией в других подсистемах.
    - Предусмотрите возможность мультиплексирования (рассылки одного сообщения по всем каналам группе клиентов) так и адресной отправки в каждый канал связи конкретным адресатам.
    - Учтите природу разных каналов связи, их потенциальные ограничения и пропускную способность (например, ограничение количества символов в СМС, стоимость СМС, лимиты пуш-очередей провайдеров, возможность попадания e-mail в спам и т.д.), введите в систему необходимые компоненты для обработки таких особенностей.
    - Обоснуйте особенности компонентов ссылками на внешние источники

## Необходимые пояснения

Так как в задании на подсистему изначально не уточняется природа оповещений, требуется предусмотреть в новой подсистеме такие компоненты и паттерны, которые не просто свяжут подсистему-источник оповещения с конкретным сервисом отправки СМС или e-mail, а позволят источнику работать в клиент-сервисной модели и переложить всю заботу о предметной области “оповещения пользователя” на разрабатываемую вами подсистему. 

> Мы предполагаем, что вы обязательно должны включить в подсистему такие компоненты как Хранилище и Очередь и при этом обосновать данное решение.

## Требования к документации

Отдельно остановимся на форме документа, который станет результатом вашей работы. Мы предполагаем, что вы последовательно опишете детали будущей подсистемы уведомлений в [шаблоне технического проекта, раздел "Целевая архитектура"](software_architecture_document.md#целевая-архитектура) с диаграмами в нотации C4. Так как по мере детализации схемы проектируемой системы вы будете принимать архитектурные решения (используемые дизайн-паттерны, технологии), вам необходимо изложить эти решения в виде **ADR**.  

> **Запись о решении по архитектуре (ADR)** — это документ, в котором фиксируется ключевое архитектурное решение с его контекстом, обоснованием и последствиями. 

ADR сохраняет логику за определенными архитектурными решениями, предоставляя исторический контекст для будущего анализа. Это помогает лучше понять эволюцию архитектуры системы и гарантирует, что новые участники проекта смогут осмыслить принятые решения. 

Для документа ADR принят ряд стандартных атрибутов (часть из которых являются обязательными), они приведены в таблице ниже.

| Атрибут | Обязательно | Описание |
| --- | --- | --- |
| ID | Да | Уникальный идентификатор решения. например, ADR-001 |
| Дата | Да | Дата принятия решения. например, 15 сентября 2023 г. |
| Статус | Да | Статус решения. Возможны варианты: Предложено/Принято/Отклонено/Заменено/Устарело |
| Участники | Да | Люди, участвующие в принятии решения или влияющие на него. например, Иван Иванов, Вадим Петров |
| Решения | Да | Краткое изложение сути решения. например, принятые микросервисы для серверной разработки. |
| Контекст | Нет | Предыстория или обоснование решения. например, потребность в масштабируемой и поддерживаемой архитектуре |
| Последствия | Нет | Результаты или последствия решения. например, улучшенная модульность, но повышенная сложность межсервисного взаимодействия. |

Набор документов ADR сформированных в ходе проектирования, разработки и эксплуатации системы называют ADL, то есть перечень документов ADR. Он представляет собой таблицу со ссылками на конкретные документы ADR.

| ID | Дата | Статус | Участники         | Решения             |
| --- | --- | --- |-------------------|---------------------|
| [ID] | [Дата] | [Статус] | [Имя] <br/> [Имя] | [Решение] <br/>[Решение] |

Ниже мы приведём пример ADR для иллюстрации того, как должен быть оформлен результат выполнения данного задания.

## Пример ADR

**Идентификатор:** ADR-001

**Дата:** 14 сентября 2023 г.

**Статус:** Принято

**Участники:**

- Иван Иванов
- Вадим Петров

**Решения:**

- Переход от монолитной к микросервисной архитектуре
- Используем k8s для оркестрации

**Контекст:**

Текущая монолитная структура нашей бэкенд-системы является узким местом, особенно с
учетом роста трафика и спроса пользователей. Возникла необходимость увеличить
масштабируемость, отказоустойчивость и гибкость системы. После анализа потенциальных решений мы рассмотрели возможность перехода к архитектуре микросервисов.

**Последствия:**

**Положительные**

- Отдельные компоненты могут масштабироваться независимо в зависимости от спроса.
- Более быстрые циклы развертывания, так как команды могут обновлять отдельные	сервисы без воздействия на всё приложение.
- Повышенная	отказоустойчивость: при сбое одного	сервиса это не обязательно влияет на	всю систему.

**Отрицательные**

- Необходимость управления, мониторинга и логирования для многих сервисов в отличие от одного приложения.
- Риск увеличения задержек из-за интеграции между сервисами, если она	спроектирована некорректно.
- Требуются более продвинутые инструменты для оркестрации и навыки, что может	потребовать дополнительного обучения.

Мы также приложим шаблон документа ADR, вы можете использовать его для оформления данного задания и в своей дальнейшей работе.

[adr_template.md](static/adr_template.md)

## Регламент сдачи работ 

1) склонируйте [репозиторий](https://github.com/Yandex-Practicum/architect-practice-tpl) и перейдите в директорию *./notificator*. Создайте и перейдите в ветку `hw_2`.
2) откройте [технический проект](software_architecture_document.md) и ознакомьтесь с текущей архитектурой и примером описания С1 уровня в разделе "Целевая архитектура".
3) допишите раздел "Целевая архитектура" согласно С4 нотации. Схемы подсистем на разных уровнях модели и схему взаимодействия конкретных компонентов (схемы вплоть до С3) следует добавить в документ технического проекта в виде картинок. Мы рекомендуем использовать `drawio` для создания диаграм и экспортировать их в формат `svg`
4) архитектурно-значимые решения необходимо обосновать в форме ADR согласно [шаблону](static/adr_template.md). ADR рекомендуется добавить в корень репозитория и добавить ссылки на них в технический проект.
5) для сдачи работы создайте pull-request с упомянутыми выше артефактами и опубликуйте ссылку на него во вкладке "Ревью"
![сдача работ](static/image.png)

## КОНТЕНТ ДЛЯ ПОДДЕРЖКИ, НЕ ЗАЛИВАТЬ 
### Как проверять работы 

В соответствии с ОР:
* умеет превратить бизнес-требования от заказчика в ТехПроект, если:
  * документ содержит описание компонентов системы
  * документ содержит графическую схему взаимодействия компонентов 
  * документ содержит ссылку на описание специфики данных (как бы на бизнес-требования)

* умеет выбрать необходимые компоненты и паттерны, если:
  * документ содержит схемы разных уровней с паттернами и с конкретными компонентами
  * документ содержит текстовое обоснование выбора компонентов в виде ADR
  * документ содержит ряд гипотез о выбранных компонентах в формате "если... , то..."
  * документ содержит критерии оценки прототипа, который должен быть создан по имеющейся схеме
  
* умеет спроектировать взаимодействие между компонентами, если:
  * документ содержит ссылку на материалы об особенностях тех или иных компонентов
  * документ содержит краткое описание использованных паттернов и описание их имплементации
  * документ содержит сопоставление бизнес-сущностей (процессы, данные) и технических сущностей (компоненты, паттерны, внешние системы, взаимодействие)
  * документ содержит описание ограничений, налагаемых тем или иным конкретным компонентом или спецификой данных 
  
В целом, к результату, так как он будет в текстовой форме, надо относиться с точки зрения здравого смысла.

Основными моментами, которые стоит проверить, является обоснованность решений и их не-абсурдность. То есть, внешние системы не пишут в нашу базу, внешние системы не лезут напрямую к смс-рассылке, наша система не взаимодействует с сервисами без прокси или хоть какой-то защиты, наша система не шлёт все полученные сообщения разом всем внешним сервисам, наша система не лезет за рамки задачи и не пытается Решить Все Вопросы вместо внешних систем (мы не проектируем внешние смс и email-гейты).

Горизонтальное масштабирование нужно проверить, как изменится схема, если продублировать какие-то узлы через балансер, и т.д. Отработает ли система при отключении внешего интернета после восстановления, то есть наличие функции хранимых с гарантией сообщений (плюс, если она будет опциональна). 
