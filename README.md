# P15 Printer

Клиент для карманного термопринтера **P15** (и совместимых по BLE / протоколу Marklife L11): печать текста и растровых этикеток по Bluetooth.

В репозитории два приложения:

| Проект | Платформа | Описание |
|--------|-----------|----------|
| `P15Printer.xcodeproj` | **iOS** (iPhone / iPad) | Основной мобильный клиент |
| `P15PrinterMac/P15PrinterMac.xcodeproj` | **macOS** 13+ | Тот же функционал на Mac (универсальный бинарь arm64 + x86_64 в Release) |

## Возможности

- Сканирование и подключение к устройствам с именем, содержащим `P15`
- Печать текста (ESC/POS-обёртка L11 при поддержке прошивки)
- Этикетка на ленте: размеры в мм, зазор, поиск метки (1D 0C), предпросмотр растра, сохранение настроек
- Журнал BLE для отладки

## Требования

- **Xcode** 15+ (рекомендуется актуальная стабильная версия)
- **Apple Developer** — для установки iOS-приложения на физическое устройство и для подписи `.ipa`
- iOS **16.0+**, macOS **13.0+**

## Сборка

Схемы **P15Printer** и **P15PrinterMac** лежат в `xcshareddata/xcschemes/` — после клонирования их сразу видно в Xcode и в `xcodebuild -scheme …`.

1. Откройте нужный `.xcodeproj` в Xcode.
2. В **Signing & Capabilities** выберите свою **Team** и при необходимости измените **Bundle Identifier** (сейчас шаблон `com.example.P15Printer` / `com.example.P15PrinterMac`).
3. Соберите (**⌘B**) или архивируйте для распространения.

### Скрипт `scripts/build_distribution.sh`

```bash
# Только macOS → dist/macos/P15PrinterMac 2.0.app (универсальный бинарь)
./scripts/build_distribution.sh --mac-only

# .ipa для iPhone (нужна подпись в Xcode или переменная окружения)
DEVELOPMENT_TEAM=XXXXXXXXXX ./scripts/build_distribution.sh --ios-only

# Всё подряд (ipa — если BUILD_IOS_IPA не равен 0)
./scripts/build_distribution.sh
```

Экспорт iOS использует `scripts/ExportOptions-Development.plist` (метод **debugging** для установки на зарегистрированные устройства).

## Структура репозитория

```
P15Printer/              # Исходники iOS
P15PrinterMac/           # Исходники macOS (в т.ч. своя копия BLE/L11)
scripts/                 # Сборка и ExportOptions
```

Исходники **не полностью общие** между таргетами: в каждом таргете свои файлы `BLEManager.swift`, `L11BitmapPrint.swift`, `ContentView.swift`. При доработках имеет смысл синхронизировать изменения в обеих копиях или вынести общий Swift Package (задача для контрибьюторов).

## Лицензия

[MIT](LICENSE)

## Участие

См. [CONTRIBUTING.md](CONTRIBUTING.md). Issues и pull requests приветствуются.

## Выложить на GitHub

В каталоге проекта (после `git config user.name` / `user.email` при необходимости):

```bash
gh repo create P15-printer --public --source=. --remote=origin --push
# или без GitHub CLI: создайте пустой репозиторий на github.com, затем:
# git remote add origin https://github.com/ВАШ_ЛОГИН/P15-printer.git
# git push -u origin main
```

Локальные папки `build/`, `dist/`, `xcuserdata/` в репозиторий не попадают (`.gitignore`).
