# SchemaInitializer

Используется для инициализации различных схем (schema). Новые схемы могут быть вставлены в любое место узла существующей схемы, включая:

```ts
{
  properties: {
    // beforeBegin 在当前节点的前面插入
    node1: {
      properties: {
        // afterBegin 在当前节点的第一个子节点前面插入
        // ...
        // beforeEnd 在当前节点的最后一个子节点后面
      },
    },
    // afterEnd 在当前节点的后面
  },
}
```

Ядро SchemaInitializer включает два компонента: `<SchemaInitializer.Button />` и `<SchemaInitializer.Item />`. Компонент `<SchemaInitializer.Button />` используется для создания кнопки выпадающего меню схемы, а элементы выпадающего меню представляют собой `<SchemaInitializer.Item />`.

### `<SchemaInitializerProvider />`

### `<SchemaInitializer.Button />`

### `<SchemaInitializer.Item/>`
