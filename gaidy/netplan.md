# netplan

Netplan это утилита для настройки сети. Настройки сети описываются yaml файлами, расположенными в каталоге /etc/netplan. По описанию из этих файлов netplan генерирует настройки для соответсующего бэкенда (на данный момент поддерживаются NetworkManager и systemd-networkd). Более детальная информация представлена на официальном сайте [https://netplan.io](https://netplan.io)

### Подключение к WI-FI

Для того, чтобы подключиться к WI-FI, используя утилиту netplan необходимо изменить файл менеджера подключений.

```
// Переходим в дирикторию с файлом настроек
cd /etc/netplan/
ls -la

// Хорошим тоном является создать бэкап исходного файла настроек до его изменениея
sudo cp vim /etc/netplan/<config_file_name> /etc/netplan/<config_file_name>.bak

// Изменяем файл
sudo vim /etc/netplan/<config_file_name>
```

В файле конфигурации хранится информация о WI-FI сети, к которой нужно подключиться и о настройка WI-FI адаптера. Вот пример подключения к обычной сети. Файл конфигурации пишется на языке yaml

```
network:
    ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
    renderer: networkd
    wifis:
        wlan0:
            optional: true
            dhcp4: true
            access-points:
                <My_WI-FI_name>:
                    password: <password>

```

Пример подключения к корпоративной сети

```
ethernets:
        eth0:
            dhcp4: true
            optional: true
    version: 2
    renderer: networkd
    wifis:
        wlan0:
            optional: true
            dhcp4: true
            access-points:
                <WI-FI_name>:
                    auth:
                        key-management: eap
                        method: peap
                        identity: <user_name>
                        password: <password>
```

Для того, чтобы изменения втупили в силу, необходимо сохранить файл и выполнилнить следующие команды.

```
sudo netplan --debug try // Тестирование файла на ошибки
sudo netplan --debug generate // Чтение настроек
sudo netplan --debug apply // Применение настроек
```

Обычно, если файл написан правительно, после первой команды произойдет подключение к WI-FI сети. Также после включения устройства, произодет попытка подключения в сети, указанной а файле конфигурации.
