# Развертывание и настройка интернет-магазина ***24farmacia.ru***

## 1. Требования к железу

**Боевая конфигурация:**

*Сервер* *API*:

Рекомендуемый объем жесткого диска: 300 Гб

Объем ОЗУ: 32 Гб

ЦПУ: 20+ ядер

*Сервер* *front*:

Рекомендуемый объем жесткого диска: 100 Гб

Объем ОЗУ: 16 Гб

ЦПУ: 10-12 ядер

*Сервер БД:*

Рекомендуемый объем жесткого диска: 200 Гб

Объем ОЗУ: 32 Гб

ЦПУ: 10-12 ядер

*Сервер* *elastic search:*

Рекомендуемый объем жесткого диска: 50 Гб

Объем ОЗУ: 32 Гб

ЦПУ: 8 ядер

Тестовая конфигурация:

*Сервер для* *api, front, бд:*

Рекомендуемый объем жесткого диска: 200 Гб

Объем ОЗУ: 16 Гб

ЦПУ: 8 ядер

*Сервер* *elastic search:*

Рекомендуемый объем жесткого диска: 50 Гб

Объем ОЗУ: 16 Гб

ЦПУ: 4 ядер

## 2. Системное программное обеспечение

Рекомендуемые ОС для каждого сервера: *Ubuntu v18+, CentOS 7, Astra Linux*

В целом, любая unix ОС, которая поддерживает установку docker

## 3. Специализированное программное

Для работы системы необходимо установить актуальную версию docker + docker compose (docker compose устанавливается вместе с docker engine).

## 4. Конфигурирование

1. Для корректной работы сайта необходимо настроить доменное имя, которое будет вести на сервер, где будет развернут сайт.

1. Настроить очередь RMQ для взаимодействия с УАСом. Очередь назвать Farmacia и добавить следующие ключи маршрутизации, если их нет:

```text
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

1. Также настроить очередь FarmaciaWarehouse с ключами

```text
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

## 5. Установка

### Backend

1. Создать папку farmacia по пути /home  
 ```cd /home sudo mkdir farmacia```

1. Создать в корне директории docker-compose.yml файл следующего содержания  

