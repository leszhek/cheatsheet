## Введение
[Вернуться](./)

Чтобы объяснить, что такое управление версиями, я буду использовать аналогии. Если нужно более точное объяснение, обратитесь к статье википедии.

### Работа - это игра

Я играл в компьютерные игры почти всю свою жизнь. А вот использовать системы управления версиями начал уже будучи взрослым. Полагаю, я такой не один, и сравнение этих двух занятий может помочь объяснению и пониманию концепции.

Представьте, что редактирование кода или документа — игра. Далеко продвинувшись, вы захотите сохраниться. Для этого вы нажмете на кнопку «Сохранить» в вашем любимом редакторе.

Но это перезапишет старую версию. Это как в древних играх, где был только один слот для сохранения: конечно, вы можете сохраниться, но вы больше никогда не сможете вернуться к более раннему состоянию. Это досадно, так как прежнее сохранение могло указывать на одно из очень интересных мест в игре, и может быть, однажды вы захотите вернуться к нему. Или, что еще хуже, вы сейчас находитесь в безвыигрышном положении и вынуждены начинать заново.

### Управление версиями

Во время редактирования вы можете «Сохранить как…» в другой файл, или скопировать файл куда-нибудь перед сохранением, чтобы уберечь более старые версии. Может быть, заархивировав их для экономии места на диске. Это самый примитивный вид управления версиями, к тому же требующий интенсивной ручной работы. Компьютерные игры прошли этот этап давным-давно, в большинстве из них есть множество слотов для сохранения с автоматическими временны́ми метками.

Давайте немного усложним условия. Пусть у вас есть несколько файлов, используемых вместе, например, исходный код проекта или файлы для вебсайта. Теперь, чтобы сохранить старую версию, вы должны скопировать весь каталог. Поддержка множества таких версий вручную неудобна и быстро становится дорогим удовольствием.

В некоторых играх сохранение — это и есть каталог с кучей файлов внутри. Игры скрывают детали от игрока и предоставляют удобный интерфейс для управления различными версиям этого каталога.

В системах управления версиями всё точно так же. У них у всех есть приятный интерфейс для управления каталогом с вашим скарбом. Можете сохранять состояние каталога так часто, как пожелаете, а затем восстановить любую из предыдущих сохраненных версий. Но, в отличие от компьютерных игр, они существенно экономят дисковое пространство. Обычно от версии к версии изменяется только несколько файлов, и то ненамного. Хранение лишь различий вместо полных копий требует меньше места.

### Распределенное управление

А теперь представьте очень сложную компьютерную игру. Ее настолько сложно пройти, что множество опытных игроков по всему миру решили объединиться и использовать общие сохранения, чтобы попытаться выиграть. Прохождения на скорость — живой пример. Игроки, специализирующиеся на разных уровнях игры, объединяются, чтобы в итоге получить потрясающий результат.

Как бы вы организовали такую систему, чтобы игроки смогли легко получать сохранения других? А загружать свои?

В былые времена каждый проект использовал централизованное управление версиями. Какой-нибудь сервер хранил все сохраненные игры. И никто больше. Каждый держал лишь несколько сохранений на своей машине. Когда игрок хотел пройти немного дальше, он выкачивал самое последнее сохранение с главного сервера, играл немного, сохранялся и закачивал уже свое сохранение обратно на сервер, чтобы остальные могли им воспользоваться.

А что если игрок по какой-то причине захотел использовать более старую сохраненную игру? Возможно, нынешнее сохранение безвыигрышно, потому что кто-то забыл взять некий игровой предмет еще на третьем уровне, и нужно найти последнее сохранение, где игру всё еще можно закончить. Или, может быть, хочется сравнить две более старые сохраненные игры, чтобы установить вклад конкретного игрока.

Может быть много причин вернуться к более старой версии, но выход один: нужно запросить ту старую сохраненную игру у центрального сервера. Чем больше сохраненных игр требуется, тем больше понадобится связываться с сервером.

Системы управления версиями нового поколения, к которым относится Git, известны как распределенные системы, их можно понимать как обобщение централизованных систем. Когда игроки загружаются с главного сервера, они получают каждую сохраненную игру, а не только последнюю. Они как бы зеркалируют центральный сервер.

Эти первоначальные операции клонирования могут быть ресурсоемкими, особенно при длинной истории, но сполна окупаются при длительной работе. Наиболее очевидная прямая выгода состоит в том, что если вам зачем-то потребуется более старая версия, взаимодействие с сервером не понадобится.

### Глупые предрассудки

Широко распространенное заблуждение состоит в том, что распределенные системы непригодны для проектов, требующих официального централизованного хранилища. Ничто не может быть более далеким от истины. Получение фотоснимка не приводит к тому, что мы крадем чью-то душу. Точно так же клонирование главного хранилища не уменьшает его важность.

В первом приближении можно сказать, что все, что делает централизованная система управления версиями, хорошо сконструированная распределенная система может сделать лучше. Сетевые ресурсы просто дороже локальных. Хотя дальше мы увидим, что в распределенном подходе есть свои недостатки, вы вряд ли ошибетесь в выборе, руководствуясь этим приближенным правилом.

Небольшому проекту может понадобиться лишь частица функционала, предлагаемого такой системой. Но использование плохо масштабируемой системы для маленьких проектов подобно использованию римских цифр в расчетах с небольшими числами.

Кроме того, проект может вырасти сверх первоначальных ожиданий. Использовать Git с самого начала — это как держать наготове швейцарский нож, даже если вы всего лишь открываете им бутылки. Однажды вам безумно понадобится отвертка и вы будете рады, что под рукой есть нечто большее, чем простая открывалка.

### Конфликты при слиянии

Для этой темы аналогия с компьютерной игрой становится слишком натянутой. Вместо этого, давайте вернемся к редактированию документа.

Итак, допустим, что Алиса вставила строчку в начале файла, а Боб — в конце. Оба они закачивают свои изменения. Большинство систем автоматически сделает разумный вывод: принять и соединить их изменения так, чтобы обе правки — и Алисы, и Боба — были применены.

Теперь предположим, что и Алиса, и Боб внесли разные изменения в одну и ту же строку. В этом случае невозможно продолжить без человеческого вмешательства. Тот из них, кто вторым закачает на сервер изменения, будет информирован о конфликте слияния (merge conflict), и должен либо предпочесть одно изменение другому, либо скорректировать всю строку.

Могут случаться и более сложные ситуации. Системы управления версиями разрешают простые ситуации сами и оставляют сложные для человека. Обычно такое их поведение поддается настройке.
