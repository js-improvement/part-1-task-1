# Первое домашнее задание

В первом домашнем задании вы познакомитесь с основными конструкциями языка JavaScript.

##Прежде чем выполнять домашнее задание:

1. Подготовить репозиторий: [инструкция подготовки репозитория](https://github.yandex-team.ru/elective/instructions/blob/master/homeworkRepo.md)
1. Выкачать репозиторий на свой компьютер.
1. Открыть index.html
1. Проверить, что в консоли появилась запись "open"

##Интерфейс общения с сервером

1. Все отправляемые и получаемые сообщения должны иметь поле ``type``. Возможные значения этого поля:
  1. ``hello`` - первое сообщение от сервера, содержит инструкции для дальнейшей работы
  1. ``hi`` - ответ клиента на первое сообщение сервера, содержит конфиги выполнения задания. Формат смотри в первом сообщении сервера.
  1. ``info`` - информационные сообщения от сервера. Формат:
  ```json
  {
    "type": "info",
    "message": "infoMessage"
  }
  ```
  1. ``error`` - сообщения об ошибках. Формат:
  ```json
  {
     "type": "error",
     "message": "errorMessage"
  }
  ```
  1. ``task`` - указываем серверу какое задание мы хотим проверить. Формат:
  ```json
  {
     "type": "task",
     "task": "taskName"
  }
  ```
  1. ``ask`` - какое задание запрашивает сервер. Формат:
  ```json
  {
     "type": "ask",
     "taskName": "taskName",
     "data": "taskData"
  }
  ```
  1. ``askComplete`` - Если сервер передает для одного задания несколько сообщений, то данный тип придет после окончания всех сообщений. Формат:
  ```json
  {
     "type": "askComplete"
  }
  ```
  1. ``answer`` - ответ на задание отправляется с данным типом. Формат:
  ```json
  {
     "type": "answer",
     "data": "answerData"
  }
  ```
  1. ``done`` - задание успешно выполнено. Формат:
  ```json
  {
     "type": "done"
  }
  ```
1. Сервер поддерживает два мода работы ``test`` и ``complete``. В первом случае вы можете сами выбрать какое задание выполнять. Во втором сервер прогоняет все задания автоматически.
 
##Типы заданий

1. ``echo`` - эхо, нужно вернуть, то что прислал сервер
1. ``reverse`` - нужно вернуть входящую строку в обратном порядке. Пример: ``asd`` -> ``dsa``
1. ``sum`` - сервер присылает несколько сообщений с числами, после всех сообщений нужно вернуть ответ. Пример:

  Запрос ``ask`` 1
  ```json
    123
  ```
  Запрос ``ask`` 2
  ```json
    234
  ```
  Запрос ``askComplete``
  
  Ответ: ``357``
1. ``calc`` - простой калькулятор, нужно вернуть число. Операции: ``+``, ``*``. Только положительные числа. Пример: ``2 + 2 * 2`` -> ``6``
1. ``median`` - нужно вернуть на каждом шаге верхнюю медиану переданных значений. [Медиана википедия](https://ru.wikipedia.org/wiki/Медиана_(статистика)) Пример: первое число ``3`` -> ``3``, второе число ``1`` (``[3, 1]``) -> ``3``, третье число ``4`` (``[3, 1, 4]``) -> ``3``, четвертое число ``6`` (``[3, 1, 4, 6]``) -> ``4`` ... 
1. ``groups`` - нужно разложить входящие данные в массивы по группам. Пример:

  Запрос ``ask`` 1
  ```json
    {  
       "group": 1,
       "value": "ue646g8pvi"
    }
  ```
  Запрос ``ask`` 2
  ```json
    {  
       "group": 0,
       "value": "oa2ouyds4i"
    }
  ```
  Запрос ``ask`` 3
  ```json
    {  
       "group": 0,
       "value": "zv2osthuxr"
    }
  ```
  Запрос ``ask`` 4
  ```json
    {  
       "group": 1,
       "value": "czcpjk0529"
    }
  ```
  Запрос ``askComplete``
  
  Ответ:
  ```json
  [  
    [  
      "oa2ouyds4i",
      "zv2osthuxr"
    ],
    [  
      "ue646g8pvi",
      "czcpjk0529"
    ]
  ]
  ```
1. ``recurrence`` - Нужно проверять было ли текущее сообщение среди предыдущих. На вход могут быть вложенные массивы и объекты. Порядок элементов массива не важно. Пример:
 
  Запрос 1:
  ```json
  {
    "oa2ouyds4i": 123
  }
  ```
  Ответ 1: ``false`` - т.к. только один запрос
  
  Запрос 2:
  ```json
  {
    "adjqkklqhyx2l9ycr5zz63l3di" : -57,
    "jwbj1qufbekjejd1hl2bro1or": [123, 432]
  }
  ```
  Ответ 2: ``false`` - т.к. первый и второй запрос отличаются
  
  Запрос 3:
  ```json
  {
    "oa2ouyds4i": 123
  }
  ```
  Ответ 3: ``true`` - т.к. совпадает с первым запросом
  
1. ```validator``` - По заданной схеме валидации нужно проверить входные данные.

    Типы валидации:
    * ``isNumber`` - значение является числом
    * ``isString`` - значение является строкой
    * ``isBoolean`` - значение является булевым типом
    * ``isJSON`` - значение является валидная json строка
    * ``moreN`` - больше числа N
    * ``lessN`` - меньше числа N
    * ``isPrime`` - является простым числом
    * ``longerN`` - длиннее N символов
    * ``shorterN`` - короче N символов
    * ``containSTR`` - содержит подстроку STR
    * ``truly`` - буленовское истиное значение (true)
    * ``false`` - буленовское ложное значение (false)
    
    Сначала приходит схема:
    ```json
    ["isNumber", "more10"]
    ```
    
    Потом приходят данные:
    ```js
    ["adf", [1,20,300], 123, 1, '{"as":1}']
    ```
    Последнее - валидная JSON  строка
    
    Ответ:
    ```json
    [false, false, true, false, false]
    ```

## Если возникли проблемы

При технической неисправности сервера пишите в [обсуждение в ВК](https://vk.com/topic-129114172_34768504)
