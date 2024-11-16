**Задание 1. Установка MicroK8S**

1. Установить MicroK8S на локальную машину или на удалённую виртуальную машину.

2. Установить dashboard.

3. Сгенерировать сертификат для подключения к внешнему ip-адресу.

**Решение 1**

Установим kubectl 

![Image alt](https://github.com/mezhibo/kubernetes1/blob/cb6e64a9e12b242685d9fdbfc6a6e7b4fb1dbfc3/IMG/1.jpg)

Активируем дашборд

```
microk8s enable dashboard
```

Зайдем в папку .kube и посмотрим конфиг 

![Image alt](https://github.com/mezhibo/kubernetes1/blob/cb6e64a9e12b242685d9fdbfc6a6e7b4fb1dbfc3/IMG/2.jpg)

Затем открываем конфиг  nano /var/snap/microk8s/current/certs/csr.conf.template и дописываем ip адрес нашей машины клиента kubectl с которой будем управлять кластером

Пррописывается адрес удаленной машины для того чтобы она смогла подключаться к мастеру microk8s

![Image alt](https://github.com/mezhibo/kubernetes1/blob/cb6e64a9e12b242685d9fdbfc6a6e7b4fb1dbfc3/IMG/3.jpg)

Далее делаем проброс на всю сеть

```
microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0
```

Видим что коннект открылся

![Image alt](https://github.com/mezhibo/kubernetes1/blob/cb6e64a9e12b242685d9fdbfc6a6e7b4fb1dbfc3/IMG/4.jpg)

Генерируем сертификат для подключения

```
sudo microk8s refresh-certs --cert front-proxy-client.crt
```


Заходим в веб интерфейс нашей виртуалки и видим  что он требует токен авторизации

![Image alt](https://github.com/mezhibo/kubernetes1/blob/cb6e64a9e12b242685d9fdbfc6a6e7b4fb1dbfc3/IMG/5.jpg)

Командой 

```
microk8s kubectl create token default
```

генерируем токен и копируем его в веб интерфейс, и видим наш работающий дашборд

![Image alt](https://github.com/mezhibo/kubernetes1/blob/cb6e64a9e12b242685d9fdbfc6a6e7b4fb1dbfc3/IMG/6.jpg)




**Задание 2. Установка и настройка локального kubectl**

1. Установить на локальную машину kubectl.

2. Настроить локально подключение к кластеру.

3. Подключиться к дашборду с помощью port-forward.


**Решение 2**

Устанавливаем kubectl на клиентскую машину для управления кластером

![Image alt](https://github.com/mezhibo/kubernetes1/blob/af16f3503af521852262d5a4c4633bd7f7d5d975/IMG/7.jpg)


Создадим папку .kube и в ней создадим файлик config ТОЧНО ТАКОЙ ЖЕ КАК И НА МАСТЕР СЕРВЕРЕ

Заполняем его содержимым файлика config с мастер сервера и видим что прописан будет наш мастер в конфиге 


![Image alt](https://github.com/mezhibo/kubernetes1/blob/af16f3503af521852262d5a4c4633bd7f7d5d975/IMG/8.jpg)


Теперь на клиенте выполняем 

```
kubectl get nodes
```

и видим что у нас подтянулась наша мастер нода



![Image alt](https://github.com/mezhibo/kubernetes1/blob/af16f3503af521852262d5a4c4633bd7f7d5d975/IMG/9.jpg)


Далее выполняем команду 


```
kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443 --address 0.0.0.0
```
![Image alt](https://github.com/mezhibo/kubernetes1/blob/af16f3503af521852262d5a4c4633bd7f7d5d975/IMG/10.jpg)


И видим что на нашем айпиадресе клиента, запустился тот же самый дашборд что работает на ip адресе мастера


![Image alt](https://github.com/mezhibo/kubernetes1/blob/af16f3503af521852262d5a4c4633bd7f7d5d975/IMG/11.jpg)


Это значит что кластер работает верно

Теперь опять генерируем токен на машине мастера 

![Image alt](https://github.com/mezhibo/kubernetes1/blob/af16f3503af521852262d5a4c4633bd7f7d5d975/IMG/12.jpg)


И заходим поод этим токеном в  В ЛЮБОЙ ВЕБ ИНТЕРФЕЙС    так как это кластер


Я сгенерировал на мастер ноде, и авторизовался на kubectl клиент ноде

И теперь зайди по ip адресу КЛИЕНТСКОЙ МАШИНЫ мы видим наш мастер microk8s

![Image alt](https://github.com/mezhibo/kubernetes1/blob/af16f3503af521852262d5a4c4633bd7f7d5d975/IMG/13.jpg)


Теперь мы наглядно убелились как работает kubectl кластер


