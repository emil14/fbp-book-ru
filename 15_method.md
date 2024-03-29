# Реализация, разделение сетей и клиент/сервер

> «Неизменно обнаруживается, что сложная система, которая работает, возникла из простой системы, которая работает», — Джон Галл (1978).

> «Системы работают лучше, когда предназначены для работы "под спуск" - системы, ориентированные на человеческую мотивацию, иногда работают. Системы, противодействующие ей, работают плохо или не работают вообще», Джон Галл (1978).

> Гибкие системы служат дольше и работают лучше, Джон Галл (1978).

Мой коллега, Дейв Олсон, изучал процесс разработки приложений в частности и крупные организации в целом и понял, что зарождающаяся наука о хаосе может многое рассказать нам о том, что происходит в этом сложном процессе и что мы можем с этим поделать. При изучении хаоса часто приходится сталкиваться с петлями обратной связи, и в нашем бизнесе, например, мы все сталкивались с тем, что происходит, когда проект начинает задерживаться. Фред Брукс описал подобные вещи в своей знаменитой книге «Мифический человеко-месяц» (1975). Хаотические системы также характеризуются областями порядка и явного беспорядка. В недавней книге Дэйва описаны эти концепции, а также, как их можно применять для объяснения и решения многих проблем, с которыми мы сталкиваемся в нашем бизнесе (Олсон, 1993). В разделе о методах уменьшения беспорядка он говорит о DFDM и его связи с методологией инверсии Джексона (упомянутой в других местах этой книги). Позже он дает более подробную информацию о DFDM и описывает, как вы могли бы подойти к разработке с его помощью следующим образом:

Чтобы использовать DFDM, пишет он, вы сначала определяете требования к данным приложения, как они передаются и преобразуются в процессе. Затем создаете модули преобразования для мест в потоке, где данные должны быть объединены, разделены, преобразованы или представлены. Полное описание приложения состоит из определений данных и потоков данных, определенных для DFDM, и модулей преобразования, которые вызываются платформой.

В личном общении со мной он расширяет это до следующего набора рекомендаций:

- Просмотрите приложение с точки зрения информации, которую необходимо перемещать с места на место.
- Создайте модули преобразования для мест, где данные должны быть объединены, разделены, преобразованы или представлены.
- Отделяйте определения данных от кода преобразования, чтобы преобразования можно было использовать повторно. Таким образом, преобразования больше заботятся о том, что должно быть сделано, а не о том, как представлены отдельные фрагменты информации.
- Обеспечьте преобразования для стандартных блоков, которые потребуются большинству приложений - хранение, извлечение, копирование файлов, печать, взаимодействие с пользователем и т. д.
- Создайте системную структуру, чтобы приложение можно было определить с точки зрения его потоков данных и преобразований; платформа обрабатывает планирование, параллелизм и передачу данных между преобразованиями.

Не очень то похоже на обычное программирование, не так ли? Однако, просмотр приложения с точки зрения данных и применяемых к нему преобразований лежит в основе многих методологий проектирования. Проблема всегда заключалась в том, как перейти от такого дизайна к работающей программе. Это и есть тот «разрыв», о котором я говорил выше. С FBP вы можете перейти от дизайна самого высокого уровня к запуску кода, не меняя "точки зрения".

Первым этапом разработки приложения является размещение наших процессов и потоков между ними. Это стандартный структурный анализ, о котором много писали Уэйн Стивенс и другие. Конечно, объем вашего приложения иногда не очевиден — мы можем нарисовать нашу сеть процессов такой большой или маленькой, как мы хотим. Система — это то, что вы обводите пунктирной линией. Под этим я подразумеваю, что граница любой системы определяется произвольным решением, согласно которому в практических целях часть мира будет рассматриваться, а часть игнорироваться. В астрономии нашей Солнечной системы мы можем считать влияние извне Солнечной системы незначительным, и в большинстве случаев этого хватает, хотя теоретически каждый объект во Вселенной влияет на все остальные. Вы можете думать, что ваша кожа отделяет четко очерченное «вы» от остальной вселенной, но биохимия учит нас тому, что всевозможные молекулы постоянно проходят через этот мнимый барьер. И действительно, все молекулы в нашем организме полностью заменяются в течение нескольких лет. Это идея дзэн, доведенная до крайности, она говорит, что объекты не имеют объективного существования. Мы просто «разбиваем» вселенную на куски. И мы также должны осознавать, как слова, которые мы используем, влияют на то, как мы воспринимаем вселенную. Как лингвист, я нахожу ряд работ Б.Л. Уорфа поразительным: в одном примере (Whorf 1956) он приводит слово на одном из индейских языков, описывающее людей, путешествующих на каноэ, в котором нет идентифицируемого корня, означающего «каноэ». Каноэ настолько предполагаемая часть их опыта, что его не нужно называть явно.

