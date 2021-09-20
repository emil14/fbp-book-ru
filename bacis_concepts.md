# Основные понятия

В середине этого столетия ожидалось, что компьютеры смогут делать все, что люди могут им объяснить. Но даже тогда некоторые люди задавались вопросом, насколько легко это удасться. Книга «Быстрее мысли» (Bowden 1963), впервые опубликованная в 1950-х годах, содержит следующее замечание:

> Математики всегда возмущаются, когда впервые сталкиваются с коммерческой работой, тому, насколько она сложна» (стр. 258)

И далее:

> ... на практике любую программу так сложно сделать правильно, что может потребоваться несколько математиков, чтобы ухаживать за большой машиной в офисе ... Похоже, опытные программисты всегда будут в цене ( стр.259)

В те дни основным препятствием считалась сложность описания точных правил бизнес-логики. Это оказалось также сложно, как мы и думали, но по совершенно другим причинам. Сегодня же мы видим - проблема фактически встроена в фундаментальные принципы проектирования нашего базового вычислительного механизма - машины фон Неймана, о которой упоминалось в главе 1.

Машина фон Неймана идеально адаптирована к математическим   алгоритмическим задачам, для которых была создана: таблицы приливов, баллистические расчеты и т.д., Но бизнес-приложения по своей другие по своей природе. В качестве примера этих различий основным строительным блоком программирования является подпрограмма, с тех пор как была впервые описана [Адой, графиней Лавлейс](https://en.wikipedia.org/wiki/Ada_Lovelace), дочерью [лорда Байрона](https://en.wikipedia.org/wiki/Lord_Byron), в прошлом веке (прим.пер. - уже позапрошлом). Неплохое достижение, особенно, если учесть, что компьютеров еще не было! Эта концепция была прочно основана на математической идее функции, и любой программист сегодня знает о ряде стандартных подпрограмм и может при необходимости придумать новые. Это может быть «квадратный корень», «двоичный поиск», «синус», «отображение денежной суммы в удобочитаемом формате» и проч.Каковы основные строительные блоки бизнес-приложений? Легко перечислить такие функции, как «слияние», «сортировка», «разбиение отчета на страницы» и т.д, Но ни одна из них, похоже, не поддается инкапсулированию в качестве подпрограмм. Они другой природы - вместо того, чтобы быть функциями, которые работают в один момент времени, все они имеют работают в течение определенного периода времени с несколькими (иногда большим множеством) элементов ввода и вывода.

Где-то в этой точке мы начинаем подозревать, в чем может состоять эта разница. Бизнес-программирование работает с данными и концентрируется на том, как эти данные преобразуются, объединяются и разделяются для получения желаемых результатов и изменения хранимых данных в соответствии с бизнес-требованиями. Вообще говоря, в то время как традиционные подходы к программированию (называемые «[потоком управления](https://en.wikipedia.org/wiki/Control_flow)» начинаются с процесса и рассматривают данные как второстепенные, бизнес-приложения обычно разрабатываются, начиная с данных и рассматривая процесс как второстепенный, как просто способ создания данных, манипуляции и уничтожения. Мы часто называем этот подход «[потоком данных](https://en.wikipedia.org/wiki/Dataflow_programming)», и это ключевая концепция многих наших методологий проектирования. Когда мы пытаемся преобразовать это представление в процедурный код, мы начинаем сталкиваться с проблемами.

Теперь взглянем под другим углом. Сравним чисто алгоритмическую задачу с похожей на вид бизнес-задачей. Мы начнем с простого числового алгоритма: вычисления числа в n-й степени, где `n` - положительный целочисленный показатель степени. Это должно быть знакомо читателям, и на самом деле это не лучший способ сделать этот расчет, но его будет достаточно, чтобы изложить мою мысль. В псевдокоде это можно выразить следующим образом:

```
/_ Calculate a to the power b _/
x = b
y = 1
do while x > 0
y = y \* a
x = x - 1
enddo
return y
```

Фрагмент 3.1

Это очень простой алгоритм - его легко понять, легко проверить, легко изменить, он основан на хорошо понятных математических концепциях. Давайте теперь повторим с, на первый взгляд похожей логикой, с файлами и записями, а не числами. Новый алгоритм имеет ту же структуру, что и предыдущий, но работает с потоками записей. Проблема в том, чтобы создать файл `OUT`, который является подмножеством другого, `IN`, где должны быть выведены записи, которые удовлетворяют заданному критерию `c`. Записи, которые не удовлетворяют `c`, должны быть исключены. Это довольно распространенное требование, которое обычно кодируется с использованием некоторой формы следующей логики:

```
read into a from IN
do while read has not reached end of file
    if c is true
        write from a to OUT
    endif
    read into a from IN
enddo
```

Figure 3.2

Какое действие применяется к тем записям, которые не удовлетворяют нашему критерию?  Что ж, они загадочно исчезают из-за того, что не записываются в OUT до того, как будут уничтожены при следующем "чтении".  Большинство программистов, читающих это, вероятно, не увидят в этом ничего странного, но, если подумать, не кажется ли это странным, что есть возможность отбрасывать важные вещи, такие как записи, с помощью того, что в действительности есть причуда времени?

Частично это связано с тем, что большинство современных компьютеров имеют единый набор ячеек для хранения, и это хранилище ведет себя совсем не так, как системы хранения в реальной жизни. В реальной жизни бумага, помещенная в ящик, остается там, пока ее намеренно не уберут. Со временем заполняется, предотвращая добавление бумаги. Сравните с хранилищем в компьютере - можно считывать ячейку любое количество раз и постоянно получать одни и те же данные (не зная, делается ли это впервые), или можно данные поверх предыдущих, и более ранние просто исчезнут... Хотя деструктивное хранилище не является неотъемлемой частью машины фон Неймана, оно предполагается во многих функциях машины, и это тот вид хранилища, который реализован в большинстве современные компьютеров. Поскольку память этих машин очень чувствительна к таймингу и поскольку последовательность каждой инструкции должна быть предопределена (а люди допускают ошибки!), невероятно сложно заставить программу выше определенной сложности работать должным образом. И, конечно же, эта парадигма хранения закреплена в большинстве наших языков более высокого уровня в концепции «переменной». В знаменитой статье [Джон Бэкус](https://en.wikipedia.org/wiki/John_Backus) (1978) фактически извинился за изобретение [ФОРТРАНА](https://en.wikipedia.org/wiki/Fortran)! Это я и имел в виду, говоря о "странном использовании знака равенства" в языках высокого уровня. Для логика утверждение `J = J + 1` является противоречием (если `J` не является бесконечностью?) - но программисты больше не видят в этом что-то странное!

Предположим, вместо этого мы решили, что запись должна рассматриваться как реальная вещь, например, записка или письмо, которые после создания существуют в течение определенного периода времени и должны быть явно уничтожены, прежде чем покинуть систему. Давайте расширим наш псевдоязык, включив в него эту концепцию, добавив оператор `discard` (на записи надо как-то уметь ссылаться). Наша программа теперь может выглядеть следующим образом:
 
```
read record a from IN
do while read has not reached end of file
    if c is true
        write a to OUT
    else
        discard a
    endif
    read record a from IN
enddo
```

Figure 3.3

Теперь мы можем переосмыслить `a`: вместо того, чтобы думать о нем как о области хранения, давайте подумаем о `a` как о «дескрипторе», который обозначает конкретную «вещь» - это способ найти вещь, а область в хранилище, в котором вещь хранится. Эти данные вообще не следует рассматривать, как находящиеся в хранилище: они находятся «где-то еще» (в конце концов, не имеет значения, куда их помещает `read`, если информация, которую они содержат, становится доступной для программы). У самих вещей больше атрибутов, чем у их моделей в хранилище. Хранимый образ можно рассматривать как [проекцию объемного объекта на плоскость](https://en.wikipedia.org/wiki/3D_projection). Если мы повторно используем дескриптор, то потеряем доступ к тому, что он описывает. Мы обязаны правильно утилизировать вещи, прежде чем мы сможем повторно использовать их.

Обратите внимание, с каким трудом находится подходящий термин для «вещи»: проблема в том, что это понятие «атомарное» в том смысле, оно не может быть разложено на еще более фундаментальные объекты. У него было несколько названий на различных диалектах FBP, и он имеет некоторое сходство с понятием «объект» в ООП, но я считаю, что лучше дать ему собственное уникальное имя. В дальнейшем мы будем использовать термин «информационный пакет» (или «IP»). IPа могут иметь разную длину от 0 байт до 2 миллиардов - преимущество работы с «дескрипторами» в том, что IPа управляются одинаково, и их отправка и получение стоят одинаково, независимо от их размера.

Пока мы добавили только одну концепцию - IP - к концептуальной модели, которую строим. Псевдокод на рис. 3.3 был программой, работающей в одиночку. Поскольку основная программа может вызывать подпрограммы, которые, в свою очередь, могут вызывать другие подпрограммы, мы имеем по существу ту же структуру, что и в привычных программах, с одной основной рутиной и сабрутинами. Теперь, вместо того, чтобы просто усложнять отдельную программу, как это делается в обычном программировании, отправимся в совершенно другом направлении: визуализируем приложение, построенное из множества таких основных программ, работающих одновременно, с передачей IP между ними. Это очень похоже на фабрику, на которой одновременно работает множество машин, соединенных ленточными конвейерами. Вещи, над которыми идёт работа (автомобили, слитки, радиоприемники, бутылки), перемещаются по конвеерам от одной машины к другой. На самом деле, есть много аналогий, которые мы могли бы использовать: кафетерии, офисы, между которыми проходят служебные записки, люди на коктейльной вечеринке и т.д. После того, как я проработал эти концепции в течение нескольких лет, я повел своих детей посмотреть завод по розливу газировки. Мы видели машины для наполнения бутылок, машины для наклеивания крышек и машины для наклеивания этикеток, но именно связь и поток между этими различными машинами гарантируют, что то, что вы покупаете в магазине, наполнено правильным веществом и не потечёт, пока вы стоите на кассе!

Приложение FBP можно рассматривать как «фабрику данных». Цель любого приложения - принимать данные и обрабатывать их, как слиток превращается в готовую металлическую деталь. Раньше мы думали, что можно будет разрабатывать фабрики программного обеспечения, но теперь мы видим, что это был неправильный образ: мы не хотим производить код массово - нам нужно меньше кода, а не больше. Оглядываясь назад, очевидно, что на фабрике в полезную информацию должны быть преобразованы данные, а не программы.

А теперь подумайте о различиях между характеристиками такой фабрики и обычной единой основной программы. На любом предприятии одновременно выполняется множество процессов, и синхронизация необходима только на уровне отдельного рабочего элемента. В обычном программировании мы должны точно знать, когда происходят события, иначе все пойдет не так. Во многом это связано с тем, как работает хранилище сегодняшних компьютеров - если данные не будут обрабатываться в точной последовательности, мы получим неверные результаты, и мы можем даже не осознать, что это произошло! Нет гибкости или приспособляемости. В нашем фабричном образе, с другой стороны, нам все равно, работает ли одна машина до или после другой, пока процессы применяются к заданному рабочему элементу в правильном порядке. Например, _бутылка должна быть наполнена до того, как она будет закрыта крышкой, но это не означает, что все бутылки должны быть заполнены, прежде чем какая-либо из них может быть закрыта_. Оказывается, _обычные программы полны такой ненужной синхронизации_, которая снижает производительность, создает ненужную когнитивную нагрузку и, как правило, делает обслуживание приложений чем-то между трудным и невозможным. На реальном заводе ненужные ограничения такого рода будут означать, что часть машин не будут использоваться эффективно. В программировании это означает, что шаги кода должны быть объединены в единую последовательность, которую людям чрезвычайно трудно правильно визуализировать из-за ошибочного убеждения, что это требуется машине. Это не так!

В качестве альтернативы приложение может быть представлено как _сеть простых программ_ с данными, перемещающимися между ними, и на самом деле мы натыкаемся здесь на преимущества визуального или «пространственного» воображения. Ещё это хорошо сочетается с методологиями проектирования, обычно упоминаемыми в рамках структурного анализа. Так называемая «[модель инверсии Джексона](https://en.wikipedia.org/wiki/Jackson_structured_programming)» (М. Джексон, 1975) разрабатывает приложения в этой форме, но затем предлагает, чтобы все процессы, кроме одного (основной программы), были «инвертированы» в подпрограммы. В этом больше нет необходимости! Интересно, что Джексон начинает свое обсуждение инверсии программ с описания простой схемы мультипрограммирования, использующей соединение с емкостью, равной единице (в терминологии FBP). Затем он продолжает: «Мультипрограммирование обходится не дёшево. Если у нас нет необычно благоприятной среды, не стоит поднимать кувалду мультипрограммирования, чтобы расколоть орешек небольшого конфликта структур». В FBP у нас есть такая «необычно благоприятная среда», он слишком быстро списал мультипрограммирование со счетов!

Как сделать так, чтобы несколько «машин» совместно использовали реальную машину? Вообще-то, мы научились этому годы назад - надо просто давать им доли машинного времени всякий раз, когда им есть чем заняться - другими словами, позволяем им «делить время». В обычном программировании разделение времени в основном использовалось для эффективного использования аппаратных средств, но оказалось, что его также можно применить для преобразования нашей модели "нескольких основных программ" в рабочее приложение. Существует множество возможных методов разделения времени, но в FBP мы обнаружили, что один из простейших работает хорошо: мы просто позволяем одной из наших «машин» (называемых «процессами») работать до тех пор, пока запрос сервиса FBP не становится удовлетворен. Когда это происходит, процесс приостанавливается, и управление получает другой готовый процесс. В конце концов, приостановленный процесс сможет продолжиться (поскольку условие блокировки будет снято) и получит управление, когда появится время. [Дейкстра](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra) назвал такие процессы «[последовательными процессами](https://en.wikipedia.org/wiki/Communicating_sequential_processes)» и объяснил, что фокус в том, что они не знают, что они были приостановлены. Их логика неизменна - они просто «растянуты» во времени.

Как вы, возможно, догадались, запросы сервиса FBP, связаны с обменом данными между процессами. Процессы подключаются с помощью очередей [FIFO](https://en.wikipedia.org/wiki/FIFO_(computing_and_electronics)) (первый пришел, первый ушел) или соединений, которые могут содержать до некоторого максимального количества IP («емкость» очереди). Для конкретной очереди эта емкость может составлять от единицы до довольно большого числа. В соединениях используются несколько отличающиеся от файлов глаголы, поэтому мы обновляем наш псевдокод, заменив:

- "read" на "receive"
- "write" на "send"
- "discard" на "drop"
- "end of file" на "end of data"

Сервис `receive` может быть заблокирован, потому что в данный момент в соединении нет данных, а `send` может быть заблокирован, поскольку соединение заполнено и какое-то время не может принимать какие-либо данные. Подумайте о конвейерной ленте, на которой достаточно места для такого количества бутылок, телевизоров или чего-то еще: она может быть пустой, полной или в каком-то промежуточном состоянии. Все эти ситуации, конечно, носят временный характер и со временем изменятся. Мы должны предоставить соединениям максимальную емкость не только для того, чтобы наши приложения поместились в доступное хранилище, но и для того, чтобы все данные в конечном итоге были обработаны (в противном случае данные могли бы просто накапливаться в соединениях и никогда не обрабатываться).

Пусть теперь процессы не могут использовать соединения - они будут обращаться к ним через порты - точки, где встречаются процессы и соединения. Подробнее о портах позже.

Наш предыдущий псевдокод отныне выглядит так:

```
receive from IN using a
do while receive has not reached end of data
    if c is true
        send a to OUT
    else
        drop a
    endif
    receive from IN using a
enddo
```

Figure 3.4

Я умышленно использовал слово `using` в `receive`, чтобы подчеркнуть природу `a` как дескриптора, но `send a` кажется более естественным, чем `send using a`. Обратите внимание на различия между нашей программой обработки файлов и нашим FBP-компонентом:

- различия в глаголах (уже упоминалось)
- IP скорее «где-то там», нежели записаны хранилище
- IP должны быть явно уничтожены
- имена портов вместо имен файлов

Ранее мы говорили, что IP - это вещи, которые должны быть явно уничтожены - в нашей многопроцессорной реализации мы требуем, чтобы все IP были учтены: у любого процесса, который получает IP, увеличивается «количество IP во владении», и он должен уменьшить этот номер до нуля перед выключением. Он может сделать это практически так же, как мы избавляемся от записки: уничтожить ее, передать или отправить в архив. Конечно, вы не можете избавиться от IP (или даже получить к нему доступ), если у вас нет его дескриптора (или если его дескриптор был повторно использован для обозначения другого IP). Как и с запиской, процесс не может повторно получить доступ к IP после его удаления (в большинстве реализаций FBP мы обнуляем дескриптор после удаления IPа, чтобы этого не произошло).

Пофилософствуем чутка. Сторонники «сборки мусора» считают, что IP должны исчезать, когда "на них никто не смотрит" (никакие дескрипторы не ссылаются на них), но большинство разработчиков реализаций FBP считали, что это неправильно. В традиционном программированием: вещи могут исчезнуть слишком легко. Поэтому мы настаивали на том, чтобы процесс явно, так или иначе, избавился от всех своих IP, прежде чем отдать управление. Если вы вводите IP на одном конце сети, то вы знаете, что он будет обрабатываться до тех пор, пока какой-либо процесс явно не уничтожит его! И наоборот, если вы создаете IP-таблицу и забываете дропнуть её, когда закончили, было бы неплохо, если бы система избавилась от неё за вас. С другой стороны... я мог бы возразить, что такая ошибка (если это ошибка) может быть признаком фундаментально неправильного дизайна. В любом случае, все недавние реализации FBP обнаруживают эту ошибку и перечисляют IP, которые не были удалены, так что легко понять, что вы сделали не так. Итак... если мы можем это сделать, почему бы не сделать сборку мусора автоматической? Что ж, наша команда провела голосование, и строгий контроль победил подавляющим большинством! Он мог бы и проиграть, будь у группы другой состав! В конце концов, мы могли бы сделать это как экологическим решением, так и атрибутом каждого компонента, чтобы мы могли определить, запускается ли «свободный» компонент в «тесной» мастерской.

Теперь, что такое `IN` и `OUT`, от которых наш псевдокод получает IP и отправляет их? Это не имена соединений, (это привязало бы код компонента к конкретному месту в сети) это вещи, называемые «портами». «Portae» на латыни означает «ворота», а порты (например, морские порты) можно рассматривать как определенные места в стене или границе, куда вещи или люди входят или выходят. Вайнберг (1975) описывает порт как

> ... особое место на границе, сквозь которое текут ввод и вывод... Только в пределах портов могут иметь место опасные процессы ввода и вывода, и, локализуя эти процессы, можно задействовать специальные механизмы, решающие особые проблемы ввода-вывода

Теперь двери имеют «внутренний» и «внешний» аспекты - имя внутреннего аспекта может использоваться людьми внутри дома для обозначения дверей, например «выпустите кошку через боковую дверь», а внешний аспект связан с тем, на что открывается дверь, и будет интересен городским планировщикам или посетителям. _Порты в FBP выполняют ту же двойную функцию_: они позволяют FBP-компоненту обращаться к себе без необходимости знать, куда они открываются. Имена портов устанавливают связь между получением и отправкой внутри программы и информацией о структуре программы, определенной вне компонента. Это похоже параметры функций, где внутреннее (параметры) и внешнее (аргументы) должны соответствовать, даже если компилируются отдельно. В случае параметров это соответствие устанавливается посредством позиции (порядкового номера). Фактически, в AMPS и DFDM порты определялись по номерам, а не по именам. Хотя это соглашение обеспечивает повышенную производительность, позволяя определять порты быстрее, наш опыт показывает, что пользователям обычно легче связать имена, чем числа - точно так же, как мы говорим «черный ход» и «входная дверь», а не "дверь 1", "дверь 3" и т.д. По этой причине THREADS использует имена портов, а не номера.

_Некоторые порты могут быть определены как массивы_, поэтому на них можно ссылаться не только по имени, но и по индексу. Таким образом, вместо отправки на один порт `OUT`, некоторые компоненты могут иметь переменное количество портов `OUT` и, следовательно, могут сказать «отправить этот IP на первый (или n-ый) порт `OUT`». Это может быть очень полезно для компонентов, которые, скажем, делают несколько копий заданного куска данных. Отдельные слоты порта-массива называются «_элементами порта_». Вы можете естественным образом использовать порты-массивы в качестве входных портов, которые могут, например, использоваться в компонентах, выполняющих различные виды операций слияния. Наконец, компоненту предоставляется метод, позволяющий узнать, сколько элементов порта типа массива подключено и какие из них. В DFDM нам не нужны были порты типа массива, поскольку порты были пронумерованы, поэтому вы могли определить, скажем, порты с 3 по 20 как единый массив.  

Теперь давайте нарисуем изображение компонента, который мы построили выше. Он называется «фильтром» и выглядит так:

Figure 3.5

This component type has a characteristic "shape", which we will encounter frequently in what follows. FBP is a graphical style of programming, and its usability is much enhanced by (although it does not require) a good picture-drawing tool. Pictures are an international language and we have found that FBP provides an excellent medium of communication between application designers and developers and the other people in an organization who need to be involved in the development process.

Now we will draw a different shape of component, called a "selector". Its function is to apply some criterion "c" to all incoming IPs (they are received at port IN), and send out the ones that match the specified criterion to one output port (ACC), while sending the rejected ones to the other output port (REJ). Here is the corresponding picture:

Figure 3.6

You will probably have figured out the logic for this component yourself. In any case, here it is:

        receive from IN using a
        do while receive has not reached end of data
            if c is true
                send a to ACC
            else
                send a to REJ
            endif
            receive from IN using a
        enddo

Figure 3.7

Writing components is very much like writing simple main-line programs. Things really get interesting when we decide to put them together. Suppose we want to apply the filter to some data and then apply the selector to the output of the filter: all we need to record is that, for this application, OUT of FILTER is connected to IN of SELECTOR. You will notice that IN is used as a port name by both FILTER and SELECTOR, but this is not a problem, as port names only have to be unique within a given component, not across an entire application.

Let us draw this schematically:

Figure 3.8

We have just drawn our first (partial) FBP structure! FBP software can execute this kind of diagram directly (without our having to convert it to procedural code), and in fact you can reconfigure it in any way you like - add components, remove them, rearrange them, etc., endlessly. This is the famous "Legoland programming" which we have all been waiting for!

Now what is the line marked "C" in the diagram? It is the connection across which IPs will travel when passing from FILTER to SELECTOR. It may be thought of as a pipe which can hold up to some maximum number of IPs (its "capacity"). So to define this structure we have to record the fact that OUT of FILTER is connected to IN of SELECTOR by means of a connection with capacity of "n".

Now how do we prove to our satisfaction that this connection is processing our data correctly? Well, there are two constraints that apply to IPs passing between any two processes. If we use the names in the above example, then:

    every IP must arrive at SELECTOR after it leaves FILTER

    any pair of IPs leaving FILTER in a given sequence must arrive at SELECTOR in the same sequence

The first constraint is called the "flow" constraint, while the second is called the "order-preserving" constraint. If you think about it using a factory analogy, this is all you need to ensure correct processing. Suppose two processes A and B respectively generate and consume two IPs, X and Y. A will send X, then send Y; B will receive X, then receive Y (order-preserving constraint). Also, B must receive X after A sends it, and similarly for Y (flow constraint). This does not mean that B cannot issue its "receives" earlier - it will just be suspended until the data arrives. It also does not matter whether A sends Y out before or after B receives X. The system is perfectly free to do whatever results in the best performance. We can show this schematically - clearly the second diagonal line can slide forward or back in time without affecting the final result.

Figure 3.9

Connections may have more than one input end, but they may only have one output. IPs from multiple sources will merge on a connection, arriving at the other end in "first come, first served" sequence. It can be argued that by allowing this, we lose the predictability of the relationship between output and input, but it is easy enough to put a code in the IPs if you ever want to separate them again.

Up to now, we have been ignoring where IPs come from originally. We have talked about receiving them, sending them and dropping them, but presumably they must have originally come into existence at some point in time. This very essential function is called "create" and is the responsibility of whichever component first decides that an IP is needed. The "lifetime" of an IP is the interval between the time it is created and the time it is destroyed. From one point of view it is obvious that something needs to be created before it can be used, but how does this apply to a business application? Well, a lot of the IPs in an application are created from file records: generally file records are turned into IPs at file reading time, and the IPs are turned back into file records (and then destroyed) at file writing time. There are two standard components to do these functions (Read Sequential and Write Sequential), which we will talk more about in succeeding chapters. However, it often happens that you decide to create a brand new IP for a particular purpose which may never appear on a file - one example of these are the "control IPs" which we will describe in a later chapter. Another example might be a counting component which counts the IPs it receives, using a "counter" IP. This IP is used to maintain the count, and is finally sent to the output port when the counter terminates. Such a component will receive the IPs being counted, but, before it starts counting, it has to create a counter IP in which the count is to be maintained.

This is the typical logic of a Counter component (by the way, this kind of component normally tries to send incoming IPs to an output port, and drops them if this port is not connected):

        create counter IP using c
        zero out counter field in counter IP
        receive from IN using a
        do while receive has not reached end of data
            increment count in counter IP
            send a to OUT
            if send wasn't successful,
                drop a
            endif
            receive from IN using a
        enddo
        send c to COUNT port

Figure 3.10

To discover whether OUT is connected, we simply try to send to this port. If the send works, the IP is disposed of; if not, we still have it, so we dispose of it by dropping it. What if COUNT is not connected? Since the whole point of this component is to calculate a count, if COUNT is not connected there's not much point in even running this component, so it would be even better to test this right up front.

As we said above, all IPs must eventually be disposed of, and this will be done by some function which knows that the IP in question is no longer needed. This will often be Writer components, but not necessarily. The Selector example above should really be enhanced as follows:

        receive from IN using a
        do while receive has not reached end of data
            if c is true
                send a to ACC
            else
                send a to REJ
            endif
            if the send wasn't successful,
                drop a
            endif
            receive from IN using a
        enddo

Figure 3.11

At the beginning of this chapter, we talked about data as being primary. In FBP it is not file records which are primary, but the IPs passing through the application's processes and connections. Our experience is that this is almost the first thing an FBP designer must think about. Once you start by designing your IPs first, you realize that file records are only one of the ways a particular process may decide to store IPs. The other thing file records are used for is to act as interfaces with other systems, but even here they still have to be converted to IPs before another process can handle them.

Let's think about the IPs passing across any given connection - what is their layout? Clearly it must be something that the upstream process can output and the downstream process can handle. The FBP methodology requires the designer to lay out the IPs first, and then define the transforms which apply to them. If two connected components have different requirements for their data, it is simple to insert a "transform" component between them. The general rule is that two neighbours must either agree on the format of data they share, or agree on data descriptions which encode the data format in some way. Suppose, for instance, that process B can handle two formats of IP. If the application designer knows that process A is always going to generate the first format, s/he may parametrize B so that it knows what to expect. If A is going to generate an unpredictable mix of the two formats, it will have to indicate to B for each IP what format it is in, e.g. by an agreed-upon code in a field, by IP length, by a preceding IP, or whatever. An interesting variant of this is to use free-form data. There may be situations where you don't want to tie the format of IPs down too tightly, e.g. when communicating between subsystems which are both undergoing change. To separate the various fields or sections, you could imbed delimiters into the data. You will pay more in CPU time, but this may well be worth it if it will reduce your maintenance costs. This is why, for instance, communication formats between PCs and hosts often use free-form ASCII separated by delimiters (binary fields present unique problems when uploading and downloading data). Lastly, the more complete FBP implementations provide mechanisms for attaching standard descriptions to IPs, called Descriptors, allowing them to be used and reused in more and more applications. Descriptors allow individual fields in IPs to be retrieved or replaced by name - I will be describing them in more detail in a later chapter.

The next concept I want to describe is the ability to group components into packages which can be used as if they were components in their own right. This kind of component is called a "composite component". It is built up out of lower-level components, but on the outside it looks just like any other component. Components which are not built up of lower-level components are called "elementary", and are usually written in some Higher Level Language (DFDM supports both PL/I and VS COBOL II), while THREADS supports C. DFDM as distributed also includes a small set of "starter set" components written in a low-level programming language for performance reasons, but it is not expected that the majority of users will need to code at this level.

To make a composite component look like other components from the outside, obviously it must have ports of its own. We will therefore take the previous diagram, and show how we can package it into a composite called COMPA:

Figure 3.12

Once we have done this, COMPA can be used by anyone who knows what formats of data may be presented at port IN of COMPA and what formats will be sent out of its ACC and REJ ports. You will notice that it is quite acceptable for our composite to have the same port names as one of its internal components. You might also decide to connect the ACC port inside to the REJ port outside, and vice versa - what you would then have is a Rejector composite process, rather than an Acceptor. Of course COMPA is not a very informative name, and in fact we probably wouldn't bother to make this function a composite unless we considered it a useful tool which we expected to be able to reuse in the future.

Notice also that we have shown the insides of COMPA - from the outside it looks like a regular component with one input port and two output ports, as follows:

Figure 3.13

Now, clearly, any port on a composite must have corresponding ports "on the inside". However, the inverse is not required - not all ports on the inside have to be connected on the outside - if an inside component tries to send to an unconnected composite port, it will get a return of "unconnected" or "closed", depending on the implementation.

We have now introduced informally the ideas of "port", "connection", "elementary component", "composite component" and "information packet".

At this point, we should ask: just what are the things we are connecting together? We have spoken as though they were components themselves, but actually they are uses or occurrences of components. There is no reason why we cannot use the same component many times in the same structure. Let us take the above structure and reverse it. We then get the following:

Figure 3.14

Let's attach another FILTER to the REJ port of SELECTOR. Now the picture looks like this:

Figure 3.15

Here we have two occurrences of the FILTER component running concurrently, one "filtering" the accepted IPs from SELECTOR, the other filtering the rejected ones. This is no different from having two copiers in the same office. If we have only one copier, we don't have to identify it further, but if we have more than one, we have to identify which one we mean by describing them or labelling them - we could call one "the big copier" and the other "the little copier", or "copier A" and "copier B". In DFDM we took the function name and qualified it - in THREADS we make what might be called the "in context" function the name, and qualify it to indicate which component we are using. These component occurrences are called processes, and it is time to explain this concept in more depth.

In conventional programming, we talk about a program "performing some function", but actually it is the machine which does the function - the program just encodes the rules the machine is to follow. In conventional programming, much of the time we do not have to worry about this, but in FBP (as also in operating system design and a number of other specialized areas of computing) we have to look a little more closely at this idea. In FBP, the different components are executed by the CPU in an interleaved manner, where the CPU gives time to each component in turn. Since you can have multiple occurrences of the same component, with each occurrence being given its own series of time slots and its own working storage, we need a term for the thing which the CPU is allocating time slices to - we call this a "process". The process is what the CPU "multithreads" (some systems distinguish between processes and threads, but we will only use the term "process" in what follows). Since multiple processes may execute the same code, we may find situations where the first process using the code gets suspended, the code is again entered at the top by another process, which then gets suspended, and then the first process resumes at the point where it left off. Obviously, the program cannot modify its own code (unless it restores the code before it can be used by another process), otherwise strange things may happen! In programming terms, the code has to be read-only. In IBM's MVS, this kind of program is called reentrant, which is not quite as stringent as read-only, but read-only implies reentrancy, and I have found that making code read-only is a good programming discipline, as it makes a clear distinction between variable data, on the one hand, and program code together with constants, on the other. Although this may sound arcane, it is not really that far removed from everyday life. Imagine two people reading a poster at the same time: neither of them needs to be aware of the point in the text the other one has reached. Either one of the readers can go away for a while and come back later and resume at the point where he or she left off, without interfering in the least with the other reader. This works because the poster does not change during the reading process. If, on the other hand, one person changes the poster while the other is trying to read it, they would have to be synchronized in some way, to prevent utter confusion on the part of the reader, at least.

We can now make an important distinction: composite components contain patterns of processes, not components. This becomes obvious when you think of a structure like the previous one - the definition of that composite has three nodes, but two of them are implemented by the same component, so they must be different processes. Of course, they don't become "real" processes until they actually run, but the nodes correspond one-to-one with processes, so they can be referred to as processes without causing confusion. Here is the same diagram shown as a composite:

Figure 3.16

When this composite runs, there will be three processes running in it, executing the code of two components.

Lastly, I would like to introduce the concepts of data streams and brackets. A "stream" is the entire series of IPs passing across a particular connection. Normally a stream does not exist all at the same time - the stream is continually being generated by one process and consumed by another, so only those IPs which can fit into the connection capacity could possibly exist at the same time (and perhaps not even that many). Think of a train track with tunnels at various points. Now imagine a train long enough that the front is in one tunnel while the end is still in the previous tunnel. All you can see is the part of the train on the track between the tunnels, which is a kind of window showing you an ever-changing section of the train. The IP stream as a whole is a well-defined entity (rather like the train) with a defined length, but all that exists at any point in time is the part traversing the connection. This concept is key to what follows, as it is the only technique which allows a very long stream of data to be processed using a reasonable amount of resources.

Just as an FBP application can be thought of as a structure of processes linked by connections, an application can also be thought of as a system of streams linked by processes. Up to now, we have sat on a process and watched the data as it is consumed or generated. Another, highly productive way of looking at your application is to sit on an IP and watch as it travels from process to process through the network, from its birth (creation) to its death (destruction). As it arrives at each process, it triggers an activity, much like the electrical signal which causes your phone to ring. Electrical signals are often shown in the textbooks like this:

trailing edge leading edge

Figure 3.17

The moment when the leading edge reaches something that can respond to it is an event. In the same way, every IP has both a data aspect and a control aspect. Not only is an IP a carrier of data, but its moment of arrival at a process is a distinct event. Some data streams consist of totally independent IPs, but most streams are patterns of IPs (often nested, i.e. smaller patterns within larger patterns) over time. As you design your application, you should decide what the various data streams are, and then you will find the processes falling out very naturally. The data streams which tend to drive all the others are the ones which humans will see, e.g. reports, etc., so you design these first. Then you design the processes which generate these, then the processes which generate their input, and so on. This approach to design might be called "output backwards"....

Clearly a stream can vary in size all the way from a single IP to many millions, and in fact it is unusual for all the IPs in the stream to be of the same type. It is much more common for the stream to consist of repeating patterns, e.g. the stream might contain multiple occurrences of the following pattern: a master record followed by 0 or more detail records. You often get patterns within patterns, e.g.

'm' patterns of:
city record, each one followed by
'n' patterns of:
customer record, each one followed by
'p' sales detail records for each customer

Figure 3.18

You will notice that this is in fact a standard hierarchical structure. The stream is in fact a "linearized" hierarchy, so it can map very easily onto (say) an IMS data base.

To simplify the processing of these stream structures, FBP uses a special kind of IP called a "bracket". These enable an upstream process to insert grouping information into the stream so that downstream processes do not have to constantly compare key fields to determine where one group finishes and the next one starts. As you might expect, brackets are of two types: "open brackets" and "close brackets". A group of IPs surrounded by a pair of brackets is called a "substream". In THREADS, we use IPs with "type" of "(" and ")" for open and close brackets, respectively.

We have one more decision to make before we can show how we might use brackets in the above example. We have two cases where a single IP of one type is followed by a variable number of IPs of a different type (or substreams). The question is whether the open bracket should go before the single IP or after it. In the former case, we might see the following (I'll use brackets to represent bracket IPs):

< city1
< cust11 detl111 detl112...>
< cust12 detl111 detl112...>>
< city2
< cust21 detl211 detl122...>
< cust22 detl221 detl222...>>
etc.

Figure 3.19

In the latter case, we would see:

    city1 <
       cust11 < detl111 detl112...>
       cust12 < detl111 detl112...>>
    city2 <
       cust21 < detl211 detl122...>
       cust22 < detl221 detl222...>>

etc.

Figure 3.20

From the point of view of processing, these two conventions are probably equivalent, but I tend to prefer the first one as it includes each customer IP in the same substream as its detail records, and similarly includes each city in the same substream as the customers that belong to that city. As of the time of writing, there is no strongly preferred convention, but you should make sure that all specifications for components which use substreams state which convention is being used. By the way, a very useful technique when processing substreams is the use of "control" IPs to "represent" a stream or substream. Both AMPS and THREADS and some of the versions of DFDM have the concept of a process-related stack, which is used to hold IPs in a LIFO sequence. In Chapter 9, I will be describing how these concepts can be combined.

We have now introduced the following concepts:

    process

    component (composite and elementary)

    information packet (IP)

    structure

    connection

    port and port element

    stream

    substream

    bracket

Normally at this stage in a conventional programming manual we would leave you to start writing programs on your own. However, this is as unreasonable as expecting an engineer to start building bridges based on text-book information about girders and rivets. FBP is an engineering-style discipline, and there is a body of accumulated experience based on the above concepts, which you can and should take advantage of. Of course, you will develop your own innovations, which you will want to disseminate into the community of FBP users, but this will be built on top of the existing body of knowledge. You may even decide that some of what has gone before is wrong, and that is standard in an engineering-type discipline also. Isaac Newton said: "If I have seen further than other men, it is because I have stood on the shoulders of giants." Someone else said about (conventional, not FBP) programming: "We do not stand on their shoulders; we stand on their toes!" Programmers can now stop wearing steel-toed shoes!

Before we can see how to put these concepts together to do real work, two related ideas remain to be discussed: the design of reusable components and parametrization of such components (see the next chapter).
