# CS2_Admin з підтримкою T3Menu

Модифікована версія плагіна **CS2_Admin** для SwiftlyS2, у якій стандартне меню замінено на Panorama-меню **T3Menu**.

## Можливості

- Усі розділи адмін-меню працюють через T3Menu.
- Підтримуються три візуальні стилі:
  - `SourceMod`
  - `Stylish`
  - `Source2`
- Підтримуються три режими керування:
  - `KeyPress`
  - `Scrollable`
  - `Clickable`
- Стандартне меню SwiftlyS2 автоматично закривається.
- Офіційне автооновлення CS2_Admin вимкнено, щоб воно не замінило модифіковану збірку.
- Додано ранню реєстрацію T3Menu API для сумісності із серверами, де інші плагіни заважають стандартному підключенню shared-інтерфейсів.

## Вимоги

- Counter-Strike 2 Dedicated Server
- SwiftlyS2
- .NET 10
- T3Menu 1.3.0
- Workshop-додаток T3Menu

Workshop:

https://steamcommunity.com/sharedfiles/filedetails/?id=3790988631

Вихідний проєкт T3Menu:

https://github.com/T3Marius/T3Menu

> Гравцям необхідно завантажити Workshop-додаток, інакше Panorama-інтерфейс, стилі та звуки T3Menu можуть не працювати.

## Встановлення

### 1. Зупиніть сервер

Перед заміною файлів повністю зупиніть сервер.

Не рекомендується встановлювати цю збірку через hot reload або команду `sw plugins load`.

### 2. Видаліть старі версії

Видаліть такі папки, якщо вони існують:

```text
addons/swiftlys2/plugins/CS2_Admin
addons/swiftlys2/plugins/T3Menu
```

Також перевірте, щоб на сервері не залишилося інших копій `CS2_Admin.dll`:

```bash
find addons/swiftlys2/plugins -type f -name "CS2_Admin.dll"
```

Має залишитися лише одна DLL.

### 3. Розпакуйте архів

Розпакуйте вміст release-архіву в:

```text
game/csgo/addons/swiftlys2/plugins/
```

Після встановлення структура має виглядати так:

```text
addons/swiftlys2/plugins/
├── CS2_Admin/
│   ├── CS2_Admin.dll
│   └── resources/
└── T3Menu/
    ├── T3Menu.dll
    └── resources/
        └── exports/
            └── T3Menu.Contract.dll
```

> Назва папки `T3Menu` має збігатися з назвою `T3Menu.dll`. Не перейменовуйте її на `T3MenuV1.3.0`.

### 4. Налаштуйте CS2_Admin

Відкрийте основний файл конфігурації CS2_Admin:

```text
addons/swiftlys2/configs/plugins/CS2_Admin/config.json
```

Перевірте такі параметри:

```json
{
  "AutoUpdate": false,
  "DisableBuiltInMenuWhenUsingT3Menu": true
}
```

Опис:

| Параметр | Значення | Призначення |
|---|---:|---|
| `AutoUpdate` | `false` | Забороняє офіційному автооновленню замінити модифіковану DLL |
| `DisableBuiltInMenuWhenUsingT3Menu` | `true` | Закриває стандартне меню SwiftlyS2 під час відкриття T3Menu |

### 5. Запустіть сервер

Після встановлення повністю запустіть сервер.

Спочатку перевірте T3Menu:

```text
!t3menu_test
```

Потім відкрийте адмін-меню:

```text
!admin
```

## Налаштування T3Menu

Налаштування зовнішнього вигляду та керування знаходяться в конфігурації T3Menu:

```text
addons/swiftlys2/configs/plugins/T3Menu/t3menu.jsonc
```

Основні параметри:

```jsonc
{
  "Navigation": "Clickable",
  "Style": "Stylish",
  "ItemsPerPage": 6
}
```

Не видаляйте інші параметри файлу. Змінюйте лише необхідні значення.

## Візуальні стилі

### SourceMod

```json
"Style": "SourceMod"
```

Компактний класичний стиль.

### Stylish

```json
"Style": "Stylish"
```

Сучасне меню з картками та анімаціями.

### Source2

```json
"Style": "Source2"
```

Стиль, наближений до інтерфейсу Counter-Strike 2.

## Режими керування

### Clickable

