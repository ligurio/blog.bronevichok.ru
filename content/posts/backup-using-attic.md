---
date: 2016-06-15T00:00:00Z
title: Как не поседеть при потере важных данных
---

Не хочу пересказывать то, что уже написано в статьях про бэкапы, а вместо
этого поделюсь тем способом сохранения резервных копий файлов, который прижился
у меня и который я использую последний год.

По мере того, как на компьютере стали скапливаться данные, которые представляли
для меня ценность, я стал задумываться о том, как бы их не потерять.
Я перепробовал множество программ: использовал
[backupfs](http://sourceforge.net/projects/backupfs), про которую я прочитал в
[интервью с Дональдом
Кнутом](http://www.informit.com/articles/article.aspx?p=1193856), потом была
утилита [dump](http://man.openbsd.org/OpenBSD-current/man8/dump.8), настраивал на
домашнем сервере монстра Bacula, потом был rsnapshot. Ещё были tarsnap и
s3cmd. У всех этих инструментов были те или иные недостатки: отсутствие
шифрования, сложный для домашнего применения, дороговизна и т.д.

Случайно я узнал про [Attic](https://attic-backup.org/).  Он умеет:
инкрементальные бэкапы, шифрование, дедупликацию, автоматическое удаление старых
архивов, [прост в использовании](https://attic-backup.org/quickstart.html).
Плюс есть возможность продолжить незавершённый бэкап и проверка целостности
резервных копий.

Дедупликация и сжатие данных сильно экономит место. Вот данные о моих бэкапах:

|                     | Original size    |  Compressed size |   Deduplicated size |
|:-------------------:|:----------------:|:----------------:|:------------------:|
|This archive:        |       48.28 GB   |          39.92 GB|            968.25 MB
|All archives:        |      779.93 GB   |         636.52 GB|             50.63 GB

Благодаря поддержке шифрования можно немного сэкономить на хранилище для
бэкапов. Многие хостеры предлагают дешёвые VPS с большим диском, их предложения
можно поискать на [serverbear.com](http://serverbear.com/compare/vps/storage)
или lowendstock.com. Я выбрал для себя хостинг
[BuyVM](http://buyvm.net/storage-vps/), который предлагает виртуальную машину с
диском в 250 Gb и возможностью установить любую ОС. Стоит всего лишь 7$ в месяц.
Установил туда OpenBSD c Attic и настроил регулярный запуск Attic на ноутбуке.
Ключи для шифрования имеет смысл распечатать и хранить в сухом, тёмном и
недоступном для детей месте.

## Ссылки

* [Правило резервного копирования "3-2-1"](https://habrahabr.ru/company/veeam/blog/188544/)
* [Резервное копирование](http://wiki.opennet.ru/Backup)
* [Схема создания резервных копий одного из разработчиков DragonFly BSD](https://www.dragonflybsd.org/~beket/backup.svg)
* http://www.usrsb.in/secret-sharing-backup.html
* [Аналог Attic, написанный на Go](https://restic.github.io/)

Fin