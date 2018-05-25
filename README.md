
# Ценности

Главная цель Python Code Conventions — сохранение низкой стоимости разработки и поддержки кода на длинной дистанции.

Основные ценности, помогающие достичь этой цели:

**Читаемость**

Код должен легко читаться, а не легко записываться. Это значит, что такие вещи как синтаксический сахар (если он направлен на ускорение записи, а не дальнейшего чтения кода) вредны.
Обратите внимание, что быстродействие кода не является ценностью, поэтому не самый оптимальный цикл, но удобный для понимания, будет лучше, чем быстрый, но сложный. Не нужно экономить переменные, буквы для их названий, оперативную память и так далее.



# Принципы

Принципы — это способы соблюдения описанных выше ценностей. Они чуть более детальны, содержат основные методологии разработки и подходы, которыми мы руководствуемся.

Код должен быть:

- Понятным, явным. Явное лучше, чем неявное. PEP8, Zen of Python.
- Удобным для использования сейчас
- Удобным для использования в будущем
- Должен стремиться к соблюдению принципов [KISS](https://ru.wikipedia.org/wiki/KISS_(%D0%BF%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF)), [SOLID](https://ru.wikipedia.org/wiki/SOLID_(%D0%BE%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)), [DRY](https://ru.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself), [GRASP](https://ru.wikipedia.org/wiki/GRASP)
- Любая часть системы должна иметь изолированную логику и при надобности внешний интерфейс, который позволяет с этой логикой работать. Любая внутренняя часть должна иметь возможность быть измененной без какого-либо ущерба внешним системам
- Код должен быть таким, чтобы его можно было автоматически отрефакторить в IDE (например, Find usages и Rename в PyCharm). В частности, не следует давать слишком общие или короткие имена классам и методам, полям.
- Последовательным. Код должен читаться сверху вниз. Читающий не должен держать что-то в уме, возвращаться назад и интерпретировать код иначе.
- Должен иметь минимальную [цикломатическую сложность](https://ru.wikipedia.org/wiki/%D0%A6%D0%B8%D0%BA%D0%BB%D0%BE%D0%BC%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%B0%D1%8F_%D1%81%D0%BB%D0%BE%D0%B6%D0%BD%D0%BE%D1%81%D1%82%D1%8C)



# Правила работы с `git`


### 📖 Решение каждой отдельной задачи оформлять в виде PR в метку master




# Смысловые правила


### 📖 Запрещен неиспользуемый код

Если код можно убрать, и работа системы от этого не изменится, его не должно быть.

Плохо:

```python
if False:
    legacy_method_call()

legacy_condition = True

if legacy_condition:
    finalize_data(data)
```

Хорошо:

```python
finalize_data(data)
```


### 📖 Не перезаписывать переменные без явной алгоритмической необходимости

Мотивация: читаемость, упрощение отладки, предсказуемость.


Плохо:

```python
account = AccountSchema.extract()
account = Account.create(account['name'], account['password'])
```

Хорошо:

```python
data = AccountSchema.extract()
account = Account.create(data['name'], data['password'])
```

Хорошо:

```python
counter = 0
while counter < 10:
    counter = counter + 2
```



### 📖 Создание объекта по возможности должно быть сокрыто в конструкторе

Мотивация: принцип единственной ответственности, транзакционность конструктора, инкапсуляция конструирования.

Плохо:

```python
def clone(self):
    report = Report()
    report.name = self.name
    report.filters = self.filters
    report.slices = self.slices
    report.columns = self.columns
    return report
```

Хорошо:
```python
def clone(self):
    return Report(
        name=self.name,
        filters=self.filters,
        slices=self.slices,
        columns=self.columns
    )
```





### 📖 Использовать факт, что пустые коллекции вычисляются в `False` в булевом контексте

Мотивация: [PEP8](https://www.python.org/dev/peps/pep-0008/#programming-recommendations):

> For sequences, (strings, lists, tuples), use the fact that empty sequences are false.

Плохо:

```python
if len(metric.functions) == 0:
    db.session.delete(metric)
```

Хорошо:

```python
if not metric.functions:
    db.session.delete(metric)
```


## **Работа с переменными**

### 📖 Название переменных должно соответствовать содержанию

Нельзя писать короткие названия, например `c`. Нельзя назвать переменную `day` и хранить в ней массив статистических данных за этот день.

### 📖 Часто упоминаемые объекты именуются одинаково во всем проекте

Плохо:

```python
customer = User()
client = User()
obj = User()
```

Хорошо:

```python
user = User()
```

### 📖 Признак объекта добавляется к названию

Если это отфильтрованный по какому-то признаку проект, то признак добавляется к названию. Например, `unpaid_project`.

### 📖 Переменные, отражающие свойства объекта, должны включать название объекта

Плохо:
```python
project = Project()
name = project.name
project = project.name
```

Хорошо:
```python
project = Project()
project_name = project.name
```

### 📖 Переменные по возможности должны называться на корректном английском

Плохо:
```python
users_stored = []
```

Хорошо:
```python
stored_users = []
```

Исключение: сгруппированные по некому признаку поля или константы. В этом случае можно использовать префикс.

```python
class ProjectInfo:
    STATUS_READY = 1
    STATUS_BLOCKED = 2

    def __init__(self):
        billing_is_paid = False
        billing_paid_date = None
        billing_sum = 0
```

### 📖 Переменные и свойства объекта должны являться существительными и называться так, чтобы они правильно читались при использовании, а не при инициализации.

Плохо:
```python
current_license->expire_at
current_license->set_expire_at(date)
current_license->get_expire_at()
```

Хорошо:
```python
current_license.expiration_date
current_license.set_expiration_date(date)
current_license.get_expiration_date()
```

# Стилевые правила

### 📖 Придерживаться PEP8

Максимально допустимая длина строки **100** символов. Допустимо превышать, если встречается неделимая длинная строка (например, URL).


### 📖 Аккуратно форматировать секцию импортов в начале файла

Сначала stdlib, затем библиотеки, затем модули текущего проекта.

Предпочтительно, но необязательно:

- Сортировка: сначала строки с `import`, потом строки с `from ... import ...`, внутри этих подсекций в алфавитном порядке.
- Добиться того, чтобы не было побочных эффектов, связанных с порядком импортов. Порядок импортов не должен иметь никакого значения (но это не всегда возможно).

Хорошо:

```python
import os
import os.path
import random
import subprocess
import time
import uuid

import arrow
from celery import Celery
from flask import current_app
from sqlalchemy import or_

import glue.cache
import glue.core.histogram
import glue.core.mill
import glue.utils
from glue.models import Account, Call, db
```

### 📖 Закрывающуюся скобку для многострочных выражений ставить на новой строке на вертикальном уровне начала выражения

Мотивация: единообразие стиля в кодовой базе, читаемость (скобки в рациональном стиле K&R задают однообразную хорошо воспринимаемую визуально структуру), скобка не меняет положения в diff-ах коммитов.

Плохо:

```python
return Report(name=self.name,
              filters=self.filters,
              slices=self.slices,
              columns=self.columns)
```

Плохо:

```python
return Report(
    name=self.name,
    filters=self.filters,
    slices=self.slices,
    columns=self.columns)
```

Хорошо:
```python
return Report(
    name=self.name,
    filters=self.filters,
    slices=self.slices,
    columns=self.columns
)
```
