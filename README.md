# Wyrm

Каталог приложений для [Wyrm](catalog/apps.json) — портативного установщика приложений для Windows.

## Как добавить приложение

Отредактируйте `catalog/apps.json`, добавьте объект в массив `apps`. Нужно только два поля:

```json
{
  "name": "Название приложения",
  "sourceUrl": "https://github.com/owner/repo"
}
```

Всё остальное (описание, издатель, иконка, ссылка на установщик) Wyrm вычисляет сам:

- Если `sourceUrl` — ссылка на репозиторий GitHub (`github.com/owner/repo`), Wyrm берёт установщик из последнего релиза, а описание/издателя/иконку — из GitHub API (аватар владельца репозитория как запасная иконка).
- Если `sourceUrl` — обычная домашняя страница приложения (например, `https://example.com`), Wyrm сканирует её HTML и сам находит на странице ссылку на инсталлятор (`.exe`/`.msi` или что-то с "download"/"setup" в адресе), а также og:title/og:description/og:image как описание и иконку.

Это эвристика, а не гарантия — сайты, которые собирают ссылку на скачивание через JavaScript, автосканированием не берутся. Для таких случаев любое поле можно переопределить вручную:

```json
{
  "name": "NVIDIA App",
  "sourceUrl": "https://www.nvidia.com/en-us/software/nvidia-app/",
  "publisher": "NVIDIA",
  "category": "Utilities",
  "iconUrl": "https://raw.githubusercontent.com/XG-jpg/Wyrm/main/catalog/icons/nvidia.svg",
  "accentBrush": "#76B900",
  "downloadUrl": "https://prямая-ссылка-на-инсталлятор"
}
```

Поля:

- `sourceUrl` (обязательно) — ссылка на GitHub-репозиторий или домашнюю страницу приложения.
- `downloadUrl` — прямая ссылка на файл, если автосканирование страницы не находит нужную ссылку.
- `category` — одно из: `Games`, `Media`, `Communication`, `Utilities` (по умолчанию `Utilities`).
- `publisher`, `description`, `iconUrl`, `accentBrush` — переопределяют автоматически найденные значения.
- `flathubId` — если указан, скриншоты для деталь-панели подтягиваются с Flathub.
- `assetPattern` — регулярка для выбора нужного файла среди ассетов GitHub-релиза, если угадывается неверно (по умолчанию Wyrm сам ищёт `.exe`/`.msi` с "win" в имени).

Иконки для оверрайдов кладите в `catalog/icons/` и ссылайтесь на них через `raw.githubusercontent.com`.

Присылайте PR с изменением `catalog/apps.json` — Wyrm подхватывает изменения из `main` в течение нескольких часов (кэш каталога на стороне приложения).
