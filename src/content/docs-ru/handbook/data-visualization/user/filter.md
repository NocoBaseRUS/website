# Блок фильтрации

Блок фильтрации в блоке диаграмм используется для динамической фильтрации нескольких диаграмм внутри текущего блока диаграмм.

## Включение/Отключение

В блоке диаграмм нажмите «Добавить блок» (Add block) — «Фильтр» (Filter), чтобы включить или отключить блок фильтрации.

![](https://static-docs.nocobase.com/d0e6b116952fa6b719acb0f858b432c3.png)

## Настройка полей фильтрации

### Поля таблицы данных

Для диаграмм, используемых в текущем блоке диаграмм, можно выбрать поля соответствующей таблицы данных, чтобы создать поле формы фильтрации.

![](https://static-docs.nocobase.com/e2ef150e9beb8c78004d9049a7536219.png)

Можно настроить поля формы:

![](https://static-docs.nocobase.com/215f0b996e69bf2d5b99746e6d521c3d.png)

- Настройка заголовка отображения поля
- Настройка описания поля
- Настройка оператора, применяемого при фильтрации этого поля  
  ![](https://static-docs.nocobase.com/d6a593a330d27da4ea78124dfdb8450d.png)

- Настройка значения по умолчанию для поля. Можно использовать переменные. Тип данных переменной должен соответствовать типу данных текущего поля.
  ![](https://static-docs.nocobase.com/37dee4008f3283db24d491fb8f0404fa.png)

  Например:

  - Настройка значения по умолчанию как ID текущего пользователя: после загрузки страницы автоматически фильтруются данные текущего пользователя.
  - Настройка значения по умолчанию как текущую дату: после загрузки страницы автоматически фильтруются данные за текущую дату.

### Пользовательские поля

В некоторых случаях может потребоваться использовать одно и то же поле фильтрации для различных полей в разных таблицах. Например, использование одного поля даты для фильтрации различных полей дат в разных таблицах. В таких случаях можно выбрать создание пользовательского поля.

![](https://static-docs.nocobase.com/87544594246453d175ef265030c0801a.png)

При добавлении пользовательского поля необходимо установить заголовок поля, выбрать компонент поля и выполнить соответствующие настройки. Также можно выбрать поле из таблицы данных, используемой в текущем блоке, чтобы автоматически применить метаданные конфигурации этого поля и избежать повторной настройки.

![](https://static-docs.nocobase.com/ef09136d674d4b7356e819350bcac804.png)

Чтобы использовать пользовательское поле фильтрации, нужно открыть соответствующую конфигурацию диаграммы, а затем добавить условия фильтрации в настройках запроса данных, используя переменные из «Текущего фильтра» (Current filter). Тип фильтруемого поля должен соответствовать типу пользовательского поля формы фильтрации.

![](https://static-docs.nocobase.com/f9f2487c4da4b2024af1556743beab6c.png)

Для пользовательских полей также можно настроить заголовок, описание и значение по умолчанию.

![](https://static-docs.nocobase.com/4a8feb12404f5cc5e74d589263307e5a.png)

## Настройка действий блока

- Фильтрация (Filter) — применение условий фильтрации.
- Сброс (Reset) — сброс формы фильтрации.
- Свернуть/Развернуть (Collapse / Expand) — свернуть в одну строку или развернуть в несколько строк.

![](https://static-docs.nocobase.com/8619ac90fa045b3a9c6d6610f7be1a81.png)