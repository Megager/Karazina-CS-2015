## Задания по распределленным системам
Четыре лабораторки с такими баллами и сроками сдачи:
- 1, 15 баллов, до 31 Марта, Hadoop
- 2, 15 баллов, до 29 Апреля, Map-Reduce
- 3, 15 баллов, Apache Sppark
- 4, 15 баллов, Cassandra

Задание можно сдать после срока, но не позже чем за неделю до экзамена, за 2/3 стоимости. Задание можно пересдать за 3/4 стоимости. Оптимальная форма сдачи задания - прислать по почте (просьба  тексте письма указывать чья работа).

Лабораторную можно делать вдвоем, при этом в письме следует указать обоих авторов. Первые две лабораторные можно сдать просто удаленно. Для третьей и четвертой помимо присланного задания надо будет также пообщаться личнно.

Если сданные работы будут черезмерно похожи, это будет считаться читерством и засчитываться такие работы не будут. Читерские работы можно один раз пересдать за 1/2 стоимости. Заметьте, что такого результата можно достигнуть не только переписывая друг у друга, но и независимо переписывая из одних и тех же открытых источников.


#### Пары
График встреч, как мы с вами договаривались, раз в две недели с каждой группой, чередуя четверги и пятницы. Пятому курсу если надо встретиться, просьба приходить на пару к четвертому курсу.

- 26.02, Пятница, 3я пара
- 03.03, Четверг, 2я пара
- 11.03, Пятница, 3я пара
- 17.03, Четверг, 2я пара
- 25.03, Пятница, 3я пара
- 31.03, Четверг, 2я пара


## Hadoop, установка

### Вариант 1 - собрать Hadoop из исходников. 

