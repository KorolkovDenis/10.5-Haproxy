# Домашнее задание к занятию 10.5 «Балансировка нагрузки. HAProxy/Nginx»

---

### Задание 1

Что такое балансировка нагрузки и зачем она нужна? 

*Приведите ответ в свободной форме.*

Ответ:

Балансировщик нагрузки — это сервис, который занимается распределением нагрузки между пулом приложений, которые находятся за ним, стараясь максимизировать скорость и утилизировать ресурсы приложений. Также, гарантирует, что приложения не будут перегружены. Бывают hardware и software решения.

В вычислительной технике балансировка нагрузки улучшает распределение рабочих нагрузок по нескольким вычислительным ресурсам: компьютерам, компьютерным кластерам, сетевым подключениям, центральным процессорам или дисковым устройствам. Балансировка нагрузки призвана оптимизировать использование ресурсов, максимально увеличить пропускную способность, минимизировать время отклика и избежать перегрузки отдельных ресурсов. Применение вместо одного компонента нескольких компонентов с балансировкой может повысить надёжность и доступность благодаря получившемуся запасу мощностей. Балансировка нагрузки обычно подразумевает использование специального ПО или оборудования вроде многоуровневого коммутатора или DNS-сервера.


---

### Задание 2

Чем отличаются алгоритмы балансировки Round Robin и Weighted Round Robin? В каких случаях каждый из них лучше применять? 

*Приведите ответ в свободной форме.*

Ответ:

Round Robin

Запросы распределяются по пулу сервером последовательно. Если в пуле все сервера
одинаковой мощности, то этот алгоритм скорее всего подойдет идеально.

Weighted Round Robin

Тот же round robin, но имеет дополнительное свойство — вес сервера. С его помощью мы можем указать балансировщику, сколько трафика отправлять на тот или иной сервер. Так сервера помощнее будут иметь больший вес и, соответственно, обрабатывать больше запросов, чем другие сервера.

Соответственно, если у нас в работе несколько работающих серверов сильно отличающихся по своим мощностям, дабы дать более мощному серверу посильную для него нагрузку и не давать простоя лучше использовать не Round Robin, а Weighted Round Robin.


---

### Задание 3

Установите и запустите Haproxy.

*Приведите скриншот systemctl status haproxy, где будет видно, что Haproxy запущен.*

Ответ:

systemctl status haproxy

![screen1](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen1.jpg)


---

### Задание 4

Установите и запустите Nginx.

*Приведите скриншот systemctl status nginx, где будет видно, что Nginx запущен.*

Ответ:

systemctl status nginx

![screen2](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen2.jpg)


---

### Задание 5

Настройте Nginx на виртуальной машине таким образом, чтобы при запросе:

`curl http://localhost:8088/ping`

он возвращал в ответе строчку: 

"nginx is configured correctly".

*Приведите конфигурации настроенного Nginx сервиса и скриншот результата выполнения команды curl http://localhost:8088/ping.*

Ответ:

Заходим в директорию

```
 /etc/nginx/conf.d
```

Открываем файл для редактирования: nano default.conf

```
server {

listen 8088;
location /ping{
return 200 ' nginx is configured correctly ' ;

}
}
```

![screen3](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen3.jpg)

Далее проводим проверку:

nginx -t

![screen4](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen4.jpg)

перезапускаем службу:

service nginx reload

curl http://localhost:8088/ping

![screen5](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen5.jpg)

Через вебку:

![screen6](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen6.jpg)
![screen7](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen7.jpg)

По IP постучал с другого хоста:

![screen8](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen8.jpg)

---

## Задания со звёздочкой*

Эти задания дополнительные. Их выполнять не обязательно. На зачёт это не повлияет. Вы можете их выполнить, если хотите глубже разобраться в материале.

---

### Задание 6*

Настройте Haproxy таким образом, чтобы при ответе на запрос:

`curl http://localhost:8080/`

он проксировал его в Nginx на порту 8088, который был настроен в задании 5 и возвращал от него ответ: 

"nginx is configured correctly". 

*Приведите конфигурации настроенного Haproxy и скриншоты результата выполнения команды curl http://localhost:8080/.*

Ответ:

Попробовал следующий код:

открываю файл:
``` 
nano /etc/haproxy.haproxy.cfg
```

![screen9](https://github.com/KorolkovDenis/10.5-Haproxy/blob/main/screenshots/screen9.jpg)

и такой

```
frontend localhost
        bind *:9090
        default_backend localhost_back

backend localhost_back
        server localhost 127.0.0.1:8088/ping

```

не вышло..

## Моя подробная работа в Google:

[Моя работа по «Балансировка нагрузки. HAProxy/Nginx»](https://docs.google.com/document/d/1yBlzk6lhrQJQL7ctJmrvZ9ZY4VYHmf73/edit?usp=share_link&ouid=104113173630640462528&rtpof=true&sd=true)

