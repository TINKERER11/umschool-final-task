# Общие замечания по финальному проекту

## 1. Форматирование кода
- Старайтесь форматировать код в соотвествии со стилем PEP8 (https://peps.python.org/pep-0008/)
- Почему?
- Всякий раз, когда вы будете смотреть на новый код и он будет в стиле, который вы знаете у вас не будет занимать время разбирание со стилем. Вам нужно будет только изучать логику кода. **Код пишется один раз, а читается -- сотни**

## 2. Конкатенация строк в цикле - это зло
- Почему?
- Всякий раз, когда вы в цикле делаете конкатенацию строк, то линейная сложность возрастает до квадратичной. Происходит это из-за **имммутабельности** строк
```python
s = ''
for i in range(n):
    s += f'n -- {n}\n'
print(s)
```
Time complexity - `O(n^2)`

- А как надо?
- Вот так. Из-за **мутебельности** списка не просиходит его пересоздание, из-за этого сложность следуещего кода -- линейная
```python
arr = []
for i in range(n):
    arr.append(f'n -- {n}')
s = '\n'.join(arr)
print(s)
```
Time complexity - `O(n)`

## 3. Дублирование кода - это зло

- В финальном проекте нужно было реализовать десяток функций помощников для общения с базой данных. И понятное дело, что кадой из них нужно подключение к бд. Но дублировать строки для подключения к ней - плохо. 

- Что же делать?
- Как минимум надо вынести подключение в отдельную функцию и звать только её. А как макссимум использовать декоратор, который будет обеспечивать открытие/закрытие соединения с базой данных. Пример декоратора `@decorator_add_connection` [тык](https://github.com/thehighestmath/umschool-final-task/blob/master/example/db_adapter.py#L8)

## 4. Коммитить чувсвительные данные в открытый репозиторий - риск потерять их
- Что такое чувствительные данные?
- Логины, пароли, ssh-ключи, токены и т.д. есть боты на гитхабе, которые бегают по вашим репозиториям и предупреждают о некоторых из таких уязвимостей. Пример [тык](https://github.com/thehighestmath/umschool-final-task/security/secret-scanning/1)

---

- А что же делать?
- Использовать файлик в репозитории (например `.env`), который лежит в `.gitignore`. Файлик `.env` долженг хранить в себе чувствительные данные. 
- Как с ним работать?
- Для питона есть такая библиотека [тык](https://pypi.org/project/python-dotenv/)

## 5. Используйте f-строки
- Всем обязательно прочитать статью [тык](https://habr.com/ru/articles/462179/)
---
Коротко:
- Вместо `%` использовать `f`
- Почему?
- Это быстрее
- Это лучше читается

## 6. Декомпозируйте проект
- Да, кажется, что 1000 строк в одном файле это немного, но когда вы захотите что-то поменять - вам придется менять всё в одном файле
- А что если в проекте не вы один работаете, тогда каждый из разработчиковы будет менять один и тот же файл. Тогда шансы на конфликты повышаются. + лезть файл должна быть 1 причина (в идеале), а когда код написан всего в 1 файле, причина далеко не одна))

- По той же причине огромные функции - тоже плохой подход.
- Хорошая функция до 50 строк

## 7. Транслит
- Просто не надо. Код будут читать другие люди, у программистов транслит не принят

## 8. Не используйте \ для продолжения строки
### Плохой подход
```python
a = 1 \
    + 2 \
    + 3 \
    - 4 \
    + 5
print(a)
```

### Хороший подход
- Скобки могут дать вам необходимую магию, если вы хотите несколько строк
- `\` можно тупо забыть :/
```python
a = (
    1
    + 2
    + 3
    - 4
    + 5
) 
print(a)
```

## 9. Исключения
Штатные ситуации **нельзя** обрабатывать исключениями

К штатным ситуациям можно отнести:
- любой пользовательский ввод (надо использовать условия)
- деление на ноль
- открытие несуществующего файла

- Почему?
- Обработка исключений `try...except` раскручивает стек вызовов функций, а это замедляет выполнение программы

> [!NOTE]
> Используйте исключения с умом!


## 10. Перебирающий цикл
### Плохой подход
```python
arr = [1, 3, 5, 7]
for i in range(len(arr)):
    print(arr[i])
```

### Хороший подход
```python
arr = [1, 3, 5, 7]
for x in arr:
    print(x)
```
- Если вам нужно пройтись по коллекции, необязательно знать её длину

## 11. Ранний выход
### Плохой подход
```python
if answer == 0:
    return False
else:
    return answer
```

### Хороший подход
Если `answer == 0` то функция завершится, а если нет, то `else` не нужен. Дополнительно мы получаем меньшую вложенность кода, а это упрощает его чтение
```python
if answer == 0:
    return False
return answer
```