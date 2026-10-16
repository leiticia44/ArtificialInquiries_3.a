# ArtificialInquiries_3a — Impressions à chaud

Outil numérique pour l'exercice **Ex3a « Rough Impressions »** du vademecum *Artificial Inquiries*.

> Alcaras, G., Ricci, D., Prinetti, T. & de Vries, Z. (2025). *Artificial Inquiries. A vademecum for workers in the age of AI*. Éditions Annexes, médialab Sciences Po. https://hal.science/hal-05327878

## 1. L'exercice

L'Ex3a ouvre l'exercice 3 « Taking Stock » (faire le point), dans le bloc 01 *Qualifying*. Avant de relire son historique de conversations avec une IA (ChatGPT, Claude, Le Chat…), on note **à chaud**, en 5 minutes, ce que l'on croit savoir de son propre usage.

Consignes du vademecum (pages 16-17) :

1. **Mettre un minuteur de 5 minutes** et réfléchir à son usage global, **sans regarder son historique**.
   - Lister les tâches confiées à l'IA avec un **pourcentage approximatif** pour chacune (ex. : un tiers de traduction, le reste en réponses à des mails).
   - Estimer la **fréquence** d'usage : tous les jours, chaque semaine, moins souvent.
   - Estimer la **structure** des conversations : échanges courts (une question, une réponse), conversations longues (plusieurs allers-retours), dialogues étendus (sur plus d'une journée).
2. **Se souvenir de 2 ou 3 conversations marquantes**, les décrire et dire pourquoi elles ont marqué.
3. **Dessiner** puis **écrire** ses impressions dans les deux cadres prévus.

Ces impressions servent ensuite de point de comparaison avec l'historique réel, dans l'Ex3b « Rolling the Dice ».

### Le formulaire papier

| Consignes (page 16) | Formulaire à remplir (page 17) |
|---|---|
| ![Consignes de l'Ex3a, page 16 du vademecum](ex3a_consignes.png) | ![Formulaire papier de l'Ex3a, page 17 du vademecum](ex3a_formulaire.png) |

*Copies d'écran du vademecum* Artificial Inquiries *(CC BY-NC).*

## 2. Ce que fait l'outil

| Consigne du vademecum | Fonction dans l'outil |
|---|---|
| Minuteur de 5 minutes | Compte à rebours avec barre de progression, pause et remise à zéro |
| Tâches et pourcentages | Liste de tâches avec curseurs, total contrôlé (100 %), bouton d'ajustement automatique, graphique en anneau |
| Fréquence d'usage | Boutons à choix unique |
| Structure des conversations | Trois curseurs liés (total toujours 100 %) et barre empilée |
| 2 ou 3 conversations marquantes | Trois fiches : titre, description, ressentis (enthousiasme, frustration, amusement, inquiétude, surprise, doute éthique) |
| Cadre « Sketch your impressions » | Zone de dessin à la souris ou au doigt |
| Cadre « Write down your impressions » | Zone de texte libre |
| Garder une trace | Export en Markdown avec métadonnées (YAML), à copier dans Zotero, Obsidian ou un LLM Wiki |

## 3. Solutions techniques

### Version actuelle : page web autonome

- **Un seul fichier** `impressions-a-chaud.html` : HTML, CSS et JavaScript sans framework ni serveur.
- **Graphiques** en CSS pur (`conic-gradient` pour l'anneau, `flex` pour la barre empilée) : aucune bibliothèque.
- **Dessin** avec l'API `<canvas>` et les *Pointer Events* (souris, stylet et tactile).
- **Sauvegarde** dans le navigateur avec `localStorage` : les réponses ne quittent pas l'ordinateur.
- **Export** en Markdown avec en-tête YAML, copié avec l'API `navigator.clipboard`.
- **Thème clair et sombre** automatique (`prefers-color-scheme`).

Pour l'utiliser : ouvrir `impressions-a-chaud.html` dans un navigateur, ou activer **GitHub Pages** sur le dépôt.

### Évolution prévue : stocker les réponses dans Omeka S

Pour rassembler les réponses de plusieurs participants (travail en cohorte, comme le recommande le vademecum), les données peuvent être enregistrées dans **Omeka S** :

- un **modèle de ressource** (*resource template*) « Ex3a – Impressions » reprenant les classes du diagramme (`diagram_class.md`) ;
- chaque session devient un **contenu** (*item*) Omeka S, lié aux tâches et aux conversations marquantes ;
- l'outil envoie les données via l'**API REST d'Omeka S** (`/api/items`, avec une clé d'API) ;
- le croquis est stocké comme **média** (image PNG) attaché au contenu.

Pile technique pour l'installation locale : **Apache + PHP + MySQL** (voir la [documentation d'installation d'Omeka S](https://omeka.org/s/docs/user-manual/install/)).

## 4. Modèle de données

Voir [`diagram_class.md`](diagram_class.md) (diagramme de classes Mermaid).

## 5. Traduction du modèle dans Omeka S

### Vocabulaire `artinq`

Le fichier [`artificial_inquiries.ttl`](artificial_inquiries.ttl) définit le vocabulaire **Artificial Inquiries** :

- préfixe : `artinq`
- espace de noms : `https://humanum-p8.fr/omk_artificialinquiries/onto/artinq#`

| Classe du diagramme | Classe RDF |
|---|---|
| `Participant` | `artinq:Participant` |
| `SessionImpressions` | `artinq:SessionImpressions` |
| `TacheDeleguee` | `artinq:TacheDeleguee` |
| `ConversationMarquante` | `artinq:ConversationMarquante` |

| Attribut ou relation du diagramme | Propriété |
|---|---|
| `Participant.metier` | `artinq:metier` |
| Participant → Session | `artinq:aPourParticipant` |
| `dureeMinuteur`, `frequence`, `llmPrincipal` | `artinq:dureeMinuteur`, `artinq:frequence`, `artinq:llmPrincipal` |
| Session → TacheDeleguee | `artinq:aPourTache` |
| `TacheDeleguee.pourcentage` | `artinq:pourcentage` |
| `StructureConversations` (3 parts) | `artinq:partEchangesCourts`, `artinq:partConversationsLongues`, `artinq:partDialoguesEtendus` |
| Session → ConversationMarquante | `artinq:aPourConversationMarquante` |
| `ConversationMarquante.rang`, `ressentis` | `artinq:rang`, `artinq:ressenti` |
| `Impression.texte` | `artinq:impression` |
| titres, dates, descriptions | `dcterms:title`, `dcterms:date`, `dcterms:description` |
| `Croquis` | média (image PNG) attaché à la session |

### Modèles de ressource

Le dossier [`resource-template/`](resource-template/) contient les modèles de ressource exportés d'Omeka S :

| Fichier | Classe |
|---|---|
| `Ex3a_Session_impressions.json` | `artinq:SessionImpressions` |
| `Ex3a_Tache_deleguee.json` | `artinq:TacheDeleguee` |
| `Ex3a_Conversation_marquante.json` | `artinq:ConversationMarquante` |
| `Ex3a_Participant.json` | `artinq:Participant` |

### Base de données

`omk.sql` est l'export de la base MySQL de l'installation Omeka S locale, avec les contenus de test.

## 6. Structure du dépôt

```
ArtificialInquiries_3a/
├── README.md                  ← ce fichier
├── diagram_class.md           ← diagramme de classes (Mermaid)
├── impressions-a-chaud.html   ← l'outil
├── ex3a_consignes.png         ← consignes papier (p. 16)
├── ex3a_formulaire.png        ← formulaire papier (p. 17)
├── artificial_inquiries.ttl   ← vocabulaire artinq
├── resource-template/         ← modèles de ressource Omeka S (JSON)
└── omk.sql                    ← export de la base Omeka S
```

## Licence

Le vademecum *Artificial Inquiries* est publié sous licence CC BY-NC.