Чтобы проектировать систему, мы должны "очертить" её. Это первое решение имеет важнейшее значение и может быть не таким уж очевидным. Поэтому рассмотрите контекст системы, которую вы проектируете: иметь четко определенные (и в не слишком большом количестве) интерфейсы с системами за пределами выбранной вами системы.

Теперь вы можете проектировать процессы и потоки вашей системы. Постепенно углубляйтесь в процессы, пока не дойдете до уровня уже существующих компонентов. В FBP вы увидите, что они начинают влиять на ваш дизайн по мере приближения к ним. Это один из моментов, где FBP отличается от обычного программирования, но это не должно нас сильно удивлять, поскольку мы не проектируем мосты или дома исключительно сверху вниз или исключительно снизу вверх. Как вы можно «разложить» проект дома так, чтобы в результате получилось изобретение гипсокартона? Наоборот, вы должны знать, куда вы идете, чтобы иметь возможность определить, приближаетесь ли вы к цели. Цели и средства должны сойтись. Дизайн — это осознанный процесс использования существующих материалов и идей или изобретения новых для достижения желаемой цели.

На пути к своей цели, как и любой путешественник, вы должны быть готовы и способны менять свои планы на ходу. Преимущество программирования в том, что вы можете очень легко переключаться между «настоящей» программой и ее симуляцией, так что вы можете попробовать что-то и модифицировать, пока не будете довольны. На самом деле, с помощью FBP мы можем реализовать старую мечту программистов о возможности вырастить приложение из его симуляции. Вы можете заменить процессы компонентами, которые просто используют «x» секунд времени, и наоборот.

Я считаю, что в программировании не должно быть строгого различия между прототипом и реальной программой до тех пор, пока реальная программа не будет запущена в производство. Если что-то в окружающей среде или в вашем понимании изменится, так что старое предположение больше не будет верным, вы должны быть в состоянии изменить свой дизайн. Хотя я прекрасно понимаю, что стоимость изменений возрастает по мере того, как вы продвигаетесь в процессе разработки, вы должны знать, когда сократить свои потери и вернуться назад и внести некоторые изменения в дизайн. Если поможет, то подумайте, насколько тяжелее это сделать, скажем, хирургу!

Если вы объедините FBP с итеративной разработкой, я считаю, вы получите очень мощную и отзывчивую методологию разработки. Вы можете попробовать разные подходы, вносить изменения дешево, а когда приходит время расставить все точки над i и зачеркнуть t, это, по сути, линейный процесс. Для сравнения, я считаю, что главный недостаток методологии «водопада» заключался в том, что возвращаться назад настолько неудобно, что команды разработчиков идут вперед, шаг за шагом погружаясь все глубже в болото. Когда надо что-то переделывать, им приходится делать вид, что они занимаются разработкой (или даже тестированием!). И снова Дэйв Олсон: «Программисты знают, что детализированные процессы линейной разработки не соответствуют тому, как делается реальное программирование, и тем не менее, планы и расписания составляются с использованием этой идеи». Далее он подчеркивает, что, хотя есть некоторые проекты, для которых процесс «водопада» будет работать нормально, вы не можете и не должны впихивать в него каждый проект.

