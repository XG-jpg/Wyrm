# Wyrm

Каталог приложений для [Wyrm](catalog/apps.json) — портативного установщика приложений для Windows.

## Как добавить приложение

Отредактируйте `catalog/apps.json`, добавьте объект в массив `apps`. Проще всего — через winget:

```json
{
  "name": "Название приложения",
  "wingetId": "Publisher.AppName"
}
```

`wingetId` — это id пакета из [Windows Package Manager](https://github.com/microsoft/winget-pkgs), тот же, что видно в `winget search <название>` или на [winget.run](https://winget.run). Wyrm сам подтягивает оттуда версию, описание, издателя и прямую ссылку на установщик — искать и вставлять ссылку самому не нужно.

Если приложения нет в winget, можно дать ссылку вместо `wingetId`:

```json
{
  "name": "Название приложения",
  "sourceUrl": "https://github.com/owner/repo"
}
```

- Если `sourceUrl` — ссылка на репозиторий GitHub, установщик берётся из последнего релиза, а описание/издателя/иконку Wyrm берёт из GitHub API.
- Если `sourceUrl` — обычная домашняя страница приложения, Wyrm сканирует её HTML и сам находит ссылку на инсталлятор, а также og:title/og:description/og:image как описание и иконку.

Это эвристика, а не гарантия — сайты, которые собирают ссылку на скачивание через JavaScript, автосканированием не берутся. Для таких случаев (и вообще для чего угодно) любое поле можно переопределить вручную:

```json
{
  "name": "NVIDIA App",
  "wingetId": "XP8CLZL93F5Z4P",
  "publisher": "NVIDIA",
  "category": "Utilities",
  "iconUrl": "https://raw.githubusercontent.com/XG-jpg/Wyrm/main/catalog/icons/nvidia.svg",
  "accentBrush": "#76B900"
}
```

Поля:

- `wingetId` или `sourceUrl` — нужен хотя бы один.
- `downloadUrl` — прямая ссылка на файл, если у `sourceUrl`-домашней страницы автосканирование не находит нужную ссылку.
- `category` — одно из: `Games`, `Media`, `Communication`, `Utilities` (по умолчанию `Utilities`).
- `publisher`, `description`, `iconUrl`, `accentBrush` — переопределяют автоматически найденные значения.
- `flathubId` — если указан, скриншоты для деталь-панели подтягиваются с Flathub.
- `assetPattern` — регулярка для выбора нужного файла среди ассетов GitHub-релиза (только для `sourceUrl`-GitHub пути), если угадывается неверно.

Иконки для оверрайдов кладите в `catalog/icons/` и ссылайтесь на них через `raw.githubusercontent.com`.

Присылайте PR с изменением `catalog/apps.json` — Wyrm подхватывает изменения из `main` в течение нескольких часов (кэш каталога на стороне приложения). Для проверки версии/установщика через winget у пользователя должен быть установлен App Installer (winget) — на актуальных Windows 10/11 он идёт из коробки.
