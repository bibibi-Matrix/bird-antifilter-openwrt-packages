# BIRD Antifilter — OpenWrt (apk) package feed

Репозиторий-фид готовых пакетов **apk** для OpenWrt 25.12 / Snapshot.
Индексы `packages.adb` + `index.json` (формат v2) — стандартные для apk-репозиториев OpenWrt.

Исходники пакетов и LuCI: [bibibi-Matrix/bird-antifilter-openwrt](https://github.com/bibibi-Matrix/bird-antifilter-openwrt).

## Пакеты

| Пакет | Описание |
|---|---|
| `bird` (v1.2) | BIRD2 antifilter — демон, конвейер синхронизации `bird-sync.sh`, UCI-схема, cron-расписание |
| `luci-app-bird` (v1.0.1) | LuCI-интерфейс: статус, настройки, BGP-пиры, источники, свои/черные списки, синхронизация, журнал |
| `kmod-amneziawg` | Ядро AmneziaWG (AWG) для OpenWrt 25.12.5 |
| `amneziawg-tools` | Утилиты `awg` / `awg-quick` |
| `luci-proto-amneziawg` | LuCI: протокол интерфейса AmneziaWG (AWG 2.0 для OpenWrt ≥24.10.3) |
| `luci-i18n-amneziawg-ru` | Русская локализация LuCI-proto-amneziawg |

Пакеты amneziawg собираются из релиза
[Slava-Shchipunov/awg-openwrt](https://github.com/Slava-Shchipunov/awg-openwrt)
`v25.12.5` (для `aarch64_cortex-a53_mediatek_filogic` и `x86_64_x86_64`).

## Доступные архитектуры

- `x86_64` — x86_64-openwrt-25.12
- `aarch64_cortex-a53` — mediatek-filogic-openwrt-25.12 (Cudy TR3000 и совместимые)

Каждая папка пакетов содержит `*.apk`, `index.json` и `packages.adb`. Индексы
пересоздаются автоматически GitHub Actions (workflow `update-index.yml`) при
изменении `.apk`: `apk mkndx` → `packages.adb` и
`apk adbdump --format json | tools/make-index-json.py` → `index.json`
(та же логика, что и `make package/index` в SDK OpenWrt).

## Установка

### 1. Вручную (скачивание `.apk`)

В одном из вариантов:

```sh
wget -O /tmp/feed.apk https://raw.githubusercontent.com/bibibi-Matrix/bird-antifilter-openwrt-packages/main/packages/<ARCH>/bird/bird-1.2-r1.apk
apk add --allow-untrusted /tmp/feed.apk
```

### 2. Как источник (репозиторий) — рекомендовано, для обновлений

Добавьте фид в `/etc/apk/repositories.d/70-bird.list` (URL должен
заканчиваться на `packages.adb`):

```
https://raw.githubusercontent.com/bibibi-Matrix/bird-antifilter-openwrt-packages/main/packages/<ARCH>/bird/packages.adb
```

где `<ARCH>` — `x86_64` или `aarch64_cortex-a53`. Затем:

```sh
apk update --allow-untrusted
apk add --allow-untrusted bird luci-app-bird kmod-amneziawg amneziawg-tools luci-proto-amneziawg
```

Русская локализация (опционально):

```sh
apk add --allow-untrusted luci-i18n-amneziawg-ru
```

После обновления пакетов из этого фида обновление до новой версии:

```sh
apk update --allow-untrusted && apk upgrade --allow-untrusted
```

> Пакеты и индекс не подписаны ключом фида, поэтому требуется `--allow-untrusted`
> (для постоянной настройки замените `--allow-untrusted` на установку публичного
> ключа фида в `/etc/apk/keys/`).