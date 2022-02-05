---
title: О паттерн матчинге в python
---

В 3.10, помимо всего прочего, завезли паттерн матчинг. И вы об этом, конечно, слышали. А вы слышали про `functools`'овый `singledispatch` который доступен аж со времён 3.4 и реализует матчинг типов аргументов в функции? Выглядит это так:

```python
Recipient = Email | SmsNumber

@singledispatch
def send_message(recipient: Recipient,  message: str):
    raise NotImplementedError()

@send_message.register
def send_message_via_email(recipient: Email,  message: str):
    # Send message via email by somehow

@send_message.register
def send_message_via_sms(recipient: SmsNumber, message: str):
    # Send message via sms by somehow

some_email = Email('some@email.com')
some_phone = SmsNumber('+123456789')

send_message(some_email, 'message')
send_message(some_phone, 'message')
```

Ну практически haskell, нет? Увы, нет, ведь в python значение — это не то же самое что и тип, поэтому и возможности заматчить по значению у нас в этом случае нет. Кроме того, важный аспект работы `singledispatch` в том, что он, ну... single. Матчинг происходит только по типу первого аргумента (но если сильно нужно, то для матчинга всех аргументов есть [батарейка](https://github.com/mrocklin/multipledispatch/)). Тем не менее, я считаю, что `singledispatch`, при всех своих недостатках, здорово помогает организовать код лаконично и читаемо.

А вот чего в python не завезли (по, в целом, понятным причинам) так это возможности запилить sync и async реализации функции под одним именем. И это печально.
