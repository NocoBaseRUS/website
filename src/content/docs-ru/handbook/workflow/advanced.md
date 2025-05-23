# Продвинутое использование

В разделе [Быстрый старт](./index) мы уже рассмотрели основы использования рабочих процессов. В этой статье более подробно описываются некоторые продвинутые концепции.

## Использование переменных

Как и переменные в языках программирования, **переменные** в рабочем процессе являются важным инструментом для связывания и организации потока данных.

При выполнении каждого узла после запуска рабочего процесса некоторые параметры могут использовать переменные. Источником переменных служат данные из предшествующих узлов, включая следующие категории:

- Данные контекста триггера: при срабатывании операции или событий таблицы данных объект данных одной строки может использоваться всеми узлами.
- Данные вышестоящих узлов: результаты данных завершенных узлов на момент достижения текущего узла.
- Локальные переменные: когда узел находится внутри специальных структур ветвления (например, циклов), можно использовать локальные переменные этой ветки. Например, в цикле можно использовать данные текущей итерации.
- Системные переменные: встроенные системные параметры, такие как текущее время.

Мы уже неоднократно использовали функциональность переменных в разделе [Быстрый старт](./index). Например, в вычислительном узле мы можем использовать переменные для ссылки на данные контекста триггера для выполнения вычислений:

