# TP5

## I. Préparation du lab

| Machines       |    net1    |    net2    |    net3    |
| -------------- | :--------: | :--------: | :--------: |
| client1.tp5.b1 |     X      | 10.5.2.10  |      X     |
| client2.tp5.b1 |     X      | 10.5.2.11  |      X     |
| router1.tp5.b1 | 10.5.1.254 |     X      | 10.5.12.10 |
| router2.tp5.b1 |     X      | 10.5.2.254 | 10.5.12.11 |
| server1.tp5.b1 | 10.5.1.10  |     X      |      X     |

Sur réseau net 12, j'ai choisie une adresse réseau suivant la logique de nos deux autres réseaux ainsi que de leurs noms.

## II. Lancement et configuration du lab

### Checklist IP VMs

Les Vm utilisé lors de ce TP sont des clones d'un patron dont le SELinus ainsi que la carte NAT sont désactivés, et  qui contient les paquets necéssaires.  
La définition d'IP statiques ainsi que du nom de dommaine et la connection au serveur SSH ne varie pas des précédent TP.

### Checklist IP Routeurs

Pour definir l'IP ainsi que le nom de domaine il faut entre en mode configuration grâce à la ligne

```cisco
R1#conf t
```
Definir l'IP (routeur1 sera utiliser pour les exemples)
```cisco
R1(config)#interface ethernet 0/1
R1(config-if)# ip address 10.5.1.254
R1(config-if)#no shut
```
Definir le nom de domaine
```cisco
R1(config)#hostname routeur1.tp5.b1
```
*j'ai fait un faute a router mais je m'en suis rendu compte assez tard j'ai donc eut la flemme de changer*

### Checklist routes

- router1

  ```cisco
  (config)# ip route 10.5.2.0 255.255.255.0 10.5.12.11
  ```

- server1:

Il faut écrire un fichier pour definir un route statique, ce sera le même chemin pour toute les VMs (*le joie de les cloner*):
```bash
[oui@serveur1 ~]$ sudo nano /etc/sysconfig/network-scripts/route-enp0s3
```
Puis écrire la ligne dans le fichier:  
10.5.2.0/24 via 10.5.2.254 dev enp0s3

- client1
10.5.1.0/24 via 10.5.2.254 dev enp0s3

- client2
10.5.2.0/24 via 10.5.1.254 dev enp0s3


## III. DHCP

### 1. Mise en place du serveur DHCP

Après avoir installer les paquets, il faut rentre la ligne  
```bash
[oui@dhcp-net2 etc]$ sudo nano /etc/dhcp/dhcpd.conf
```
Puis recopier dans le fichier le modèle.  
Ensuite il faut démarer le serveur:
```bash
[oui@dhcp-net2 etc]$ sudo systemctl start dhcpd
```
Il ne se passe rien ça fonctionne donc youpi  
![](http://gif.toutimages.com/images/fete/anniversaire/anniversaire_010.gif)

*niquel je sais pas ce que j'ai mais mon client1 vien de me lacher quel bonheur*  
*sasoul je vois avec toi ou antoine demain mais je n'arrive plus a me connecter en ssh même après reboot*
