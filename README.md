# TP3
Remarque globale: pour créer et éditer un fichier il faut faire << nano/vi "nom du fichier" >> dans le terminal et après on se retouve dans un éditeur et il faut mettre le contenu du fichier. Quand on fini d'éditer on enregistre et on quitte. pour exécuter on fait << bash "nom du fichier" >>
1. Exercice : paramètres
code:
#!/bin/bash
echo "Bonjour, vous avez rentré $1 paramètres."
echo "Le nom du script est $0"
echo "Le 3ème paramètre est $3"
echo "Voici la liste des paramètres : $@"
Pour exécuter le fichier il faut faire: ./analyse.sh param1 param2 param3



2. Exercice : vérification du nombre de paramètres
code:
#!/bin/bash
if [ $# -ne 2 ]
     then echo "Vous devez entrer exactement 2 paramètres."
    exit 1
   else
    mot1="$1"
    mot2="$2"
    CONCAT="$mot1$mot2"
    echo $CONCAT
fi
Pour exécuter le fichier il faut faire: ./concat.sh mot1 mot2



4. Exercice : argument type et droits
code:
#!/bin/bash

if [ $# -ne 1 ]; then
    echo "Usage: $0 <chemin du fichier>"
    exit 1
fi

fichier="$1"

if [ ! -e "$fichier" ]; then
    echo "Le fichier $fichier n'existe pas."
    exit 1
fi

type=""
permissions=""
if [ -d "$fichier" ]; then
    type="répertoire"
    permissions="accessible par root en lecture écriture exécution"
else
    type="fichier ordinaire"
    if [ -s "$fichier" ]; then
        permissions="accessible par $(whoami) en lecture."
    else
        permissions="vide et inaccessible en lecture."
    fi
fi

echo "Le fichier $fichier est un $type"
echo "\"$fichier\" est $permissions"
Pour exécuter le fichier il faut faire: ./test-fichier.sh /etc



4. Exercice : Afficher le contenu d’un r´epertoire
code:
#!/bin/bash
if [ $# -ne 1 ]; then
    echo "Usage: $0 <répertoire>"
    exit 1
fi

repertoire="$1"

if [ ! -d "$repertoire" ]; then
    echo "$repertoire n'est pas un répertoire valide."
    exit 1
fi

echo "####### fichiers dans $repertoire/"
for fichier in "$repertoire"/*; do
    if [ -f "$fichier" ]; then
        echo "$fichier"
    fi
done

echo "####### répertoires dans $repertoire/"
for sousrepertoire in "$repertoire"/*; do
    if [ -d "$sousrepertoire" ]; then
        echo "$sousrepertoire"
    fi
done
Pour exécuter le fichier il faut faire: ./afficher-repertoire.sh /chemin/du/repertoire




5. Exercice : Lister les utilisateurs
code 1:
#!/bin/bash
awk -F: '$3 > 100 {print $1}' /etc/passwd

code2:
#!/bin/bash
cut -d: -f1,3 /etc/passwd | awk -F: '$2 > 100 {print $1}'
Pour exécuter le fichier il faut faire:



6. Exercice : Mon utilisateur existe-t-il ?
code: 
#!/bin/bash

if [ $# -ne 1 ]; then
    echo "Usage: $0 <login ou UID>"
    exit 1
fi

recherche="$1"

if id "$recherche" &>/dev/null; then
    echo "L'utilisateur existe et a un UID : $(id -u "$recherche")"
else
    echo "L'utilisateur n'existe pas."
fi
Pour exécuter le fichier il faut faire: ./lister-utilisateurs.sh 



7. Exercice : Création utilisateur
code:
#!/bin/bash

if [ "$(id -u)" -ne 0 ]; then
    echo "Ce script doit être exécuté en tant que superutilisateur (root)."
    exit 1
fi

echo "Création d'un compte utilisateur :"
read -p "Login : " login
read -p "Nom : " nom
read -p "Prénom : " prenom
read -p "UID : " uid
read -p "GID : " gid
read -p "Commentaires : " commentaires

if [ -d "/home/$login" ]; then
    echo "Le répertoire home existe déjà pour cet utilisateur."
    exit 1
fi

useradd -c "$commentaires" -d "/home/$login" -m -u "$uid" -g "$gid" "$login"

echo "Utilisateur créé avec succès."
Pour exécuter le fichier il faut faire: ./creer-utilisateur.sh




8. Exercice : Lecture au clavier
code:
#!/bin/bash

if [ $# -ne 1 ]; then
    echo "Usage: $0 <répertoire>"
    exit 1
fi

repertoire="$1"

if [ ! -d "$repertoire" ]; then
    echo "Le répertoire $repertoire n'existe pas."
    exit 1
fi

for fichier in "$repertoire"/*; do
    if [ -f "$fichier" ]; then
        if file "$fichier" | grep -q "text"; then
            read -p "Voulez-vous visualiser le fichier $fichier ? (o/n) " choix
            if [ "$choix" = "o" ]; then
                more "$fichier"
            fi
        fi
    fi
done
Pour exécuter le fichier il faut faire: ./lire-fichiers.sh /chemin/du/repertoire




9. Exercice : Appréciation
code:
#!/bin/bash

read -p "Saisissez une note : " note

if ((note >= 16 && note <= 20)); then
    echo "Très bien"
elif ((note >= 14 && note < 16)); then
    echo "Bien"
elif ((note >= 12 && note < 14)); then
    echo "Assez bien"
elif ((note >= 10 && note < 12)); then
    echo "Moyen"
elif ((note < 10)); then
    echo "Insuffisant"
else
Pour exécuter le fichier il faut faire:
    echo "Note invalide"
fi
Pour exécuter le fichier il faut faire: ./appreciation-note.sh

Remarque Finale:

  *Les exercices 3, 4 et 8 sont effectivement plus complexes que les précédents en raison de leur nature spécifique. Voici quelques observations sur ces exercices :

  -Exercice Argument type et droits : Il requiert la manipulation de fichiers et la vérification de leur type et de leurs permissions, ce qui peut était un peu complexe.

  -Exercice Afficher le contenu d'un répertoire : Cet exercice impliquait la gestion des répertoires et des fichiers ce qui nécessitede comprendre la manipulation des fichiers.

  -Exercice Lecture au clavier : Il implique l'interaction avec l'utilisateur pour la visualisation des fichiers texte c'était assez compliqué.

Source: chatgpt, cours , https://tldp.org/ , https://www.shellscript.sh/ et https://www.gnu.org/software/bash/manual/bash.html  
  

