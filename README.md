# python-code-conventions



## Не перезаписывать переменные без явной алгоритмической необходимости


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