Помимо итеративного процесса углубления для сетей, который мы только что описали (вертикальный), я обнаружил, что в FBP есть еще один процесс, который ортогонален ему. Это процесс разрезания сетей по горизонтали, чтобы решить, как и где они будут работать. В этом процессе мы назначаем части нашей сети различным «движкам» или средам. Поскольку системы FBP спроектированы с точки зрения коммуникационных процессов, эти процессы могут выполняться на самых разных машинах или платформах — фактически везде, где есть средства связи.

Когда вы думаете о том, как можно разделить поток, помните, что вы можете использовать все, что взаимодействует с помощью данных, что в обработке данных означает практически любое оборудование или программное обеспечение. Я называю этот процесс «расщеплением». Сети могут быть расщеплены (или раздвоены) любым из следующих способов (в произвольном порядке и не исключая друг друга):

- Несколько компьютеров (хост, средний уровень, ПК или смесь)
- Несколько процессоров на одном компьютере
- Несколько задач
- Различные географические объекты
- Несколько шагов задания внутри задания
- Вторичные транзакции
- Клиент/сервер
- Несколько виртуальных машин
- Etc.

Все они имеют свои особенности, но это похоже на реальный мир, где есть много разных процессов, и все они взаимодействуют по-разному. Сегодня я могу позвонить другу по телефону, отправить ему или ей написанное от руки письмо, отправить его по факсу, связаться по электронной почте, послать телеграмму или сунуть записку в бутылку, бросить ее в Атлантику и надеяться, что она дойдет. Доставили в итоге! У каждого из них есть плюсы и минусы.