```yaml
services:
  #   Первый сервис, nginx
  nginx:
    image: nginx:latest
    # Пробрасываем порты 80 для http и 443 для https
    ports:
      - "80:80"
      - "443:443"
    # Опциональный параметр с именем контейнера
    container_name: proxy_nginx
    restart: always
    depends_on:
      - nuxt
      - admin
    volumes:
      # Используем свой nginx конфиг, он заменит дефолтный в контейнере
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf
      - ./nginx/.htpasswd:/etc/nginx/conf.d/.htpasswd
      # Монтируем папку с логами на хост машину для более удобного доступа
      - ./logs:/var/log/nginx/
      - ./data/certbot/conf:/etc/letsencrypt
      - certbot-var:/var/lib/letsencrypt
      - web-root:/var/www/html

  db:
    image: postgres:15.0
    env_file:
      .env
    volumes:
      - db_data:/var/lib/postgresql/data/
      - "/etc/localtime:/etc/localtime:ro"
    ports:
      - "5432:5432"
    restart: always

  # Второй сервис Nuxt.js приложение
  nuxt:
    # Используем ранее собранный образ
    image: registry.gitlab.com/n.gankin/farm-front:pre-prod
    container_name: nuxt_app
    restart: always
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
    # Также пробрасываем порт на котором висит приложение
    ports:
      - "3000:3000"
  api:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - migrations
    ports:
      - "9997"
    volumes:
      - ${LOG_DIR}/api:/opt/app/logs
      - "/etc/localtime:/etc/localtime:ro"
      - /home/fabrika/farmacia/public:/opt/app/public
      - /home/fabrika/farmacia/files:/opt/app/files
    restart: always
    command: "npm run api"
    env_file:
      - .env
    deploy:
      replicas: 1
    labels:
      - "com.centurylinklabs.watchtower.enable=true"
      - "com.centurylinklabs.watchtower.lifecycle.post-update=/home/fabrika/farmacia/after-create.sh"

  migrations:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    depends_on:
      - db
    restart: on-failure
    env_file:
      - .env
    command: "npm run migrate:docker"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  seeders:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    depends_on:
      - db
    restart: on-failure
    env_file:
      - .env
    command: "npm run seed:docker"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  rabbit:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/rabbit:/opt/app/logs
    command: "npm run rabbit"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  rabbit-sender:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/rabbit-sender:/opt/app/logs
    command: "npm run rabbit-sender"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  rabbit-self:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/rabbit-self:/opt/app/logs
    command: "npm run rabbit-self"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  rabbit-int:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/rabbit-int:/opt/app/logs
    command: "npm run rabbit-int:prod"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  jobs:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    depends_on:
      - redis
    restart: always
    env_file:
      - .env
    ports:
      - "3997:3997"
    volumes:
      - ${LOG_DIR}/jobs:/opt/app/logs
    command: "npm run job-queue"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  certbot:
    image: certbot/certbot
    container_name: certbot
    restart: always
    volumes:
      - ./data/certbot/conf:/etc/letsencrypt
      - certbot-var:/var/lib/letsencrypt
      - web-root:/var/www/html
    depends_on:
      - nginx
    #entrypoint: "/bin/sh -c 'trap exit TERM; while :; do certbot renew; sleep 12h & wait $${!}; done;'"

  admin:
    image: registry.gitlab.com/n.gankin/farm-admin:pre-prod
    container_name: admin
    restart: always
    env_file:
      - .env
    ports:
      - "8080:8080"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  redis:
    image: bitnami/redis:latest
    restart: always
    environment:
      - REDIS_PASSWORD=${REDIS_PASSWORD}
    ports:
      - "6379:6379"

  recipe-checker:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/recipe-checker:/opt/app/logs
    command: "npm run recipe-checker"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  cart-checker:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/cart-checker:/opt/app/logs
    command: "npm run cart-checker"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  upload-leftovers-to-aggregator:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/upload-leftovers-to-aggregator:/opt/app/logs
    command: "npm run upload-leftovers-to-aggregator"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  megapteka-exchange:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/megapteka-exchange:/opt/app/logs
    command: "npm run megapteka-exchange"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  watchtower:
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ~/.docker/config.json:/config.json
    environment:
    #   - WATCHTOWER_NOTIFICATION_URL=telegram://<token>@telegram/?channels=<channel_id>
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_POLL_INTERVAL=30
      - WATCHTOWER_INCLUDE_STOPPED=true
      - WATCHTOWER_REVIVE_STOPPED=true
      - WATCHTOWER_LIFECYCLE_HOOKS=true
      - WATCHTOWER_LABEL_ENABLE=true

  feeds:
    image: registry.gitlab.com/n.gankin/farmacia-backend:pre-prod
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - migrations
    restart: always
    env_file:
      - .env
    volumes:
      - ${LOG_DIR}/feeds:/opt/app/logs
      - /home/fabrika/farmacia/public:/opt/app/public
    command: "npm run feeds-generate"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  warehouse-api:
    image: registry.gitlab.com/24farmacia/farmacia-warehouse:pre-prod
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    ports:
      - '9995:9995'
    environment:
      - NODE_ENV=production
    env_file:
      - .env.warehouse
    volumes:
      - ${LOG_DIR}/warehouse/api:/opt/app/logs
    command: "npm run api:prod"
    depends_on:
      - warehouse-migration
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  warehouse-migration:
    image: registry.gitlab.com/24farmacia/farmacia-warehouse:pre-prod
    restart: on-failure
    environment:
      - NODE_ENV=production
    env_file:
      - .env.warehouse
    volumes:
      - ${LOG_DIR}/warehouse/migration:/opt/app/logs
    command: "npm run migrate"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  warehouse-rabbit:
    image: registry.gitlab.com/24farmacia/farmacia-warehouse:pre-prod
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - warehouse-migration
    env_file:
      - .env.warehouse
    environment:
      - NODE_ENV=production
    volumes:
      - ${LOG_DIR}/warehouse/rabbit:/opt/app/logs
    command: "npm run rabbit:prod"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  warehouse-rabbit-self:
    image: registry.gitlab.com/24farmacia/farmacia-warehouse:pre-prod
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    depends_on:
      - warehouse-migration
    environment:
      - NODE_ENV=production
    env_file:
      - .env.warehouse
    volumes:
      - ${LOG_DIR}/warehouse/rabbit-self:/opt/app/logs
    command: "npm run rabbit-self:prod"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  warehouse-rabbit-sender:
    image: registry.gitlab.com/24farmacia/farmacia-warehouse:pre-prod
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "1m"
    depends_on:
      - warehouse-migration
    environment:
      - NODE_ENV=production
    env_file:
      - .env.warehouse
    volumes:
      - ${LOG_DIR}/warehouse/rabbit-sender:/opt/app/logs
    command: "npm run rabbit-sender:prod"
    labels:
      - "com.centurylinklabs.watchtower.enable=true"

  rabbit-server:
    image: bitnami/rabbitmq:latest
    restart: always
    logging:
      driver: "json-file"
      options:
        max-size: "50m"
    ports:
      - '5672:5672'
      - '15672:15672'
    environment:
      - RABBITMQ_DEFAULT_USER=farmacia
      - RABBITMQ_DEFAULT_PASS=farmacia_rabbit_23


volumes:
  db_data:
  es_data:
  certbot-etc:
  certbot-var:
  db_data15:
  portainer_data:
  web-root:
    driver: local
    driver_opts:
      type: none
      device: /var/www/html
      o: bind

networks:
  default:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 192.168.238.0/24
          gateway: 192.168.238.1
```

