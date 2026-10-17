# Домашня робота № 1. Основи комп’ютерних мереж

**ПІБ:** Владислав Воробйов

## 1. Дослідження мережі

```sh
ip a
```

```text
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eno1: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether a0:d3:c1:28:a4:f8 brd ff:ff:ff:ff:ff:ff
    altname enp0s25
    altname enxa0d3c128a4f8
3: enp1s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN group default qlen 1000
    link/ether a0:d3:c1:28:a4:f9 brd ff:ff:ff:ff:ff:ff
    altname enxa0d3c128a4f9
4: tailscale0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1280 qdisc fq_codel state UNKNOWN group default qlen 500
    link/none 
    inet 100.95.176.26/32 scope global tailscale0
       valid_lft forever preferred_lft forever
    inet6 fd7a:115c:a1e0::4f2b:b01b/128 scope global 
       valid_lft forever preferred_lft forever
    inet6 fe80::b3fc:7bcb:39ad:8a2a/64 scope link stable-privacy proto kernel_ll 
       valid_lft forever preferred_lft forever
5: wlp8s0u4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 50:3e:aa:54:53:db brd ff:ff:ff:ff:ff:ff
    altname wlx503eaa5453db
    inet 192.168.50.227/24 brd 192.168.50.255 scope global dynamic noprefixroute wlp8s0u4
       valid_lft 68145sec preferred_lft 68145sec
    inet6 fe80::76e0:32c7:3778:35d/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```

| Параметр | Значення |
| --- | --- |
| Інтерфейс | `wlp8s0u4` |
| Тип підключення | Wi-Fi |
| MAC-адреса | `50:3e:aa:54:53:db` |
| IP-адреса | `192.168.50.227`; мережевий рівень OSI, рівень 3 |
| Маска підмережі | `255.255.255.0` (`/24`); мережевий рівень OSI, рівень 3 |
| Gateway | `192.168.50.1` |
| Broadcast | `192.168.50.255` |
| Hosts | `192.168.50.1–192.168.50.254`; 254 доступні адреси вузлів |

## 2. Дослідження маршрутизації

```sh
ip r
```

```text
default via 192.168.50.1 dev wlp8s0u4 proto dhcp src 192.168.50.227 metric 600 
192.168.50.0/24 dev wlp8s0u4 proto kernel scope link src 192.168.50.227 metric 600
```

**Протокол отримання IP-адреси — DHCP:** у `ip a` адресу позначено `dynamic`, а в `ip r` маршрут за замовчуванням має позначку `proto dhcp`.

## 3. Знаходження публічної IP-адреси

```sh
curl ifconfig.io
```

```text
85.114.196.20
```

Локальна адреса `192.168.50.227` і публічна адреса `85.114.196.20` не збігаються. Локальна адреса є приватною та використовується всередині домашньої мережі. У типовій домашній IPv4-мережі роутер виконує NAT: замінює приватну адресу джерела адресою зовнішнього підключення та передає відповіді відповідному пристрою. Це пояснює, чому зовнішній сервіс бачить іншу адресу.
