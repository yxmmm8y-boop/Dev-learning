# Команда chmod

## Задание 1:

### Создай файл test.txt и сделай так, чтобы:

* владелец мог читать и писать
* остальные только читать

#### Решение:
	yxmmmy@fedora:~/DevOps/GitHub/Dev-learning/linux$ touch test.txt
	yxmmmy@fedora:~/DevOps/GitHub/Dev-learning/linux$ chmod 644 test.txt
	yxmmmy@fedora:~/DevOps/GitHub/Dev-learning/linux$ ls -l test.txt
	-rw-r--r--. 1 yxmmmy yxmmmy 0 Mar 21 17:43 test.txt

## Задание 2:

### Создай файл script.sh и сделай его исполняемым только для владельца

#### Решение:

	yxmmmy@fedora:~/DevOps/GitHub/Dev-learning/linux$ touch script.sh && chmod 700 script.sh 
	yxmmmy@fedora:~/DevOps/GitHub/Dev-learning/linux$ ls -l script.sh
	-rwx------. 1 yxmmmy yxmmmy 0 Mar 21 17:52 script.sh

## Задание 3:

### Создай директорию mydir и дай права:

* владелец — всё
* остальные — только читать и заходить

### Решение:

	yxmmmy@fedora:~/DevOps/GitHub/Dev-learning/linux$ mkdir mydir && chmod 755 mydir
	yxmmmy@fedora:~/DevOps/GitHub/Dev-learning/linux$ ls -ld mydir
	drwxr-xr-x. 1 yxmmmy yxmmmy 0 Mar 21 17:55 mydir