1. Создаем .env файл в папке /home/farmacia

1. Вставляем следующие переменные в этот файл  

```dosini
APP_PORT=9997
UV_THREADPOOL_SIZE=128

POSTGRES_PASSWORD=<db_password>
POSTGRES_USER=farm
POSTGRES_DB=farm
DB_HOST=db

START_RABBIT=true
START_RABBIT_SENDER=true

LOG_DIR=/home/farmacia/logs
BACKEND_DIR=farm-backend
RECIPE_MONITORING_TIMEOUT=60
IMAGE_HOST=<host>
NODE_TLS_REJECT_UNAUTHORIZED=0
ESIA_CERT_PIN=12345678
PHONE_TEST_NUMBERS=9131852484,3333333333,3333333332,3333333331,1111111111,9831597282

RT_TIME_LIFE=2592000000
JWT_TIME_LIFE=15m

APP_PROTOCOL=https

EMAIL_HOST=smtp.ethereal.email
EMAIL_PORT=587
EMAIL_USER=kris.denesik@ethereal.email
EMAIL_PASSWORD=gYEYneH8fXCC14Zeqq

elasticsearchUrl=<elasitc_search_url> #http://172.17.80.157:9200/
recreateElasticIndex=false
ELASTICSEARCH_PASSWORD=<elastic_search_password>

INITIAL_UPLOAD_LIMIT=20000

REDIS_HOST=redis
REDIS_PASSWORD=<redis_password>
firebaseAdminCredentials=firebase-creds.json

CHECK_FORGOTTEN_CART_INTERVAL="*/1 * * * *"

MTS_REQUEST_TIMEOUT=120

CODE_GENERATION_COOLDOWN_TIME=120

APP_HOST=0.0.0.0

RABBIT_HOST=<rabbit_host>
RABBIT_PORT=5672
RABBIT_LOGIN=<rabbit_login>
RABBIT_PASSWORD=<rabbit_password>
RABBIT_QUEUE=Farmacia
RABBIT_VHOST=<rabbit_vhost>

RABBIT_CLIENT_HOST=<rabbit_host>
RABBIT_CLIENT_PORT=5672
RABBIT_CLIENT_LOGIN=<rabbit_login>
RABBIT_CLIENT_PASSWORD=<rabbit_password>
RABBIT_CLIENT_EXCHANGE=data
RABBIT_CLIENT_VHOST=<rabbit_vhost>

TZ=Asia/Krasnoyarsk

FRONTEND_URL=<host> #https://sklad-farmacia.ru/
BITRIX_API_URL=https://admin.24farmacia.ru/api_new

ZABBIX_HOST=localhost
ZABBIX_PORT=10051
ZABBIX_TIMEOUT=5000
ZABBIX_HOSTNAME=Farmacia
ZABBIX_ENABLED=false

dadataKey=<dadata_key>
dadataSecret=<dadata_secret>
dadataTimeRateLimit=1
dadataRequestsLimit=60

SBERBANK_SECRET=<sber_secret>

FTP_24LEK_HOST='ftp.24lek.ru'
FTP_24LEK_USER='Slava_farmcope'
FTP_24LEK_PASSWORD='1q2a3z1q2a3z'

FTP_009RF_HOST='ftp.009.am'
FTP_009RF_USER='gubernskie-apteki'
FTP_009RF_PASSWORD='lY7bFx'

UPLOAD_LEFTOVERS_24LEK_INTERVAL="0 0 */1 * *"
UPLOAD_LEFTOVERS_009RF_INTERVAL="0 0 */1 * *"

SECTION_CODE_APTEKI="000000001"
MAX_LOGIN_ATTEMPTS=100

MEGAPTEKA_API_URL="https://api.megapteka.dev/exch/v4/"
MEGAPTEKA_API_MC="gubernskie"
MEGAPTEKA_API_AUTH_TOKEN="bcf693537184fa85a1be01b8b845065d3417873a501113bb83a45f5e7bc04ead"
MEGAPTEKA_LEFTOVERS_EXCHANGE_INTERVAL="*/15 * * * *"
MEGAPTEKA_LEFTOVERS_FULL_EXCHANGE_INTERVAL=23
MEGAPTEKA_ORDERS_EXCHANGE_INTERVAL="* * * * *"
MEGAPTEKA_PHARMACIES_EXCHANGE_INTERVAL="0 */1 * * *"

FTP_MEGAPTEKA_HOST="gubernskie-test.megapteka.ru"
FTP_MEGAPTEKA_USER="gubernskie"
FTP_MEGAPTEKA_PASSWORD="f933cd9bd4c11523"
FTP_MEGAPTEKA_IMPORT_PATH="import"

MILK_KITCHEN_FORCE_JOB_DELAY=1
MILK_KITCHEN_PICK_POINT_CHANGE_END_DAY=32

RECAPTCHA_SECRET_KEY=<secret_key_recaptcha>
MOBILE_AUTH_KEY=RealaHmAtInGIlickLiAlOGnoNdRafERhAlerMaXORAundubAS
RECAPTCHA_THRESHOLD=0

RABBIT_INT_HOST=rabbit-server
RABBIT_INT_PORT=5672
RABBIT_INT_LOGIN=farmacia
RABBIT_INT_PASSWORD=<some_password>
RABBIT_INT_QUEUE=FarmaciaMain
RABBIT_INT_VHOST=/

SMS_USER=<sms_user>
SMS_PASS=<sms_password>

WAREHOUSE_API_URL=http://warehouse-api:9995/api
WAREHOUSE_SERVICE_ACTIVE=true

ADMIN_JWT_SECRET=<some_random_secret_string>

SHA_CERT=1789565cf6226f7aed7352d093d6f852215dafd5
ESIA_CERT_PIN=cktgj,hz,
ESIA_REDIRECT_URI=https://preprod.24farmacia.ru/api/esia/code
ESIA_URL=https://esia.gosuslugi.ru
CRYPTO_PRO_URL=http://172.17.80.157:8080/

RECIPE_SERVICE_URL=http://10.61.103.60:8080/drugstore/RecipeService.asmx
DOWNLOAD_SERVICE_URL=http://10.61.103.60:8080/drugstore/DownloadService.asmx
RECIPE_CLIENT_ID=A0004

COURIER_SECRET_CODE=019910

NODE_OPTIONS=--max_old_space_size=2048
```