![fig15.1](https://jpaulmorrison.com/fbp/Fig15.1.gif)

Фрагмент 15.1

Предположим, что по какой-то причине мы хотим запустить это как несколько этапов одной батч-джобы. Теперь мы знаем, что этапы задания могут выполняться только последовательно. Хотя их можно пропустить, их нельзя повторить. Это означает, что B и C должны находиться на одном шаге задания. Однако при желании A и D можно разделить на разные этапы. Предположим, мы решили сделать A и D отдельными шагами — в итоге получится 3 этапа джобы.

Мы также знаем, как взаимодействуют этапы — с помощью файлов данных (еще один вид потока данных). В FBP мы можем заменить любое соединение последовательным файлом, поэтому мы можем изменить соединения, ведущие из A, и те, что ведут в D, к файлам. Как только мы разделим два процесса на разные этапы, конечно, все соединения между ними должны быть заменены файлами. Каждому из них, в свою очередь, потребуется писатель и читатель. Используя пунктирные линии для обозначения границ шагов и [X] для обозначения файлов, наша диаграмма теперь выглядит следующим образом:

![fig15.2](https://jpaulmorrison.com/fbp/Fig15.2.gif)

Фрагмент 15.2

Я показал авторам и читателям файлы буквами W и R, чтобы сохранить сходство между двумя предыдущими рисунками. Это обычные процессы, хоть они не показаны в виде прямоугольников. Таким образом, шаги 1 и 3 имеют по 3 процесса каждый; шаг 2 имеет 4.

Если мы покажем эту картинку на уровне шага, мы увидим, мы могли спроектировать ее таким образом ранее в процессе, а затем разбить ее, чтобы получить предыдущую цифру.

![fig15.3](https://jpaulmorrison.com/fbp/Fig15.3.gif)

Фрагмент 15.3

Однако, гораздо лучше спроектировать полное приложение, а затем разделить его, так как другие соображения могут повлиять на наши решения, как сеть должна быть разделена. (Например, проблема, о которой я упоминал в предыдущей главе - невозможность перекрытия сортировок, заставляет многие из них выполнять отдельные этапы работы!) На самом деле, поскольку так легко перемещать процессы с одного этапа на другой, особый момент в раннем разделении нашей сети - вы также можете оставить это в последний момент. В обычном программировании перемещение логики с одного этапа работы на другой очень сложно из-за резкого изменения майндсета при переходе от проектирования потока данных к реализации потока управления, поэтому решение о том, какими будут этапы работы, должно быть принято заранее. Сделанное очень рано оно имеет тенденцию влиять на весь процесс реализации с этого момента.

Я хотел бы сделать еще одно замечание относительно этого довольно простого примера: на рис. 15.2 вы видите, что связь между процессами A и B управляется двумя процессами и файлом. В FBP это обычный шаблон, когда любые два процесса взаимодействуют не через соединения FBP. Таким образом, линия связи может «управляться» процессом отправки [VTAM](https://www.ibm.com/docs/en/zos/2.1.0?topic=guide-vtam-networking-concepts) на одном конце и процессом получения VTAM на другом. Позже в этой главе я приведу несколько примеров других видов связи между сетями, и вы увидите этот шаблон во всех них.

Более тонкий момент заключается в том, что переход от рис. 15.1 к рис. 15.2 осуществлен исключительно за счет манипулирования сетью без необходимости усложнения лежащего в её основе программного обеспечения. Это связано с принципом открытых архитектур, о котором я говорил в предыдущей главе: наш опыт показывает, что гораздо лучше реализовать такие подсистемы, как обработка файлов и обмен данными, как видимые процессы под контролем разработчика, чем похоронить их в программной инфраструктуре. Поскольку последнее является обычной практикой среди разработчиков, все, что я могу сказать - это предпочтение основано на солидном опыте. Подумайте о преимуществах: Write-file-Read становится всего лишь одним механизмом, поддерживающим определенный способ расщепления сетей — мы могли бы использовать множество других и даже переходить от одного к другому в процессе разработки.

Чтобы показать, что было бы, пойди мы другим путем, рассмотрим, что произошло бы, если бы мы решили иметь два типа соединений: внутришаговое соединение и межшаговое соединение. Давайте возьмем рис. 15.1 и назначим каждое соединение одной из этих двух категорий в зависимости от того, пересекает ли оно границу шага или нет (мы предполагаем, что вы делаете это одновременно с разделением сети на шаги задания).

![fig15.4](https://jpaulmorrison.com/fbp/Fig15.4.gif)

Фрагмент 15.4

Я утверждаю, что мы усложнили и реализацию, и ментальную модель, не предоставив разработчику никаких дополнительных функций. Добавленная сложность не только затруднит визуализацию системы и, следовательно, усложнит устранение неполадок, но мы также можем сделать некоторые полезные функции операционной системы недоступными или доступными только через еще более сложный интерфейс. И если программист хочет заменить межшаговые соединения, скажем, базой данных [DB2](https://www.ibm.com/products/db2), ему все равно придется добавлять дополнительные процессы и менять межшаговые соединения обратно на внутришаговые...

Другой способ представить это - при разработке ПО очень важно определиться с его областью применения. Объем данной программной системы, очевидно, зависит от конкретной среды, в которой она работает, но иногда не очевидно, какая из различных конструкций этой среды должна быть основной «единицей». В случае DFDM из всех вышеприведенных описаний и примеров, вероятно, будет очевидно, что сеть DFDM соответствует одному из:

- Шаг задания MVS
- IMS-транзакция
- БМП
- Batch job
- CMS "программа"

Однако, на ранних стадиях DFDM вовсе не было очевидно, что это правильное решение. Даже приведенное выше утверждение CMS несколько расплывчато, потому что слово «программа» — одно из самых запутанных во всей науке «программирования»! Рассмотрим рис. 15.4 выше: если бы мы реализовали DFDM таким образом, областью действия приложения DFDM было бы задание MVS, а не шаг задания. Этого аналога нет ни в IMS (за исключением, возможно, «разговора»), ни в CMS, поэтому программистам было бы сложнее перейти из одной из этих сред в другую. Я всегда считал наш выбор масштаба сети одним из ключей к удобству использования DFDM. В THREADS сеть реализована в виде файла .EXE, который, я думаю, также обычно называют «программой».

Таким образом, система должна иметь компоненты с четко определенным объемом, с видимыми, управляемыми интерфейсами между ними — фактически очень похожими на саму FBP! Такие системы (и, я полагаю, только такие системы) затем легко объединяются в открытые архитектуры.

Давайте еще раз взглянем на рисунок 15.2. Мы могли бы нарисовать прямоугольник вокруг каждого из шаблонов, показанных как `----W [X] R---- `

Сделать его самостоятельным процессом. Этот процесс может прекрасно выполняться в пределах шага задания, и фактически мы уже встречали этот паттерн в главе 13 (Правила планирования), где мы нарисовали его следующим образом:

![fig15.5](https://jpaulmorrison.com/fbp/Fig15.5.gif)

Фрагмент 15.5

Помните, мы использовали автоматические порты, чтобы гарантировать, что программа чтения не запустится, пока программа записи не закончит запись файла? В целом, этот процесс больше похож на сортировку без функции сортировки! Он записывает весь поток данных на диск, затем считывает его и отправляет дальше. Мы можем сформулировать это немного иначе: он продолжает принимать IP, сохраняя их в порядке поступления, пока не обнаружит конец данных; затем он извлекает их и отправляет дальше. Это наводит на мысль о другом названии этой структуры — мы еще встретимся с ним в главе о взаимных блокировках, где она несколько причудливо названа «бесконечной очередью». Вы увидите, что это один из наиболее полезных способов предотвращения взаимоблокировок.

Почему мы назвали это «бесконечной очередью»? а) Раньше мы называли «соединения» «очередями». б) Использование файла для этого типа структуры дает нам фактически бесконечную емкость, потому что, хотя соединение имеет любую емкость, указанную разработчиком (или по умолчанию, если она не была указана), шаблон, показанный на рис. 15.6, может продолжать поглощать IP до тех пор, пока не достигнет максимального количества записей, разрешенного операционной системой или устройством хранения. На самом деле, мы могли бы еще больше увеличить его мощность, используя "смарт-код" внутри специального компонента.

Если процессы связаны очередями, как процесс определенного типа может сам называться «очередью»? В начале главы мы предположили, что процессы могут быть сколь угодно большими или маленькими. Из прагматических соображений мы решили выбрать определенное «зерно» процесса, которое мы будем реализовывать, и соединить их с помощью другого механизма, соединения. Существуют процессы более высокого уровня, которые мы называем подсетями, а самые высокие подсети — это шаги задания. Над этим находятся еще более высокие процессы, называемые заданиями, приложениями и т. д. Различные уровни этой иерархии, как правило, реализуются с помощью разных механизмов, но при необходимости мы всегда можем перемещать процессы вверх или вниз по иерархии. Относиться к соединению как к особому типу процесса — все равно, что использовать увеличительное стекло для увеличения части нашей сети. Каждый раз, когда мы хотим, чтобы соединение вело себя иначе, чем обычные соединения, мы просто заменяем его процессом (плюс соединения на обоих концах). Мы также могли бы использовать эту технику, если бы хотели иметь другой тип соединения между процессами, скажем, соединение, поддерживающее «приоритетную почту». Однако для этого потребуются дополнительные имена портов — многие авторы описывали процессы, реализующие соединения FBP (также известные как «ограниченные буферы»), но мы сочли более удобным поместить их в инфраструктуру.

По мере того, как я читал постоянно растущий объем литературы о параллельных процессах, я был поражен разницей в степени детализации в различных реализациях, от мелкозернистой до очень грубой. И на самом деле вы могли заметить, что процессы FBP варьируются от очень простых до довольно сложных. Так или иначе, они, похоже, поддерживают определение «размера зерна». В главе, посвященной производительности, мы поговорим о том, как степень детализации влияет на производительность и как этот факт можно использовать для достижения компромисса между ремонтопригодностью и производительностью.

Кроме того, мы не утверждаем, что уровень детализации, описанный в этой книге, является лучшим или окончательным для всех целей — только то, что это уровень, который, как мы обнаружили, обеспечивает хороший баланс между производительностью и выразительной силой. Он содержит полезные компоненты, и разработчики, кажется, могут с ним смириться! Вот сеть, которая реализует то, что раньше называлось «полусумматор», используя IP для представления битов и процессы FBP для моделирования логических операций (мы могли бы построить медленную симуляцию машины)!

![fig15.6](https://jpaulmorrison.com/fbp/Fig15.6.gif)

Фрагмент 15.6

Где RPL означает репликацию, AND — логический оператор AND, а XOR — логический исключающий OR.

# TODO
