# nsfw-ext

Мини-репозиторий расширений в формате, совместимом с `index.min.json`/`index.json`, как у Keiyoushi.

Сейчас в проект перенесены:

- `E-Hentai`
- `Hitomi`

## Что это дает

После сборки появится каталог `repo/` со следующими файлами:

- `repo/index.json`
- `repo/index.min.json`
- `repo/index.html`
- `repo/repo.json`
- `repo/apk/*.apk`
- `repo/icon/*.png`

Именно `index.min.json` можно использовать как URL репозитория в Mihon-подобных клиентах.

## Что нужно для сборки

- JDK 17
- Android SDK c `build-tools`
- переменная окружения `ANDROID_HOME`
- файл `signingkey.jks` в корне проекта
- переменные окружения `ALIAS`, `KEY_STORE_PASSWORD`, `KEY_PASSWORD`

## Локальная сборка репозитория

```bash
./scripts/build_repo.sh
```

Скрипт:

1. собирает все текущие расширения из `src/*/*`
2. складывает APK в `repo/apk`
3. скачивает `extensions-inspector`
4. генерирует `index.json`, `index.min.json`, `index.html`, `repo.json`

## Локальная раздача

```bash
python3 -m http.server 8000 --directory repo
```

После этого URL репозитория будет:

```text
http://127.0.0.1:8000/index.min.json
```

## Публикация на GitHub

Для публикации в `gh-pages` после локальной сборки:

```bash
./scripts/build_repo.sh
./scripts/publish_pages.sh
```

Скрипт `publish_pages.sh` публикует содержимое `repo/` в отдельный worktree на ветку `gh-pages`.

Самый простой вариант без GitHub Pages:

1. закоммитить содержимое `repo/`
2. использовать raw-ссылку вида

```text
https://raw.githubusercontent.com/<user>/<repo>/<branch>/repo/index.min.json
```

Если захотите именно публикацию через GitHub Pages, можно следующим шагом добавить workflow, который будет выкладывать содержимое `repo/` как статический сайт.

## Важно

- не коммитьте `signingkey.jks`
- храните один и тот же ключ подписи, иначе обновления перестанут распознаваться как обновления
