# Лабораторная №1 по DevOps

Выполнили:

**Орлова Алёна (К3223)**

**Феофанов Никита (К3222)**

**Зубов Алексей (К3220)**

# Задание

Настроить nginx по заданному тз:
1. Должен работать по https c сертификатом
2. Настроить принудительное перенаправление HTTP-запросов (порт 80) на HTTPS (порт 443) для обеспечения безопасного соединения.
3. Использовать alias для создания псевдонимов путей к файлам или каталогам на сервере.
4. Настроить виртуальные хосты для обслуживания нескольких доменных имен на одном сервере.

# Устанавливаем NGINX

Что вообще такое энджинкс? **NGINX** - это HTTP-сервер, обратный прокси сервер с поддержкой кеширования и балансировки нагрузки, TCP/UDP прокси-сервер, а также почтовый прокси-сервер (много умных слов). Если более кратко - это такое ПО, которое дает возможность создать легкий и мощный веб-сервер, а еще его можно использовать в качестве почтового или обратного прокси-сервера 

Чтобы установить энджинкс пишем вот такой вот список команд:

```
sudo apt update # обновляем списки пакетов из репозиториев
```
![image](https://github.com/user-attachments/assets/c6c73967-c96c-4395-9b2d-2e4a82d323a5)

```
sudo apt install nginx # устанавливаем nginx
```
![image](https://github.com/user-attachments/assets/978d1cca-47f1-4b4f-a5a3-184547f310ce)

```
sudo systemctl status nginx # проверяем статус работы nginx
```
![image](https://github.com/user-attachments/assets/faa9cfce-5353-4041-9099-2083d6ede22b)

И напоследок проверим работу nginx с локального хоста: введем в адресную строку браузера **http://127.0.0.1** и увидим стандратную страницу

![image](https://github.com/user-attachments/assets/61fe23be-9455-4d5f-9851-de3620c461c5)

# Создаем сайты и настраиваем виртуальные хосты

Перед тем как создать виртуальный хост, мы создаем директорию для сайта, где уже будут размещаться все нужные файлы. 

Создаем директории для наших сайтов при помощи команд: 
```
sudo mkdir -p /var/www/domain_one/html
sudo mkdir -p /var/www/domain_two/html
```

Добавляем в них файлы **index.html** с интересным текстом внутри:

![image](https://github.com/user-attachments/assets/f7a1a94e-080f-42ee-b3d8-96c38d565368)

Теперь, перейдя в директорию **/etc/nginx/sites-available**, здесь создаем файлы-конфигурации для созданных нами сайтов.

В каждом из файлов по аналогии мы проделали следующее:

1) В блоке server прописали:

**listen 80** - это значит, что сервер ждет запросы пользователей на порту 80

**server_name domain_one** - то есть теперь веб-сервер будет ожидать запросы, которые адресованы конкретно к этому домену

**location / - root /var/www/...** - это просто путь к фалам нашего сайта, в этой директории энджинкс будет искать файлы для сайта, когда придет запрос

**index index.html** - это название главного html файла, который мы создали выше

**return 301 https://...** - смена с http на https соединение

2) Во втором блоке server просто добавлены еще 2 строки, которые создают безопасное соединение с сервером с помощью ssl

![image](https://github.com/user-attachments/assets/a030912b-d2dd-4c14-a357-c0b68f8a0e08)

![image](https://github.com/user-attachments/assets/f0f26a7a-5d7c-47a9-94bc-48ee2f3f7f21)

В хосты также добавим домены сайтов: перемещаемся в hosts и прописываем там одинаковые айпи сервера - **127.0.0.1** 

![image](https://github.com/user-attachments/assets/1539ad93-71a1-44dc-a4f0-1cdb2845aae0)

# SSL-сертификаты

Выше мы упомянули добавление ссл-сертификата. Мы решили создать самоподписанные с помощью OpenSSL. 

Сначала устанавливаем сервис:
```
sudo apt update
sudo apt install openssl
```

Затем прописываем вот такую команду, в которой основную роль играют парметр **-keyout** - сюда мы сохраняем приватный ключ и **-out -** - сюда сохраняем сертификат:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/ssl/private/domain_one.key \
  -out /etc/ssl/certs/domain_one.crt
```

То же самое мы делаем и для второго сайта.

![image](https://github.com/user-attachments/assets/6623d9dc-ba93-4fd9-b80d-707b7762edac)

Теперь необходимо вернуться в файл-конфигуратор и там дописать пути к нашим ключам и сертификатам:

![image](https://github.com/user-attachments/assets/b8c24173-520c-4d42-8927-b182e48a44b2)

# Проверка работы сервера

Для начала активируем вируальные хосты:
```
sudo ln -s /etc/nginx/sites-available/domain_one.conf /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/domain_two.conf /etc/nginx/sites-enabled/
```

Затем, с помощью команды **sudo nginx -t** проверим конфигурацию:

![image](https://github.com/user-attachments/assets/8dd53817-3aa4-4e6f-bd1f-53103a5e0220)

Ошибок нет, значит можно перезапустить энджинкс с помощью **sudo systemctl reload nginx**

Заходим на наши сайты с помощью **https://domain_one** и **https://domain_two**, и... всё работает!

![image](https://github.com/user-attachments/assets/a356a808-fe8b-48f9-bba8-c609c3c31c63)
![image](https://github.com/user-attachments/assets/e72d1903-67da-4f09-9b79-ffc26d620ad2)

# Alias

Теперь займемся созданием псевдонимов путей к файлам или каталогам на сервере. Создаем дополнительную директорию **newfile** с ещё одним html файлом. Чтобы каждый раз не прописывать полный путь к нему, создаем **alias**. В новом блоке **location** определим сокращение нашего пути в директории с новым файлом.

![image](https://github.com/user-attachments/assets/2d48dc14-5cd0-48a7-aec8-3f9d55377f91)

Всё работает!

![image](https://github.com/user-attachments/assets/0b1a60b7-febe-403f-9c0d-f236bc2a08b1)

![image](https://github.com/user-attachments/assets/260aac4c-aac6-4dba-8ea1-45dd29dd1f45)
