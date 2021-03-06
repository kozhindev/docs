# Верстка (БЭМ, SCSS)

### БЭМ

Вся стилизация сайтов, приложений и их компонентов делается только по
[методологии БЭМ](https://ru.bem.info/methodology/quick-start/) от Яндекса. Сама методология очень большая, но мы
используем от нее только основу — разделение на блоки, элементы и модификаторы. Такое использование иногда называют "
БЭМ-lite".

### Один БЭМ блок — один компонент

Каждый блок — один `*.scss` файл, который соответствует React компоненту. В итоге каждый компонент обычно выглядит как
папка с файлами:

```
- MyComponent // Папка компонента
  - MyComponent.scss // Файл со стилями для этого компонента
  - MyComponent.tsx // Функциональный React компонент
  - MyComponent.story.js // Storybook для компонента
  - index.ts // Файл, экспортирующий данный компонент - для того чтобы в других компонентах
      при импорте MyComponent не нужно было писать путь включающий папку (.../MyComponent/MyComponent)
```

### Именование селекторов

Все блоки именуются в CamelCase, а названия элементов, модификаторов и значений — маленькими буквами в kebab-case;

### Хук `useBem()`**

Во все проектах, где используется наш фреймворк Steroid (а это почти все проекты), мы используем хук `useBem()`,
чтобы задавать именования классов html элементам. Эта небольшая утилита позволяет более строго задавать формат
именований и упрощает прописывания модификаторов при наличии условий.

Хук `useBem()` первым аргументом принимает имя БЭМ блока, который совпадает с названием React компонента. На выходе он
возвращает функцию-объект `bem`, который имеет следующие методы:

- `bem('a', 'b', false && 'c')` -> `a b` - При использовании как функции, может объединять названия классов
- `bem.block()` -> `MyComponent` - Блок
- `bem.element('title')` -> `MyComponent__title` - Элемент
- `bem.element('title', {color: 'info'})` -> `MyComponent__title MyComponent__title_color_info` - Модификатор со строковым значением
- `bem.element('title', {active: true})` -> `MyComponent__title MyComponent__title_active` - Модификатор с булевым значением
- `bem.element('title', 'active')` -> `MyComponent__title MyComponent__title_active` - То же, что и предыдущий

Более наглядно использование хука можно увидеть в следующем примере:

```tsx
export default function MyComponent(props) {
    const bem = useBem('MyComponent'); // Вызов утилиты с объявление имени блока

    return (
        <div className={bem.block()}> {/* MyComponent */}
            <h1 className={bem.element('title')}> {/* MyComponent__title */}
                {props.title}
            </h1>
            <div className={bem.element('card', {active: props.isActive})}> {/* MyComponent__card MyComponent__card_active */}
                <div className={bem( // Объединение нескольких селекторов с проверкой на их существование
                    bem.element('card-description'), // MyComponent__card-description
                    props.isError && 'text-error', // Глобальный класс 'text-error' из фреймворка bootstrap
                    props.descriptionClassName, // Кастомный класс из родительского компонента
                )}>
                    {props.description}
                </div>
                <p className={bem.element('card-hint', {color: 'info'})}> {/* MyComponent__card-hint MyComponent__card-hint_color_info */}
                    ...
                </p>
                <img
                    className={bem.element('image', 'thumbnail')} // MyComponent__image MyComponent__image_thumbnail
                    src={props.url}
                    alt={props.title}
                />
            </div>
        </div>
    );
}
```

### Виды компонентов

В проекте мы используем такие виды React компонентов:

1. Страница - компонент, который представляет собой отдельную страницу сайта (например главная страница, страница
   контактов и др.)

   Название компонентов такого типа всегда оканчивается на "Page", например "IndexPage", "DemoPage", ... Такие
   компоненты располагаются в `src/routes`.

2. Отдельные view-компоненты - блоки, на которые разбивается страница. Такие блоки используются только в родительском
   компоненте и нигде более. Например, на странице "ContactsPage" может потребоваться для упрощения кода вынести блок с
   телефонами в отдельный компонент "PhoneNumbers"

   Такие компоненты располагаются в подпапке `views` родительского компонента. Например
   `src/routes/ContactsPage/views/PhoneNumbers`

3. Общие компоненты - используются в различных родительских компонентах. Например, компонент "SideMenu" может
   использоваться на нескольких страницах.

   Такие компоненты располагаются в `src/shared`, например `src/shared/SideMenu`

### Storybook

Для каждого компонента должен быть создан storybook-файл `*.story.js`

В нем необходимо представить компонент в различных вариантах, чтобы можно было в едином месте просмотреть все возможные
виды компонента с разными настройками.

Например, если компонент - кнопка, то нужно в storybook-файле показать кнопку обычную, в нажатом виде, с наведением
курсора, и пр.

### Pixel Perfect

По-умолчанию, у нас предполагается верстка в режиме "пиксель перфект", когда все отступы, шрифты, цвета и прочее
полностью совпадают с дизайном. Однако стоит учитывать, что дизайнер тоже человек и может ошибаться.

Поэтому, если вы видите какую-либо неточность или не логичность в отступах, лишних дополнительный цвет или
несовпадение с сеткой, то лучше обратиться к проект-менеджеру или напрямую к дизайнеру, чтобы обсудить этот момент.

Не нужно в одностороннем порядке видоизменять дизайн или наоборот — добавлять "костыли" в код верстки. Для нас всегда
в приоритете, чтобы интерфейс был повторяющимся, а кода — меньше.

### Grid, Flex

Использование современных способов расположения элементов через "гриды" и "флексы" — считается необходимым, поскольку
их уже поддерживают современные браузеры. Под старые браузеры, например Internet Explorer мы адаптацию не делаем.

### Браузеры

Современными браузерами (последних версий), в которых должны хорошо работать наши приложения следующие:

- Chrome
- Safari
- Mozilla
- Opera
- Edge
- Яндекс.Браузер



..

TODO...

- фиксированные размеры — в пикселях, резиновые размеры — в процентах
- не пишите размеры в пикселях в явном виде, опирайтесь на переменные Bootstrap/Steroids или заводите свои (но всегда
  старайтесь отталкиваться от существующих)

- вся верстка должна быть адаптивной и резиновой между переходными размерами экрана
- ориентируемся на следующий диапазон ширины экранов: от 320px до 3000px (4к). Верстка как минимум не должна
  съезжать/ломаться
- стили подключаем через index.scss
- нельзя определять глобальные стили (исключения составляю стили, поставляемые библиотекой Bootstrap, но расширять их
  нельзя).
- при подключении библиотеки, где есть стили - эти стили должны подключаться только к рамках блока
- переменных храним в custom-variables.scss
- ссылки на внешние источники всегда должны быть с target=’_blank’
- Чеклист по верстке https://habr.com/ru/post/319664/
- Одиннадцатиклассница https://habr.com/ru/company/2gis/blog/246831/
- не нужны фигурные скобки, если значение строка className={'qwe'}
