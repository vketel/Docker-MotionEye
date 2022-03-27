# Установка Docker
1. Обновить список репозиториев: `sudo apt-get update`
2. Установить пакеты необходимые для добавления репозиториев: `sudo apt-get install ca-certificates curl gnupg lsb-release`
3. Добавить официальный GPG ключ Docker'a:  
`curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`
4. Добавить репозиторий Docker'a в систему:  
`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
5. Еще раз обновить список репозиториев: `sudo apt-get update`
6. Установить пакеты, необходимые для работы Docker'a: `sudo apt-get install docker-ce docker-ce-cli containerd.io`
7. Проверка работоспособности Docker'a: `sudo docker run hello-world`

# Установка контейнера с ПО MotionEye
Как известно, в репозитории Docker'a существует огромное количество готовых образов, в частности, там имеется образ MotionEye: [cсылка на MotionEye в DockerHub](https://hub.docker.com/r/ccrisan/motioneye/).  
1. В разделе "Tags" выбираем необходимый образ (**master-amd64** - для ВМ, **master-armhf** - для одноплатников).
2. Узнаем перечень камер, к которым у нас есть доступ: `ls -la /dev/ | grep video`
3. Устанавливаем необходимый образ с заданными параметрами:  
`docker run --name="motioneye" -p 8765:8765 --hostname="motioneye" -v /etc/localtime:/etc/localtime:ro -v /etc/motioneye:/etc/motioneye -v /var/lib/motioneye:/var/lib/motioneye --device=/dev/video0 --device=/dev/video1 --device=/dev/video2 --restart="always" --detach=true ccrisan/motioneye:master-armhf`
4. Проверяем статус запуска контейнера: `docker ps -a`
5. Заходим в веб интерфейс MotionEye. По умолчанию пользователь - admin, пароль - <пусто>.

# Настройка MotionEye
1. Жмём кнопку *"You have not configured any camera yet. Click here to add one..."*
2. Добавляем локальную камеру:  
`Camera Type -> Local V4L2 Camera`  
`Camera -> <Наша камера>`
3. Сразу отключаем раздел *"Video Streaming"*
4. [Необязательно] Изменяем название камеры на своё: `Video Device -> Camera Name`
5. Выбираем максимальное возможное разрешение изображения: `Video Device -> Video Resolution`
6. Выбираем максимально возможный frame rate: ``
7. Включаем режим фотографирования при обнаружении движения: `Still Images -> Capture Mode -> Motion Triggered (One Picture)`
8. Включаем режим детекции движения:  
`Motion Detection -> Motion Gap -> 5 seconds`  
`Motion Detection -> Mask(ON) -> Mask Type -> Editable -> Edit Mask`
9. Включаем режим отправки уведомлений на эл. почту:  
*!!!Разрешить в Google аккаунте сторонние приложения: [ссылка](https://myaccount.google.com/lesssecureapps)*  
`Motion Notifications -> Send An Email`  
`Email Addresses: <mail>@gmail.com`  
`SMTP Server: smtp.gmail.com`  
`SMTP Port: 587`  
`SMTP Account: <mail>@gmail.com`  
`SMTP Password: <password>`  
`From Address: <mail>@gmail.com`  
`Use TLS: ON`  
`Attached Pictures Time Span: 5`  

# Несколько полезных команд Docker'a
1. Первоначальный запуск контейнера: `docker run <image> <other commands>`
2. Остановка определенного контейнера: `docker stop <CONTAINER_ID>`
3. Запуск определенного контейнера после остановки: `docker start <CONTAINER_ID>`
4. Удаление определенного контейнера: `docker rm <CONTAINER_ID>`
5. Остановка всех контейнеров: `docker stop $(docker ps -a -q)`
6. Удаление всех контейнеров: `docker rm $(docker ps -a -q)`
