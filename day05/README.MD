# Введение параметров команды. Вычисление текущего значения счета
## Введение параметров команды.
Возникает вопрос - как передать боту числа? Ведь команды типа `/20` не доступны.  
Для передачи боту числовых значений можно использовать параметр команды: написать команду а после нее цифровое значение `/income 100`  
С точки зрения обработчика вводимой строки - у нас есть два значения, разделенных пробелом.  
Для получения таких значений используется метод `split` - он разделяет строку на части по выбранному разделителю и отправляет каждый элементик между разделителем в список.
Например, программа
```py
string_in = '/income 100'
elements = string_in.split(' ')
print(elements) # Выведет ['/income', '100']
print(elements[0]) # Выведет /income
print(elements[1]) # Выведет 100
```
**Список** - это специальная переменная, в которой может хранится несколько значений. К каждому значению можно получить доступ по номеру, при этом отсчет элементов начинается с нуля.

Мы видим, что с помощью метода `split` - можем получать значения параметров. В нашем случае - нам необходим второй элемент (с номером `1`).  
```py
string_in = '/income 100'
elements = string_in.split(' ')
print(elements[1]) # Выведет 100
```
Для того, чтобы накапливать значения дохода нам необходимо создать переменную, в которой будем зранить значение счета (`accaunt`) и при вводе строки с параметром - будем увеличивать ее на значение параметра.
```py
account = 0
string_in = '/income 100'
elements = string_in.split(' ')
account = account + elements[1]
```
Такая программа вызовет ошибку `TypeError: unsupported operand type(s) for +: 'int' and 'str'` потому, что результатом работы метода `split` будут строки, в то время, как у нас `account` - числовая переменная.
Для того, чтобы исправаить эту ошибку нам необходимо привест строковое значение к числовому, используя функцию `int`. Она получает строковое значение и возвращает числовое значение. `int('100')` вернет число

```py
account = 0
string_in = '/income 100'
elements = string_in.split(' ')
account = account + int(elements[1])
print(account) # Выведет 100
```

...
* Вычитание
* Чтение данных
* Команды `income` и `costs`

```py
from telegram.ext import Updater, MessageHandler, Filters, CommandHandler
# формируем схему - переменная-флаг задает состояние. После команды - она становится равно 1. При следующем вводе - равной 0.
account = 0

def costs(update, context):
    global account
    string_in = update.message.text
    elements = string_in.split(' ')
    account = account - int(elements[1])
    string_out = str(account)
    update.message.reply_text(string_out)

def income(update, context):
    global account
    string_in = update.message.text
    elements = string_in.split(' ')
    account = account + int(elements[1])
    string_out = str(account)
    update.message.reply_text(string_out)

def echo(update, context):
    string_in = update.message.text

    if string_in == '/start':
        string_out = 'Hello! This is own finances bot!'
    else:
        string_out = string_in

    update.message.reply_text(string_out)

updater = Updater("")

dispatcher = updater.dispatcher

dispatcher.add_handler(CommandHandler('costs', costs))
dispatcher.add_handler(CommandHandler('income', income))
dispatcher.add_handler(MessageHandler(Filters.all, echo))


updater.start_polling()
updater.idle()

```


 