# hackchange
Source code for 2021.11.26 -2021.11.28 Hack&amp;Change hackathon

В файлах producer.ipynb и consumer.ipynb написаны этапы работы с топиком KAFKA. Producer наполняет topic данными, взятыми из баз ивентов (событий) mobile_client и web_client, в них содержатся id пользователей и отметки времени, когда пользователь использовал web-приложение и (или) mobile-приложение. Затем consumer обращается к данным в топике и обрабатывает их по следующей логике: 
 1. Если пользователь воспользовался mobile-приложением - то с ним будет происходить свзять по каналу "push".
 2. Если web-приложение - то будет использован "sms" канал связи.
 3. Если после использования web-приложения в течение 5 минут абонент воспользовался mobile-приложением, то используется канал "push".
