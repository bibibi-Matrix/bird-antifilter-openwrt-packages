# BIRD Antifilter — OpenWrt (apk) package feed

Репозиторий-фид готовых пакетов **apk** для OpenWrt 25.12 / Snapshot (легаси-система
opkg.ipkg также поддерживается имеющимися `index.json`/`packages.adb`).

Исходники пакетов и LuCI: [bibibi-Matrix/bird-antifilter-openwrt](https://github.com/bibibi-Matrix/bird-antifilter-openwrt).

## Пакеты

| Пакет | Описание |
|---|---|
| `bird` (v1.2) | BIRD2 antifilter — демон, конвейер синхронизации `bird-sync.sh`, UCI-схема, cron-расписание |
| `luci-app-bird` (v1.0.1) | LuCI-интерфейс: статус, настройки, BGP-пиры, источники, свои/черные списки, синхронизация, журнал |

## Доступные архитектуры

- `x86_64` — x86_64-openwrt-25.12
- `aarch64_cortex-a53` — mediatek-filogic-openwrt-25.12 (Cudy TR3000 и совместимые)

Каждая папка пакетов содержит `*.apk`, `index.json` и `packages.adb`, сгенерированные
`make package/index` в официальном SDK OpenWrt 25.12.

## Установка

### 1. Вручную (скачивание `.apk`)

В одном из вариантов:

```sh
wget -O /tmp/feed.apk https://raw.githubusercontent.com/bibibi-Matrix/bird-antifilter-openwrt-packages/main/packages/<ARCH>/bird/bird-1.2-r1.apk
apk add --allow-untrusted /tmp/feed.apk
```

### 2. Как источник (репозиторий) — рекомендовано, для обновлений

Добавьте фид в `/etc/apk/repositories.d/70-bird.list`:

```
https://raw.githubusercontent.com/bibibi-Matrix/bird-antifilter-openwrt-packages/main/packages/<ARCH>/bird
```

где `<ARCH>` — `x86_64` или `aarch64_cortex-a53`. Затем:

```sh
apk update
apk add bird luci-app-bird
```

После обновления пакетов из этого фида обновление до новой версии:

```sh
apk update && apk upgrade
```

> Пакеты не подписаны ключом фида — при `apk add` может потребоваться
> `--allow-untrusted` либо добавление `--force-broken-world`, если используется
> режим строгой проверки подписей.