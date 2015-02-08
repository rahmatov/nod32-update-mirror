# ESET Nod32 Update Mirror

![license MIT](http://gitshields.com/v2/text/license/MIT/green.png)&nbsp;![OS Linux](http://gitshields.com/v2/text/OS/Linux/blue.png)&nbsp;![version](http://gitshields.com/v2/text/version/0.4.2/lightgrey.png)

Набор скриптов для создания зеркала базы обновления антивируса "Eset Nod32". Для работы требуется:

  - Unix-система (тестировался на CentOS 7, FreeNas 9.3 и FreeBSD 8.3);
  - Bash (тестировался на версиях 4.1.11(2), 4.2.24(1) и 4.2.45(1));
  - Установленные `curl`, `wget`, `unrar` (опционально), и некоторые другие стандартные приложения.

![Console screenshot](http://habrastorage.org/files/f31/87a/2de/f3187a2dec1c4dc4872d62fa85b4fd0e.png)

----------
### <i class="icon-download"></i>Установка

- Скачиваем крайнюю версию и распаковываем:
```
$ cd /tmp
$ wget https://github.com/tarampampam/nod32-update-mirror/archive/master.zip
$ unzip master.zip; cd ./nod32-update-mirror-master/
```
- Переносим набор скриптов в директорию недоступную "извне", но доступную для пользователя, который будет его запускать:
```
$ mv -f ./nod32upd/ /home/
```
- Переходим в новое расположение скриптов и выполняем их [настройку](#настройка):
```
$ cd /home/nod32upd/
$ nano ./settings.cfg
```
- Даем права на запуск скриптов:
```
$ chmod +x ./*.sh
```
- Проверяем наличие `unrar`, если планируем обновляться с официальных зеркал Eset NOD32:
```
$ type -P unrar
```
> Для установки `unrar` в **CentOS 7** можете воспользоваться следующим способом:
> ```
> $ wget http://pkgs.repoforge.org/unrar/unrar-4.0.7-1.el6.rf.x86_64.rpm 
> $ rpm -Uvh unrar-4.0.7-1.el6.rf.x86_64.rpm; rm unrar-4.0.7-1.el6.rf.x86_64.rpm
> ```

- Выполняем пробный запуск:
```
$ ./update.sh
```
> Если у вас возникнут проблемы с запуском - вы всегда можете [сообщить нам об этом](https://github.com/tarampampam/nod32-update-mirror/issues/new).
> К сообщению **обязательно** прикладывайте содержимое файла `settings.cfg`, версии используемого ПО (`cat /proc/version`, `bash --version`, `wget -V`), и вывод работы скрипта **в неизмененном виде**!

----------
### <i class="icon-cog"></i>Настройка

Основные параметры указываются в файле `settings.cfg`, их названия и описания приведены в таблице ниже _(по умолчанию уже установлены оптимальные значения)_:

&nbsp; | Описание
---: | :-------------
`BASEPATH` | Путь к директории, где располагается набор скриптов
`updServer{N}` | Список серверов, с которых следует брать обновления. Указывается в формате `updServer{N}=('http://mirror.url/path/' 'username' 'password')`, где `{N}` является числом от 0 до 10 - порядковым номером сервера в очереди проверки
`getFreeKey` | (`true`\|`false`) Означает необходимость обновляться с официального зеркала ESET Nod32, ключ к которому необходимо взять с "пиратского" ресурса. При активировании данного функционала значение `updServer0` автоматически будет изменено на официальное зеркало. **ВНИМАНИЕ! ДАННЫЙ ФУНКЦИОНАЛ ТОЛЬКО ДЛЯ ОЗНАКОМЛЕНИЯ И ИЗУЧЕНИЯ! ВЫ САМИ НЕСЕТЕ ОТВЕТСТВЕННОСТЬ ЗА ЕГО ИСПОЛЬЗОВАНИЕ!**
`updPlatforms` | Указывает платформы и продукты, для которых брать обновления
`updTypes` | Типы файлов обновления, которые стоит включить в зеркало
`updLevels` | Уровни файлов обновлений
`updLanguages` | Языки файлов обновлений (касается модулей), полный список значений можно подсмотреть по [этой ссылке](http://kb.eset.com/esetkb/index?page=content&id=SOLN3308)
`checkSubdirsList` | Включить в зеркало так же указанные в этом параметре под-директории
`createLinksOnly` | (`true`\|`false`) Включает функцию создания зеркала путем создания лишь файла update.ver, **не** скачивая сами файлы обновления, а поддерживая лишь актуальные ссылки на оригинальный источник
`pathToSaveBase` | Путь до директории, в которой создавать зеркало обновления (с указанием `/` в конце пути). **ПАРАМЕТР НЕОБХОДИМО ИЗМЕНИТЬ**
`pathToTempDir` | Путь до директории временных файлов
`LOGFILE` | Путь до лог-файла, в который ведется запись основных событий (можно установить значение `""` для отключения данной функции)
`pathToGetFreeKey` | Путь до скрипта получения бесплатного ключа _(необходимо для работы параметра `getFreeKey`)_
`pathToKeysDir` | Путь до директории, где будут храниться ключи
`validKeysFile` | Путь до файла с рабочими ключами
`invalidKeysFile` | Путь до файла с нерабочими ключами (можно установить значение `""` для отключения данной функции)
`wgetDelay` | Максимальный интервал случайной задержки в секундах перед скачиванием каждого файла обновления (используется для уменьшения нагрузки раздающего сервера, можно установить значение `""` для отключения данной функции)
`wgetLimitSpeed` | Лимит на скорость скачивания файлов (в формате `1024k`) обновления (используется для разгрузки канала, можно установить значение `""` для отключения данной функции)
`createRobotsFile` | (`true`\|`false`) Создавать в директории зеркала файл `robots.txt` с запретом на индексирование поисковыми роботами (для сокрытия зеркала от поисковых роботов если его содержимое доступно по протоколу http(s))
`createTimestampFile` | (`true`\|`false`) Создавать в директории зеркала файл `lastevent.txt` с записанной в нем датой крайнего обновления


----------
#### <i class="icon-file"></i>Особенности

 - Если произошла ошибка при обновлении с сервера, который указан, например, в `updServer0` - производится попытка обновиться с сервера, указанного в `updServer1`, `updServer2`..`updServer10`;
 > При необходимости ограничение (до 10 серверов) можно изменить в файле `update.sh`, в цикле `for i in {0..10}; do` в районе ~250 строки
 
 - Скачивает только обновленные файлы обновлений (проверка выполняется с помощью `wget --timestamping`);
 - Умеет поддерживать в актуальном состоянии только лишь файл `update.ver`, не скачивая сами файлы обновлений (при этом зеркало работает, но загрузка происходит не с вашего сервера, а с сервера-источника обновлений);
 - В комплекте идет заготовка для веб-интерфейса зеркала обновления (директория `./webface`).

----------
#### <i class="icon-upload"></i>Прочие ссылки
 - [Пост в блоге](http://tmblr.co/ZYW79o1CrHcIG)
 - [Пост на хабре](http://habrahabr.ru/post/232163/)
 - [Редактор readme.md](https://stackedit.io/)

----------
#### <i class="icon-pencil"></i>История изменений
* **0.4.2** - Исправлена ошибка обновления с официальных зеркал, отказался от использования `grep -P` потому как под freebsd и centos7 - не работет из коробки, добавил возможность исключения некоторых секций из парсинга, `getFreeKey=true;` теперь включен по-умолчанию;
* **0.4.1** - Масштабное обновление, не совместимое с предыдущими версиями. Произведен рефакторинг, изменены имена переменных, переработана логика работы (теперь не просто ищется строка, начинающаяся на 'file=', а разбирается файл по секциям и собивается заново). Доступна возможность выбора необходимых языков и типов обновлений. Версия - не стабильная, при обнаружении ошибок - пожалуйста, создавайте соответствующие тикеты;
* **0.3.11** - Исправление ошибки с определением пути к файлу настроек (`$(pwd)` изменен на `$(dirname $0)`), исправлен путь проверки для валидации ключа (`../mod_000_loader_1080/..` на `../mod_000_loader_1082/..`), вместо умершего `www.nod327.net` используем `tnoduse2.blogspot.ru`, для отключения ограничений wget достаточно установить значения `wget_wait_sec` и `wget_limit_rate` пустыми (или закомментировать их вовсе);
* **0.3.9** - Решение проблемы валидации ключей (скрипт `get-nod32-key.sh`);
* **0.3.8** - Решение проблемы обновления с официальных зеркал для версий v5..v7;
* **0.3.7** - Рефакторинг, вынос настроек в отдельный файл, добавление логирования в скрипт обновления, незначительные доработки и изменения;
* **0.3.6** - Чертовски мощный апдейт. В комплект добавлен скрипт "**get-nod32-key.sh**" который самостоятельно получает валидный ключ к официальным серверам. Более того - исправлена ошибка с отсутствием в пути пары логин:пароль при выполнении двух условий - 'createLinksOnly' включен и обновляемся с официального зеркала;
* **0.3.4** - Поправки в GUI, 'footer.html' и 'header.html' перенесены в '.\.webface\', добавлена поддержка 'mod_geoip' в .htaccess, сам скрипт обновления не изменен;
* **0.3.3** - Незначительные поправки;
* **0.3.2** - Незначительные поправки;
* **0.3.1** - Добавлен "цветной" вывод сообщений, добавлена опция 'createLinksOnly' которая позволяет НЕ скачивать все файлы обновления целиком, а только лишь указывать ссылки на них в 'update.ver'. Улучшено комментирование кода, исправлена пара мелких ошибок, выявленных после теста на боевом сервере. Мелкие исправления;
* **0.3** - Полностью переписан **bash** скрипт, иной алгоритм работы;
* **0.2.5-sh** - **PHP** версия более не поддерживается, скрипт переписан на **bash**;
* **0.2.5** - Масса мелких исправлений в .htaccess и верстке;
* **0.2.4** - Релиз на гитхабе.

----------
#### <i class="icon-refresh"></i>Лицензия MIT

> Copyright (c) 2014 [github.com/tarampampam](http://github.com/tarampampam/)

> Данная лицензия разрешает лицам, получившим копию данного программного обеспечения и сопутствующей документации (в дальнейшем именуемыми «Программное Обеспечение»), безвозмездно использовать Программное Обеспечение без ограничений, включая неограниченное право на использование, копирование, изменение, добавление, публикацию, распространение, сублицензирование и/или продажу копий Программного Обеспечения, также как и лицам, которым предоставляется данное Программное Обеспечение, при соблюдении следующих условий:

> Указанное выше уведомление об авторском праве и данные условия должны быть включены во все копии или значимые части данного Программного Обеспечения.

> ДАННОЕ ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ ПРЕДОСТАВЛЯЕТСЯ «КАК ЕСТЬ», БЕЗ КАКИХ-ЛИБО ГАРАНТИЙ, ЯВНО ВЫРАЖЕННЫХ ИЛИ ПОДРАЗУМЕВАЕМЫХ, ВКЛЮЧАЯ, НО НЕ ОГРАНИЧИВАЯСЬ ГАРАНТИЯМИ ТОВАРНОЙ ПРИГОДНОСТИ, СООТВЕТСТВИЯ ПО ЕГО КОНКРЕТНОМУ НАЗНАЧЕНИЮ И ОТСУТСТВИЯ НАРУШЕНИЙ ПРАВ. НИ В КАКОМ СЛУЧАЕ АВТОРЫ ИЛИ ПРАВООБЛАДАТЕЛИ НЕ НЕСУТ ОТВЕТСТВЕННОСТИ ПО ИСКАМ О ВОЗМЕЩЕНИИ УЩЕРБА, УБЫТКОВ ИЛИ ДРУГИХ ТРЕБОВАНИЙ ПО ДЕЙСТВУЮЩИМ КОНТРАКТАМ, ДЕЛИКТАМ ИЛИ ИНОМУ, ВОЗНИКШИМ ИЗ, ИМЕЮЩИМ ПРИЧИНОЙ ИЛИ СВЯЗАННЫМ С ПРОГРАММНЫМ ОБЕСПЕЧЕНИЕМ ИЛИ ИСПОЛЬЗОВАНИЕМ ПРОГРАММНОГО ОБЕСПЕЧЕНИЯ ИЛИ ИНЫМИ ДЕЙСТВИЯМИ С ПРОГРАММНЫМ ОБЕСПЕЧЕНИЕМ.
