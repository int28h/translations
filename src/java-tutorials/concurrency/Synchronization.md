# Синхронизация  

[Сурс](https://docs.oracle.com/javase/tutorial/essential/concurrency/sync.html)  

Потоки взаимодействуют преимущественно путем разделения доступа к полям и ссылкам на объекты, на которые поля ссылаются. Эта форма коммуникации очень эффективная, но влечет за собой два рода ошибок - интерференцию потоков (thread interference) и ошибки согласованности памяти. Инструмент, который нужен для предотвращения этих ошибок - синхронизация.  

Однако синхронизация может породить конфликт потоков, когда два и более потоков пытаются получить доступ к одному и тому же ресурсу одновременно и в итоге Java выполняет один или более потоков медленнее или даже приостанавливает их выполнение. Голодание и лайвлоки (активные блокировки) - формы конкуренции потоков.  

В этом разделе будут рассмотрены следующие вопросы:  

+ Интерференция потоков (thread interference) - как появляются ошибки когда несколько потоков работают с общими данными;  
+ Ошибки согласованности памяти (memory consistency errors) - ошибки, возникающие в результате несогласованных представлений различных потоков об общей памяти;  
+ Синхронизированные методы (synchronized methods) -  простая идиома для решения проблем интерференции потоков и согласованности памяти;  
+ Внутренние блокировки (implicit locks) и синхронизация - описание синхронизации в более общем виде, основанной на внутренних блокировках;  
+ Атомарный доступ (atomic access) - общая идея операций, на которые не могут повлиять другие потоки.  
