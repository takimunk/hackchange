# hackchange
Source code for 2021.11.26 -2021.11.28 Hack&amp;Change hackathon

Для запуска окружения необходимо
1. Собрать образ Jupyter Notebook из папки hackchange-notebook (aerospike connector работает только со spark < 3.1, поэтому нельзя использовать готовый image docker-stacks)
2. С помощью docker-compose.yml развернуть окружение, включающее в себя 4 контейнера:
 * kafka
 * zookeeper
 * jupyter
 * aerospike
3. Запустить сбор витрин и их загрузку в aerospike  notebooks/batch_data_marts.ipynb
4. Запустить producer и consumer для кафки 
В файлах producer.ipynb и consumer.ipynb написаны этапы работы с топиком KAFKA. Producer наполняет topic данными, взятыми из баз ивентов (событий) mobile_client и web_client, в них содержатся id пользователей и отметки времени, когда пользователь использовал web-приложение и (или) mobile-приложение. Затем consumer обращается к данным в топике и обрабатывает их по следующей логике: 
 1. Если пользователь воспользовался mobile-приложением - то с ним будет происходить свзять по каналу "push".
 2. Если web-приложение - то будет использован "sms" канал связи.
 3. Если после использования web-приложения в течение 5 минут абонент воспользовался mobile-приложением, то используется канал "push".

В процессе работы алгоритма изменяются ключи и значения внутри каждого сообщения об использовании приложений, добавляются ключ-значение "chanel": "sms" либо "chanel": "push" и удаляются ключи-значения "client": "mobile" и "client": "web". Далее осуществляется обогащение данных сообщений информацией об использовании интернет-трафика и времени разговора в секундах абонента в течение периода времени. После этих операций готовые сообщения направляются в топик KAFKA "out_cm"
