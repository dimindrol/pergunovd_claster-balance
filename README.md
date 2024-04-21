# Домашнее задание к занятию "Кластеризация и балансировка нагрузки" - Пергунов Д.В

### Задание 1

1. Запустили два python3 сервера
![image](https://github.com/dimindrol/pergunovd_claster-balance/assets/103885836/eab0099c-c602-427f-8793-dd6551b7e8d4)
2. Установите и настроили HAProxy 
3. Настроили балансировку на 4 модели OSI
4. Конфигурация
```
listen stats 
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example  
        mode tcp
        bind :8088
        default_backend web_servers

backend web_servers   
        mode tcp
        balance roundrobin
        option httpchk
        http-check send meth GET uri /index.html
        server s1 127.0.0.1:8888 check
        server s2 127.0.0.1:9999 check
```
5. ![image](https://github.com/dimindrol/pergunovd_claster-balance/assets/103885836/80857c5f-aff5-429c-84c2-9ca0c6e21578)
6. ![image](https://github.com/dimindrol/pergunovd_claster-balance/assets/103885836/f8243c02-f857-4f70-8abd-dd1ad2812da6)

### Задание 2
1. Запустили три три simple python сервера
2. Настроили балансировку Weighted Round Robin на 7 уровне, со следующими весами 2,3,4
3. Настроили балансировку только http-трафика, который адресован домену example.local
4.Конфигурация
```
listen stats  # веб-страница со статистикой
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example  # секция фронтенд
        mode http
        bind :8088
        acl ACL_example.com hdr(host) -i example.com
        use_backend web_servers if ACL_example.com

backend web_servers    # секция бэкенд
        mode http
        balance roundrobin
        option httpchk
        http-check send meth GET uri /index.html
        server s1 127.0.0.1:8888 weight 2 check
        server s2 127.0.0.1:9999 weight 3 check
        server s3 127.0.0.1:11111 weight 4 check
```
5. Обращение к серверу без заголовка

![image](https://github.com/dimindrol/pergunovd_claster-balance/assets/103885836/dbabb4d2-74ad-4e56-be9c-de1211a0d5d6)

6.Обращение к серверу с заголовком
![image](https://github.com/dimindrol/pergunovd_claster-balance/assets/103885836/fc5456b6-6413-4fb9-ae64-7ebf3105262c)

7. Статистика сервера 

![image](https://github.com/dimindrol/pergunovd_claster-balance/assets/103885836/d90bca11-daaf-4088-9dc5-cd61a6092a0c)




