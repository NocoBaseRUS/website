# Настройка диапазона данных

## Введение

Настройка диапазона данных означает определение условий фильтрации по умолчанию для блока данных. Пользователи могут гибко настраивать диапазон данных блока в зависимости от различных потребностей.

## Руководство по использованию

![20240407180322](https://static-docs.nocobase.com/20240407180322.png)

Поля фильтрации поддерживают выбор полей текущей таблицы и полей связанных таблиц (до трех уровней отношений).

![20240422113637](https://static-docs.nocobase.com/20240422113637.png)

### Операторы

Разные типы полей поддерживают разные операторы. Например, текстовые поля поддерживают операторы «равно», «не равно», «содержит» и т. д., числовые поля поддерживают операторы «больше», «меньше» и т. д., а поля дат поддерживают операторы «в пределах диапазона», «до определенной даты» и т. д.

![20240424154003](https://static-docs.nocobase.com/20240424154003.png)

### Статические значения

Пример: статус заказа равен «Отправлено».

<video width="100%" height="440" controls>
      <source src="https://static-docs.nocobase.com/20240415204206.mp4" type="video/mp4">
</video>

### Переменные значения

Пример: «Дата отправки» раньше «Вчера».

![20240422090134](https://static-docs.nocobase.com/20240422090134.png)

<video width="100%" height="440" controls>
      <source src="https://static-docs.nocobase.com/20240415214709.mp4" type="video/mp4">
</video>

Более подробная информация о переменных доступна в разделе [Переменные](/handbook/ui/variables).