### Subnetting
Conversion décimal / binaire
Un nombre est composée de chiffre. et est exprimé dans une base (binaire, hexa...)
Chaque chiffre a une place dans le nombre -> rang
base 10 ex -> 6⁴2³7²1¹3⁰ -> 6 x 10⁴ + 2 x 10³ + 7 x 10² + 1 x 10¹ + 3 x 10⁰

#### Conversion binaire / décimal
1⁷1⁶0⁵1⁴0³1²0¹1⁰ = 1 x 2⁷ + 1 x 2⁶ + 1 x 2⁴ + 1 x 2² + 1 x 2⁰
128 + 64 + 16 + 4 +1 = 213


187 - 128(2⁷) = 59
59 - 32 (2⁵) = 27
27 - 16 (2⁴) = 11
11 - 8 (2³) = 3
3 - 2 (2¹) = 1
1 - 1 (2⁰) = 0


| poids     | 1   | 2   | 4   | 8   | 16  | 32  | 64  | 128 | 256 | 512 | 1024 | 2048 | 4096 | 8192 |
| --------- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | ---- | ---- | ---- | ---- |
| Puissance | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10   | 11   | 12   | 13   |


#### Méthode
Soit le réseau 192.168.10.0/24 (255.255.255.0)
on veut des SR d'au moins 25 IP
1. On ajoute 2 au besoin (@Réseau & @Broadcast)
2. Combien de bits pour coder 27 possibilités -> 5 car 2⁵ = 32 ce qui est la valeur binaire égale ou supérieur à 27
3. Masque : on réserve 5 bits à 0 dans la partie classful (vers la gauche) avec des 1
255.255.255.224 = 1111 1111 . 1111 1111 . 1111 1111 . 1110 0000
pour le pas 256 - 224 = 32 -> les SR sont de 32 en 32 sur le 4e octet
IP directed Broadcast
