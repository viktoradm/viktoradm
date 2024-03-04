# Развертывание и настройка интернет-магазина ***24farmacia.ru***

## 1. Требования к железу

**Боевая конфигурация:**

*Сервер* *API*:

- Рекомендуемый объем жесткого диска: 300 Гб
- Объем ОЗУ: 32 Гб
- ЦПУ: 20+ ядер

*Сервер* *front*:

- Рекомендуемый объем жесткого диска: 100 Гб
- Объем ОЗУ: 16 Гб
- ЦПУ: 10-12 ядер

*Сервер БД:*

- Рекомендуемый объем жесткого диска: 200 Гб
- Объем ОЗУ: 32 Гб
- ЦПУ: 10-12 ядер

*Сервер* *elastic search:*

- Рекомендуемый объем жесткого диска: 50 Гб
- Объем ОЗУ: 32 Гб
- ЦПУ: 8 ядер

**Тестовая конфигурация:** 

*Сервер для* *api, front, бд:*

- Рекомендуемый объем жесткого диска: 200 Гб
- Объем ОЗУ: 16 Гб
- ЦПУ: 8 ядер

*Сервер* *elastic search:*

- Рекомендуемый объем жесткого диска: 50 Гб
- Объем ОЗУ: 16 Гб
- ЦПУ: 4 ядер

## 2. Системное программное обеспечение

Рекомендуемые ОС для каждого сервера: *Ubuntu v18+, CentOS 7, Astra Linux*

В целом, любая unix ОС, которая поддерживает установку docker

## 3. Специализированное программное

Для работы системы необходимо установить актуальную версию docker + docker compose (docker compose устанавливается вместе с docker engine).
**Установка docker и docker compose на Centos 7 **
1. Устанавливаем необходимые пакеты
```sudo yum install -y yum-utils device-mapper-persistent-data lvm2```
2. Добавляем репозиторий docker-ce
```sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo```
3. Устанавливаем Docker-CE
```sudo yum install -y docker-ce```
4. Добавляем нашего пользователя, под которым настраиваем ОС, в группу Docker
```sudo usermod -aG docker $(whoami)```
5. Применяем изменения к группам
```newgrp docker```
6. Добавляем сервис в автозагрузку и запускаем его
```sudo systemctl enable --now docker```
7. Добавляем репозиторий EPEL
```sudo yum install -y epel-release```
8.Устанавливаем Python-pip
```sudo yum install -y python-pip python-devel gcc```
```sudo yum install -y python3-pip```
9. Устанавливаем Docker Compose
```sudo pip3 install docker-compose```
10. Делаем симлинк на файл docker-compose
```sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose```
11. Проверяем установку Docker compose(должна появится версия)
```sudo docker-compose version```

## 4. Конфигурирование

1. Для корректной работы сайта необходимо настроить доменное имя, которое будет вести на сервер, где будет развернут сайт.

