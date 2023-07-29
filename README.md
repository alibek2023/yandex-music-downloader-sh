# yandex-music-downloader-sh

## О программе
Вспомогательный скрипт для проекта https://github.com/llistochek/yandex-music-downloader

## Возможности
- Конфигурационный файл с профилями.
- Автоматический перезапуск при сбоях.
- Пакетный режим работы.

## Требования
- Linux
- Bash

## Использование
Для работы скрипта требуется ini-файл. В качестве шаблона можно использовать файл `yamusic.ini.example`, переименовав его в `yamusic.ini`.
Для работы скрипта в пакетном режиме требуется lst-файл. В качестве шаблона можно использовать файл `yamusic.lst.example`, переименовав его в `yamusic.lst`.

### Формат вызова скрипта:
`yamusic <opts> <profile> <mode> <data>`

- <opts> - дополнительные опции:
  - /loop - работать в режиме автоматического перезапуска при сбоях
  - /once - обычный режим работы, при сбоях скрипт завершает работу
  - /debug - отладочный режим (передает `--log-level DEBUG`)
  - /trace - трассировочный режим (передает `--log-level TRACE`)
- <profile> - имя конфигурационного профиля из файла `yamusic.ini`

### Примеры вызова скрипта:
- `yamusic /loop u1` - загрузить плейлист "Мне нравится"
- `yamusic u1 123` - загрузить плейлист с идентификатором 123
- `yamusic u1 ya.playlist/1250` - загрузить плейлист "Вечные хиты" пользователя "Музыкальные подборки"
- `yamusic u1 artist 792433` - загрузить все альбомы исполнителя "Треки Twenty One Pilots"
- `yamusic u1 album 294912` - загрузить альбом "Nevermind"
- `yamusic u1 https://music.yandex.ru/album/11644078/track/6705392` - загрузить трек "Трек Seven Nation Army"
- `yamusic /loop u1 queue` - загрузить задания из файла `yamusic.lst`

### Информация "из под капота"
- Скрипт всю работу передает загрузчику в данной строчке: `CMD="/bin/python3.9 ./yamusic.py"` (строка 159). Предполагается использования Python версии 3.9 и скрипта-загрузчика из проекта https://github.com/llistochek/yandex-music-downloader, переименованного в yamusic.py.
- Опция `/loop` добавляет внутренний цикл (пока загрузка не будет завершена успешно) и передает в вызываемый скрипт опцию `--skip-existing`.
- Опция `/debug` передает в вызываемый скрипт опцию `--log-level DEBUG`.
- Опция `/trace` передает в вызываемый скрипт опцию `--log-level TRACE`.
- Аргумент `<id>` передается в вызываемый скрипт как `--playlist-id <profile>/<id>`.
- Аргумент `<user>/<id>` передается в вызываемый скрипт как `--playlist-id <user>/<id>`.
- Аргумент `track <id>` передается в вызываемый скрипт как `--track-id <id>`.
- Аргумент `album <id>` передается в вызываемый скрипт как `--album-id <id>`.
- Аргумент `artist <id>` передается в вызываемый скрипт как `--artist-id <id>`.
- Аргумент, начинающийся с `https://` передается в вызываемый скрипт как `--url <arg>`.
- Имя пользователя Яндекс загрузается из конфигурационного профиля. Если оно не задано, то используется имя профиля.
- Прогресс текущего задания выводится в файл `yamusic.log`.
