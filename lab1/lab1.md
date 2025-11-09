# Лабораторная работа №1

## Цель:

**Настроить nginx по заданному тз:**

1. Должен работать по https c сертификатом
2. Настроить принудительное перенаправление HTTP-запросов (порт 80) на HTTPS (порт 443) для обеспечения безопасного
   соединения.
3. Использовать alias для создания псевдонимов путей к файлам или каталогам на сервере.
4. Настроить виртуальные хосты для обслуживания нескольких доменных имен на одном сервере.
5. Что угодно еще под требования проекта

**Результат:**

Предположим, что у вас есть два пет проекта на одном сервере, которые должны быть доступны по https. Настроенный вами
веб сервер умеет работать по https, относить нужный запрос к нужному проекту, переопределять пути исходя из требований
пет проектов.
В качестве пет проектов можете использовать что-то свое, можете что-то опенсорсное, можете просто код из трех строчек
как например [здесь](https://dev.to/maxcore/basic-ultimate-guide-nginx-1ngd)

## Ход работы

1. Установил Nginx и OpenSSL и 
запустил

`brew install nginx openssl`

`brew services start nginx`

2. Создал простейшие пет проекты

`mkdir -p ~/Sites/project1 ~/Sites/project2`

`echo "<h1>Project 1 works!</h1>" > ~/Sites/project1/index.html`

`echo "<h1>Project 2 works!</h1>" > ~/Sites/project2/index.html`

3. Настроил hosts через `sudo nano /etc/hosts` добавил

`127.0.0.1 project1.local`

`127.0.0.1 project2.local`


<img width="403" height="181" alt="image" src="https://github.com/user-attachments/assets/a7df2a12-1181-44e2-9244-72331c66f0e2" />

4. Генерация самоподписанных SSL-сертификатов
`mkdir -p /usr/local/etc/nginx/ssl`

`cd /usr/local/etc/nginx/ssl`

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
 -keyout project1.local.key -out project1.local.crt \
 -subj "/CN=project1.local"`

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
 -keyout project2.local.key -out project2.local.crt \
 -subj "/CN=project2.local"`

5. Создал отдельные конфиги

<img width="522" height="351" alt="image" src="https://github.com/user-attachments/assets/e47d6d9c-2665-47b0-8862-92fbb412ac85" />

<img width="522" height="351" alt="image" src="https://github.com/user-attachments/assets/4dc3c6c6-26b7-48c0-b22a-9ecf05e0d948" />


6. Подключил виртуальные хосты к основному nginx.conf

 Для этого в конце nginx.conf добавил данную строчку

 `include servers/*.conf;`

7. Проверка работы

При открытии http://project1.local направляет на https://project1.local

<img width="182" height="36" alt="image" src="https://github.com/user-attachments/assets/cbec317c-de5d-41ad-b0c9-c799d9eb3b54" />
<img width="1916" height="862" alt="image" src="https://github.com/user-attachments/assets/44473e14-a1a4-4e78-b808-d10fd1de68cb" />

Так же при открытии http://project2.local

<img width="179" height="34" alt="image" src="https://github.com/user-attachments/assets/e1841031-0438-4588-8419-759068a8765e" />
<img width="1914" height="869" alt="image" src="https://github.com/user-attachments/assets/1b191501-7b5a-4e6c-bd97-de7dd254f5e1" />

8. Проверка alias

Для проверки alias добавил в ~/Sites/project1/assets/ style.css

При открытии http://project1.local/static/style.css так же перенаправляет на https и выводит содержимое style.css

<img width="247" height="26" alt="image" src="https://github.com/user-attachments/assets/a76e080c-03f9-423a-afca-0eefc774f4d2" />
<img width="1906" height="865" alt="image" src="https://github.com/user-attachments/assets/711539f5-3b7f-42e6-b9ff-fbb6c088306d" />

Если вместо static в поисковой строке написать что-нибудь другое, то выдаст ошибку
Например, https://project1.local/nestatic/style.css
<img width="1906" height="865" alt="image" src="https://github.com/user-attachments/assets/28c5adb8-a0de-4101-8d72-7385df2ae103" />


## Вывод

В ходе лабораторной работы я научился астраивать nginx, работать с https и сертификатами, перенаправлять http на https, создавать alias для файлов и каталогов, настраивать виртуальные хосты, использовать hosts для локальных доменов, создавать и тестировать конфигурации сайтов.