##RABBITMQ
1. Настроить очередь RMQ для взаимодействия с УАСом.
2. Заходим в веб интерфейс rabbitmq на uas-dev
``` uas-dev.gpkk.local:15672```
3. Создаем Virtual host для ИМ. Переходим в Admin затем в Virtual host. Пишем название виртуального хоста и жмем ОК.
![image](https://github.com/viktoradm/viktoradm/assets/136047592/185231c9-6070-410e-a683-709efdec77a9)

4.  Создаем нужные очереди для ИМ. Переходим в “Queues” листаем в самый низ в разделe  ”add a new queue” создаем очередь с названием “Farmacia”. Выбираем виртуальный хост который мы создали и выбираем “Arguments” “Maximum priority”, выставляем ему значение 10. Нажимаем добавить очередь. Очередь создалась
![image](https://github.com/viktoradm/viktoradm/assets/136047592/5c3fcc1f-b165-4709-b02a-f319927abf51)

6. По аналогии создаем очередь с названием “FarmaciaWarehouse”
7. Переходим во вкладку “Exchanges”. Ищем свой виртуальный хост, который создавали. Нажимаем в столбце “Name” data
![image](https://github.com/viktoradm/viktoradm/assets/136047592/a9971915-2a04-4769-b1ce-508748f5e907)
8. Переходим к разделу “Add binding from this exchange”
![image](https://github.com/viktoradm/viktoradm/assets/136047592/38c7daef-ab54-4f6b-a67e-8955eb5cb6bf)
10. Поочередно создаем ключи для очереди  Farmacia:

```
#.ARM.Справочник.Пользователи.#
#.Farmacia.#
#.checkout.#
#.cheсkout.#
#.xml.РегистрСведений.га_УсловияДоставкиРегионовИМ.#
#.Документ.ЗаказКлиента.#
#.Документ.га_СпецификацияГосконтракта.#
#.Документ.га_ЭкспедиционныйЛист.#
#.РегистрНакопления.ТоварыКОтгрузке.#
#.РегистрНакопления.ТоварыНаСкладах.#
#.РегистрНакопления.га_КонтрольРеализацииМолочнойКухни.#
#.РегистрСведений.НоменклатураСегмента.#
#.РегистрСведений.СубъектыОбращенияМДЛП.#
#.РегистрСведений.УАС_РеглСпискиНоменклатуры.#
#.РегистрСведений.ЦеныНоменклатуры.#
#.РегистрСведений.ШтрихкодыНоменклатуры.#
#.РегистрСведений.га_АналогиНоменклатурыПотребностиСлужебный.#
#.РегистрСведений.га_ДополнительныеРеквизитыОписанияНоменклатуры.#
#.РегистрСведений.га_ЗначенияПараметровФильтрации.#
#.РегистрСведений.га_ИзображенияНоменклатуры.#
#.РегистрСведений.га_ИзъятаяИзОборотаНоменклатура.#
#.РегистрСведений.га_КонтрольРеализацииМолочнойКухни.#
#.РегистрСведений.га_НаправленияПодразделений.#
#.РегистрСведений.га_НоменклатураКатегорийЗапроса.#
#.РегистрСведений.га_ПараметрыФильтрацииСегментов.#
#.РегистрСведений.га_ПолучателиМолочнойКухни.#
#.РегистрСведений.га_ПунктыВыдачиПолучателейМолочнойКухни.#
#.РегистрСведений.га_СвязкиПотребительскихКатегорий.#
#.РегистрСведений.га_СкладыККМ.#
#.РегистрСведений.га_СоответствиеНоменклатурыСпецификаций.#
#.РегистрСведений.га_СтатусыЗаказКлиентаВИнтернетМагазине.#
#.РегистрСведений.га_СтатусыУпаковокМДЛП.#
#.РегистрСведений.га_ЦеныУслуг.#
#.Сайт.ТоварыНаСкладах.#
#.Справочник.ВидыНоменклатуры.#
#.Справочник.ВидыЦен.#
#.Справочник.КассыККМ.#
#.Справочник.Марки.#
#.Справочник.МестаДеятельностиМДЛП.#
#.Справочник.Номенклатура.#
#.Справочник.Партнеры.#
#.Справочник.Производители.#
#.Справочник.СегментыНоменклатуры.#
#.Справочник.Склады.#
#.Справочник.СтруктураПредприятия.#
#.Справочник.УАС_КАТАТХ.#
#.Справочник.УАС_КАТДействующиеВеществаМНН.#
#.Справочник.УАС_КАТСтраны.#
#.Справочник.УАС_КАТТорговыеМарки.#
#.Справочник.УАС_КАТФормыВыпуска.#
#.Справочник.УАС_КАТХарактеристики.#
#.Справочник.УпаковкиЕдиницыИзмерения.#
#.Справочник.ФизическиеЛица.#
#.Справочник.адаптер_СхемыДанных.#
#.Справочник.га_АналогиНоменклатуры.#
#.Справочник.га_Госконтракты.#
#.Справочник.га_Должности.#
#.Справочник.га_ИнтервалыДоставки.#
#.Справочник.га_ИнтервалыСамовывоза.#
#.Справочник.га_КаталогиТСР.#
#.Справочник.га_КатегорииЗапроса.#
#.Справочник.га_МотивационныеПрограммы.#
#.Справочник.га_НаправленияИнтернетМагазинов.#
#.Справочник.га_НоменклатураСпецификаций.#
#.Справочник.га_ПараметрыФильтрации.#
#.Справочник.га_ПотребительскиеКатегории.#
#.Справочник.га_РегионыИнтернетМагазина.#
#.Справочник.га_РозничныеПокупатели.#
#.Справочник.га_ЦелевыеПрограммы.#
```

11. Также настроить очередь FarmaciaWarehouse с ключами

```
#.РегистрНакопления.ТоварыКОтгрузке.#
#.РегистрНакопления.ТоварыНаСкладах.#
#.РегистрСведений.ЦеныНоменклатуры.#
#.РегистрСведений.га_НаправленияПодразделений.#
#.Справочник.ВидыЦен.#
#.Справочник.СерииНоменклатуры.#
#.Справочник.Склады.#
#.Справочник.СтруктураПредприятия.#
#.Справочник.га_ВидыСкладов.#
#.Справочник.га_НаправленияИнтернетМагазинов.#
#.Справочник.га_РегионыИнтернетМагазина.#
```

## 5. Установка ИМ на одной виртуалке

### Backend

1. Создать папку farmacia по пути /home  
 ```cd /home && sudo mkdir farmacia```

1. Копируем архив с файлами для ИМ на виртуалку. Архив лежит по пути 
```\\dit\Shara\- АДМИНЫ\24farmacia```

2. Распаковываем архив в папку /home/farmacia
3. Открываем по очереди файлы и вносим изменения согласно комментариям в файлах :
- .env
- .env.warehouse
- .env_front
  
4. Запускаем контейнер БД командой(нужно находиться в папке /home/farmacia)
```docker compose up db```
5. Для доступа к образам приложений с сервера необходимо выполнить ```docker login``` с указанием deploy token, который нужно создать в гитлабе для вашего окружения. Данные действия просить сделать разработчиков. Но это можно и не делать, т.к. есть уже созданные токены для доступа, созданы для Коноваленко Виктора(в кипас). После создания токена выполняем
```docker login -u <deploy_token_login> -p <deploy_token_password> https://registry.gitlab.com/24farmacia```
6. Запускаем API командой
 ```docker compose up api```

### Сервис остатков

1. Для сервер остатков используется файл *.env.warehouse*. Проверить его если не заполняли 
2. Создать базу данных farm-warehouse в СУБД/

```docker exec -it farmacia-db-1 psql -U farm -c 'create database "farm-warehouse";'```

Логинимся на репозитории используя токены 
```docker login -u <deploy_token_login> -p <deploy_token_password> https://registry.gitlab.com/24farmacia/farmacia-warehouse```

1. Затем, запускаем API сервиса остатков ```docker compose up warehouse-api```

1. Убедиться, что база farm-warehouse заполнилась таблицами из миграций

### Front

1. Переходим в папку */home/farmacia* ```cd /home/farmacia```

1. Создаем папку nginx и файл nginx.conf внутри со следующим содержимым  

```nginx
map $sent_http_content_type $expires {
    "text/html" epoch;
    "text/html; charset=utf-8"  epoch;
    default off;
}

server {
    resolver 127.0.0.11;
    set $api_url http://api:9997;
    set $warehouse_api_url http://warehouse-api:9995;
    listen 80; # Порт который слушает nginx
    server_name preprod.24farmacia.ru; # домен или ip сервера(нужно поставить свой)
    client_max_body_size 50M;

    add_header X-Robots-Tag "noindex, nofollow" always; #отключение индексации

    location ~ /.well-known/acme-challenge {
              allow all;
              auth_basic off;
              root /var/www/html;
            }

    location ^~ /api {
        proxy_connect_timeout 600;
        proxy_send_timeout 600;
        proxy_read_timeout 600;
        send_timeout 600;
        proxy_pass $api_url;
    }

    location / {
                    rewrite ^ https://$host$request_uri? permanent;
            }
}

server {
        resolver 127.0.0.11;
        set $api_url http://api:9997;
        set $warehouse_api_url http://warehouse-api:9995;
        listen 443 ssl http2;
        listen [::]:443 ssl http2;
        server_name preprod.24farmacia.ru; ##Нужно написать свое доменное имя
        client_max_body_size 50M;

        server_tokens off;

        ssl_certificate /etc/letsencrypt/live/preprod.24farmacia.ru/fullchain.pem;    ###используется эти сертификаты если выпускаем сертификаты через letsenscypt
        ssl_certificate_key /etc/letsencrypt/live/preprod.24farmacia.ru/privkey.pem;   ###используется эти сертификаты если выпускаем сертификаты через letsenscypt
#        ssl_certificate /etc/nginx/certs/gpkk_2024.crt;  ###используется эти сертификаты если используем сертификат gpkk.ru
#        ssl_certificate_key /etc/nginx/certs/gpkk_2024.key;   ###используется эти сертификаты если используем сертификат gpkk.ru

        ssl_buffer_size 8k;

        ssl_protocols TLSv1.2 TLSv1.1 TLSv1;
        ssl_prefer_server_ciphers on;

        ssl_ciphers ECDH+AESGCM:ECDH+AES256:ECDH+AES128:DH+3DES:!ADH:!AECDH:!MD5;

        ssl_ecdh_curve secp384r1;
        ssl_session_tickets off;

        ssl_stapling on;
        ssl_stapling_verify on;

        add_header X-Robots-Tag "noindex, nofollow" always; #отключение индексации

        if ($request_uri ~ "^[^?]*?//") {
            rewrite "^" $scheme://$host$uri permanent;
        }

        if ($request_uri ~* "^(.*/)index\.php$") {
                return 301 $1;
        }

        location /logs {
            alias /var/log/nginx;
            autoindex on;
        }

        location /tls/ {
            root /var/www/html;
            try_files $uri / =301;
        }

        location ~* .yml$ {
            proxy_pass $api_url/static/feeds/$request_uri;
        }

        location /robots.txt {
            expires $expires;
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto  $scheme;
            proxy_read_timeout 3m;
            proxy_connect_timeout 3m;
            proxy_send_timeout 600;
            send_timeout 600;
            # Адрес нашего приложения, так как контейнеры связаны при помощи
            # docker-compose мы можем обращаться к ним по имени контейнера, в данном случае nuxt_app
            proxy_pass http://nuxt_app:3000;
        }

        location ^~ /sitemap {
                    expires $expires;
                    proxy_redirect off;
                    proxy_set_header Host $host;
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header X-Forwarded-Proto  $scheme;
                    proxy_read_timeout 3m;
                    proxy_connect_timeout 3m;
                    proxy_send_timeout 600;
                    send_timeout 600;
                    # Адрес нашего приложения, так как контейнеры связаны при помощи
                    # docker-compose мы можем обращаться к ним по имени контейнера, в данном случае nuxt_app
                    proxy_pass http://nuxt_app:3000;
                }

        location / {
            auth_basic "Доступ к сайту";
            auth_basic_user_file /etc/nginx/conf.d/.htpasswd;
            expires $expires;
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto  $scheme;
            proxy_read_timeout 3m;
            proxy_connect_timeout 3m;
            proxy_send_timeout 600;
            send_timeout 600;
            # Адрес нашего приложения, так как контейнеры связаны при помощи
            # docker-compose мы можем обращаться к ним по имени контейнера, в данном случае nuxt_app
            proxy_pass http://nuxt_app:3000;
        }

        location /warehouse-api/ {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_connect_timeout 600;
            proxy_send_timeout 600;
            proxy_read_timeout 600;
            send_timeout 600;
            proxy_pass http://warehouse-api:9995/api/;
        }

        location /warehouse/api/ {
            proxy_connect_timeout 600;
            proxy_send_timeout 600;
            proxy_read_timeout 600;
            send_timeout 600;
            proxy_pass http://warehouse-api:9995/api/;
        }

        location ^~ /api {
            proxy_connect_timeout 600;
            proxy_send_timeout 600;
            proxy_read_timeout 600;
            send_timeout 600;
            proxy_pass $api_url;
        }

        location ^~ /static {
            proxy_pass $api_url;
        }


        location /admin/ {
            proxy_pass http://admin:8081/;
        }

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;
}
```

1. Поле **server_name** необходимо заполнить в соответствии с доменом, для которого настраивается окружение (*preprod.24farmacia.ru* в случае препрода). Рекомендуется сделать в файле поиск по строке *preprod.24farmacia.ru* и выполнить замену на нужный домен

Создаем файл .env_front со след содержимым
```
NODE_TLS_REJECT_UNAUTHORIZED=0
NUXT_ENV_DEBUG_API=false
NUXT_ENV_DOMAIN=24farmacia-dev2.gpkk.ru #preprod.24farmacia.ru  ##Заполняем свой домен
NUXT_ENV_API_URL=https://admin.24farmacia.ru/api_new/
NUXT_ENV_API_URL_BROWSER=https://admin.24farmacia.ru/api_new/
NUXT_ENV_API_URL_SF=http://172.17.59.29:9997/api #http://api:9997/api   ###Указываем ip адрес сервера где крутиться api(тот же ip где делаем все действия)
NUXT_ENV_API_BROWSER_URL_SF=https://24farmacia-dev2.gpkk.ru/api  #заполняем url со свои доменом
NUXT_ENV_ORDER_DELIVERY_ID=1
NUXT_ENV_ORDER_PICKUP_ID=2
NUXT_ENV_REGION_CODE_KRASNOYARSK=krasnoyarsk
NUXT_ENV_REGION_CODE_KK=kk
NUXT_ENV_REGION_CODE_NORILSK=norilsk
NUXT_ENV_REGION_CODE_HAKASYA=hakasya
NUXT_ENV_VUEX_RAW_ERROR=true
NUXT_ENV_SENTRY_DNS=
NUXT_ENV_SENTRY_PUBLISH_RELEASE=true
NODE_ENV_COMAGIC_TOKEN=
NODE_ENV_YANDEX_METRIKA_TOKEN=
NODE_ENV_GOOGLE_ANALYSTICS_TOKEN=
NUXT_ENV_SECTION_CODE_VETERINARIY=veterinariy
NODE_ENV_ONLINE_CHAT_TOKEN=
NODE_ENV_INSERT_ONLINE_CHAT=false
NODE_ENV_INSERT_LIVETEX_CHAT=false
NODE_ENV_INSERT_APPLE_APP_SITE_ASSOCIATION=false
NODE_ENV_GOOGLE_TAG_MANAGER_ID=
NODE_ENV_SESSION_LIFETIME=300000
NUXT_ENV_RECAPTCHA_KEY=6LeTwHcpAAAAAHSY3zCI2RgVrzC0rWwwC6938-Pl   ###Заполняем recaptch публичного ключа/ это выдают разрабы 
```

1. Нужно создать пароль для доступа к сайту, если окружение не должно быть доступно всем пользователям (стейдж, прерпод...). Для этого нужно установить утилиту *apache2-utils* и вызвать команду ```sudo htpasswd -c /home/farmacia/nginx/.htpasswd <username>```. Далее утилита попросит ввести пароль для пользователя и информация будет сохранена в файл .htpasswd

 Логинимся на репозитории используя токены 
```docker login -u <deploy_token_login> -p <deploy_token_password> https://registry.gitlab.com/n.gankin/farm-admin```
Запускаем контейнер 
``` docker compose up admin -d```
 Логинимся на репозитории используя токены 
```docker login -u <deploy_token_login> -p <deploy_token_password> https://registry.gitlab.com/n.gankin/farm-front```
Запускаем контейнер
``` docker compose up nuxt -d```

1. Запускаем фронт админки и самого сайта командой ```docker compose up admin nuxt```. Если появляется ошибка доступа к хранилищу образов, то необходимо создать deploy token для репозиториев админки и фронта соответственно с ранее указанной инструкцией и произвести вход.

### Запуск nginx
1. Создаем каталог на сервере
``` mkdir /var/www/html```
2. Запускаем контейнер nginx
``` docker compose up nginx -d ```



### Получение сертификатов(Делаем жтот пункт только если выпускаем сертификат через letsenscrypt)

1. Создаем файл init-certbot.sh с содержимым:

```bash
#!/bin/bash

domains=(preprod.24farmacia.ru) # заменить на нужные домены через пробел (preprod.24farmacia.ru sklad.24farmaica.ru) и т.д.
rsa_key_size=4096
data_path="./data/certbot"
email="it@gpkk.ru" # Adding a valid address is strongly recommended
staging=0 # Set to 1 if you're testing your setup to avoid hitting request limits

if [ -d "$data_path" ]; then
  read -p "Existing data found for $domains. Continue and replace existing certificate? (y/N) " decision
  if [ "$decision" != "Y" ] && [ "$decision" != "y" ]; then
    exit
  fi
fi


if [ ! -e "$data_path/conf/options-ssl-nginx.conf" ] || [ ! -e "$data_path/conf/ssl-dhparams.pem" ]; then
  echo "### Downloading recommended TLS parameters ..."
  mkdir -p "$data_path/conf"
  curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot-nginx/certbot_nginx/_internal/tls_configs/options-ssl-nginx.conf > "$data_path/conf/options-ssl-nginx.conf"
  curl -s https://raw.githubusercontent.com/certbot/certbot/master/certbot/certbot/ssl-dhparams.pem > "$data_path/conf/ssl-dhparams.pem"
  echo
fi

echo "### Creating dummy certificate for $domains ..."
path="$data_path/conf/live/$domains"
mkdir -p "$path"
openssl req -x509 -nodes -newkey rsa:$rsa_key_size -days 1\
  -keyout "$path/privkey.pem" \
  -out "$path/fullchain.pem" \
  -subj '/CN=localhost'
echo


echo "### Starting nginx ..."
docker compose up --force-recreate -d nginx
echo

echo "### Deleting dummy certificate for $domains ..."
docker compose run --rm --entrypoint "\
  rm -Rf /etc/letsencrypt/live/$domains && \
  rm -Rf /etc/letsencrypt/archive/$domains && \
  rm -Rf /etc/letsencrypt/renewal/$domains.conf" certbot
echo


echo "### Requesting Let's Encrypt certificate for $domains ..."
#Join $domains to -d args
domain_args=""
for domain in "${domains[@]}"; do
  domain_args="$domain_args -d $domain"
done

# Select appropriate email arg
case "$email" in
  "") email_arg="--register-unsafely-without-email" ;;
  *) email_arg="--email $email" ;;
esac

# Enable staging mode if needed
if [ $staging != "0" ]; then staging_arg="--staging"; fi

docker compose run --rm --entrypoint "\
  certbot certonly --webroot -w /var/www/html \
    $staging_arg \
    $email_arg \
    $domain_args \
    --rsa-key-size $rsa_key_size \
    --agree-tos \
    --force-renewal" certbot
echo

echo "### Reloading nginx ..."
docker compose exec nginx nginx -s reload
```

1. Переменную *staging* в начале файла лучше приравнять 1 для первых попыток получения сертификата, иначе можно получить блокировку на 30 минут при 5 неудачных попытках.

1. Рекомендуется проверить, закомментировано ли поле *entrypoint*

```yaml
 entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"
```

в *docker-compose.yaml*, чтобы избежать ошибок при первом запуске

1. Выполняем команду ```sudo chmod +x ./init-certbot.sh```, чтобы сделать файл исполняемым

1. Запсукаем файл командой ```./init-certbot.sh``` и ждем окончания генерации сертификатов. При появлении ошибок рекомендуется обратиться к документации let's encrypt.

1. При успешном запуске, изменяем значение переменной staging на =0 в файле *init-certbot.sh*

1. Повторяем шаг с запуском файла и ждем результата. После успешного запроса сертификатов, nginx перезапустится и можно будет перейти по ссылке домена и проверить сертификат через браузер.





### Rabbit и прочее

1. Для запуска контейнеров обработчиков очередей RMQ и Redis необходимо выполнить команду ```docker compose up```, которая запустит все конейнеры, которые указаны в docker-compose.yaml файле. Или же можно перечислить сервисы вручную ```docker compose up redis watchtower jobs rabbit rabbit-int rabbit-self rabbit-sender rabbit-server warehouse-rabbit warehouse-rabbit-self warehouse-rabbit-sender```
