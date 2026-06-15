# Stash VPN Configs

Публичные example-конфиги для Stash на iOS/macOS, синхронизированные по routing-логике с Clash-профилями там, где это безопасно и переносимо.

## Что внутри

- `Abroad.stash.example.yaml` - переносимый шаблон Abroad-профиля.
- `Russia.stash.example.yaml` - переносимый шаблон Russia-профиля.
- `Default.example.yaml` - базовый пример Stash-профиля.
- `override/collapsed-tiles-example.stoverride` - Stash override с диагностическими tiles для Ozon, Bybit, Yandex, Google, GitHub, YouTube, Rutube и Instagram.
- `resources/rules/` - переносимые rule lists без приватных provider-данных.

## Как использовать

1. Скопируйте нужный `*.example.yaml` в рабочий Stash-профиль.
2. Замените `https://YOUR_STASH_PROVIDER_URL/example.yaml` на URL своего Stash-compatible provider.
3. Замените правило `DOMAIN,your-provider.example,DIRECT` на домен своего provider.
4. Импортируйте профиль в Stash и проверьте правила через Stash Dashboard / Records.

## Важная логика

Domain rules остаются основной переносимой логикой, потому что на iOS process rules ограничены Network Extension-моделью. Process rules можно использовать как macOS-оптимизацию, но не как единственную основу маршрутизации.

Abroad-профиль не должен вести широкий `legiz-ru` `ru-bundle` через российский
VPN: этот bundle содержит много иностранных anti-block сервисов. Russia-профиль
может держать `ru-bundle` только как поздний foreign `PROXY` fallback после
явных `DIRECT` правил для российских сервисов и TLD.

## Cisco / Enterprise DNS

Examples не содержат конкретные корпоративные Cisco AnyConnect DNS suffixes. Если вам нужно автоматизировать такую логику, updater может добавлять managed blocks в:

- `dns.fake-ip-filter` - чтобы внутренние корпоративные домены не попадали в fake-ip.
- `dns.nameserver-policy` - чтобы эти домены резолвились через `system`, то есть через DNS, который Cisco AnyConnect выдал системе.

Это опционально: во многих сценариях Cisco AnyConnect сам настраивает локальный DNS, и отдельная автоматизация нужна только если Stash перехватывает DNS раньше системы.

## Безопасность

Реальные профили, provider-cache и proxy-provider файлы исключены из Git, потому что могут содержать приватные URL, UUID и реальные ноды.
