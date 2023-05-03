 # Домашнее задание к занятию «Репликация и масштабирование. Часть 1» - `Коробов Евгений`

### Задание 1.
На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.
### Ответ
```
Мастер-слейв (Master-Slave):
Один сервер принимает записи, другие только читают.
Обеспечивает высокую доступность и отказоустойчивость.

Мастер-мастер (Master-Master):
Несколько серверов обрабатывают записи и чтение.
Обеспечивает более высокую производительность и масштабируемость.
```
 
## Задание 2. 
Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.
### Ответ

## Статус MASTER
![sdb](https://github.com/nespaces/sdb-homeworks/blob/main/img/5.png)
## Статус SLAVE
![sdb](https://github.com/nespaces/sdb-homeworks/blob/main/img/9.png)
## Конфигурация MASTER
![sdb](https://github.com/nespaces/sdb-homeworks/blob/main/img/6.png)
## Конфигурация SLAVE
![sdb](https://github.com/nespaces/sdb-homeworks/blob/main/img/7.png)
## Создаём БД на MASTER
![sdb](https://github.com/nespaces/sdb-homeworks/blob/main/img/10.png)
## Также видим её на SLAVE
![sdb](https://github.com/nespaces/sdb-homeworks/blob/main/img/11.png)
