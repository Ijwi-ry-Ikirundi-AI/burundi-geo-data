# Nouvelle Subdivision Administrative du Burundi (JSON)

Ce fichier JSON contient la **nouvelle structure administrative du Burundi**, organisée en **5 provinces**, conformément à la réforme territoriale du pays.

---

## Contenu du fichier

Le fichier `burundi-geo.json` regroupe l'ensemble du découpage administratif :

| Niveau             | Nombre |
|--------------------|--------|
| Provinces          | 5      |
| Communes           | 42     |
| Zones              | 450    |
| Collines/Quartiers | 3 002  |

---

## Les 5 provinces

| Province        | Capitale   | Communes | Zones | Collines/Quartiers |
|-----------------|------------|----------|-------|---------------------|
| **BUHUMUZA**    | Cankuzo    | 7        | 59    | 473                 |
| **BUJUMBURA**   | Bujumbura  | 11       | 95    | 535                 |
| **BURUNGA**     | Makamba    | 7        | 93    | 548                 |
| **BUTANYERERA** | Ngozi      | 8        | 102   | 783                 |
| **GITEGA**      | Gitega     | 9        | 101   | 663                 |

---

## Structure du fichier JSON

Le fichier suit une organisation hiérarchique simple :

```
Burundi
 └── Province
      └── Commune
           └── Zone
                └── Collines / Quartiers
```

Chaque province possède un nom, une capitale et une liste de communes.
Chaque commune contient des zones, et chaque zone liste ses collines ou quartiers.

---

## Exemple

Voici un extrait pour illustrer la structure :

```json
{
  "country": "Burundi",
  "provinces": [
    {
      "name": "BUHUMUZA",
      "capital": "Cankuzo",
      "communes": [
        {
          "name": "Butaganzwa",
          "zones": [
            {
              "name": "Bisinde",
              "collines_quarters": [
                "Bisinde",
                "Bugarama",
                "Karambi",
                "Kazimya",
                "Nyabigugo",
                "Rutimbura",
                "Rutonganyikwa"
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

---

## Liste des communes par province

### BUHUMUZA
Butaganzwa, Butihinda, Cankuzo, Gisagara, Gisuru, Muyinga, Ruyigi

### BUJUMBURA
Bubanza, Bukinanyana, Cibitoke, Isare, Mpanda, Mugere, Mugina, Muhuta, Mukaza, Ntahangwa, Rwibaga

### BURUNGA
Bururi, Makamba, Matana, Musongati, Nyanza, Rumonge, Rutana

### BUTANYERERA
Busoni, Kayanza, Kiremba, Kirundo, Matongo, Muhanga, Ngozi, Tangara

### GITEGA
Bugendana, Gishubi, Gitega, Karusi, Kiganda, Muramvya, Mwaro, Nyabihanga, Shombo

---

## Utilisation

Ce fichier peut servir pour :

- Des applications web ou mobiles affichant la carte administrative du Burundi
- Des formulaires avec sélection en cascade (province → commune → zone → colline)
- Des projets de recherche, statistiques ou cartographie
- Tout projet nécessitant la liste complète des subdivisions du Burundi

---

## Format

- **Fichier** : `burundi-geo.json`
- **Format** : JSON
- **Encodage** : UTF-8

---

## Licence

Données ouvertes. Libre d'utilisation pour tout projet personnel, éducatif ou commercial.
