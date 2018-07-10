=======
API для экспорта данных
=======

Для менеджеров и супервайзеров компании доступен специально разработанный API, позволяющий
экспортировать собранные данные для собственных нужд в формате JSON. 

Авторизация
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Чтобы совершать запросы к API, необходимо получить токен.

+--------+-------------------------------------------+
|POST    | https://Heedbook.com/account/generatetoken|
+--------+-------------------------------------------+
|username| адрес электронной почты пользователя      |
+--------+-------------------------------------------+
|pswd    | пароль                                    |
+--------+-------------------------------------------+


Если авторизация пройдет успешно, пользователю будет доступен собственный уникальный идентификатор и подписанный JWT токен. 
Эти данные будут необходимы для непосредственного экспорта данных.

Экспорт
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Получив сгенерированный токен, пользователь может выгрузить данные:

+-------------+--------------------------------------------------------------------------------+
|GET/POST     | https://hb-api-cons-csharp.azurewebsites.net/api/Data_Http_Export?PARAMETERS   |
+-------------+--------------------------------------------------------------------------------+
|Authorization| JWT токен                                                                      |
+-------------+--------------------------------------------------------------------------------+
|UserID       | ID пользователя в системе                                                      |
+-------------+--------------------------------------------------------------------------------+

Пользователю доступны опциональные параметры экспорта, вписываемые в строку запроса:

* begTime, endTime

  дата в формате yyyMMdd, дни за которые необходимо получить данные.
* sas=true   

  дополнить диалоги временными ссылками на ресурсные файлы (аватар клиента, видеозапись диалога клиента и сотрудника)
* words=true   

  дополнить данные повременной разметкой слов фигурировавших в диалоге
 
 
 
.. code-block:: JSON

  {
    "Id": "c6d2a92f-a81d-4c31-ac4d-02c7aa2082c3",
    "Employee": {
      "FullName": "Firstname Secondname",
      "Email": "mail@example.com",
      "EmployeeId": "f29a2d6c-di8a-13c4-d4ca-3c2802aa7c20",
      "WorkerTypeName": "Employee"
    },
    "BegTime": "2018-04-19T09:28:47",
    "EndTime": "2018-04-19T09:37:14",
    "InStatistic": true,
    "Satisfaction": 94.5,
    "Language": "ru-RU",
    "Hints": ["будьте внимательнее к пожеланиям клиента"],
    "Client": {
      "Age": 30.475,
      "Gender": "male"
    },
    "Visuals": [
      {
        "Attention": 100,
        "Contempt": 0.4552209675312042,
        "Disgust": 0.03817020356655121,
        "Fear": 0.039861049503088,
        "Happiness": 0.6173103451728821,
        "Neutral": 97.114013671875,
        "Sadness": 0.5519139766693115,
        "Surprise": 1.0521512031555176
      }
    ],
    "Audio": [
      {
        "NegativeTone": 18.223333333333336,
        "NeutralityTone": 21.630000000000003,
        "PositiveTone": 10.150000000000002
      }
    ],
    "Speeches": [
      {
        "PositiveShare": 0.5552486181259155,
        "SilenceShare": 8.890030832476882,
        "SpeechSpeed": 6.0631697687535295
      }
    ],
    "Phrases": [
      {
        "PhraseText": "Вообще",
        "IsClient": true
      },
      {
        "PhraseText": "Вот",
        "IsClient": false
      }
    ],
    "Words": [
      {
        "BegTime": "2018-06-19T10:47:05.9",
        "EndTime": "2018-06-19T10:47:26.8",
        "Word": "Здраствуйте",
        "IsClient": true
      },
      {
        "BegTime": "2018-06-19T10:47:33.8",
        "EndTime": "2018-06-19T10:47:41.3",
        "Word": "До свидания",
        "IsClient": false
      }
    ],
    "Video": "https://x.y.z/video.webm?sas_token",
    "Avatar": "https://x.y.z/avatar.jpg?sas_token"
  }


1. Id - уникальный идентификатор диалога

2. Employee - информация о сотруднике, который вел диалог

2. a. FullName - полное имя сотрудника

2. b. Email - почтовый адрес, на который зарегистрирован аккаунт сотрудника

2. c. EmployeeId - уникальный ID сотрудника

2. d. WorkerTypeName - тип работника

3. BegTime - время начала диалога

4. EndTime - время конца диалога

5. InStatistic - включен ли данный диалог в статистику

6. Satisfaction - удовлетворенность клиента

7. Language - язык на котором велся диалог

8. Hints - подсказки, выданные сотруднику.

8. Client - информация о клиенте

8. a. Age - оценка возраста клиента

8. b. Gender - пол клиента

9. Visuals - оценка эмоций клиента, полученная из визуальных данных

9. a. Attention - степень внимания клиента

9. b. Contempt - индикатор презрения

9. c. Disgust - индикатор отвращения

9. d. Fear - индикатор страха

9. e. Happiness - индикатор счастья

9. f. Neutral - индикатор нейстальной эмоции

9. g. Surprise - индикатор удивления

10. Audio - оценка эмоций клиента, полученная из аудио-данных

10. a. NegativeTone - степень негатива

10. b. NeutralTone - степень нейтрального тона

10. c. PositiveTone - степень положительного тона

11. Speeches - речевая статистика

11. a. PositiveShare - доля положительных эмоций в речи

11. b. SilenceShare - доля молчания

11. c. SpeechSpeed - скорость диалога

12. Phrases - распознанные ключевые фразы из диалога

12. a. PhraseText - распознанная фраза

12. b. IsClient - было ли оно произнесено клиентом или сотрудником

13. Words - распознанные слова из диалога

13. a. BegTime - начало произношения

13. b. EndTime - конец произношения

13. c. Word - распознанное слово

13. d. IsClient - было ли оно произнесено клиентом или сотрудником

14. Video - ссылка на .webm / .mkv файл с записью диалога

15. Avatar - ссылка на .jpg файл с аватаром клиента