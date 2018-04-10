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
    report.filters = self.filters,
    report.slices = self.slices,
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
        columns=self.columns,
    )
```


## Закрывающуюся скобку для многострочных выражений ставить на новой строке на вертикальном уровне начала выражения

Мотивация: единообразие стиля в кодовой базе, читаемость (скобки в стиле K&R задают однообразную визуальную структуру), скобка не меняет положения в diff-ах коммитов.

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
