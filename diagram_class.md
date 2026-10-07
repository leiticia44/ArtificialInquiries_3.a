# Diagramme de classes — Ex3a « Rough Impressions »

Modèle des données gérées par l'outil `impressions-a-chaud.html`. À coller dans [mermaid.live](https://mermaid.live/) pour le visualiser (GitHub l'affiche aussi directement).

```mermaid
classDiagram
    direction LR

    class Participant {
        +String id
        +String pseudo
        +String metier
    }

    class SessionImpressions {
        +String id
        +Date date
        +int dureeMinuteur = 300
        +Frequence frequence
        +String llmPrincipal
        +demarrerMinuteur()
        +totalTaches() int
        +ajusterA100()
        +exporterMarkdown() String
    }

    class TacheDeleguee {
        +String nom
        +int pourcentage
    }

    class StructureConversations {
        +int echangesCourts
        +int conversationsLongues
        +int dialoguesEtendus
        +verifierTotal() bool
    }

    class ConversationMarquante {
        +int rang
        +String titre
        +String description
        +Ressenti[] ressentis
    }

    class Croquis {
        +String imagePNG
        +effacer()
    }

    class Impression {
        +String texte
    }

    class Frequence {
        <<enumeration>>
        PLUSIEURS_FOIS_PAR_JOUR
        TOUS_LES_JOURS
        CHAQUE_SEMAINE
        MOINS_SOUVENT
    }

    class Ressenti {
        <<enumeration>>
        ENTHOUSIASME
        FRUSTRATION
        AMUSEMENT
        INQUIETUDE
        SURPRISE
        DOUTE_ETHIQUE
    }

    Participant "1" --> "*" SessionImpressions : remplit
    SessionImpressions "1" *-- "1..*" TacheDeleguee : liste
    SessionImpressions "1" *-- "1" StructureConversations : estime
    SessionImpressions "1" *-- "2..3" ConversationMarquante : se souvient de
    SessionImpressions "1" *-- "0..1" Croquis : dessine
    SessionImpressions "1" *-- "0..1" Impression : écrit
    SessionImpressions ..> Frequence
    ConversationMarquante ..> Ressenti
```

## Règles

- La somme des `pourcentage` des `TacheDeleguee` d'une session doit faire **100**.
- `echangesCourts + conversationsLongues + dialoguesEtendus = 100`.
- Une session contient **2 ou 3** conversations marquantes (consigne du vademecum).
- `dureeMinuteur` vaut 300 secondes (5 minutes).

## Correspondance avec Omeka S

| Classe | Omeka S |
|---|---|
| `SessionImpressions` | Contenu (*item*) avec le modèle de ressource « Ex3a – Impressions » |
| `TacheDeleguee`, `ConversationMarquante` | Contenus liés à la session (propriété de type *resource*) |
| `Croquis` | Média (image PNG) attaché à la session |
| `Participant` | Contenu de classe `foaf:Person` |
