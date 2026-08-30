# Wyrm

Каталог приложений и релизы для [Wyrm](catalog/apps.json) — портативного установщика приложений для Windows.

## Как добавить приложение в каталог

Отредактируйте `catalog/apps.json`, добавьте объект в массив `apps`:

```json
{
  "name": "Название приложения",
  "publisher": "Издатель",
  "description": "Короткое описание.",
  "homepageUrl": "https://example.com",
  "category": "Utilities",
  "flathubId": "org.example.App",
  "iconUrl": "https://raw.githubusercontent.com/XG-jpg/Wyrm/main/catalog/icons/example.svg",
  "accentBrush": "#2F6FED",
  "resolver": "GitHubLatest",
  "gitHubOwner": "owner",
  "gitHubRepo": "repo",
  "assetPattern": "windows.*setup.*\\.exe$"
}
```

Поля:

- `category` — одно из: `Games`, `Media`, `Communication`, `Utilities`.
- `resolver` — `GitHubLatest` (ссылка на установщик берётся из последнего релиза GitHub-репозитория по `assetPattern`) или `Static` (прямая ссылка на файл через `staticUrl`/`staticFileName`).
- `iconUrl` — прямая ссылка на иконку (svg/png/ico). Положите файл в `catalog/icons/` и сошлитесь на него через `raw.githubusercontent.com`.
- `flathubId` необязателен — если указан, скриншоты для деталь-панели подтягиваются с Flathub.

Присылайте PR с изменением `catalog/apps.json` (и, если нужно, новым файлом иконки) — Wyrm подхватывает изменения из `main` в течение нескольких часов (кэш каталога на стороне приложения).
