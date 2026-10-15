# Лабораторная работа 1 — сбор информации об ОС

Кроссплатформенный Python-скрипт, который определяет операционную систему (Windows, macOS, Linux), собирает её основные параметры и сохраняет результат в файл `system_info.json`.

## Требования

- Python 3.10+ (для Linux нужна функция `platform.freedesktop_os_release()`, появившаяся в 3.10)
- Сторонние библиотеки не нужны — используются только модули стандартной библиотеки: `platform`, `os`, `shutil`, `json`

## Запуск

```bash
python my_script.py
```

В Linux/macOS при необходимости используйте `python3 my_script.py`.

## Как работает скрипт

1. **Определение ОС.** С помощью `platform.system()` скрипт узнаёт тип системы:
   - `Windows` → версия берётся из `platform.win32_ver()`;
   - `Darwin` → это macOS, версия берётся из `platform.mac_ver()`;
   - `Linux` → название дистрибутива читается из `/etc/os-release` через `platform.freedesktop_os_release()`; если файла нет, используется версия ядра;
   - любая другая система помечается как `Unknown`.
2. **Сбор параметров.** Скрипт собирает данные о ядре, железе, интерпретаторе Python и диске (полный список ниже).
3. **Сохранение.** Результат записывается в `system_info.json` в текущей папке (кодировка UTF-8, отступ 4 пробела) и дублируется в консоль.

## Собираемые параметры

| Раздел | Поле | Описание | Источник |
|---|---|---|---|
| `os` | `name` | Название ОС: Windows / macOS / Linux / Unknown | `platform.system()` |
| `os` | `version` | Версия ОС (для Linux — название дистрибутива, например `Ubuntu 22.04.4 LTS`) | `win32_ver()` / `mac_ver()` / `freedesktop_os_release()` |
| `kernel` | `name` | Имя ядра/системы (`Windows`, `Darwin`, `Linux`) | `platform.system()` |
| `kernel` | `release` | Релиз ядра | `platform.release()` |
| `kernel` | `version` | Полная версия/сборка ядра | `platform.version()` |
| `hardware` | `architecture` | Архитектура процессора (`AMD64`, `x86_64`, `arm64`) | `platform.machine()` |
| `hardware` | `processor` | Модель процессора (на некоторых Linux может быть пустой строкой) | `platform.processor()` |
| `hardware` | `logical_cpu_count` | Количество логических ядер | `os.cpu_count()` |
| `hardware` | `hostname` | Сетевое имя компьютера | `platform.node()` |
| `python` | `version` | Версия интерпретатора Python | `platform.python_version()` |
| `disk` | `path` | Корень диска, на котором запущен скрипт (`/` в Linux/macOS, например `C:\` в Windows) | `os.path.abspath(os.sep)` |
| `disk` | `total_gib` | Общий объём диска, ГиБ | `shutil.disk_usage()` |
| `disk` | `used_gib` | Занято, ГиБ | `shutil.disk_usage()` |
| `disk` | `free_gib` | Свободно, ГиБ | `shutil.disk_usage()` |

## Структура JSON (пример для Windows)

```json
{
    "os": {
        "name": "Windows",
        "version": "11"
    },
    "kernel": {
        "name": "Windows",
        "release": "11",
        "version": "10.0.26300"
    },
    "hardware": {
        "architecture": "AMD64",
        "processor": "Intel64 Family 6 Model 154 Stepping 3, GenuineIntel",
        "logical_cpu_count": 16,
        "hostname": "MY-PC"
    },
    "python": {
        "version": "3.13.7"
    },
    "disk": {
        "path": "D:\\",
        "total_gib": 931.5,
        "used_gib": 756.77,
        "free_gib": 174.73
    }
}
```
