# Assistant Informatique

![Version](https://img.shields.io/badge/version-2.1.1-2f67d8)
![AutoIt](https://img.shields.io/badge/AutoIt-3.3.16.1-17324d)
![Plateforme](https://img.shields.io/badge/plateforme-Windows-0078d4)

**Assistant Informatique** est un utilitaire Windows développé en AutoIt pour collecter rapidement les informations essentielles d'un poste et préparer une demande d'assistance.

L'application peut copier le diagnostic dans le presse-papiers, l'enregistrer dans un fichier texte ou préparer un brouillon dans Outlook classique. Aucun e-mail n'est envoyé automatiquement.

## Aperçu

![Interface d'Assistant Informatique](APERCU-INTERFACE.png)

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

Pour exécuter les sources :

- Windows 10 ou Windows 11 ;
- [AutoIt v3](https://www.autoitscript.com/site/autoit/downloads/) ;
- SciTE4AutoIt3 recommandé pour l'édition et la compilation.

Pour la fonction e-mail :

- Outlook classique pour Windows avec un profil configuré ;
- le nouvel Outlook ne fournit pas l'interface COM utilisée par l'application.

## Installation depuis les sources

1. Téléchargez ou clonez le dépôt.
2. Conservez le dossier `icones` à côté du script.
3. Ouvrez `Assistant Informatique v2.au3` dans SciTE.
4. Appuyez sur `F5` pour lancer l'application.

Arborescence principale :

```text
Assistant-Informatique-v2/
├── Assistant Informatique v2.au3
├── AssistantConfig.ini
├── APERCU-INTERFACE.png
├── AUDIT-ET-REFONTE.md
├── README.md
└── icones/
    ├── AssistantInformatique.ico
    ├── close.ico
    ├── confirm.ico
    ├── copy.ico
    ├── download.ico
    ├── mail.ico
    └── parameter.ico
```

Les chemins des ressources sont construits depuis `@ScriptDir`. L'application peut donc être lancée depuis un raccourci ou un autre dossier courant.

## Compilation

Dans SciTE4AutoIt3 :

1. ouvrez `Assistant Informatique v2.au3` ;
2. lancez d'abord **Au3Check** pour contrôler la syntaxe ;
3. appuyez sur `F7` pour compiler ;
4. conservez le fichier `AssistantConfig.ini` et le dossier `icones` avec l'exécutable.

Les directives AutoIt3Wrapper intégrées au script configurent une compilation x64, l'icône et les informations de version.

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

Avant une diffusion en entreprise, il est recommandé de signer numériquement l'exécutable et de publier son empreinte SHA-256.

## Dépannage

### Les icônes ou l'icône de l'application sont absentes

Vérifiez que le dossier `icones` est placé au même niveau que le script ou l'exécutable.

### Outlook ne s'ouvre pas

La fonction nécessite Outlook classique avec un profil déjà configuré. Le nouvel Outlook n'est pas compatible avec l'automatisation COM utilisée ici.

### L'organisation affiche « Non renseignée »

C'est normal lorsque `RegisteredOrganization` n'existe pas dans le registre Windows. Cette valeur n'a aucun lien avec l'appartenance au domaine.

### Une mauvaise carte réseau est sélectionnée

L'application privilégie une interface IPv4 active disposant d'une passerelle par défaut. Une configuration VPN particulière peut toutefois nécessiter un ajustement de la règle de priorité.

## Tests recommandés

- Windows 10 et Windows 11 ;
- poste joint à un domaine et poste en groupe de travail ;
- Ethernet, Wi-Fi, VPN et poste hors ligne ;
- Outlook classique avec et sans signature HTML ;
- compte standard et compte administrateur ;
- mise à l'échelle Windows à 100 %, 125 % et 150 % ;
- exécution depuis un dossier protégé ou un partage réseau.

## Feuille de route

- export JSON pour intégration à un outil ITSM ;
- tests réseau ciblés : DNS, passerelle, proxy et connectivité ;
- ouverture ou création directe d'un ticket GLPI, Jira Service Management ou ServiceNow ;
- signature numérique et mécanisme de mise à jour contrôlé ;
- prise en charge du thème sombre et du redimensionnement dynamique.

## Historique récent

| Version | Évolution principale |
|---|---|
| 2.1.1 | Statut d'export corrigé et libellé « Organisation Windows » clarifié |
| 2.1.0 | Boutons textuels, auteur ajouté et champ « Disque libre » simplifié |
| 2.0.2 | Compatibilité avec AutoIt 3.3.16.1 pour les styles de boutons |
| 2.0.1 | Correction des collisions avec `ColorConstants.au3` |
| 2.0.0 | Refonte complète de l'interface et de la collecte |

Le détail de l'audit et des corrections est disponible dans [`AUDIT-ET-REFONTE.md`](AUDIT-ET-REFONTE.md).

## Contribution

Les rapports de bugs et propositions d'amélioration sont bienvenus. Pour faciliter le diagnostic, indiquez :

- la version d'Assistant Informatique ;
- la version de Windows et d'AutoIt ;
- le message complet d'Au3Check ou une capture ;
- les étapes permettant de reproduire le problème.

## Auteur

**Benjamin Lequeux**

## Licence

Aucune licence open source n'est fournie pour le moment. Tous droits réservés.
