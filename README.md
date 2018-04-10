# python-code-conventions



## Не перезаписывать переменные без явной алгоритмической необходимости


Плохо:

    account = AccountSchema.extract()
    account = Account.create(account['name'], account['password'])

Хорошо:

    data = AccountSchema.extract()
    account = Account.create(data['name'], data['password'])

Хорошо:

    counter = 0
    while counter < 10:
        counter = counter + 2
