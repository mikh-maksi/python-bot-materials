# Калькулятор. Вычисление текущего значения счета
# Вычисление общего значения
Мы уже научились создавать обработчики для команд. Ниже приведен код бота, который при отправке пользователем команды `/start` пишет ему в ответ строку `'Hello! This is own finances bot!'`
```py
from telegram.ext import Updater, MessageHandler, Filters, CommandHandler

def start(update, context):
    string_in = update.message.text

    string_out = 'Hello! This is own finances bot!'

    update.message.reply_text(string_out)

updater = Updater("2034824924:AAFc0q0PYPezeZ6G5kE10uBWhWSurKks-8A")

dispatcher = updater.dispatcher

dispatcher.add_handler(CommandHandler('start', start))


updater.start_polling()
updater.idle()
```
В прошлых примерах мы уже создавали информационного бота, который при отправке различных команд возвращает различные тексты.  

В данном дне нма нобходимо научиться отправлять не только название команды, но и параметр вместе с ней.
```py
/costs 30

```