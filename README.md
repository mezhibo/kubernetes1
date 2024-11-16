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






