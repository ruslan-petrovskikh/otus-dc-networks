## **VxLAN. L2VNI.**

### **Цель:**

 1) Настроить Overlay на основе VxLAN EVPN для L2-связности между клиентами.

 ### **План:**

1) Настроить iBGP peering между Leaf и Spine в AF l2vpn evpn.
2) Настроить связность между клиентами в первой и второй зоне и убедиться в её наличии.
3) Зафиксировать в документации - план работы, адресное пространство, схему сети, конфигурацию устройств.

### **Ход работы.**

1) **На устройствах выполнены следующие настройки:**
    
    - Spines:
        * p2p-адресация;
        * loopback1 интерфейсы;
        * протокол IS-IS (из настроек убран BFD, т.к. с ним IS-IS не поднимается из-за ограничения виртуализации);
        * протокол iBGP;
        * протокол BFD (из-за ограничений виртуализации сессии BFD не поднимаются).
    
    - Leafs:
        * p2p-адресация;
        * loopback1 интерфейсы;
        * loopback2 интерфейсы;
        * протокол IS-IS (из настроек убран BFD, т.к. с ним IS-IS не поднимается из-за ограничения виртуализации);
        * протокол iBGP;
        * протокол BFD (из-за ограничений виртуализации сессии BFD не поднимаются);
        * VLAN;
        * VNI.

2) **Документация.**

 **Адресное пространство:**
|    IP-подсеть     |      IP-адрес     |      Устройство     |     Интерфейс      |     Назначение     |     VLAN      |     VNI     |
|:-----------------:|:-----------------:|:-------------------:|:------------------:|:------------------:|:-------------:|:-----------:|
| **10.1.241.0/31** |         -         |          -          |          -         |         P2P        |       -       |      -      |
|                   |    10.1.241.0     |      DC01-S01       |        E1/1        |          -         |       -       |      -      |
|                   |    10.1.241.1     |      DC01-L01       |        E1/7        |          -         |       -       |      -      |
| **10.1.241.2/31** |         -         |          -          |          -         |         P2P        |       -       |      -      |
|                   |    10.1.241.2     |      DC01-S01       |        E1/2        |          -         |       -       |      -      |
|                   |    10.1.241.3     |      DC01-L02       |        E1/7        |          -         |       -       |      -      |
| **10.1.241.4/31** |         -         |          -          |          -         |         P2P        |       -       |      -      |
|                   |    10.1.241.4     |      DC01-S01       |        E1/3        |          -         |       -       |      -      |
|                   |    10.1.241.5     |      DC01-L03       |        E1/7        |          -         |       -       |      -      |
| **10.1.242.0/31** |         -         |          -          |          -         |         P2P        |       -       |      -      |
|                   |    10.1.242.0     |      DC01-S02       |        E1/1        |          -         |       -       |      -      |
|                   |    10.1.242.1     |      DC01-L01       |        E1/8        |          -         |       -       |      -      |
| **10.1.242.2/31** |         -         |          -          |          -         |         P2P        |       -       |      -      |
|                   |    10.1.242.2     |      DC01-S02       |        E1/2        |          -         |       -       |      -      |
|                   |    10.1.242.3     |      DC01-L02       |        E1/8        |          -         |       -       |      -      |
| **10.1.242.4/31** |         -         |          -          |          -         |         P2P        |       -       |      -      |
|                   |    10.1.242.4     |      DC01-S02       |        E1/3        |          -         |       -       |      -      |
|                   |    10.1.242.5     |      DC01-L03       |        E1/8        |          -         |       -       |      -      |
| **10.1.253.1/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.253.1     |      DC01-S01       |        Lo1         |          -         |       -       |      -      |
| **10.1.253.2/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.253.2     |      DC01-S02       |        Lo1         |          -         |       -       |      -      |
| **10.1.254.1/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.254.1     |      DC01-L01       |        Lo1         |          -         |       -       |      -      |
| **10.1.254.2/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.254.2     |      DC01-L02       |        Lo1         |          -         |       -       |      -      |
| **10.1.254.3/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.254.3     |      DC01-L03       |        Lo1         |          -         |       -       |      -      |
| **10.1.255.1/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.255.1     |      DC01-L01       |        Lo2         |          -         |       -       |      -      |
| **10.1.255.2/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.255.2     |      DC01-L02       |        Lo2         |          -         |       -       |      -      |
| **10.1.255.3/32** |         -         |          -          |         -          |       Loopback     |       -       |      -      |
|                   |    10.1.255.3     |      DC01-L03       |        Lo2         |          -         |       -       |      -      |
|  **10.1.1.0/24**  |         -         |          -          |         -          |       Network      |      101      |   5550101   |
|                   |    10.1.1.11      |     DC01-SRV01      |      e0(ens3)      |          -         |       -       |      -      |
|                   |    10.1.1.12      |     DC01-SRV03      |      e0(ens3)      |          -         |       -       |      -      |
|  **10.1.2.0/24**  |         -         |          -          |         -          |       Network      |      102      |   5550102   |
|                   |    10.1.2.11      |     DC01-SRV02      |      e0(ens3)      |          -         |       -       |      -      |
|                   |    10.1.2.12      |     DC01-SRV04      |      e0(ens3)      |          -         |       -       |      -      |


**Схема сети:**

![hw5_img1](attach/HW5_topology.png)


**Конфигурация устройств:**

* [DC01-L01](attach/DC01-L01.conf)
* [DC01-L02](attach/DC01-L02.conf)
* [DC01-L03](attach/DC01-L03.conf)
* [DC01-S01](attach/DC01-S01.conf)
* [DC01-S02](attach/DC01-S02.conf)


3) **Проверка состояний коммутаторов:**

    - DC01-L01:

        ![hw5_img2](attach/L01_bgp_nve.png)
        ![hw5_img3](attach/L01_mac.png)

    - DC01-L02:

        ![hw5_img4](attach/L02_bgp_nve.png)
        ![hw5_img5](attach/L02_mac.png)

    - DC01-L03:

        ![hw5_img6](attach/L03_bgp_nve1.png)
        ![hw5_img7](attach/L03_bgp_nve2.png)
        ![hw5_img8](attach/L03_mac.png)

    - DC01-S01:

        ![hw5_img9](attach/S01_bgp.png)

    - DC01-S02:

        ![hw5_img10](attach/S02_bgp.png)


4) **Проверка L2-связности между устройствами:**

 - проверка связности в VNI 5550101:
    *  SRV01:

    ![hw4_img11](attach/SRV01_ping.png)

    *  SRV03:

    ![hw4_img11](attach/SRV03_ping.png)

- проверка связности в VNI 5550102:
    *  SRV02:

    ![hw4_img11](attach/SRV02_ping.png)

    *  SRV04:

    ![hw4_img11](attach/SRV04_ping.png)
