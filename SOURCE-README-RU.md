# Соответствующие исходники fheroes2 1.1.18 для Miyoo Mini Plus

Ревизия порта: **miyoo-2026-11-24**. Этот архив сопровождает `fheroes2-miyoo-mini-plus-onion-1.1.18.zip`.

Движок: https://github.com/ihhub/fheroes2, тег `1.1.18`, commit `9280fe5ee3af2cfc1036a0fa1c1e69eeb1b11f59`. SDL: https://github.com/steward-fu/sdl2, commit `0631abc8e8916db6f9bc7e2afd0c22913d092a29` с нашими изменениями.

## Состав

- `fheroes2/`: исходники движка, встроенных сторонних компонентов, ресурсов, переводов и сборочных файлов именно этой версии; изменения Miyoo уже применены.
- `sdl/`: фактически использованные изменённые исходники SDL2 и интерфейсные заголовки Mini. Неиспользуемый SwiftShader исключён.
- `port/`, `scripts/`, `tests/`: управление, launcher, конфигурация, сборка, упаковка и проверки.
- `patches/`, `MODIFIED-FILES.txt`: отличия от зафиксированных исходных ревизий и перечень изменённых файлов с датой.
- `release/`: сведения о бинарном выпуске и SHA256 исполняемого файла и библиотек.
- `SOURCE-SHA256.json`: контрольные суммы файлов архива.
- `release-support/licenses/`: лицензии и уведомления библиотек установочного комплекта.
- `release-support/dependency-sources/`: дополнительные upstream-архивы SDL2_mixer 2.0.4, zlib 1.2.11, json-c 0.15, libogg 1.3.5 и Tremor. Эти библиотеки в бинарном выпуске взяты из SDK; приложенные upstream-архивы не выдаются за подтверждённую точную копию внутренних исходников SDK. Для Tremor точная ревизия SDK не установлена, приложен явно обозначенный справочный snapshot.

## Лицензии и распространение

Лицензия fheroes2 сохраняется **GPL-2.0-or-later**, текст в `fheroes2/LICENSE`; это не изменение на GPL-2.0-only. Собственные файлы управления и сборочные скрипты этого порта также предоставляются под GPL-2.0-or-later. Уведомления авторов upstream сохранены. SDL имеет собственную лицензию zlib, изменения fork распространяются с LGPL-2.1 из `sdl/LICENSE`; исходники модифицированного SDL включены целиком для используемого варианта без GLES.

SDL_mixer/zlib/json-c/Ogg/Tremor имеют отдельные разрешительные лицензии; см. приложенные тексты. GCC runtime (`libstdc++`, `libgcc`) поставляется по GPLv3 с GCC Runtime Library Exception; тексты исключения и лицензий приложены. Оригинальные данные Heroes II, glibc и библиотеки прошивки SigmaStar в бинарный и исходный архивы не включены.

При размещении бинарного ZIP публикуйте рядом именно этот исходный ZIP и оба SHA256-файла. Не заменяйте исходники одной ссылкой на основной репозиторий: там нет изменений этого порта. Публикацию от имени пользователя эта задача не выполняет.

## Повторная сборка

Рабочая среда выпуска: Ubuntu 22.04 / WSL `Heroes2Build`, GNU ARM 8.2.1, host gettext 0.21. Рекомендуется распаковать этот ZIP в Linux-файловую систему. Все команды далее выполняются из корня распакованного архива.

1. Установить host-инструменты: `sudo apt-get install build-essential autoconf automake libtool gettext python3 pkg-config git ca-certificates unzip qemu-user`.
2. Скачать SDK https://github.com/steward-fu/website/releases/download/miyoo-mini/mini_toolchain-v1.0.tar.gz. SHA256 записан в `release-support/sdk.json`. Распаковать и обеспечить пути `/opt/mini` и `/opt/prebuilt` к соответствующим каталогам SDK. Архив SDK не включён в исходный ZIP.
3. Для этого SDK выбрать рабочий линкер:

```sh
ln -sf /opt/prebuilt/arm-linux-gnueabihf/bin/ld.bfd /opt/prebuilt/arm-linux-gnueabihf/bin/ld
ln -sf /opt/prebuilt/arm-linux-gnueabihf/bin/ld.bfd /opt/prebuilt/bin/arm-linux-gnueabihf-ld
```

4. Загрузить отдельно библиотеки прошивки, нужные только для линковки, из зафиксированной ревизии SDL fork. Скрипт проверяет SHA256; они не будут включены в установочный ZIP:

```sh
python3 scripts/fetch-link-libs.py "$PWD/sdl"
```

5. Собрать из уже изменённых исходников:

```sh
chmod +x sdl/sdl2/autogen.sh sdl/sdl2/build-scripts/*.sh
bash scripts/build-sdl.sh "$PWD/sdl"
bash scripts/build-engine.sh "$PWD/fheroes2" "$PWD/sdl"
python3 scripts/package.py "$PWD/fheroes2" "$PWD/sdl"
```

Патчи повторно применять не нужно. Скрипты patch-source.py и patch-sdl.py идемпотентны. Для переводов используется host GNU gettext/iconv: SDK-замена msgfmt ранее портила русскую кодировку. Пакетирование проверяет соответствие кодировок и известную русскую строку.

Результат появится в `dist/`. Скрипт `archive-release.py` служит для выпуска из рабочего проекта и требует ещё pristine Git-каталоги `upstream/` и `miyoo-sdl2/`; они не нужны для повторной сборки бинарников из этого ZIP. Пути сборки, timestamps и версии host-инструментов могут влиять на побайтовое совпадение; идентичность хеша при произвольном окружении не заявляется.

## Исправления Miyoo и границы проверки

Сохранены ARGB8888 и полная конверсия/загрузка кадра, обход нулевой области MI_GFX, корректная обработка EV_KEY repeat, движение мыши по времени, правильное имя аудиодрайвера и host-переводы. Новый выпуск проверяется ARM-компиляцией, загрузчиком, тестами курсора/палитры/масштабирования и целостностью архивов. Полный тест fheroes2 1.1.18 на физической Miyoo пока не выполнен.
