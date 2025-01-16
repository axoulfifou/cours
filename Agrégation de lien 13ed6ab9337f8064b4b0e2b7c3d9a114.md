# Agrégation de lien :

```bash
Switch1(config)# interface range GigabitEthernet0/1 - 2
Switch1(config-if-range)# channel-group 1 mode active
Switch1(config-if-range)# exit
Switch1(config)# interface Port-channel1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport trunk allowed vlan all
```

Pour vérifier la configuration des liens 

```bash
show etherchannel summary
```

![image.png](image%2064.png)

```bash
sh int status
```