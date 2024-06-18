Функция `sorted` возвращает новый отсортированный список, который получен из итерируемого объекта, который был передан как аргумент. Функция также поддерживает дополнительные параметры, которые позволяют управлять сортировкой.

Первый аспект, на который важно обратить внимание - sorted всегда возвращает список.

Если сортировать список элементов, то возвращается новый список:

In [1]: list_of_words = ['one', 'two', 'list', '', 'dict']

In [2]: sorted(list_of_words)
Out[2]: ['', 'dict', 'list', 'one', 'two']

При сортировке кортежа также возвращается список:

In [3]: tuple_of_words = ('one', 'two', 'list', '', 'dict')

In [4]: sorted(tuple_of_words)
Out[4]: ['', 'dict', 'list', 'one', 'two']

Сортировка множества:

In [5]: set_of_words = {'one', 'two', 'list', '', 'dict'}

In [6]: sorted(set_of_words)
Out[6]: ['', 'dict', 'list', 'one', 'two']

Сортировка строки:

In [7]: string_to_sort = 'long string'

In [8]: sorted(string_to_sort)
Out[8]: [' ', 'g', 'g', 'i', 'l', 'n', 'n', 'o', 'r', 's', 't']

Если передать sorted словарь, функция вернет отсортированный список ключей:

In [9]: dict_for_sort = {
   ...:         'id': 1,
   ...:         'name': 'London',
   ...:         'IT_VLAN': 320,
   ...:         'User_VLAN': 1010,
   ...:         'Mngmt_VLAN': 99,
   ...:         'to_name': None,
   ...:         'to_id': None,
   ...:         'port': 'G1/0/11'
   ...: }

In [10]: sorted(dict_for_sort)
Out[10]:
['IT_VLAN',
 'Mngmt_VLAN',
 'User_VLAN',
 'id',
 'name',
 'port',
 'to_id',
 'to_name']