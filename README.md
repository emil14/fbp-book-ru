# Потоко-Ориентированное Программирование: Новый подход к разработке приложений

> Представьте, кто-то говорит вам, что одна странная техника программирования, используемая крупнейшими банками Канады с 70х, даёт простое решение множеству проблем, с которыми сталкиваются сегодняшние программисты (многоядерные процессоры, распределенные вычисления, и т.д.), упрощает поддержку и обеспечивает плавный переход от стадии дизайна к рабочему коду. Сочтёте ли за сказку? Вероятно, да!

![fbp](media/fbp.png)

Тем не менее, это не вымысел! Первая реализация этих концептов, теперь зовущихся "потоко-ориентированным программированием", была использована, в частности, американским банком с 5,000,000 клиентов в середине 70х. С тех пор прошло больше 40 лет, а Потоко-Ориентированное Программирование (далее FBP) позволяет жить и развиваться этому приложению. В качестве бонуса, такие приложения ещё и работают, часто, быстрее, чем созданные привычным образом.

FBP это новейший стиль мышления, освобождающий программиста от Фон-Неймановских рамок - важнейшего барьера на пути в новый, многоядерный мир. FBP был реализован на Java и C# с помощью многопоточности. Уже сейчас эти наработки можно использовать и выжимать максимум из всех ядер. Конечно, потребуется сдвиг парадигмы, но сделав его однажды, назад не захочется!

FBP был изобретён/открыт ещё в конце 1960х и ушло несколько лет, чтобы раскрыть его потенциал. За это время мы поняли - эта техника не только решает кучу проблем процедурного подхода, но также может упросить ряд новых сложностей, с которыми мы только начинаем сталкиваться. FBP вышел на тропу войны.

Всё чаще и чаще от программистов слышно: **Go with the flow!**

FBP это подход к написанию приложений, определяющий последние через метафору "фабрики-данных". Некоторые его корни можно проследить у самых истоков компьютерных вычислений.

Приложение определяется как сеть асинхронно выполняющихся процессов, общающихся в понятиях потоков пакетов данных, называемых "информационные пакеты" (IP). Сами соединения являются внешними по отношению к процессу, так что процессы ничего не знают "о своих соседях". Соединения и процессы могут переподключаться бесконечно, удовлетворяя специфике самых разных приложений, без необходимости модифицировать сами процессы. Эта модель является _компонентно-ориентированной_.

В FBP часто нельзя предсказать порядок выполнения вычислений, что заставляет нервничать старомодных программистов. Так или иначе, как выяснилось, это и не обязательно (и никогда не было!) и посему акцент сдвигается с последовательности действий на трансформации потоков данных.

FBP - отличный выбор для многоядерных/многопроцессорных систем, а также современного встраиваемого ПО. FBP гораздо ближе, чем традиционные техники, к настоящей фабрике, где предметы движутся от станции к станции, подвергаясь различным трансформациям. Представьте фабрику газировки - на одной станции бутылки наполняются, на другой закупориваются и на третей обклеиваются этикеткой. Таким образом, FBP ещё и крайне легко поддаётся визуализации.

Как ни удивительно, но будучи на острие прогресса в решении прикладных задач, FBP легко поддаётся изучению т.к. даёт набор более простых, в сравнении с традиционными техниками, вычислительных абстракций.
