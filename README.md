# LAB-17-Cracker-OWASP-Uncrackable-Android-Level-3

Étape 1 : Analyse de la couche Java 

L'analyse du code Java avec JADX montre que l'application gère la vérification via une bibliothèque native C/C++.

La classe MainActivity définit une chaîne statique nommée xorkey contenant la valeur "pizzapizzapizzapizzapizz". Elle est passée à la méthode native init().

<img width="1488" height="747" alt="image" src="https://github.com/user-attachments/assets/eb1234c7-ae95-466f-9eb6-18f9752ab663" />

Étape 2 : Localisation de la logique de vérification native

La bibliothèque libfoo.so a été extraite et chargée dans Ghidra.

<img width="1489" height="748" alt="image" src="https://github.com/user-attachments/assets/b25b531a-af42-4ac5-ac1d-d6caa37486e8" />

La fonction JNI exportée Java_sg_vantagepoint_uncrackable3_CodeCheck_bar contient la boucle de validation.

<img width="1249" height="617" alt="image" src="https://github.com/user-attachments/assets/e9733c5f-d993-4673-83b7-7c4018e1f24a" />

Étape 3 : Contournement de l'obfuscation

La fonction FUN_001012c0 remplit le buffer local_48. Malgré une forte obfuscation du flux de contrôle (fausses allocations de mémoire), la fin de la fonction révèle le vrai payload.

<img width="1171" height="311" alt="image" src="https://github.com/user-attachments/assets/56f1932f-2f88-48fd-bb92-2975f16697b8" />

Étape 4 : Correction de l'Endianness 

L'architecture ARM (émulateur Android) stocke la mémoire au format Little Endian. Les entiers 64 bits extraits de Ghidra sont donc inversés. Il faut inverser les octets (par paires) de chaque bloc pour obtenir le payload réel :

0x1549170f1311081d -> 1d0811130f174915        
0x15131d5a1903000d -> 0d0003195a1d1315        
0x14130817005a0e08 -> 080e5a0017081314      

Payload chiffré final : 1d0811130f1749150d0003195a1d1315080e5a0017081314

Étape 5 : Déchiffrement du Flag

Avec la clé Java et le payload hexadécimal corrigé, le secret a été déchiffré statiquement avec CyberChef.

L'entrée brute a été convertie avec l'opération "From Hex".
Le résultat a subi une opération "XOR" avec la clé UTF-8 "pizzapizzapizzapizzapizz".
Flag final : making owasp great again

<img width="1600" height="742" alt="image" src="https://github.com/user-attachments/assets/d1407fd7-40b2-40d8-b0e0-60205f580b1e" />

esseyer le code 

<img width="420" height="848" alt="image" src="https://github.com/user-attachments/assets/0c044da5-be0c-4560-b58b-d6b3444ec075" />

