# JavaScript (TypeScript)

TODO

- Code style (eslint)
- Весь код пишем в TypeScript Расширение файлов с jsx кодом — *.tsx, без — *.ts Придерживайтесь следующего стиля jsx кода

```tsx
<div
    /* больше одного свойства — пишем в несколько строк */
    key={index}
    className={bem.element('article-item')}
>
    <h3>
        /* текст внутри тегов пишем в новой строке */
        {item.title}
    </h3>
    /* одно свойство — пишем в одну строку */
    <span className={bem.element('article-comments-count')}>
        /* все строки оборачиваем в __('...') для локализации */
        /* фразу (ключ) нельзя переносить на новую строку */
        /* фраза должна быть целой строкой */
        {__('Количество комментариев: {count}', {
            count: item.commentsCount,
        })}
    </span>
</div>
```

- Используем только функциональные компоненты (даже если в старых проектах встречаете классовый подход — новые компоненты
- пишем как функциональные, на хуках)
- любые колбеки внутри компонента — оборачиваем в useCallback()
- любые объекты, передаваемые в пропсы — объявляем выше и оборачиваем в useMemo()
- выносите большие функции вне компонентов, если они не зависят от большого числа переменных из замыкания всегда
- прописывайте deps для useEffect(), useCallback(), useMemo() и так далее. Настройте, чтобы IDE автоматически проставляла
- эти зависимости (см. раздел “Настройка IDE”, “save --fix”)
- при проверке длины массива — ожидайте, что вместо массива может быть null, ставьте знак вопроса перед точкой — if (items?.length > 0) { … } не забывайте для списков прописывать key
