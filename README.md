# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose»

### Инструкция к выполению

1. Для выполнения заданий обязательно ознакомьтесь с [инструкцией](https://github.com/netology-code/devops-materials/blob/master/cloudwork.MD) по экономии облачных ресурсов. Это нужно, чтобы не расходовать средства, полученные в результате использования промокода.
2. Практические задачи выполняйте на личной рабочей станции или созданной вами ранее ВМ в облаке.
3. Своё решение к задачам оформите в вашем GitHub репозитории в формате markdown!!!
4. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.

## Задача 1

Сценарий выполнения задачи:
- Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
- Если dockerhub недоступен создайте файл /etc/docker/daemon.json с содержимым: ```{"registry-mirrors": ["https://mirror.gcr.io", "https://daocloud.io", "https://c.163.com/", "https://registry.docker-cn.com"]}```
- Зарегистрируйтесь и создайте публичный репозиторий  с именем "custom-nginx" на https://hub.docker.com (ТОЛЬКО ЕСЛИ У ВАС ЕСТЬ ДОСТУП);
- скачайте образ nginx:1.21.1;
- Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
- Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 (ТОЛЬКО ЕСЛИ ЕСТЬ ДОСТУП). 
- Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .

## Решение 1
1. Установил Docker:
   - sudo apt-get update;
   - sudo apt install git curl;
   - curl -fsSL get .docker.com -o get -docker.sh;
   - chmod +x get-docker.sh;
   - ./get-docker.sh
   - docker pull hello-world;
   - docker run hello-world

2.  Установил Docker compose:
   - curl -L https://github.com/docker/compose/releases/download/v2.5.1/docker compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose;
   - chmod +x /usr/bin/docker-compose
   - docker-compose version

3. Зарегистрировался на https://hub.docker.com
4. Скачал образ nginx:1.21.1
   - docker pull nginx:1.21.1
5. Создал Docerfile с заменой дефолтной индекс-страницы согласно задания
   FROM nginx:1.21.1
   COPY ./index.html /usr/share/nginx/html/index.html
   EXPOSE 80
7. Собрал докер образ:
   dokcer build -t custom-nginx:1.0.0 .
8. Собрал и отправил созданный образ в свой dockerhub-репозиторий
   docker tag castom-nginx:1.0.0 kargapoltcevks/custom-nginx:1.0.0
   docker push kargapoltcevks/custom-nginx:1.0.0
10. Ссылка в dockerhub-репозиторий:
    https://hub.docker.com/repository/docker/kargapoltcevks/custom-nginx/general
   

## Задача 2
1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
- имя контейнера "ФИО-custom-nginx-t2"
- контейнер работает в фоне
- контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Не удаляя, переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду ```date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html```
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Решение 2
1. Запустил созданный образ
  docker run -it --rm -d -p 8080:80 --name kks-custom-nginx-t2 custom-nginx:1.0.0
2.  Переименовываю контейнер
  docker rename kks-custom-nginx-t2 custom-nginx-t2
3. Выполняю команду
![2-3](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/2-3.png)
![2-4-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/2-4-1.png)
![2-4](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/2-4.png)
   

## Задача 3
1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните ```docker ps -a``` и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду ```nginx -s reload```, а затем внутри контейнера ```curl http://127.0.0.1:80 ; curl http://127.0.0.1:81```.
9. Выйдите из контейнера, набрав в консоли  ```exit``` или Ctrl-D.
10. Проверьте вывод команд: ```ss -tlpn | grep 127.0.0.1:8080``` , ```docker port custom-nginx-t2```, ```curl http://127.0.0.1:8080```. Кратко объясните суть возникшей проблемы.
11. * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер. Останавливать контейнер можно. [пример источника](https://www.baeldung.com/linux/assign-port-docker-container)
12. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Решение 3

1. Подключаемся к контейнеру через docker attach
![3-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/3-1.png)
2.3 Нажимаю Ctrl-C и выполняю команду docker ps -a
Завершилась работа контейнера, т.к. Ctrl-C это сигнал завершения процесса, для фоновой работы процесса необходимо установить ключ -d и тогда контейнер продолжит работу, а мы отключимся от него.
![3-2-3-3](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/3-2-3-3.png)
4.5. Перезапускаем контейнер и заходим в интерактивный режим контейнера
docker exec -it custom-nginx-t2 bash
6. Устанавливаю текстовый редактор nano
apt install nano
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81"
![3-7](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/3-7.png)
8. Рестартуем сервер и проверяем доступность портов
80 порт закрылся и открылся 81
![3-8](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/3-8.png)
9. Вышел из контейнера
exit
10. Обращение на порт 8080 выдает ошибку, т.к. 80 порт нашего контейнера который прокидывался на порт 8080 мы поменяли 81
![3-10](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/3-10.png)
12. Удаляем контейнер без остатка с ключем -f
docker rm custom-nginx-t2 -f custom-nginx-t2
![3-12](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/3-12.png)


## Задача 4


- Запустите первый контейнер из образа ***centos*** c любым тегом в фоновом режиме, подключив папку  текущий рабочий каталог ```$(pwd)``` на хостовой машине в ```/data``` контейнера, используя ключ -v.
- Запустите второй контейнер из образа ***debian*** в фоновом режиме, подключив текущий рабочий каталог ```$(pwd)``` в ```/data``` контейнера. 
- Подключитесь к первому контейнеру с помощью ```docker exec``` и создайте текстовый файл любого содержания в ```/data```.
- Добавьте ещё один файл в текущий каталог ```$(pwd)``` на хостовой машине.
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в ```/data``` контейнера.


В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Решение 4

1. Скачал образ centos:7
docker pull centos:7
2. Скачал образ debian
docker pull debian
3. Запустил centos и пробросил нашу текущую директорию внутрь контейнера в каталог data
![4-3](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/4-3.png)
4. Запустил debian и пробросил нашу текущую директорию внутрь контейнера в каталог data
![4-4](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/4-4.png)
![4-3-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/4-3-1.png)
5. Подключился к контейнеру centos и в каталоге /data создал файл file1
![4-5](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/4-5.png)
6. Создал файл file2 на хостовой машине в текущей директории
![4-6](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/4-6.png)
7. Подключился к контейнеру debian и вывел файлы содержащиеся в директории /data 
![4-7](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/4-7.png)

## Задача 5

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него.
"compose.yaml" с содержимым:
```
version: "3"
services:
  portainer:
    network_mode: host
    image: portainer/portainer-ce:latest
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```
"docker-compose.yaml" с содержимым:
```
version: "3"
services:
  registry:
    image: registry:2

    ports:
    - "5000:5000"
```

И выполните команду "docker compose up -d". Какой из файлов был запущен и почему? (подсказка: https://docs.docker.com/compose/compose-application-model/#the-compose-file )

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла. (подсказка: https://docs.docker.com/compose/compose-file/14-include/)

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. Дополнительная документация: https://distribution.github.io/distribution/about/deploying/
4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)
5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local  окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml).  Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

---

## Решение 5

1. Создал 2 файла с содержимым из задания
![5-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-1.png)

Выполнил команду docker compose up -d
![5-2](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-2.png)
Docker compose в связи с наличием нескольких файлов конфигураций, выбирает первый файл который он находит согласно внутреннему порядку приоритетов. В данном случае это файл compose.yaml 
![5-2-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-2-1.png)

2. 
```
version: "3"
include:
  - docker-compose.yaml
services:
   portainer:
     image: portainer/portainer-ce:latest
     network_mode: host
     ports:
       - "9000:9000"
     volumes:
       - /var/run/docker.sock:/var/run/docker.sock
```
![5-2-2](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-2-2.png)

3. скрин 5-3
![5-3-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-3-1.png)

4. Произвел начальную настройку porainer
![5-4](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-4.png)

5. 
![5-5](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-5.png)
![2-3](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-6.png)
![5-7](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-7.png)
![5-8](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/5-8.png)

6. 
![6-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/6-1.png)

7. Удалил файл compose.yaml и запустим команду docker compose up -d
![7-1](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/7-1.png)
Предложено запустить команду с флагом --remove-orphans для очистки сиротского контейнера task5-portainer-1
Выполнил предложенные дейстрия 
![7-2](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/7-2.png)
Погасил проект docker stop $(docker ps -a -q)
![7-3](https://github.com/KargapoltcevKS/Docker-Compoce/blob/main/img/7-3.png)

### Правила приема

Домашнее задание выполните в файле readme.md в GitHub-репозитории. В личном кабинете отправьте на проверку ссылку на .md-файл в вашем репозитории.
