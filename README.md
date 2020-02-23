#TP 2 AZZOUZ KARA

##Exercice 1

1. ***Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?***
    ```
   /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin
   :/bin:/usr/games:/usr/local/games:/snap/bin
   ```
2. ***Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans
      votre répertoire personnel ?***       
      Il s'agit de la variable HOME
3. ***Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.***    
    -  ```LANG ``` : locale courante  + encodage des caractères
    -  ```PWD ``` : chemin du répertoire courant
    -  ```OLDPWD ``` : chemin du répertoire dans lequel on se trouvait **avant** d'arriver sur le répertoire courant
    -  ```SHELL ``` : chemin de l'interpréteur de commande courant
    -  ``` _  ```: chemin de la plus récente
4. ***Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe***    
   ```
   MYVAR="owarida"
   echo $MYVAR
   ```

5. ***Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin
      de cette question, tapez la commande exit pour revenir dans votre session initiale.***    
      Il s'agit d'un interpréteur. ```MYVAR``` n'existe pas
6. ***Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.***   
    ```
       ~$ export MYVAR
       ~$ bash
       ~$ printenv MYVAR
        "owarida"
    ```

7. ***Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace.
   Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.***  
    ```
        ~$ export NOMS="KARA AZZOUZ"
        ~$ printenv NOMS
        KARA AZZOUZ
    ```

8. ***Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2
      sont vos deux noms) en utilisant la variable NOMS.***
      ```echo "bonjour à vous 2, $NOMS !"```

9. ***Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande
      unset ?***    
    ```unset``` détruit la variable d'environement tandis que si on lui affecte une valeur vide, la variable existe 
    toujours

10. ***Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre
       dossier personnel d’après bash)***
    ```
        echo "\$HOME = $HOME"
    ```
    
    * ajout de script au PATH : 
    ```
        export PATH=$PATH:~/Desktop/script
    ```
##EXERCICE 2
***Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au
contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par
l’utilisateur ne doit pas s’afficher.***
```
#!/bin/bash
PASSWORD="mdp"
read -p "Entrez un mot de passe : " -s mdp
if [ "$mdp" = "$PASSWORD" ]; then
        echo 'BRAVO'
else
        echo 'Mauvais mot de passe'
fi
```

##Exercice 3
***Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre
est un nombre réel :***
```
#!/bin/bash
   
   function is_number()
   {
           re='^[+-]?[0-9]+([.][0-9]+)?$'
           if ! [[ $1 =~ $re ]] ; then
                   return 1
           else
                   return 0
           fi
   }
   
   is_number $1
   if [ $? -eq 1 ]; then
           echo 'Nombre réel'
   else
           echo 'Nombre pas réel'
   fi
```

##Exercice 4
***Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le
script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”,
où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre
script, le message doit changer automatiquement)***

  ```
        #!/bin/bash
        usersss=$(sudo cat /etc/passwd | cut -d: -f1)
        exists=0
        if [ -z $1 ]; then
                echo 'username not registered'
        else
                while IFS= read -r line; do
                if [ $line = $1 ]; then
                        echo 'user exists'
                        exists=1
                        break
                else
                        continue
                fi
                done <<< $usersss
        fi
        
        if [ $exists -eq 0 ]; then
                echo "user does not exist"
        fi
   ```
## Exercice 5
***Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que
   l’utilisateur saisit toujours un entier naturel).***
   
```
#!/bin/bash
f=1
i=$1
while [ $i -gt 0 ]; do
        f=$(($f * $i))
        i=$(( $i - 1 ))
done
echo "$f"    
```

##Exercice 6
***Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner.
Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).***
```
    #!/bin/bash
    
    rand=$(($RANDOM % 1000))
    prix=1001
    while true; do
            read -p "Entrez un prix : " prix
    
            if [ $prix -lt $rand ]; then
                    echo "C'est plus !"
            elif [ $prix -gt $rand ]; then
                   echo "C'est moins"
            else
                    echo 'Gagné !'
                    break
            fi
    done
```

##Exercice 7
1. ***Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max
et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres
sont bien des entiers.***
    ```
    #!/bin/bash
    
    function is_number()
    {
            re='^[+-]?[0-9]+([.][0-9]+)?$'
            if ! [[ $1 =~ $re ]] ; then
                    return 1
            else
                    return 0
            fi
    }
    
    function quit()
    {
            echo "$1 not an integer"
            exit 1
    }
    
    min=100
    max=-100
    
    is_number $1
    if [ $? -eq 1 ]; then
            quit $1
    fi
    
    is_number $2
    if [ $? -eq 1 ]; then
            quit $2
    fi
    
    is_number $3
    if [ $? -eq 1 ]; then
            quit $3
    fi
    
    if [ $1 -lt $min ]; then
            min=$1
    fi
    
    if [ $2 -lt $min ]; then
            min=$2
    fi
    
    if [ $3 -lt $min ]; then
            min=$3
    fi
    
    if [ $1 -gt $max ]; then
            max=$1
    fi
    
    if [ $2 -gt $max ]; then
            max=$2
    fi
    
    if [ $3 -gt $max ]; then
            max=$3
    fi
    
    echo "Minimum : $min
    Maximum : $max
    Moyenne : $((($1+$2+$3)/3))"

    ```

2. ***Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT)***
    ```
    
    ```

3. ***Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et
stockées au fur et à mesure dans un tableau.***

    ```
    
    ```