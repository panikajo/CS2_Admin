# CS2_Admin с поддержкой T3Menu

Модифицированная версия плагина **CS2_Admin** для SwiftlyS2, в которой стандартное меню заменено на Panorama-меню **T3Menu**.

## Возможности

- Все разделы админ-меню работают через T3Menu.
- Поддерживаются три визуальных стиля:
  - `SourceMod`
  - `Stylish`
  - `Source2`
- Поддерживаются три режима управления:
  - `KeyPress`
  - `Scrollable`
  - `Clickable`
- Стандартное меню SwiftlyS2 автоматически закрывается.
- Официальное автообновление CS2_Admin отключено, чтобы оно не заменило модифицированную сборку.
- Добавлена ранняя регистрация T3Menu API для совместимости с серверами, где другие плагины мешают стандартному подключению shared-интерфейсов.

## Требования

- Counter-Strike 2 Dedicated Server
- SwiftlyS2
- .NET 10
- T3Menu 1.3.0
- Workshop-аддон T3Menu

Workshop:

https://steamcommunity.com/sharedfiles/filedetails/?id=3790988631

Исходный проект T3Menu:

https://github.com/T3Marius/T3Menu

> Игрокам необходимо загрузить Workshop-аддон, иначе Panorama-интерфейс, стили и звуки T3Menu могут не работать.

## Установка

### 1. Остановите сервер

Перед заменой файлов полностью остановите сервер.

Не рекомендуется устанавливать эту сборку через hot reload или команду `sw plugins load`.

### 2. Удалите старые версии

Удалите следующие папки, если они существуют:

```text
addons/swiftlys2/plugins/CS2_Admin
addons/swiftlys2/plugins/T3Menu
```

Также проверьте, чтобы на сервере не осталось других копий `CS2_Admin.dll`:

```bash
find addons/swiftlys2/plugins -type f -name "CS2_Admin.dll"
```

Должна остаться только одна DLL.

### 3. Распакуйте архив

Распакуйте содержимое release-архива в:

```text
game/csgo/addons/swiftlys2/plugins/
```

После установки структура должна выглядеть так:

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

> Название папки `T3Menu` должно совпадать с названием `T3Menu.dll`. Не переименовывайте её в `T3MenuV1.3.0`.

### 4. Настройте CS2_Admin

Откройте основной файл конфигурации CS2_Admin:

```text
addons/swiftlys2/configs/plugins/CS2_Admin/config.json
```

Проверьте следующие параметры:

```json
{
  "AutoUpdate": false,
  "DisableBuiltInMenuWhenUsingT3Menu": true
}
```

Описание:

| Параметр | Значение | Назначение |
|---|---:|---|
| `AutoUpdate` | `false` | Запрещает официальному автообновлению заменить модифицированную DLL |
| `DisableBuiltInMenuWhenUsingT3Menu` | `true` | Закрывает стандартное меню SwiftlyS2 при открытии T3Menu |

### 5. Запустите сервер

После установки полностью запустите сервер.

Сначала проверьте T3Menu:

```text
!t3menu_test
```

Затем откройте админ-меню:

```text
!admin
```

## Настройка T3Menu

Настройки внешнего вида и управления находятся в конфигурации T3Menu:

```text
addons/swiftlys2/configs/plugins/T3Menu/t3menu.jsonc
```

Основные параметры:

```jsonc
{
  "Navigation": "Clickable",
  "Style": "Stylish",
  "ItemsPerPage": 6
}
```

Не удаляйте остальные параметры файла. Изменяйте только необходимые значения.

## Визуальные стили

### SourceMod

```json
"Style": "SourceMod"
```

Компактный классический стиль.

### Stylish

```json
"Style": "Stylish"
```

Современное меню с карточками и анимациями.

### Source2

```json
"Style": "Source2"
```

Стиль, приближённый к интерфейсу Counter-Strike 2.

## Режимы управления

### Clickable

```json
"Navigation": "Clickable"
```

Управление мышкой непосредственно в меню.

- Tab удерживать не нужно.
- T3Menu самостоятельно показывает курсор.
- После закрытия меню курсор отключается.

Рекомендуемая настройка:

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

Управление клавишами:

| Клавиша | Действие |
|---|---|
| `W` | Вверх |
| `S` | Вниз |
| `E` | Выбрать |
| `R` | Закрыть |

Пример:

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

Управление командами в чате:

| Команда | Действие |
|---|---|
| `!1`–`!6` | Выбрать пункт |
| `!7` | Назад |
| `!8` | Следующая страница |
| `!9` | Закрыть меню |

Обычное нажатие клавиш `1–9` может переключать оружие, поэтому в этом режиме используются команды `!1`, `!2` и так далее.

## Проверка установки

### T3Menu недоступен

Ошибка:

```text
T3Menu is not available. Install and load the T3Menu plugin before CS2_Admin.
```

Проверьте наличие файлов:

```text
addons/swiftlys2/plugins/T3Menu/T3Menu.dll
addons/swiftlys2/plugins/T3Menu/resources/exports/T3Menu.Contract.dll
```

Также выполните:

```text
!t3menu_test
```

Если команда неизвестна, T3Menu не загрузился.

### SwiftlyS2 ищет неправильную DLL

Ошибка:

```text
Plugin entrypoint DLL not found:
T3MenuV1.3.0/T3MenuV1.3.0.dll
```

Папка названа неправильно. Она должна называться:

```text
T3Menu
```

И содержать:

```text
T3Menu/T3Menu.dll
```

### Открывается стандартное меню

Проверьте настройки:

```json
"AutoUpdate": false,
"DisableBuiltInMenuWhenUsingT3Menu": true
```

Также убедитесь, что на сервере нет второй копии CS2_Admin:

```bash
find addons/swiftlys2/plugins -type f -name "CS2_Admin.dll"
```

После замены файлов выполните полный перезапуск сервера.

### Меню видно, но оно не нажимается

Проверьте параметр `Navigation`.

Для управления мышкой:

```json
"Navigation": "Clickable"
```

Для управления клавишами `W/S/E/R`:

```json
"Navigation": "Scrollable"
```

Для команд `!1`–`!9`:

```json
"Navigation": "KeyPress"
```

Также убедитесь, что Workshop-аддон T3Menu установлен и загружен клиентом.

## Сборка из исходников

Требуется .NET SDK 10.

```bash
dotnet restore CS2_Admin.csproj
dotnet publish CS2_Admin.csproj -c Release
```

Готовая сборка будет создана в каталоге:

```text
build/publish/CS2_Admin/
```

T3Menu необходимо собирать отдельно:

```bash
dotnet restore T3Menu/T3Menu/T3Menu.csproj
dotnet build T3Menu/T3Menu/T3Menu.csproj -c Release
```

После сборки разместите файлы следующим образом:

```text
T3Menu/T3Menu.dll
T3Menu/resources/exports/T3Menu.Contract.dll
```

## Обновление

Не устанавливайте официальный архив CS2_Admin поверх этой версии — он вернёт стандартное меню SwiftlyS2.

Перед обновлением:

1. Сделайте резервную копию конфигурации.
2. Полностью остановите сервер.
3. Замените папки `CS2_Admin` и `T3Menu`.
4. Не заменяйте пользовательские файлы конфигурации.
5. Запустите сервер заново.

## Благодарности

- Авторы оригинального CS2_Admin
- [T3Marius/T3Menu](https://github.com/T3Marius/T3Menu)
- Команда [SwiftlyS2](https://github.com/swiftly-solution/swiftlys2)

## Примечание

Это модифицированная сборка CS2_Admin с интеграцией T3Menu. Она не является официальным релизом оригинального проекта.
