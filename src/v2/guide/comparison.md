---
title: Сравнение с другими фреймворками
type: guide
order: 29
---

Определённо, этот раздел руководства — самый трудный для написания, но он очень важен. Вероятно, вы уже решаете определённые задачи, используя тот или иной фреймворк или библиотеку, а сюда вас привело желание узнать, не позволит ли Vue упростить и улучшить вашу работу. На этот вопрос мы и надеемся ответить.

Мы очень постараемся не быть предвзятыми. Будучи членами основной команды разработки Vue, мы, разумеется, сами его очень любим. На наш взгляд, с некоторыми задачами Vue справляется лучше, чем какой-либо другой существующий фреймворк. Если бы мы не верили в это, мы бы наверное и не работали над этим проектом, верно? И тем не менее, нам бы хотелось быть предельно честными и точными в оценках. В тех случаях, когда альтернативные библиотеки имеют существенные преимущества, как например обширнейшая экосистема альтернативных рендереров React'а или поддержка браузеров вплоть до IE6 Knockout'ом, мы постараемся не забыть о них упомянуть.

Кроме того, мы очень высоко ценим **вашу** помощь в деле поддержания актуальности этого документа, потому что мир JavaScript развивается стремительно! Если вы заметите какую-либо неточность или ошибку — пожалуйста, дайте нам знать, [открыв issue на Github](https://github.com/vuejs/vuejs.org/issues/new?title=Inaccuracy+in+comparisons+guide).

## React

React и Vue во многом похожи. Они оба:

- используют Virtual DOM
- предоставляют реактивность и компонентную структуру
- фокусируются на корневой библиотеке, вынося прочие вопросы, такие как роутинг или управление глобальным состоянием приложения, в дополнительные библиотеки

Из-за столь похожих ниш, мы уделили сравнению этих фреймворков больше всего времени. Нашей целью было не только удостовериться в технической точности, но также и сохранить баланс. Мы указываем, где React превосходит Vue, например — в богатстве экосистемы и изобилии доступных пользовательских рендереров.

Тем не менее, многие рассматриваемые моменты — в известной мере субъективны, и видится неизбежным то, что некоторым пользователям React дальнейшее сравнение всё равно покажется предвзятым. Мы понимаем, что и в технических вопросах существуют элементы вкуса, и в основном ставим своей целью в этом сравнении указать причины, по которым Vue может оказаться вам полезным — на случай если ваши вкусы совпадают с нашими.

Помощь React-сообщества [была неоценима](https://github.com/vuejs/vuejs.org/issues/364) в деле достижения этого баланса. Особо хотелось бы поблагодарить Даниила Абрамова из команды разработки React. Он был крайне щедр на время и опыт, помогая нам довести этот документ до состояния, когда обе стороны [остались довольны](https://github.com/vuejs/vuejs.org/issues/364#issuecomment-244575740) финальным результатом.

### Быстродействие

React и Vue предлагают сравнимую производительность в наиболее часто встречающихся случаях использования, при этом Vue слегка быстрее из-за более легковесной реализации Virtual DOM. Если вас интересуют цифры, то можете изучить [стороннее сравнение производительности](https://rawgit.com/krausest/js-framework-benchmark/master/webdriver-ts/table.html), которое фокусируется на сырой производительности рендеринга/обновления страницы. Обратите внимание, что сравнение не учитывает комплексные структуры компонентов, поэтому не стоит его рассматривать как вердикт.

#### Усилия для оптимизации

В React когда состояние компонента изменяется, оно запускает повторный рендеринг всего поддерева компонента, начиная с себя. Чтобы избежать ненужного повторного рендеринга дочерних компонентов, вам нужно либо использовать `PureComponent`, либо реализовывать `shouldComponentUpdate` везде где это возможно. Вам также может потребоваться использовать неизменяемые (immutable) структуры данных, чтобы сделать изменения вашего состояния более удобными к оптимизации. Однако, в некоторых случаях вы не можете рассчитывать на такую оптимизацию, потому что `PureComponent/shouldComponentUpdate` предполагают, что отображение всего поддерева определяется данными текущего компонента. Если это не так, то такая оптимизация может привести к несогласованному состоянию DOM.

В Vue зависимости компонента автоматически отслеживаются во время рендеринга, поэтому система точно знает, какие компоненты действительно необходимо отрендерить повторно при изменении состояния. Каждый компонент можно рассматривать как имеющий `shouldComponentUpdate`, автоматически реализованную для вас, без ограничений для вложенных компонентов.

В целом, это устраняет необходимость целого класса оптимизаций производительности для разработчика, что позволяет больше сосредоточиться на построении самого приложения по мере его масштабирования.

### HTML & CSS

В React, абсолютно всё — это JavaScript. Не только структуры HTML, выраженные через JSX, последние тенденции также включают управление CSS внутри JavaScript. Этот подход имеет свои преимущества, но также заставляет идти на компромиссы, которые могут показаться неэффективными для каждого разработчика.

Vue охватывает классические веб-технологии и основывается на них. Чтобы показать вам что это значит, мы рассмотрим несколько примеров.

#### JSX vs Шаблоны

В React все компоненты реализуют свой UI в render-функциях с использованием JSX, декларативныим XML-подобным синтаксисом, который работает в JavaScript.

Render-функции с JSX имеют несколько преимуществ:

- Вы можете использовать все возможности языка программирования (JavaScript) для построения своего представления. Это включает в себя временные переменные, управление ветвлением и прямые ссылки на значения JavaScript в области видимости.

- Поддержка инструментов (например, линтинг, проверка типов, автодополнение в редакторе) для JSX в некоторых отношениях более продвинута, чем то, что доступно в настоящее время для шаблонов Vue.

В Vue у нас также есть [render-функции](render-function.html) и даже [поддержка JSX](render-function.html#JSX), потому что иногда вам требуются эти возможности. Тем не менее, по умолчанию мы предлагаем шаблоны как более простую альтернативу. Любой валидный HTML также будет валидным шаблоном Vue, и это приводит к нескольким преимуществам:

- Для многих разработчиков, которые работают с HTML, шаблоны просто более естественны для чтения и написания. Само это предпочтение может быть несколько субъективным, но если это делает разработчика более продуктивным, то преимущество налицо.

- HTML-шаблоны облегчают постепенную миграцию существующих приложений для использования возможностей реактивности Vue.

- Это также облегчает дизайнерам и менее опытным разработчикам разбираться и вносить доработки в текущую кодовую базу.

- Вы можете даже использовать пре-процессоры, такие как Pug (ранее известный как Jade), чтобы создавать ваши шаблоны в Vue.

Некоторые утверждают, что важно изучить дополнительный язык DSL (Domain-Specific Language) для написания шаблонов — мы считаем, что эта разница в лучшем случае поверхностна. Во-первых, JSX не означает, что пользователю не нужно ничего учить — это дополнительный синтаксис на основе простого JavaScript, поэтому он прост в освоении для любого, кто знаком с JavaScript, но говорить, что это просто для всех, было бы заблуждением. Точно также шаблон является просто дополнительным синтаксисом поверх простого HTML и, следовательно, имеет очень низкий порог вхождения для тех, кто уже знаком с HTML. С помощью DSL мы также можем помочь пользователю сделать больше с меньшим количеством кода (например, с `v-on` модификаторами). Похожая задача может включать в себя намного больше кода при использовании простых функций JSX или render-функций.

На более высоком уровне мы можем разделить компоненты на две категории: презентационные и логические. Мы рекомендуем использовать шаблоны для презентационных компонентов и render-функции / JSX для логических. Процентное соотношение этих компонентов зависит от типа вашего приложения, но обычно презентационные компоненты более распространены.

#### Модульный (компонентный) CSS

За исключением случаев разделения компонентов на несколько файлов (например, посредством [CSS-модулей](https://github.com/gajus/react-css-modules)), для ограничения области видимости CSS в React обычно используется подход CSS-in-JS. В этой области существует множество соревнующихся решений, каждое с собственными недостатками. Общими проблемами являются такие вопросы как: hover states, media queries, псевдоселекторы — и эти вопросы зачастую либо требуют использования дополнительных "тяжёлых" зависимостей для переизобретения уже существующего в CSS функционала, либо вовсе не работают. Кроме того, без специального внимания к оптимизации, CSS-in-JS может нетривиально повлиять на производительность. Что ещё важнее, использование этих инструментов в любом случае существенно отличается от привычного опыта написания обыкновенного CSS.

Vue же позволяет напрямую использовать CSS в [однофайловых компонентах](single-file-components.html):

``` html
<style scoped>
  @media (min-width: 250px) {
    .list-container:hover {
      background: orange;
    }
  }
</style>
```

Опциональный атрибут `scoped` автоматически ограничивает область видимости CSS текущим компонентом, добавляя элементам уникальные атрибуты (такие как `data-v-1-21e5b78`), и компилируя `.list-container:hover` во что-нибудь вроде `.list-container[data-v-1-21e5b78]:hover`.

Разработчикам, уже знакомым с CSS-модулями мы рады сообщить, что Vue их [полностью поддерживает](http://vue-loader.vuejs.org/en/features/css-modules.html).

Наконец, как и с HTML, у вас есть возможность использования любых препроцессоров (или постпроцессоров) на ваш вкус. Это позволяет применять централизованные операции, такие как управление цветами, на этапе сборки. При этом нет необходимости импортировать специализированные JavaScript-библиотеки, увеличивающие как размер результирующей сборки, так и сложность вашего приложения.


### Масштабирование

#### Масштабирование вверх

Для крупных приложений, как Vue так и React предоставляют надёжные решения для роутинга. Сообщество React также породило весьма инновационные решения в области управления состоянием приложения (см. Flux/Redux). Эти подходы, и [даже сам Redux](https://github.com/egoist/revue) легко интегрируются в приложения на Vue. В действительности, Vue сделал следующий шаг, создав [Vuex](https://github.com/vuejs/vuex) — вдохновлённую Elm реализацию паттерна управления состоянием приложения. Vuex глубоко интегрирован с Vue, что, на наш взгляд, изрядно облегчает жизнь разработчикам.

В качестве ещё одного важного различия между React и Vue можно упомянуть тот факт, что все дополнительные библиотеки Vue, включая библиотеки для управления состоянием приложения и для роутинга (среди [прочих задач](https://github.com/vuejs)), официально поддерживаются в актуальном соответствии с ядром библиотеки. React, напротив, предпочитает отдать эти вопросы на откуп сообществу, тем самым создавая более фрагментированную экосистему. Впрочем, как уже замечалось ранее, в силу популярности React, его экосистема значительно обширнее, чем у Vue.

Наконец, у Vue есть [инструменты командной строки для генерации проектов](https://github.com/vuejs/vue-cli), упрощающие создание новых проектов с использованием подходящей системы сборки, включая [Webpack](https://github.com/vuejs-templates/webpack), [Browserify](https://github.com/vuejs-templates/browserify), или даже [вовсе без таковой](https://github.com/vuejs-templates/simple). В сообществе React также существуют наработки в этом направлении — например, [create-react-app](https://github.com/facebookincubator/create-react-app), но на данный момент функционал этого решения имеет ряд ограничений:

- Нет возможности конфигурации проекта в процессе генерации. Vue позволяет [Yeoman](http://yeoman.io/)-подобную настройку шаблонов.
- Существует только один шаблон для одностраничного приложения, в то время как Vue позволяет выбрать подходящий шаблон из довольно широкого их многообразия.
- Нет возможности создавать проекты из пользовательских шаблонов, что может быть особенно полезно для enterprise-окружений с установившимися ранее соглашениями.

Важно заметить, что многие из этих ограничений — следствия сознательно принятых решений команды create-react-app, и в них есть и свои плюсы. Например, коль скоро ваш проект не требует пользовательской настройки процесса сборки, её можно будет обновлять как зависимость. Прочитать больше об этом подходе [можно здесь](https://github.com/facebookincubator/create-react-app#philosophy).

#### Масштабирование вниз

React известен своей довольно крутой кривой изучения. До того момента, когда новичок сможет что-то написать, ему придётся узнать о JSX, а вероятно — и о ES2015+, поскольку многие примеры используют синтаксис ES2015-классов. Кроме того придётся разобраться с системами сборки, поскольку, хотя технически и существует возможность использовать Babel самостоятельно для live-компиляции кода, для production этот подход в любом случае не годится.

Vue масштабируется вверх ничуть не хуже (если не лучше), чем React, и в то же время его можно масштабировать и вниз — вплоть до варианта использования вместе с jQuery. Именно так — в простейшем случае достаточно просто добавить тег скрипта на HMTL-страницу.

``` html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```

Начиная с этого момента можно писать код на Vue, и даже использовать production-версию, не мучаясь угрызениями совести и волнениями насчёт производительности.

Поскольку знания JSX, ES2015 и систем сборки не требуется для начала работы с Vue, в среднем у новых разработчиков уходит не больше дня на чтение [руководства](./), позволяющего узнать достаточно для построения нетривиальных приложений.

### Нативный рендеринг

React Native позволяет писать нативные приложения для iOS и Android, используя ту же самую модель компонент React'а. Это — прекрасно, так как позволяет разработчикам применить знание одного и того же фреймворка на различных платформах. В этой области, Vue официально поддерживает проект [Weex](https://alibaba.github.io/weex/) — кросс-платформенный UI-фреймворк, разрабатываемый Alibaba Group и использующий Vue в качестве основного JavaScript-фреймворка. Это значит, что с Weex вы можете использовать тот же синтаксис Vue для создания нативных элементов iOS и Android!

На данный момент Weex всё ещё находится в активной фазе разработки, и ещё не столь матёр и проверен опытом, как React Native. Однако, его разработка мотивируется реальными требованиями крупнейшего бизнеса электронной коммерции в мире. Команда разработки Vue также активно взаимодействует с разработчиками Weex, гарантируя отсутствие неожиданностей для Vue-разработчиков.

### Сравнение с MobX

MobX стал довольно популярным в сообществе React. Он использует почти идентичную Vue систему реактивности. В некотором смысле, связку React + MobX можно считать несколько более многословным вариантом Vue, так что если вы используете её, и она вам нравится, переход на Vue может оказаться следующим логичным шагом.

## AngularJS (Angular 1)

Некоторые части синтаксиса Vue выглядят очень похоже на синтаксис AngularJS (например, сравните `v-if` и `ng-if`). Это — не случайность: многие идеи, лежащие в основе AngularJS мы считаем верными, и вдохновлялись ими на ранних этапах разработки Vue. Впрочем, в AngularJS немало и болезненных проблем, и в этих областях мы постарались добиться значительных улучшений.

### Сложность

Vue значительно проще AngularJS, как в смысле API, так и в смысле архитектуры. Получение достаточных знаний для написания нетривиальных приложений обычно происходит менее чем за день, чего нельзя сказать об AngularJS.

### Гибкость и модульность

AngularJS имеет жёсткое мнение насчёт структуры вашего приложения, в то время как Vue проявляет гибкость и является более модульным решением. Хотя это и делает Vue пригодным для большего разнообразия проектов, мы понимаем и то, что когда решения уже приняты за тебя, можно сразу начать программировать, и в этом есть свои преимущества.

По этой причине мы предоставляем [шаблон webpack](https://github.com/vuejs-templates/webpack), позволяющий начать работу уже в течении нескольких минут, давая при этом доступ к таким продвинутым возможностям, как горячая замена модулей, линтинг, извлечение CSS и т.д..

### Связывание данных

AngularJS использует двунаправленное связывание данных между областями видимости, в то время как Vue концентрируется на однонаправленном потоке данных между компонентами. Это позволяет облегчить понимание потока данных в нетривиальных приложениях.

### Директивы и компоненты

Vue чётче разделяет директивы и компоненты. Директивы предназначены только для инкапсуляции низкоуровневых манипуляций с DOM, в то время как компоненты являют собой полноценные автономные объекты, со своей собственной логикой данных и представления. В AngularJS эти две концепции в значительной мере смешаны.

### Производительность

Vue производительнее AngularJS. Кроме того, из-за отсутствия dirty-checking, оптимизировать Vue-приложения намного-намного проще. AngularJS замедляется при увеличении количества наблюдателей, поскольку каждый раз при изменении чего-либо в области видимости все эти наблюдатели должны быть перезапущены. Кроме того, цикл может повториться несколько раз перед стабилизацией, поскольку реакция наблюдателей может спровоцировать следующее обновление. Пользователям AngularJS нередко приходится прибегать к весьма эзотерическим техникам для обхода этих трудностей, а в некоторых случаях оптимизация и вовсе становится невозможной.

Vue не подвержен таким проблемам по причине использования прозрачного механизма учёта зависимостей с асинхронной очередью — все изменения рассматриваются независимо, кроме случаев явного указания наличия связи между ними.

Любопытно, что есть немало общих черт, как Angular и Vue решают эти проблемы AngularJS.

## Angular (известный также как Angular 2)

Мы выделяем отдельную секцию для нового Angular, поскольку по сути он полностью отличен от AngularJS. Например, теперь он содержит полноценную компонентную систему; кроме того, многие детали имплементации были полностью переписаны, а API очень существенно изменился.

### TypeScript

Для Angular требуется использовать TypeScript, поскольку почти вся его документация и учебные ресурсы основаны на TypeScript. TypeScript имеет свои очевидные преимущества — проверка статических типов может быть очень полезна для крупных приложений, и может добавить производительности разработчикам, работающим на Java и C#.

Однако не все хотят использовать TypeScript. Часто для небольших приложений введение системы типов может привести к большему увеличению накладных расходов нежели увеличению производительности разработки. В таких случаях вам лучше воспользоваться Vue, так как использовать Angular без TypeScript может быть сложным.

Наконец, хотя Vue и не так глубого интегрирован с TypeScript как Angular, но предоставляет [официальные декларации типов](https://github.com/vuejs/vue/tree/dev/types) и [официальный декоратор](https://github.com/vuejs/vue-class-component) для тех, кто хочет использовать TypeScript с Vue. Мы также активно сотрудничаем с командами TypeScript и VSCode в Microsoft, чтобы улучшить опыт TS/IDE для пользователей Vue + TS.

### Размер и быстродействие

В смысле производительности, оба фреймворка весьма быстры, и пока нет достаточных данных из реального мира чтобы вынести окончательный вердикт. Но если вы всё же хотите цифр, похоже что Vue 2.0 всё-таки обгоняет Angular, по крайней мере если верить этому [стороннему исследованию производительности](http://stefankrause.net/js-frameworks-benchmark4/webdriver-ts/table.html).

Последние версии Angular, с AOT-компиляцией и tree-shaking, смогли значительно уменьшить размер сборок. Однако полнофункциональный проект Vue 2 с включёнными Vuex + vue-router (~30kb gzip) по-прежнему значительно легче из коробки, чем AOT-скомпилированное приложение, созданное с помощью `angular-cli` (~130kb gzip).

### Гибкость

Vue значительно менее упрям, чем Angular, и поддерживает множество различных систем сборок, не ограничивая разработчиков в том, какую структуру использовать для приложения. Многим программистам эта свобода нравится, хотя есть и те, кто предпочитают иметь единственно-правильный способ построения приложения.

### Кривая обучения

Всё что необходимо для начала работы с Vue — это знакомство с HTML и обыкновенным (ES5) JavaScript'ом. С этими базовыми навыками вы уже можете начать строить нетривиальные приложения после менее чем однодневного прочтения [руководства](./).

Кривая обучения у Angular гораздо круче. API фреймворка просто огромное и вам как пользователю нужно будет разобраться с большим количеством концепций, прежде чем стать продуктивным. Очевидно, что сложность Angular во многом обусловлена его направленностью только на большие, комплексные приложения — но это делает платформу намного более трудной для менее опытных разработчиков.

## Ember

Ember — это полнофункциональный фреймворк, изначально созданный "имеющим и отстаивающим своё мнение". Он требует соблюдения множества соглашений и конвенций, и когда вы с ними освоитесь — он может сделать вас весьма продуктивными. Однако, это означает ещё и высокую и крутую кривую обучения, не говоря о страдающей гибкости. Выбор между жёстко структурированным фреймворком и библиотекой со слабосвязанными совместно работающими инструментами — это всегда компромисс. Последняя даёт больше свободы, но и требует принятия большего количества самостоятельных архитектурных решений.

Учитывая вышесказанное, вероятно будет разумнее сравнивать ядро Vue и слои [шаблонизации](https://guides.emberjs.com/v2.7.0/templates/handlebars-basics/) и [объектной модели](https://guides.emberjs.com/v2.7.0/object-model/) Ember:

- Vue предлагает ненавязчивую реактивность для простых JavaScript-объектов и полностью автоматические вычисляемые свойства. В Ember от вас ожидается заворачивание всего в "Объекты Ember" и ручное указание зависимостей для вычисляемых свойств.

- Синтаксис шаблонов Vue позволяет использовать все возможности выражений JavaScript, в то время как возможности выражений Handlebars и синтаксис хелперов в Ember намеренно существенно ограничены.

- В вопросах производительности Vue [существенно выигрывает](https://rawgit.com/krausest/js-framework-benchmark/master/webdriver-ts/table.html) — даже с учётом последнего обновления Glimmer engine в Ember 2.x. Vue автоматически объединяет операции обновления, в то время как в Ember требуется ручное управление циклом выполнения в ситуациях, где производительность критична.

## Knockout

Knockout был пионером MVVM-подхода и отслеживания изменений в данных. Его система реактивности очень похожа на используемую Vue. Список [поддерживаемых браузеров](http://knockoutjs.com/documentation/browser-support.html) — впечатляет, особенно с учётом всех немалых возможностей фреймворка, доступных даже в IE6! Vue же поддерживает только IE9+.

Со временем, впрочем, разработка Knockout замедлилась и он понемногу начал показывать признаки своего возраста. Например, компонентной системе недостаёт полного набора хуков жизненного цикла, а интерфейс передачи дочерних компонентов, хоть и используется очень широко, выглядит по сравнению со [слотами Vue](components.html#Распределение-контента-слотами) не очень выигрышно.

Кроме того, похоже что существует разница в философских подходах к API, которая, если вам интересно, может быть продемонстрирована различиями при создании [простого списка todo](https://gist.github.com/chrisvfritz/9e5f2d6826af00fcbace7be8f6dccb89). Конечно же, это — субъективный вопрос, но многим API Vue кажется проще и лучше структурированным.

## Polymer

Polymer — это ещё один проект, спонсируемый Google. В действительности, он тоже послужил источником вдохновения для Vue. Компоненты Vue можно приблизительно сравнивать с пользовательскими элементами Polymer. Стиль разработки с использованием обоих фреймворков довольно похож. Самая существенная разница состоит в том, что Polymer базируется на последних возможностях Web Components и требует для работы использования весьма нетривиальных полифиллов (с потерей быстродействия в браузерах без нативной поддержки эти возможностей). Vue, напротив, без каких-либо зависимостей или полифиллов работает во всех браузерах, начиная с IE9.

В Polymer 1.0, разработчики существенно ограничили систему связывания данных для улучшения производительности. Например, единственными поддерживаемыми в шаблонах Polymer выражениями являются булево отрицание и одиночные вызовы методов. Реализация вычисляемых свойств — тоже не очень гибкая.

Пользовательские элементы Polymer пишутся в HTML-файлах, что ограничивает авторов использованием обыкновенного JavaScript/CSS (и возможностей языка, поддерживаемых браузерами на данный момент). Vue же позволяет использовать в однофайловых компонентах какие угодно пре- и построцессоры, включая, разумеется, и ES2015+.

При работе в production-окружении, Polymer рекомендует загружать всё "на лету" при помощи HTML Import'ов, что подразумевает как имплементацию этой технологии браузерами, так и поддержку HTTP/2 и сервером, и клиентом. Не для всякой целевой аудитории и не для всякого окружения это может быть выполнимо. В таких случаях предполагается использовать специальный инструмент под названием Vulcanizer, объединяющий элементы Polymer. Vue позволяет комбинировать возможности асинхронной загрузки компонентов c функциями по разделению кода Webpack'а, организуя "ленивую" загрузку частей приложения. Таким образом мы сохраняем совместимость со старыми браузерами, не теряя производительности на этапе загрузки приложения.

В дальнейшем скорее всего будет возможно углубить интеграцию между компонентами Vue и Web Compoments — однако пока что мы всё ещё ожидаем стабилизации и взросления спецификации, не озвучивая серьёзных обещаний до момента широкого распространения её поддержки всеми распространёнными браузерами.

## Riot

Riot 2.0 предлагает похожую модель разработки, основывающуюся на компонентах (в Riot называемых "тегами"), с минималистичным и прекрасно организованным API. Вероятно, Riot и Vue во многом основаны на схожих философских подходах. Однако, несмотря на немного больший размер по сравнению с Riot, Vue имеет некоторые существенные преимущества:

- [Система анимации переходов](transitions.html). У Riot её просто нет.
- Значительно более мощный роутер. API роутинга Riot'а — предельно минималистичен.
- Лучшая производительность. [Несмотря на заявленное](https://github.com/vuejs/vuejs.org/issues/346) использование Virtual DOM, в действительности Riot применяет dirty checking, и потому страдает от тех же проблем, что и AngularJS.
- Поддержка более зрелого инструментария. Vue официально поддерживает [Webpack](https://github.com/vuejs/vue-loader), [Browserify](https://github.com/vuejs/vueify), и [SystemJS](https://github.com/vuejs/systemjs-plugin-vue), в то время как Riot полагается в вопросах интеграции с системами сборки на поддержку сообщества.
