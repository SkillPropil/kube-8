# kube-8
## Задание 1 Создать Deployment приложения и решить возникшую проблему с помощью ConfigMap. Добавить веб-страницу
Файл [nginx-multitool-deployment](nginx-multitool-deployment-v1.yaml)  
Возникшую проблему решил конфигмапом конфига nginx (забиндил на порт 8080)  

<img width="792" alt="Снимок экрана 2024-09-30 в 23 58 05" src="https://github.com/user-attachments/assets/78ce8a2b-122b-44af-b06a-6defceae3490">  

## Задание 2 Создать приложение с вашей веб-страницей, доступной по HTTPS
Вот тут уже было сложнее. Сгенерировал серт и ключ, сначала пробовал создать секрет в файле. Получалось так себе, поэтому сделал секрет командой и примаунтил его.  
Далее возникла проблема - отдавалась 503 ошибка, долго не понимал почему. В итоге оказалось, что с предыдущих заданий у меня был ингрес, который редиректил 'вникуда'.  
При этом я видел, что два ингреса, но почему-то пазл в голове долго не складывался. Переписывал конфиги, мапил серты конфигмапом (конфиг nginx на ssl). В общем попыток было много. Но да ладно, теперь к заданию:  

Файлы [configmap](configmap.yaml) [deployment2](deployment2.yaml) [service](service.yaml) [ingress](ingress.yaml)  

Сгенерировал сертификат командой:  
```
openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout tls.key -out tls.crt -subj "/CN=mysite.com" -days 365
```
Далее создал секрет:   
```
kubectl create secret generic nginx-ssl --from-file=./tls.crt --from-file=./tls.key
```

Создал ингрес и подключил серт, в общем доказательство работоспособности:  

<img width="1385" alt="Снимок экрана 2024-10-04 в 18 43 53" src="https://github.com/user-attachments/assets/e074464d-f129-4d7e-908a-c5cc2a942fac">