![Вычислительный узел использует функции и переменные](https://static-docs.nocobase.com/837e4851a4c70a1932542caadef3431b.png)

В узле обновления данных мы используем данные контекста триггера в качестве переменной для фильтрации условий и ссылаемся на результат вычислительного узла в качестве значения поля для обновления:

![Переменные в узле обновления данных](https://static-docs.nocobase.com/2e147c93643e7ebc709b9b7ab4f3af8c.png)

Внутренняя структура переменной представляет собой JSON. Обычно можно обращаться к определённым частям данных по пути JSON. Поскольку многие переменные основаны на структуре таблиц данных NocoBase, связанные данные будут организованы в древовидную структуру по уровням в виде свойств объекта. Например, можно выбрать значение определённого поля из связанных данных запроса. Если связанные данные имеют отношение "один-ко-многим", то переменная может быть массивом.

Выбор переменной обычно заканчивается выбором последнего уровня — атрибута значения, который обычно является простым типом данных, например числом или строкой. Однако, если в иерархии переменной есть массив, то конечный атрибут также будет отображен как массив. Только соответствующие узлы, поддерживающие массивы, смогут корректно обрабатывать такие данные. Например, в вычислительном узле некоторые движки расчетов имеют специальные функции для работы с массивами; в узле цикла объект цикла также может быть выбран как массив.

Приведём пример. Когда узел запроса возвращает несколько записей, результат узла будет массивом, содержащим несколько однородных строк данных:

```json
[
  {
    "id": 1,
    "title": "Заголовок 1"
  },
  {
    "id": 2,
    "title": "Заголовок 2"
  }
]
```

Однако, при использовании в последующих узлах, если выбранная переменная имеет вид `Данные узла/Узел запроса/Заголовок`, то результатом будет массив, содержащий значения соответствующего поля.

```json
["Заголовок 1", "Заголовок 2"]
```

Если это многомерный массив (например, поле связи многие-ко-многим), то результатом будет одномерный массив, содержащий значения соответствующего поля после его "выравнивания" (уплощения).

## План выполнения (история)

После запуска каждого рабочего процесса создается соответствующий план выполнения для отслеживания процесса выполнения этой задачи. У каждого плана выполнения есть значение состояния, которое указывает текущий статус выполнения. Этот статус можно просмотреть как в списке, так и в деталях истории выполнения:

![Состояние плана выполнения](https://static-docs.nocobase.com/d4440d92ccafac6fac85da4415bb2a26.png)

Когда все узлы в главной ветке процесса достигают конечной точки с состоянием "завершено", весь план выполнения завершается со статусом "завершено". Если узел в главной ветке процесса переходит в одно из терминальных состояний, таких как "неудача", "ошибка", "отмена" или "отклонение", то весь план выполнения **преждевременно завершается** с соответствующим статусом. Если узел переходит в состояние "ожидание", выполнение всего плана приостанавливается, но он остается в состоянии "в процессе", пока узел в состоянии ожидания не будет возобновлен. Разные типы узлов обрабатывают состояние ожидания по-разному: например, ручные узлы ожидают ручной обработки, а узлы задержки ожидают наступления определенного времени.

Состояния плана выполнения представлены в следующей таблице:

| Состояние | Соответствующее состояние последнего узла в главной ветке | Описание                                                                 |
| --------- | ------------------------------------------------------- | ------------------------------------------------------------------------ |
| В очереди | -                                                       | Процесс запущен, создан план выполнения, который ожидает назначения планировщиком. |
| В процессе | Ожидание                                                | Узел требует приостановки и ожидает дополнительного ввода или обратного вызова для продолжения. |
| Завершено | Завершено                                               | Все узлы выполнились в соответствии с ожиданиями, без каких-либо проблем.       |
| Неудача   | Неудача                                                 | Сбой из-за невыполнения условий конфигурации узла.                        |
| Ошибка    | Ошибка                                                  | Узел столкнулся с непредвиденной программной ошибкой и преждевременно завершился. |
| Отмена    | Отмена                                                  | Ожидающий узел был отменен внешним администратором процесса.               |
| Отклонено | Отклонено                                               | В узле ручной обработки было отклонено дальнейшее выполнение процесса.      |

В примере из раздела [Быстрый старт](./index) мы уже узнали, что проверка деталей истории выполнения рабочего процесса позволяет убедиться, что все узлы выполнились корректно, а также проверить состояние выполнения каждого узла и данные результата. В некоторых сложных процессах и узлах результат может содержать несколько значений; например, результат узла цикла:

![Результаты многократного выполнения узла](https://static-docs.nocobase.com/bbda259fa2ddf62b0fc0f982efbedae9.png)

:::info{title=Примечание}
Рабочие процессы могут запускаться параллельно, но их выполнение происходит поочередно, даже если несколько рабочих процессов запущены одновременно. Поэтому, если появляется статус "в очереди", это означает, что другой рабочий процесс выполняется в данный момент, и нужно дождаться своей очереди.

Статус "в процессе" только указывает на то, что выполнение плана началось и, как правило, приостановлено из-за состояния ожидания внутри узла. Это не означает, что выполнение этого плана блокирует выполнение других. Другие планы со статусом "в очереди" всё равно могут быть запущены планировщиком.
:::

## Состояния выполнения узлов

Состояние плана выполнения определяется состоянием выполнения каждого узла. После срабатывания триггера каждый узел, выполнившись, переходит в определенное состояние, которое решает, будет ли процесс продолжаться. Обычно после успешного выполнения узла происходит переход к следующему узлу, пока все узлы не будут выполнены последовательно или пока выполнение не будет прервано. При встрече управляющих узлов (ветвление, цикл, параллельные ветки, задержка и т.д.) направление выполнения определяется условиями конфигурации узла и данными контекста выполнения.

Возможные состояния выполнения узла представлены в следующей таблице:

| Состояние | Является ли конечным | Преждевременное завершение | Описание                                                                 |
| --------- | :------------------: | :------------------------: | ------------------------------------------------------------------------ |
| Ожидание  |          Нет         |             Нет            | Узел приостановлен и ожидает дополнительных данных или обратного вызова для продолжения. |
| Завершено |          Да          |             Нет            | Выполнено успешно без проблем; процесс продолжается до завершения всех узлов.      |
| Неудача   |          Да          |             Да             | Сбой из-за невыполнения условий конфигурации узла.                        |
| Ошибка    |          Да          |             Да             | Произошла непредвиденная программная ошибка, выполнение преждевременно завершено.   |
| Отмена    |          Да          |             Да             | Ожидающий узел был отменен внешним администратором процесса.               |
| Отклонено |          Да          |             Да             | В узле ручной обработки выполнение было отклонено, дальнейшие шаги пропущены.      |

За исключением состояния "ожидание", все остальные состояния являются конечными. Только узлы со статусом "завершено" позволят процессу продолжиться; любое другое состояние приведет к преждевременному завершению всего рабочего процесса. Когда узел находится внутри ветвления (параллельные ветви, условные узлы, циклы и т.д.), конечное состояние передается на обработку узлу, открывающему ветвление, который затем управляет дальнейшим потоком всего процесса.

Например, если мы используем условный узел в режиме "продолжить только при 'да'", то при выполнении, если результат будет "нет", весь процесс завершится досрочно с состоянием "неудача", и последующие узлы выполняться не будут, как показано на рисунке ниже:

![Сбой выполнения узла](https://static-docs.nocobase.com/993aecfa1465894bb574444f0a44313e.png)

:::info{title=Примечание}
Все состояния завершения, кроме "завершено", могут считаться неудачей, но причины неудач различны. Дополнительную информацию о причинах сбоя можно узнать, проверив результат выполнения узла.
:::

## Режимы выполнения

Рабочие процессы выполняются либо асинхронно, либо синхронно в зависимости от типа триггера, выбранного при создании. Асинхронный режим означает, что процесс после срабатывания события попадает в очередь рабочего процесса и выполняется поочередно планировщиком в фоновом режиме. В синхронном режиме процесс запускается немедленно после срабатывания и сразу же возвращает ответ после завершения.

События таблиц данных, события после операции, события пользовательских операций, события планировщика задач и события утверждений по умолчанию выполняются асинхронно, тогда как события перед операцией выполняются синхронно. Таблицы данных и события форм поддерживают оба режима, и выбор режима можно сделать при создании рабочего процесса:

![Синхронный режим_Создание синхронного рабочего процесса](https://static-docs.nocobase.com/39bc0821f50c1bde4729c531c6236795.png)

:::info{title=Примечание}
Рабочие процессы в синхронном режиме ограничены своим режимом и не могут использовать узлы, которые создают состояние "ожидание", такие как "ручная обработка".
:::

## Автоматическое удаление истории

Когда рабочий процесс запускается часто, можно настроить автоматическое удаление истории, чтобы уменьшить отвлекающие факторы и снизить нагрузку на базу данных.

В окне создания или редактирования рабочего процесса можно настроить автоматическое удаление истории для соответствующего процесса:

![Настройка автоматического удаления истории](https://static-docs.nocobase.com/b2e4c08e7a01e213069912fe04baa7bd.png)

Автоматическое удаление может быть настроено в зависимости от состояния выполнения. В большинстве случаев рекомендуется отмечать только состояние "завершено", чтобы сохранить записи о неудачных выполнениях для последующего анализа проблем.

Рекомендуется не включать автоматическое удаление истории при отладке рабочего процесса, чтобы проверять логику выполнения через историю выполнений и убедиться, что она работает как ожидается.

:::info{title=Примечание}
Удаление истории рабочих процессов не уменьшает количество уже выполненных запусков.
:::

## Версии рабочих процессов

После того как настроенный рабочий процесс был запущен хотя бы один раз, любые изменения в конфигурации или узлах должны производиться через создание новой версии. Это гарантирует, что при просмотре истории выполнения уже сработавших процессов изменения в будущем не повлияют на прошлые данные.

На странице конфигурации рабочего процесса можно просмотреть существующие версии рабочего процесса через меню "Версии" в правом верхнем углу:

![Просмотр версий рабочего процесса](https://static-docs.nocobase.com/ad93d2c08166b0e3e643fb148713a63f.png)

В меню дополнительных действий («…») справа можно выбрать опцию "Копировать в новую версию" на основе текущей просматриваемой версии:

![Копирование рабочего процесса в новую версию](https://static-docs.nocobase.com/2805798e6caca2af004893390a744256.png)

После копирования в новую версию нажмите переключатель "Включить"/"Отключить", чтобы перевести соответствующую версию в активное состояние, после чего новая версия рабочего процесса начнет действовать.

Если необходимо вернуться к старой версии, выберите её из меню версий и снова используйте переключатель "Включить"/"Отключить", чтобы активировать её. После этого последующие запуски будут использовать процесс этой версии.

Чтобы отключить рабочий процесс полностью, переведите переключатель "Включить"/"Отключить" в состояние "Отключено". После этого рабочий процесс больше не будет запускаться.

:::info{title=Примечание}
В отличие от функции "Копировать" рабочий процесс в списке управления рабочими процессами, "Копирование в новую версию" оставляет рабочий процесс в той же группе, просто добавляя новую версию. Однако "копирование" рабочего процесса создает совершенно новый рабочий процесс, который не связан с предыдущими версиями, а количество выполнений обнуляется.
:::