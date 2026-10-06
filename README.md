# Госреестры РФ: открытые данные каталога «Реестр реестров»

Паспорт 129 государственных реестров России: название, ведомство, закон, год появления, открытость, есть ли API и открытые данные, официальный сайт и где проверить запись. Источник: каталог [reestr-reestrov.ru](https://reestr-reestrov.ru/?utm_source=github&utm_medium=referral&utm_campaign=gosreestry-data).

Здесь нет данных из самих реестров (компаний, людей, лицензий). Это справочник о реестрах.

## Цифры

Посчитаны скриптом из `data/registries.json` при выпуске версии 1.0.1 (2026-10-06), руками не правятся.

- Реестров: **129**, ведомств, которые их ведут: **39**
- Открыты для всех: 103, частично: 18, закрыты: 8
- Есть API или платная выгрузка: 19; есть набор открытых данных: 34; ни того ни другого: 79
- Появились в 2020 году и позже: 25; самый старый: Коды разработчиков по ЕСКД (1980)

## Поля (12)

| Поле | Что это |
|---|---|
| `id` | Постоянный идентификатор реестра (латиница). Карточка на сайте: /r/&lt;id>/ |
| `short_name` | Короткое название, как его ищут люди |
| `name` | Официальное полное название |
| `agency` | Ведомство, которое ведёт реестр (короткое название) |
| `law` | Основной нормативный акт: номер, дата, название, статья |
| `year` | Год появления реестра (по первому акту или запуску) |
| `access` | Открытость: open (открыт всем), partial (частично), closed (закрыт для публики) |
| `has_api` | Есть API или платная машиночитаемая выгрузка |
| `has_opendata` | Есть набор открытых данных |
| `official_url` | Официальный сайт реестра |
| `check_url` | Где проверить запись на официальном сайте; null, если единой точки проверки нет |
| `url` | Карточка реестра на reestr-reestrov.ru |

Подробнее с типами и примерами: [SCHEMA.md](SCHEMA.md). Идентификатор `id` постоянный.

## Файлы

| Файл | Что внутри |
|---|---|
| [data/registries.json](data/registries.json) | массив объектов, 12 полей |
| [data/registries.csv](data/registries.csv) | то же для Excel: UTF-8 с BOM, разделитель «;» |
| [schema/registries.schema.json](schema/registries.schema.json) | JSON Schema |
| [SCHEMA.md](SCHEMA.md) | описание полей по-русски |

## Как использовать

### Python (pandas)

```python
import pandas as pd

df = pd.read_csv("https://raw.githubusercontent.com/devladpopov/gosreestry-data/main/data/registries.csv", sep=";", encoding="utf-8-sig")

# сколько реестров ведёт каждое ведомство
print(df.groupby("agency").size().sort_values(ascending=False).head(10))

# открытые реестры с набором открытых данных
print(df[(df.access == "open") & df.has_opendata][["short_name", "agency", "url"]])
```

### jq

```bash
# реестры с API: название, ведомство, карточка
curl -s https://raw.githubusercontent.com/devladpopov/gosreestry-data/main/data/registries.json \
  | jq -r '.[] | select(.has_api) | [.short_name, .agency, .url] | @tsv'
```

### curl

```bash
curl -LO https://raw.githubusercontent.com/devladpopov/gosreestry-data/main/data/registries.json
curl -LO https://raw.githubusercontent.com/devladpopov/gosreestry-data/main/data/registries.csv
```

Конкретную версию можно закрепить, заменив `main` в ссылке на тег, например `v1.0.1`.

## Подробные данные

Это открытый «паспорт» каждого реестра. Подробные карточки (что внутри реестра, как проверить запись, стоимость, размер, структура полей реестров, сквозные поля между реестрами, инструкции, пояснения к законам) © ООО «Стадика», доступны на сайте [reestr-reestrov.ru](https://reestr-reestrov.ru/?utm_source=github&utm_medium=referral&utm_campaign=gosreestry-data). Расширенная выгрузка и API по договору: [оставить заявку](https://reestr-reestrov.ru/open-data/?utm_source=github&utm_medium=referral&utm_campaign=gosreestry-data#dogovor).

## Нашли ошибку?

Откройте [Issue](https://github.com/devladpopov/gosreestry-data/issues/new): укажите `id` реестра, что не так и ссылку на официальный источник. Файлы здесь собираются из основного каталога, поэтому правка попадёт в следующую версию.

В открытую выгрузку не входят 10 политически чувствительных перечней, которые есть на сайте.

## Лицензия и цитирование

Данные: [CC BY 4.0](LICENSE). Можно использовать в любых целях, в том числе коммерческих, с обязательной ссылкой на источник:

> Реестр реестров, ООО «Стадика», https://reestr-reestrov.ru, CC BY 4.0

Формат для цитирования: [CITATION.cff](CITATION.cff). История изменений: [CHANGELOG.md](CHANGELOG.md).

Правообладатель: ООО «Стадика».

---

## English

**gosreestry-data** is an open, machine-readable list of 129 Russian state registries: name, agency, legal basis, year, public access level, API and open data availability, official website, where to look up a record, and a link to the registry page on reestr-reestrov.ru (12 fields). It does not contain the registries' own records. Field names are in English, values are in Russian; see [SCHEMA.md](SCHEMA.md). Detailed registry cards, field structures, cross-registry fields and guides are © ООО «Стадика» and available at [reestr-reestrov.ru](https://reestr-reestrov.ru/?utm_source=github&utm_medium=referral&utm_campaign=gosreestry-data); extended exports and API are available under contract. License: CC BY 4.0, attribution with a link to https://reestr-reestrov.ru is required. Report errors via [Issues](https://github.com/devladpopov/gosreestry-data/issues).