```json
"Navigation": "Clickable"
```

Керування мишкою безпосередньо в меню.

- Tab утримувати не потрібно.
- T3Menu самостійно показує курсор.
- Після закриття меню курсор вимикається.

Рекомендоване налаштування:

```jsonc
{
  "Navigation": "Clickable",
  "Style": "Stylish",
  "ItemsPerPage": 6
}
```

### Scrollable

```json
"Navigation": "Scrollable"
```

Керування клавішами:

| Клавіша | Дія |
|---|---|
| `W` | Вгору |
| `S` | Вниз |
| `E` | Вибрати |
| `R` | Закрити |

Приклад:

```jsonc
{
  "Navigation": "Scrollable",
  "Style": "Source2",
  "ItemsPerPage": 6
}
```

### KeyPress

```json
"Navigation": "KeyPress"
```

Керування командами в чаті:

| Команда | Дія |
|---|---|
| `!1`–`!6` | Вибрати пункт |
| `!7` | Назад |
| `!8` | Наступна сторінка |
| `!9` | Закрити меню |

Звичайне натискання клавіш `1–9` може перемикати зброю, тому в цьому режимі використовуються команди `!1`, `!2` тощо.

## Перевірка встановлення

### T3Menu недоступний

Помилка:

```text
T3Menu is not available. Install and load the T3Menu plugin before CS2_Admin.
```

Перевірте наявність файлів:

```text
addons/swiftlys2/plugins/T3Menu/T3Menu.dll
addons/swiftlys2/plugins/T3Menu/resources/exports/T3Menu.Contract.dll
```

Також виконайте:

```text
!t3menu_test
```

Якщо команда невідома, T3Menu не завантажився.

### SwiftlyS2 шукає неправильну DLL

Помилка:

```text
Plugin entrypoint DLL not found:
T3MenuV1.3.0/T3MenuV1.3.0.dll
```

Папку названо неправильно. Вона має називатися:

```text
T3Menu
```

І містити:

```text
T3Menu/T3Menu.dll
```

### Відкривається стандартне меню

Перевірте налаштування:

```json
"AutoUpdate": false,
"DisableBuiltInMenuWhenUsingT3Menu": true
```

Також переконайтеся, що на сервері немає другої копії CS2_Admin:

```bash
find addons/swiftlys2/plugins -type f -name "CS2_Admin.dll"
```

Після заміни файлів виконайте повний перезапуск сервера.

### Меню видно, але воно не натискається

Перевірте параметр `Navigation`.

Для керування мишкою:

```json
"Navigation": "Clickable"
```

Для керування клавішами `W/S/E/R`:

```json
"Navigation": "Scrollable"
```

Для команд `!1`–`!9`:

```json
"Navigation": "KeyPress"
```

Також переконайтеся, що Workshop-додаток T3Menu встановлено та завантажено клієнтом.

## Збірка з вихідного коду

Потрібен .NET SDK 10.

```bash
dotnet restore CS2_Admin.csproj
dotnet publish CS2_Admin.csproj -c Release
```

Готову збірку буде створено в каталозі:

```text
build/publish/CS2_Admin/
```

T3Menu необхідно збирати окремо:

```bash
dotnet restore T3Menu/T3Menu/T3Menu.csproj
dotnet build T3Menu/T3Menu/T3Menu.csproj -c Release
```

Після збірки розмістіть файли таким чином:

```text
T3Menu/T3Menu.dll
T3Menu/resources/exports/T3Menu.Contract.dll
```

## Оновлення

Не встановлюйте офіційний архів CS2_Admin поверх цієї версії — він поверне стандартне меню SwiftlyS2.

Перед оновленням:

1. Зробіть резервну копію конфігурації.
2. Повністю зупиніть сервер.
3. Замініть папки `CS2_Admin` і `T3Menu`.
4. Не замінюйте користувацькі файли конфігурації.
5. Запустіть сервер знову.

## Подяки

- Авторам оригінального CS2_Admin
- [T3Marius/T3Menu](https://github.com/T3Marius/T3Menu)
- Команді [SwiftlyS2](https://github.com/swiftly-solution/swiftlys2)

## Примітка

Це модифікована збірка CS2_Admin з інтеграцією T3Menu. Вона не є офіційним релізом оригінального проєкту.
