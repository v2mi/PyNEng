##Передача аргументов скрипту (argv)

Очень часто скрипт решает какую-то общую задачу.
Например, скрипт обрабатывает как-то файл конфигурации.
Конечно, в таком случае, не хочется каждый раз руками в скрипте править название файла. 

Гораздо лучше будет передавать имя файла как аргумент скрипта и затем использовать уже указанный файл.

В Python, в модуле sys есть очень простой и удобный способ для работы с аргументами - argv.

Посмотрим на пример. У нас есть скрипт access_template_argv.py:
```python
from sys import argv

interface, vlan = argv[1:]

access_template = ['switchport mode access',
                   'switchport access vlan %d',
                   'switchport nonegotiate',
                   'spanning-tree portfast',
                   'spanning-tree bpduguard enable']

print 'interface %s' % interface
print '\n'.join(access_template) % int(vlan)
```

Проверяем работу скрипта:
```
$ python access_template_argv.py Gi0/7 4
interface Gi0/7
switchport mode access
switchport access vlan 4
switchport nonegotiate
spanning-tree portfast
spanning-tree bpduguard enable
```

То есть, аргументы, которые мы передали скрипту, были подставлены как значения в наш шаблон.

Тут надо пояснить несколько моментов:
* argv это список (будем рассматривать дальше)
* argv содержит не только аргументы, которые передали скрипту, но и название самого скрипта

В данном случае, в списке argv находятся такие элементы:
```
['access_template_argv.py', 'Gi0/7', '4']
```

Сначала идет имя самого скрипта, затем аргументы, которые мы передавали, в том же порядке.

Ещё один момент, который может быть не очень понятным:
```
interface, vlan = argv[1:]
```

Выражение ```argv[1:]``` должно быть знакомым. Это срез списка.
То есть, в правой стороне у нас остается список, с двумя элементами: ```['Gi0/7', '4']```.


Теперь разберемся с двойным присваиванием.

В Python есть возможность за раз присвоить значения нескольким переменным. Простой пример:
```python
In [16]: a = 5
In [17]: b = 6
In [18]: c, d = 5, 6
In [19]: c
Out[19]: 5

In [20]: d
Out[20]: 6
```

А, если вместо чисел список, как в случае с argv:
```python
In [21]: arg = ['Gi0/7', '4']
In [22]: interface, vlan = arg

In [23]: interface
Out[23]: 'Gi0/7'

In [24]: vlan
Out[24]: '4'
```

> Обратите внимание, что argv всегда возвращает аргументы в виде строки. А, так как в списке access_template в синтаксисе форматирования строк, у нас указано число, мы преобразуем переменную vlan в число (последняя строка). 
