# Реализация проекта

## 1. Шаг 1:

1. Так как я работаю на Fedora, я скачал виртуализацию сразу на нее

```bash
sudo dnf install -y @virtualization
```
Эта команда устанавливает группу пакетов:
 1. KVM (ядро виртуализации) - “двигатель”:
  - встроен в Linux
  - использует железо (CPU с поддержкой виртуализации)
  - делает ВМ быстрыми

 2. libvirt - “менеджер”:
  - управляет виртуальными машинами
  - запускает, останавливает, настраивает

 3. virt-manager - “интерфейс”:
  - графическая программа
  - через неё ты создаёшь ВМ


2. Запускаю сервер командой
```bash 
sudo systemctl enable --now libvirtd
```
Она запускает сервис виртуализации и включает его автозапуск

Команда:
```
sudo systemctl status libvirtd
```
выполняет проверку и выводит такие данные:
```
libvirtd.service - libvirt legacy monolithic daemon
     Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; preset: disabled)
    Drop-In: /usr/lib/systemd/system/service.d
             └─10-timeout-abort.conf
     Active: active (running) since Wed 2026-03-18 21:49:07 MSK; 2min 38s ago
 Invocation: 75f5e4815f5d4ab7b51f72fd1fbf6603
TriggeredBy: ● libvirtd.socket
             ● libvirtd-admin.socket
             ● libvirtd-ro.socket
       Docs: man:libvirtd(8)
             https://libvirt.org/
   Main PID: 40310 (libvirtd)
      Tasks: 23 (limit: 32768)
     Memory: 49.7M (peak: 51.5M)
        CPU: 998ms
     CGroup: /system.slice/libvirtd.service
             ├─40310 /usr/bin/libvirtd --timeout 120
             ├─40432 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro >
             └─40433 /usr/bin/dnsmasq --conf-file=/var/lib/libvirt/dnsmasq/default.conf --leasefile-ro >

Mar 18 21:49:07 fedora systemd[1]: Started libvirtd.service - libvirt legacy monolithic daemon.
Mar 18 21:49:08 fedora dnsmasq[40432]: started, version 2.92 cachesize 150
```

3. Добавляю себя в группу командой:
```
sudo usermod -aG libvirt $(whoami)
```
и перезагружаю систему 

## 2. Шаг 2:

1. Устанавливаю virt-manger — это графическая программа для управления виртуальными машинами в Linux :

```
sudo dnf install virt-manager
```

## 3. Скачиваем ISO Rocky Linux 9

Скачиваем ее в папку ~/Downloads
Делаем это через  терминал:
```
wget https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9-latest-x86_64-dvd.iso
```

(**WGET** - это консольная утилита для скачивания файлов из интернета (через HTTP, HTTPS, FTP). Это «браузер в командной строке»)

Cистема установилась!)


## 4. Установка Ansible

Использую команду 

```
sudo dnf install -y ansible
```
**Ansible** - инструмент для автоматизации настройки серверов и развертывания программ, который позволяет управлять сотнями компьютеров с одного рабочего места.

## 5. Настройка Виртуальной машины

1. Создание новый машины
2. Выбор Local install media (ISO)
3. Создал ВМ со следующими характеристиками:
	оперативная память: 2–4 ГБ

	процессор: 2 vCPU

	объем диска: 20 ГБ (формат qcow2)

	тип сети: NAT (виртуальная сеть libvirt)
4. Задал host name "rocky-vm" и проверили айпи "192.XX8.XX2.XX9"
5. Выбрал автоматический режим разметки диска - Automatic partitioning
6. Создали юзера и наделили правами админа
7. Машина загрузиласт (похожа на федору)

## 6. Работа в ВМ

1. Узнаем ip командой "ip a" и выдало вот:

###1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
      valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
###2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:34:a8:13 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.209/24 brd 192.168.122.255 scope global dynamic noprefixroute enp1s0
       valid_lft 3451sec preferred_lft 3451sec
    inet6 fe80::5054:ff:fe34:a813/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

адрес ВМ **192.168.122.209**

2. Включаем SSH (сетевой протокол, позволяющий безопасно управлять удаленными компьютерами и серверами через зашифрованный канал связи)
команды:
```
	sudo dnf install -y openssh-server
	sudo systemctl enable --now sshd
```
