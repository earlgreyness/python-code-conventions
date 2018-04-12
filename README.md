# python-code-conventions



## Не перезаписывать переменные без явной алгоритмической необходимости

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



## Создание объекта по возможности должно быть сокрыто в конструкторе

Мотивация: принцип единственной ответственности, транзакционность конструктора, инкапсуляция конструирования.

Плохо:

```python
def clone(self) -> 'Report':
    report = Report()
    report.name = self.name
    report.filters = self.filters
    report.slices = self.slices
    report.columns = self.columns
    return report
```

Хорошо:
```python
def clone(self) -> 'Report':
    return Report(
        name=self.name,
        filters=self.filters,
        slices=self.slices,
        columns=self.columns
    )
```


## Закрывающуюся скобку для многострочных выражений ставить на новой строке на вертикальном уровне начала выражения

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


## Обязательно использовать факт, что пусные коллекции вычисляются в `False` в булевом контексте

Мотивация: PEP8:

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
