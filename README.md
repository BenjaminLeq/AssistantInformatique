# Assistant Informatique

![Version](https://img.shields.io/badge/version-2.1.1-2f67d8)
![AutoIt](https://img.shields.io/badge/AutoIt-3.3.16.1-17324d)
![Plateforme](https://img.shields.io/badge/plateforme-Windows-0078d4)

**Assistant Informatique** est un utilitaire Windows développé en AutoIt pour collecter rapidement les informations essentielles d'un poste et préparer une demande d'assistance.

L'application peut copier le diagnostic dans le presse-papiers, l'enregistrer dans un fichier texte ou préparer un brouillon dans Outlook classique. Aucun e-mail n'est envoyé automatiquement.

## Aperçu

<p align="center">
  <img alt="1" src="https://github.com/user-attachments/assets/e3b447d5-183f-4b2b-beba-6b0a4837f94e" width="32%">
  <img alt="2" src="https://github.com/user-attachments/assets/99e471aa-cb4e-46b8-b31b-262021c7503f"  width="32%">
  <img alt="3" src="https://github.com/user-attachments/assets/0fd00049-84af-45b4-bc75-647b7a3d170f" width="32%">
</p>

## Fonctionnalités

- collecte actualisable sans redémarrer l'application ;
- détection de Windows 10/11, de la version, du build et de l'architecture ;
- sélection cohérente de la carte réseau active, de son IPv4 et de sa MAC ;
- affichage du processeur, de la mémoire et de l'espace disque disponible ;
- zone libre pour décrire le problème rencontré ;
- copie du rapport complet dans le presse-papiers ;
- export du rapport au format texte UTF-8 ;
- création d'un brouillon Outlook avec la signature configurée ;
- objet d'e-mail personnalisable avec le jeton `{PC}` ;
- prise en charge de plusieurs destinataires séparés par un point-virgule ;
- option permettant d'exclure le numéro de série et la MAC des rapports ;
- paramètres distincts pour chaque utilisateur Windows.

## Informations collectées

| Catégorie | Informations |
|---|---|
| Identité | Utilisateur, domaine de connexion, nom du poste |
| Matériel | Constructeur, modèle, numéro de série, processeur, mémoire |
| Windows | Édition, version, build, architecture, dernier démarrage |
| Réseau | Carte active, adresse IPv4, adresse MAC |
| Stockage | Espace disponible sur le lecteur système |
| Enregistrement Windows | Organisation enregistrée dans `RegisteredOrganization`, lorsqu'elle existe |

> **Organisation Windows n'est pas le domaine.** Il s'agit d'une valeur facultative du registre, renseignée lors de l'installation ou de l'enregistrement de Windows. Elle peut être vide ou obsolète.

## Prérequis

Pour la fonction e-mail :

- Outlook classique pour Windows avec un profil configuré ;
- le nouvel Outlook ne fournit pas l'interface COM utilisée par l'application.

## Utilisation

1. Lancez l'application.
2. Vérifiez les informations collectées ou utilisez **Actualiser**.
3. Ajoutez éventuellement le contexte de la demande.
4. Choisissez une action :
   - **Copier le rapport** pour le presse-papiers ;
   - **Exporter…** pour choisir un fichier texte ;
   - **Préparer l'e-mail** pour ouvrir un brouillon Outlook ;
   - **Paramètres** pour modifier le modèle d'e-mail.

Le statut situé en bas de la fenêtre confirme l'action. Après un export, survolez-le pour afficher le chemin complet du fichier.

## Configuration

`AssistantConfig.ini` sert de modèle lors du premier lancement. La configuration active est ensuite enregistrée ici :

```text
%LOCALAPPDATA%\Assistant Informatique\AssistantConfig.ini
```

Exemple :

```ini
[MailSettings]
EmailDest=support@example.com
EmailSubject=Demande d'assistance - {PC}
EmailIntro=Bonjour,\n\nVoici les informations du poste concerné :
EmailOutro=Cordialement

[Privacy]
IncludeSensitive=1
```

| Clé | Description |
|---|---|
| `EmailDest` | Une ou plusieurs adresses séparées par `;` |
| `EmailSubject` | Objet du message ; `{PC}` est remplacé par le nom du poste |
| `EmailIntro` | Texte placé avant le diagnostic |
| `EmailOutro` | Texte placé après le diagnostic |
| `IncludeSensitive` | `1` inclut le numéro de série et la MAC, `0` les masque du rapport |

Les retours à la ligne sont enregistrés sous la forme `\n` dans le fichier INI.

## Confidentialité et sécurité

- les informations sont collectées localement avec les API Windows, le registre et WMI ;
- l'application n'envoie aucune donnée vers un serveur ;
- l'export est effectué uniquement à l'emplacement choisi par l'utilisateur ;
- le bouton e-mail crée un brouillon et ne déclenche aucun envoi automatique ;
- le numéro de série et la MAC peuvent être exclus depuis les paramètres.

## Dépannage

### Outlook ne s'ouvre pas

La fonction nécessite Outlook classique avec un profil déjà configuré. Le nouvel Outlook n'est pas compatible avec l'automatisation COM utilisée ici.

### L'organisation affiche « Non renseignée »

C'est normal lorsque `RegisteredOrganization` n'existe pas dans le registre Windows. Cette valeur n'a aucun lien avec l'appartenance au domaine.

### Une mauvaise carte réseau est sélectionnée

L'application privilégie une interface IPv4 active disposant d'une passerelle par défaut. Une configuration VPN particulière peut toutefois nécessiter un ajustement de la règle de priorité.

## Contribution

Les rapports de bugs et propositions d'amélioration sont bienvenus. Pour faciliter le diagnostic, indiquez :

- la version d'Assistant Informatique ;
- la version de Windows ;
- le message complet d'erreur ou une capture ;
- les étapes permettant de reproduire le problème.

## Auteur

**Benjamin Lequeux**

## Licence

Aucune licence open source n'est fournie pour le moment. Tous droits réservés.