1. Все строки в <> скобках необходимо заменить на те, что подходят под разворачиваемое окружение. Например, ```<host>``` надо заменить на ```https://sklad-farmacia.ru/``` для домена sklad-farmacia.ru

1. Запускаем БД командой ```docker compose up db```

1. Все образы приложений хранятся и собираются в gitlab, в docker-compose.yaml указаны ссылки на эти образы. Существует 2 версии образов (тэга): **stage** и **pre-prod**. В ссылке необходимо указать тэг, который нужен в вашем окружении.

1. Для доступа к образам приложений с сервера необходимо выполнить ```docker login``` с указанием deploy token, который нужно создать в гитлабе для вашего окружения. [инструкция](https://docs.gitlab.com/ee/user/project/deploy_tokens/). После создания токена выполняем ```docker login -u <deploy_token_login> -p <deploy_token_password> https://registry.gitlab.com/24farmacia```

1. Запускаем API командой ```docker compose up api```

1. Подключиться к БД и проверить, что миграции были выполнены успешно

### Сервис остатков

1. Создать в папке с проектом файл *.env.warehouse* следующего содержания

```dosini
POSTGRES_DB=farm-warehouse
POSTGRES_USER=farm
POSTGRES_PASSWORD=<db_password>
DB_HOST=db

RABBIT_HOST=<rabbit_host>
RABBIT_PORT=5672
RABBIT_LOGIN=<rabbit_login>
RABBIT_PASSWORD=<rabbit_password>
RABBIT_QUEUE=Farmacia
RABBIT_VHOST=<rabbit_vhost>

RABBIT_INT_HOST=rabbit-server
RABBIT_INT_PORT=5672
RABBIT_INT_LOGIN=farmacia
RABBIT_INT_PASSWORD=<rabbit_int_password>
RABBIT_INT_QUEUE=FarmaciaWarehouse
RABBIT_INT_VHOST=/

JWT_ADMIN_SECRET=<jwt_admin_secret>

INITIAL_UPLOAD_LIMIT=100
```

1. **JWT_ADMIN_SECRET** должен совпадать с .env файлом!!!

1. Создать базу данных farm-warehouse в СУБД

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
    server_name preprod.24farmacia.ru; # домен или ip сервера
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
        server_name preprod.24farmacia.ru;
        client_max_body_size 50M;

        server_tokens off;

        ssl_certificate /etc/letsencrypt/live/preprod.24farmacia.ru/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/preprod.24farmacia.ru/privkey.pem;

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

1. Нужно создать пароль для доступа к сайту, если окружение не должно быть доступно всем пользователям (стейдж, прерпод...). Для этого нужно установить утилиту *apache2-utils* и вызвать команду ```sudo htpasswd -c /home/farmacia/nginx/.htpasswd <username>```. Далее утилита попросит ввести пароль для пользователя и информация будет сохранена в файл .htpasswd

1. Запускаем фронт админки и самого сайта командой ```docker compose up admin nuxt```. Если появляется ошибка доступа к хранилищу образов, то необходимо создать deploy token для репозиториев админки и фронта соответственно с ранее указанной инструкцией и произвести вход.

### Получение сертификатов

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
