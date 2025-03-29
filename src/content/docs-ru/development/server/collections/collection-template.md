# Шаблон Collection

<Alert>
📢 Шаблон Collection планируется к выпуску в четвертом квартале 2022 года.
</Alert>

В реальных бизнес-сценариях различные collection могут иметь свои собственные правила инициализации и бизнес-логику. NocoBase предоставляет шаблоны Collection для решения подобных проблем.

## Обычная таблица

```ts
db.collection({
  name: 'posts',
  fields: [
    {
      type: 'string',
      name: 'title',
    },
  ],
});
```

## 树结构表

```ts
db.collection({
  name: 'categories',
  tree: 'adjacency-list',
  fields: [
    {
      type: 'string',
      name: 'name',
    },
    {
      type: 'string',
      name: 'description',
    },
    {
      type: 'belongsTo',
      name: 'parent',
      target: 'categories',
      foreignKey: 'parentId',
    },
    {
      type: 'hasMany',
      name: 'children',
      target: 'categories',
      foreignKey: 'parentId',
    },
  ],
});
```

## Таблица наследования родительско-дочерних отношений

```ts
db.collection({
  name: 'a',
  fields: [],
});

db.collection({
  name: 'b',
  inherits: 'a',
  fields: [],
});
```

## Дополнительные шаблоны

Например, таблица календаря, где каждая инициализированная таблица требует наличия специальных полей cron и exclude, и определение таких полей выполняется с помощью шаблонов.

```ts
db.collection({
  name: 'events',
  template: 'calendar',
});
```
