# Блок формы

## Введение

Блок формы является важным компонентом для создания интерфейсов ввода и редактирования данных. Он обладает высокой степенью настройки, использует соответствующие компоненты на основе модели данных для отображения необходимых полей. С помощью правил взаимодействия блок формы может динамически отображать поля. Кроме того, его можно интегрировать с рабочими процессами для автоматического запуска процессов и обработки данных, что повышает эффективность работы или позволяет организовать логику.

## Добавление блока

<video width="100%" height="440" controls>
      <source src="https://static-docs.nocobase.com/20240416215917.mp4" type="video/mp4">
</video>

## Параметры конфигурации блока

![20240416220148](https://static-docs.nocobase.com/20240416220148.png)

### Правила взаимодействия

Управление поведением полей формы с помощью правил взаимодействия.

![20240416220254](https://static-docs.nocobase.com/20240416220254.png)

Более подробная информация доступна в разделе [Правила взаимодействия](/handbook/ui/blocks/block-settings/linkage-rule).

### Шаблон данных формы (поддерживается только для форм добавления новых данных)

Цель шаблона данных формы — упростить процесс ввода данных и повысить эффективность. Через фильтрацию диапазона данных выбирается одна или группа записей в качестве шаблона, и выбранный шаблон будет автоматически заполнен как значения по умолчанию в форме.

![20240408143719](https://static-docs.nocobase.com/20240408143719.png)

![20240426212024](https://nocobase-docs.oss-cn-beijing.aliyuncs.com/20240426212024.png)

1. Отфильтруйте одну или группу записей, чтобы использовать их в качестве шаблона.
2. Выберите поле заголовка для идентификации шаблонных данных.
3. Установите флажки для полей шаблона, и выбранные поля будут автоматически заполнены в форму.

#### Синхронизация полей формы

- Поля, уже настроенные в текущем блоке формы, автоматически анализируются и используются как поля шаблона;
- Если поля блока формы были изменены (например, компоненты полей отношений были скорректированы), можно снова открыть конфигурацию шаблона и нажать кнопку синхронизации формы, чтобы обеспечить согласованность между формой и шаблоном;

#### В записях, выбранных в качестве шаблона данных, будут отфильтрованы следующие поля:
- Первичный ключ
- Внешний ключ
- Поля, которые не могут дублироваться
- Поля сортировки
- Поля автоматической кодировки
- Пароли
- Создатель
- Дата создания
- Последний пользователь, обновивший запись
- Дата последнего обновления

#### Для полей отношений
- Обычные поля, а также поля отношений hasOne и hasMany копируются;
- Поля отношений belongsTo и belongsToMany ссылаются. При определенных изменениях (например, при переходе от select к sub-form) ссылка может превратиться в копирование. После превращения в копирование все поля становятся необязательными;

#### Примеры использования

Описание сценария: платформа электронной коммерции часто добавляет новые товары, и многие атрибуты этих новых товаров могут быть похожи или идентичны существующим товарам.

Решение: выбрать существующий товар в качестве шаблона и использовать его атрибуты в качестве шаблона данных формы. При создании нового товара пользователь может применить этот шаблон, что позволит быстро скопировать информацию о свойствах шаблонного товара в новый товар, повышая эффективность ввода новых товаров.

- Создание шаблона товарной акции

![20240408145855](https://static-docs.nocobase.com/20240408145855.png)

- Быстрый ввод акционных товаров

<video width="100%" height="440" controls>
      <source src="https://static-docs.nocobase.com/20240408150250.mp4" type="video/mp4">
</video>

- [Редактирование заголовка блока](/handbook/ui/blocks/block-settings/block-title)
- [Сохранение как шаблон блока](/handbook/ui/blocks/block-settings/block-template)

## Настройка полей

### Поля текущей таблицы

![20240416230739](https://static-docs.nocobase.com/20240416230739.png)

### Поля связанных таблиц

Поля связанных таблиц доступны только для чтения в форме и обычно используются совместно с полями отношений. Они могут отображать значения нескольких полей связанных данных.

![20240416230811](https://static-docs.nocobase.com/20240416230811.png)

<video width="100%" height="440" controls>
      <source src="https://static-docs.nocobase.com/20240416231152.mp4" type="video/mp4">
</video>

Параметры настройки полей формы можно найти в разделе [Поля формы](/handbook/ui/fields/generic/form-item).

## Настройка операций

![20240417115249](https://static-docs.nocobase.com/20240417115249.png)

- [Отправка](/handbook/ui/actions/types/submit)
- [Сохранение данных](/handbook/ui/actions/types/save-record)
- [Пользовательский запрос](/handbook/action-custom-request)
- [Запуск рабочего процесса](/handbook/workflow/manual/triggers/custom-action)