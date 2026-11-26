# Сайт обновлений Muznik

Это обычная статическая папка. Она может лежать на любом хостинге: своём сервере, GitHub Pages,
Netlify, Vercel — лишь бы файлы отдавались по HTTPS.

## Файлы

- `index.html` — страница со ссылкой на APK (можно не публиковать, но удобно).
- `update.json` — описание последней версии. Именно его адрес вводится в приложении.
- `Muznik.apk` — сама сборка.
- `.nojekyll` — чтобы GitHub Pages не трогал файлы (безвредно и на обычном сервере).

## Как обновить приложение

1. В `app/build.gradle.kts` увеличить `versionCode` (обязательно) и `versionName`.
2. Собрать APK: `JAVA_HOME=~/Library/Java/JavaVirtualMachines/zulu-17.jdk/Contents/Home ./gradlew assembleDebug`.
3. Скопировать `app/build/outputs/apk/debug/app-debug.apk` в `site/Muznik.apk`.
4. Пересчитать `size` и `sha256`:
   ```bash
   stat -f%z site/Muznik.apk
   shasum -a 256 site/Muznik.apk
   ```
5. Обновить `site/update.json`.
6. Залить папку `site/` на сервер, не меняя адрес `update.json`.

## Что вводить в приложении

Полный адрес файла манифеста, например:

```text
https://example.com/muznik/update.json
```

Проверка раз в день включена по умолчанию. Приложение скачивает только `update.json`; APK — только
если `versionCode` в нём больше установленного. Установку подтверждает системный диалог Android:
без этого sideload-приложение не может обновить само себя.