Для Линукса описано здесь [HadoopInstal](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html), для Windows здесь [HadoopOnWindows](http://wiki.apache.org/hadoop/Hadoop2OnWindows). Под Линукс я ставить не пробовал, поэтому все дальнейшее относится к установке под Windows.

Подразумевается, что Hadoop собирается из исходников при помощи Maven. Перед сборкой рекоменддуется ознакомиться с требованиями к установке [building.txt](https://svn.apache.org/viewvc/hadoop/common/branches/branch-2/BUILDING.txt?view=markup). Без них действительно не собирается.

После сборки конфигурация осуществляется ссогласно части 3 в документе [HadoopOnWindows](http://wiki.apache.org/hadoop/Hadoop2OnWindows) и включае в себя настройку конфигурационных файлов, создание нового раздела HDFS (Hadoop Distributed File System), запуск демонов и наконец собственно тестовую операцию по помещению файла в hdfs. Вуглядеть это должно примерно так

```
C:\deploy>%HADOOP_PREFIX%\bin\hdfs dfs -put myfile.txt /

C:\deploy>%HADOOP_PREFIX%\bin\hdfs dfs -ls /
Found 1 items
drwxr-xr-x   - username supergroup          4640 2014-01-18 08:40 /myfile.txt

C:\deploy>
```

Следует заметить, что у меня собрать Hadoop таким образом так и не получилось.

### Вариант 2 - поставить виртуальную машину. 

От Cloudera [www.cloudera.com](http://www.cloudera.com/downloads/quickstart_vms/5-5.html#)
или Hortonworks [www.hortonworks.com](http://hortonworks.com/products/hortonworks-sandbox/#install). 

Это два самые популярные коробочные решения для Hadoop, доступны образы для VMWare или Oracle ViirtualBox. В обоих случаях сказано, что на виртуальную машину нужно выделить 4GB памяти, соответственно у меня ни один из вариантов не запустился. Хотя [здесь](https://beyondparadiseblog.wordpress.com/2013/09/04/installing-cloudera-quickstart-vm-with-virtalbox-on-mac-step-by-step/), например, вроде как ставят виртуалку на Мак с 2GB оперативки, выделяя на виртуальную машину 1GB.

### Вариант 3 - Hadoop из jar архива (получилось). 

Установка согласно вот этому описанию: [Tutorial](http://v-lad.org/Tutorials/Hadoop/03%20-%20Prerequistes.html). Качаете jar архив и запускаете его через особым образом сконфигурированный CygWin (шелл Линукса под Windows). Несколько замечаний:

- для корректной работы надо чтоб была прописана переменная окружения JAVA_HOME, у меня C:\Java\jdk1.8.0_74 (без \bin) в конце. Важно, чтоб путь не содержал пробелов, поэтому если у вас Java установлена в Program Files, то теоретически можно прописать в переменно Progra~1, но лучше переустановить.
- в туториале описывается интеграция Hadoop с Eclipse, но если собственно интеграция не нужна, то шаги связанные с установкой и настройкой Eclipse можно пропустить, Hadoop будет работать и сам по себе.
- в туториале используется Hadoop версии 0.19.1 - достаточно старой. Вероятно все заработает и с последней версией, но я не пробовал.
- по сравнению с установкой, описанной на сайте Хадупа, обращения выглядят немного иначе, в частнности команды ддля файловой системы в туториале начинаются с `hadoop fs`, на сайте - с `hdfs`.

Документация по командам hdfs находится на сайте [Apache](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html). Небольшой гайд по типичному использованию навскидку нашелся на сайте [Hortonworks](http://hortonworks.com/hadoop-tutorial/using-commandline-manage-files-hdfs/). 

Результат работающей hdfs ориентировочно такой:

![Happy Hadooping](https://github.com/BChornomaz/Karazina-CS-2015/blob/master/Hadoop/HapppyHadooping.jpg)

#### Скрипт

Запуск пяти компонентов Hadoop, описанных в тьюториале удобно делать из скрипта (например .bat). Идея в том, чтоб запустить пять экземпляров cygwin (файл mintty.exe), в каждом из которых вызвать запуск линуксового скрипта, сохраненного в отдельном файле (.sh). Для запуска линуксового скрипта используется утилита bash. 

Описание того, как делать скрипты для bash можно посмотреть [здесь](http://www.kossboss.com/windows---cygwin---how-to-start-a-sh-shell-script-from-a-windows-batch-bat-script).

### Лаба 1 - Hadoop, срок до 31 марта, 15 баллов.

Установить Hadoop одним из предложеных выше способов. В качестве результата работы загрузить в кластер файл со своим именем и вывести его содержимое на консоль. Прислать скриншот с консоли на которой выполнены соответствующие комманды.

Кроме того, для третьего способа  также написать скрипт (например .bat), который будет ззапускать hadoop-кластер, подсказки по написанию скрипта см. выше. Для остальных вариантов написать краткое описание установки. 

Владу Полупану как первопроходцу засчитывается в текущем виде.

### Лаба 2 - MapReduce, срок до 29 апреля, 15 баллов.

Написать MapReduce задачу, которая посчитает количество "Attribute, Case and Vote Lines" для [этого датасета](https://archive.ics.uci.edu/ml/datasets/Anonymous+Microsoft+Web+Data). Описание датасета там-же, но [на всякий случай](https://archive.ics.uci.edu/ml/machine-learning-databases/anonymous/anonymous-msweb.info).

Инструкции по написанию программ для Map-Reduce можно посмотреть [здесь](http://www.tutorialspoint.com/hadoop/hadoop_mapreduce.htm).

Несколько моментов, которые для меня сначала были проблематичны:
- запуск Map-Reduce делается при помощи модуля Yarn (входит в стандартный пакет Hadoop), хотя в старых версиях это может быть не так;
- запуск делается командой `jar` в `yarn`. Первый ее аргумент - запускаемый jar (находящийся локально), второй - запускаемый класс;
- выходной каталог для map-reduce задачи на момент запуска не должен существовать;
- jar-ы для компиляции типичного map-reduce приложения лежат в `$HADOOP_PATH/share/hadooop`. Скорее всего понадобятся `hadoop-common`, `hadoop-mapreduce-client-core` и `hadoop-mapreduce-client-common`.


### Лаба 3 - Apache Spark, TBA.

### Лаба 4 - Cassandra, TBA.




