**Типизация**

```typescript
type FindOneOptions = Omit<FindOptions, 'limit'>;
```

**Параметры**

Большинство параметров совпадают с методом `find()`. Отличие заключается в том, что метод `findOne()` возвращает только одну запись, поэтому параметр `limit` не требуется, и при выполнении запроса лимит всегда равен `1`.