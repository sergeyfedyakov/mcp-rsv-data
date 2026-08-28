# RSVData — исходники расширения (1C:EDT)

Эта ветка форка добавляет в репозиторий исходники расширения **RSVData** в формате
проекта **1C:EDT** — для отслеживания изменений, ревью и сборки `.cfe` силами сообщества.
В апстриме расширение распространяется собранным бинарником `RSVData.cfe` через Releases;
здесь — те же исходники в EDT-формате.

> **Оригинальная документация** — [RSVData.md](RSVData.md) (README апстрима).
> Полное руководство пользователя — [guide.md](guide.md).

## Состав

- `RSVData/` — проект 1C:EDT (`.project`, `.settings`, `src/`):
  - `src/CommonModules/RSVData_Ядро`, `RSVData_Сервер`, `RSVData_Справка`,
    `RSVData_Анонимизация` — логика инструментов MCP
    (диспетчеризация `ОбработатьСообщение`, исполнение, справка, обезличивание ПДн);
  - `src/HTTPServices/RSVData_MCP` — HTTP-сервис для публикации на веб-сервере;
  - `src/Configuration`, `src/Constants`, `src/InformationRegisters`, `src/Subsystems`.

## Отличия от релиза

База — расширение RSVData **1.3.0** (релиз автора). В этой ветке — версия **1.3.1**
с доработкой: инструмент **eventlog** — чтение журнала регистрации 1С с отборами
(период, уровни, события, приложения, пользователи, поиск по комментарию):

- область `ЖурналРегистрации` в `RSVData_Ядро`;
- диспетчеризация инструмента в `RSVData_Сервер`;
- тема справки `eventlog` в `RSVData_Справка` (см. `help topic=eventlog`).

## Как открыть

1. 1C:EDT (актуальной версии).
2. File → Import… → General → Existing Projects into Workspace → выбрать папку `RSVData`.
3. Базовая конфигурация не нужна — это самостоятельное расширение.

## Как собрать RSVData.cfe

В 1C:EDT: правой кнопкой по проекту → Экспорт → файл поставки расширения (`.cfe`).

## Релизный архив MCP-RSV-Data.zip

Как у апстрима, релиз этой ветки — архив **`MCP-RSV-Data.zip`** (в GitHub → Releases
этого форка) из пяти файлов:

| Файл | Откуда берётся |
|---|---|
| `RSVData.cfe` | экспорт проекта `RSVData/` из 1C:EDT (см. раздел выше) |
| `rsvdata-bridge.exe` | из [релиза апстрима](https://github.com/prepod2003/mcp-rsv-data/releases/latest) (мост в этой ветке не менялся) |
| `guide.html`, `guide.md` | корень репозитория |
| `README.txt` | краткое описание архива (правьте под версию) |

Сборка: сложить файлы в папку и упаковать
`Compress-Archive -Path RSVData.cfe,rsvdata-bridge.exe,guide.html,guide.md,README.txt -DestinationPath MCP-RSV-Data.zip`.

Публикация: GitHub → форк → вкладка **Releases** → *Draft a new release* →
тег (например `v1.3.1`) на ветке `rsvdata-sources` → заголовок и описание →
перетащить `MCP-RSV-Data.zip` в блок «Attach files» → *Publish release*.
Постоянная ссылка `releases/latest/download/MCP-RSV-Data.zip` появится сама.

## Проверка eventlog

После установки расширения в базу (веб-публикация HTTP-сервиса или мост `rsvdata-bridge`)
вызвать у MCP-сервера:

- `help topic=eventlog` — справка;
- `eventlog values=true` — допустимые значения отборов (приложения, события);
- `eventlog from=2026-01-01 levels=Error,Warning limit=50` — чтение журнала.
