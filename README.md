
# Лабораторная работа №3

*Илья Востров Анатольевич, ITMO ID 465456,  группа К3139*

## Задание

Необходимо настроить виртуальную машину А с Ubuntu (желательно, но можно и другую Linux подобную ОС) в VirtualBox. Обеспечить доступ в сеть Интернет. Осуществить проверку этого доступа и приложить скриншот из терминала. Следующим шагом настроить ещё одну виртуальную машину Б. После чего обеспечить сетевой доступ от машины А к машину Б. Приложить скриншот из терминала. Поднять ещё одну виртуальную машину В. Организовать сетевой доступ:

1. из машины А в машину Б.
2. из машины А в машину В,
3. но запретить доступ из машины Б в машину В.
4. Приложить скриншот, на котором видно терминалы всех трёх машин.
5. 
## Ход работы
**1. Установка VirtualBox и виртуальных машин.**

Так как я работаю на macbook m1, то у меня 64-битная ARM версия процессора. Поэтому я использовал Ubuntu для сервера, так как он для arm64. Далее я создал 3 виртуальные машины.

![image](https://github.com/WillowRussia/ITMO-INFO-LAB3/blob/main/Assets/image1.png?raw=true)

**2. Обеспечение и проверка доступа в интернет для машины ITMO_A.**

Сначала я в настройках машины подключил ее к сети NAT, чтобы у нее был доступ в интернет.

![image](https://github.com/WillowRussia/ITMO-INFO-LAB3/blob/main/Assets/image2.png?raw=true)

Далее с помощью команду **`ping`** я отправляю пакеты на  **`google.com`**. Как показаны на скриншоте все выполнилось успешно.

![image](https://github.com/WillowRussia/ITMO-INFO-LAB3/blob/main/Assets/image_a.png?raw=true)

**3. Организация сетевого доступа между машинами ITMO_A, ITMO_B, ITMO_C.**

Перед началом работы создаю сеть NAT

![image](https://github.com/WillowRussia/ITMO-INFO-LAB3/blob/main/Assets/image3.png?raw=true)

Добавляю сетевые адаптеры машинам с типом подключения "Сеть NAT"

![image](https://github.com/WillowRussia/ITMO-INFO-LAB3/blob/main/Assets/image4.png?raw=true)

Далее настраиваю доступ к машине ITMO_C через ufw:

```sudo ufw default deny incoming``` - запрещаю все входящие в машину C ssh запросы

```sudo ufw default allow outgoing``` - разрешаю все исходящие из машины C ssh запросы

```sudo ufw allow from 10.0.2.15 port 22``` - разрешаю входящие ssh запросы в машину C с адреса 10.0.2.15 (машина ITMO_A)

```sudo ufw enable``` - включаю файервол

```sudo ufw status verbose``` - проверяю текущий статус

![image](https://github.com/WillowRussia/ITMO-INFO-LAB3/blob/main/Assets/image_b.png?raw=true)

Проверяю доступ из машины A в машины B и C:
```
timeout 5 bash -c "</dev/tcp/10.0.2.9/22"
echo $? #0 доступ есть

timeout 5 bash -c "</dev/tcp/10.0.2.8/22"
echo $? #0 доступ есть
```

Проверяю доступ из машины B в C:
```
timeout 5 bash -c "</dev/tcp/10.0.2.9/22"
echo $? #124 доступа нет
```
![image](https://github.com/WillowRussia/ITMO-INFO-LAB3/blob/main/Assets/image_c.png?raw=true)
