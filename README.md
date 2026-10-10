# Анализ каталога фильмов

**Студент:** Семёшин  
**Группа:** М26-555

Учебный проект на Python 3.12: один скрипт, стандартный модуль `math`
и Ruff как единственная зависимость для разработки.

## Запуск и проверка

```bash
uv sync --locked
uv run catalog_analysis.py
uv run ruff check .
```

Версия Python закреплена в `.python-version`, зависимости — в `uv.lock`.
Окружение `.venv` создаётся автоматически и не попадает в Git.

## Этапы

1. Средний рейтинг, возраст фильмов, перевод минут в часы.
2. Категории рейтинга и года выпуска.
3. Циклы `for`, `while`, `continue`, `break`, `else`.
4. Нормализация названий, слаги, строки отчёта.
5. Сортировка списка и топ фильмов.
6. Статистика жанров, фильмография, генератор словаря.
7. Объединение, пересечение и разность множеств.
8. Генераторы и суммарная длительность.
9. Итоговый консольный отчёт.

Каждый этап зафиксирован отдельным коммитом после проверки. Перед ними
есть коммит настройки проекта. Локальные коммиты созданы с техническим
автором `Codex <codex@localhost>`; глобальные настройки Git не менялись.

## Соответствие задачам

| Этап | Задачи | Функции |
| --- | --- | --- |
| 1 | 1–3 | `average_rating`, `catalog_age_stats`, `duration_in_hours` |
| 2 | 4–5 | `rating_tier`, `decade_label` |
| 3 | 6–8 | `print_non_comedies`, `find_first_masterpiece`, `count_long_movies` |
| 4 | 9–11 | `normalize_title`, `make_slug`, `format_report_line` |
| 5 | 12–13 | `titles_sorted_by_rating`, `top_n_by_rating` |
| 6 | 14–16 | `count_by_genre`, `actor_filmography`, `ratings_above_average` |
| 7 | 17–19 | `all_genres`, `common_actors`, `genres_only_in_one` |
| 8 | 20–21 | `iter_high_rated`, `print_high_rated`, `total_duration_above_seven` |
| 9 | 22 | `build_report` |

## Демонстрации циклов и генераторов

Обычный запуск печатает только итоговый отчёт по образцу. Демонстрации
из этапов 3 и 8 можно вызвать отдельно, например в интерактивном Python:

```bash
uv run python
```

```python
import catalog_analysis as c

c.print_non_comedies(c.movies)  # for и continue: семь названий
first = c.find_first_masterpiece(c.movies)  # while и break
print(first["title"])  # The Quiet Algorithm
c.find_first_masterpiece(c.movies[:7])  # else: Шедевров не найдено

c.print_high_rated(c.movies)  # for, yield и format_report_line: три фильма
print(c.total_duration_above_seven(c.movies))  # 713
print(c.ratings_above_average(c.movies))  # генератор словаря: пять фильмов
```

Импорт модуля ничего не печатает. В блоке `if __name__ == "__main__":`
находится единственный вызов `build_report(movies)`.

## Уточнения поведения

- Возраст считается относительно `2026`, как задано в условии; другой год
  можно передать в `catalog_age_stats`.
- Для пустого каталога средний рейтинг равен `0.0`, статистика возраста —
  `(0, 0, 0)`. Списки, словари и множества результатов пусты.
- Порог «выше» строгий, а «не ниже» включает границу. Например, рейтинг
  `9.0` относится к категории «шедевр», но поиск в этапе 3 требует `> 9.0`.
- `top_n_by_rating` возвращает пустой список при `n <= 0`, а при слишком
  большом `n` — все доступные фильмы.
- `normalize_title` меняет первую букву каждого слова согласно формуле
  из задания, сохраняет остальные буквы и убирает лишние пробелы.
- Исходные названия сохраняются в данных, сортировках и фильмографиях;
  нормализация применяется при форматировании строк отчёта.
- При равном количестве фильмов жанры в отчёте идут по алфавиту, поэтому
  вывод не зависит от порядка обхода множества.

## Ожидаемый отчёт

```text
ОТЧЕТ ПО КАТАЛОГУ
Средний рейтинг: 7.2
Средний возраст фильмов: 8 лет

Топ-3 фильма:
  "The Quiet Algorithm" (2024) — 9.2/10, 1ч 58м, жанры: drama, sci-fi
  "Midnight In Oslo" (2020) — 8.9/10, 2ч 4м, жанры: mystery, thriller
  "The Dune Chronicles" (2021) — 8.6/10, 2ч 35м, жанры: drama, sci-fi

Фильмов по жанрам:
  drama — 5
  comedy — 3
  sci-fi — 3
  thriller — 3
  action — 2
  mystery — 1

Все жанры каталога: action, comedy, drama, mystery, sci-fi, thriller
```
