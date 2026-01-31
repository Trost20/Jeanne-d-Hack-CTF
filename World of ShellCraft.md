# World of ShellCraft
**Difficulté** : Insane | **Points** : 500 | Catégorie : System / Web

## Solution
Le chall nous donne un site web sur lequel on peut se connecter et créer un compte. Une fois dessus, on peut changer notre avatar. Le serveur n'accepte que les PNG/JPG.

![](Assets/Pasted%20image%2020260131160708.png)
On peut retrouver notre image dans `/avatars/{username}.jpg`. Après avoir passé une éternité à trouver des payloads pour avoir un shell, on peut avoir la lumière divine en testant un username très grand pour voir comment le serveur se comporte. Par exemple avec :

`aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa`

Et à ce moment, quand on upload une image, on a cette erreur :

```
rename(/var/www/html/avatars/a34eb24687740f01d7b04f1da94b596f93750ba3cf73eca48a7bb9c6f2420e2b.jpg,/var/www/html/avatars/aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa.jpg):
File name too long in **/var/www/html/profile.php** on line **94**
```

Donc avant de renommer notre image, celle-ci est stockée dans `/avatars/{hash}.{ext}`. Donc si on upload un fichier PHP, il sera stocké dans `/avatars/{hash}.php`. On a donc une Race Condition qui nous permet d'exécuter du code PHP. Mais on est encore confronté à un petit problème, il faut que l'image soit valide. Pour vérifier qu'il s'agit d'une image, le serveur va juste regarder si les magic bytes sont présents et s'il y a le chunk IHDR.

1 - On cherche d'abord à obtenir le hash.

![](Assets/Pasted%20image%2020260131164026.png)
2 - On trigger la race condition en exécutant une nouvelle fois la requête au-dessus et en actualisant immédiatement cette url :
```
http://world_of_shellcraft.web01.jeanne-hack-ctf.org/avatars/92fc4a95a29d181d748d812e6dde0d27e5ecb28a67ee9475d11e472b01911f64.php
```
Et on obtient le flag

![](Assets/Pasted%20image%2020260131164242.png)
## 🚩 Flag
```
JDHACK{KACHOW_yOu_4RE_sO_F4St!}
```
