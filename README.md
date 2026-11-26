# Nouvelle Subdivision Administrative du Burundi (JSON)

Ce fichier JSON contient la **nouvelle structure administrative du Burundi**, organisée en **5 provinces**, conformément à la réforme territoriale du pays.

---

## Contenu du fichier

Le fichier `burundi-geo.json` regroupe l'ensemble du découpage administratif :

| Niveau             | Nombre |
|--------------------|--------|
| Provinces          | 5      |
| Communes           | 42     |
| Zones              | 451    |
| Collines/Quartiers | 3 044  |

---

## Les 5 provinces

| Province        | Capitale   | Communes | Zones | Collines/Quartiers |
|-----------------|------------|----------|-------|---------------------|
| **Buhumuza** (`BH`)    | Cankuzo    | 7        | 64    | 509                 |
| **Bujumbura** (`BJ`)   | Bujumbura  | 11       | 95    | 535                 |
| **Burunga** (`BR`)     | Makamba    | 7        | 89    | 546                 |
| **Butanyerera** (`BT`) | Ngozi      | 8        | 102   | 790                 |
| **Gitega** (`GT`)      | Gitega     | 9        | 101   | 664                 |

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

Chaque niveau possède un identifiant unique (`id`) et un nom (`name`). Chaque province a aussi une capitale (`capital`), des totaux (`stats`) et une liste de communes. Chaque commune contient des zones, et chaque zone liste ses collines ou quartiers (`hills_neighborhoods`).

---

## Totaux par province

L'objet `stats` de chaque province indique le nombre de communes, de zones et de collines ou quartiers présents dans ce fichier. Ces valeurs doivent être recalculées après chaque modification des listes.

Le champ `population` indique la population (`value`) et l'année du recensement (`year`). Les chiffres proviennent du recensement de 2024 et ne sont pas calculés à partir des listes.

---

## Référence des champs

| Champ | Type | Niveau | Description |
|-------|------|--------|-------------|
| `country` | texte | racine | Nom du pays |
| `provinces` | liste | racine | Liste des provinces |
| `id` | texte | province, commune, zone, colline ou quartier | Identifiant unique |
| `name` | texte | province, commune, zone, colline ou quartier | Nom |
| `capital` | texte | province | Capitale de la province |
| `stats` | objet | province | Totaux de la province |
| `stats.communes` | nombre | province | Nombre de communes |
| `stats.zones` | nombre | province | Nombre de zones |
| `stats.hills_neighborhoods` | nombre | province | Nombre de collines ou quartiers |
| `stats.population` | objet | province | Population recensée |
| `stats.population.value` | nombre | province | Nombre d'habitants |
| `stats.population.year` | nombre | province | Année du recensement |
| `communes` | liste | province | Liste des communes |
| `zones` | liste | commune | Liste des zones |
| `hills_neighborhoods` | liste | zone | Liste des collines ou quartiers |

---

## Identifiants

| id   | Province    |
|------|-------------|
| BH   | Buhumuza    |
| BJ   | Bujumbura   |
| BR   | Burunga     |
| BT   | Butanyerera |
| GT   | Gitega      |

Format des identifiants : `BH-C01-Z01-H01` (province, commune, zone, colline ou quartier).

Les identifiants sont stables : un `id` publié ne doit pas être modifié ni réutilisé. Un nouvel élément reçoit le numéro libre suivant.

Les `id` sont propres à ce projet. Ce ne sont pas des codes officiels ni des codes ISO 3166-2.

---

## Exemple

Voici un extrait (liste raccourcie) pour illustrer la structure :

```json
{
  "country": "Burundi",
  "provinces": [
    {
      "id": "BH",
      "name": "Buhumuza",
      "capital": "Cankuzo",
      "stats": {
        "communes": 7,
        "zones": 64,
        "hills_neighborhoods": 509,
        "population": { "value": 2052261, "year": 2024 }
      },
      "communes": [
        {
          "id": "BH-C01",
          "name": "Butaganzwa",
          "zones": [
            {
              "id": "BH-C01-Z01",
              "name": "Bisinde",
              "hills_neighborhoods": [
                { "id": "BH-C01-Z01-H01", "name": "Bisinde" },
                { "id": "BH-C01-Z01-H02", "name": "Bugarama" }
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

### Buhumuza
Butaganzwa, Butihinda, Cankuzo, Gisagara, Gisuru, Muyinga, Ruyigi

### Bujumbura
Bubanza, Bukinanyana, Cibitoke, Isare, Mpanda, Mugere, Mugina, Muhuta, Mukaza, Ntahangwa, Rwibaga

### Burunga
Bururi, Makamba, Matana, Musongati, Nyanza, Rumonge, Rutana

### Butanyerera
Busoni, Kayanza, Kiremba, Kirundo, Matongo, Muhanga, Ngozi, Tangara

### Gitega
Bugendana, Gishubi, Gitega, Karusi, Kiganda, Muramvya, Mwaro, Nyabihanga, Shombo

---

## Utilisation

Ce fichier peut servir pour :

- Des applications web ou mobiles affichant la carte administrative du Burundi
- Des formulaires avec sélection en cascade (province → commune → zone → colline)
- Des projets de recherche, statistiques ou cartographie
- Tout projet nécessitant la liste complète des subdivisions du Burundi

---

## Exemples de code

Chaque exemple charge `burundi-geo.json`, cherche un élément par son `id` et affiche son nom (`Bisinde`).

### JavaScript (Node.js)

```javascript
const fs = require("fs");

function findById(node, id) {
  if (node.id === id) return node;
  for (const value of Object.values(node)) {
    if (!Array.isArray(value)) continue;
    for (const child of value) {
      const found = findById(child, id);
      if (found) return found;
    }
  }
  return null;
}

const data = JSON.parse(fs.readFileSync("burundi-geo.json", "utf8"));
console.log(findById(data, "BH-C01-Z01")?.name); // Bisinde
```

### Python

```python
import json

def find_by_id(node, item_id):
    if node.get("id") == item_id:
        return node
    for value in node.values():
        if isinstance(value, list):
            for child in value:
                found = find_by_id(child, item_id)
                if found:
                    return found
    return None

with open("burundi-geo.json", encoding="utf-8") as f:
    data = json.load(f)

print(find_by_id(data, "BH-C01-Z01")["name"])  # Bisinde
```

### Java

Bibliothèque requise : [Jackson Databind](https://github.com/FasterXML/jackson-databind).

```java
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;
import java.io.IOException;

public class Main {
    static JsonNode findById(JsonNode node, String id) {
        if (node.path("id").asText().equals(id)) return node;
        for (JsonNode value : node) {
            if (!value.isArray()) continue;
            for (JsonNode child : value) {
                JsonNode found = findById(child, id);
                if (found != null) return found;
            }
        }
        return null;
    }

    public static void main(String[] args) throws IOException {
        JsonNode data = new ObjectMapper().readTree(new File("burundi-geo.json"));
        JsonNode item = findById(data, "BH-C01-Z01");
        if (item != null) System.out.println(item.get("name").asText());
    }
}
```

### C#

Utilise `System.Text.Json`, inclus dans .NET.

```csharp
using System;
using System.IO;
using System.Text.Json;

using var doc = JsonDocument.Parse(File.ReadAllText("burundi-geo.json"));
var item = FindById(doc.RootElement, "BH-C01-Z01");
if (item is JsonElement found)
    Console.WriteLine(found.GetProperty("name").GetString());

static JsonElement? FindById(JsonElement node, string id)
{
    if (node.TryGetProperty("id", out var current) && current.GetString() == id)
        return node;

    foreach (var property in node.EnumerateObject())
    {
        if (property.Value.ValueKind != JsonValueKind.Array) continue;
        foreach (var child in property.Value.EnumerateArray())
        {
            var result = FindById(child, id);
            if (result != null) return result;
        }
    }
    return null;
}
```

### C++

Bibliothèque requise : [nlohmann/json](https://github.com/nlohmann/json).

```cpp
#include <fstream>
#include <iostream>
#include <string>
#include <nlohmann/json.hpp>

using json = nlohmann::json;

const json* findById(const json& node, const std::string& id) {
    if (node.value("id", "") == id) return &node;
    for (const auto& value : node) {
        if (!value.is_array()) continue;
        for (const auto& child : value) {
            if (const json* found = findById(child, id)) return found;
        }
    }
    return nullptr;
}

int main() {
    std::ifstream file("burundi-geo.json");
    if (!file) return 1;
    const json data = json::parse(file);

    if (const json* item = findById(data, "BH-C01-Z01")) {
        std::cout << item->at("name").get<std::string>() << '\n';
    }
    return 0;
}
```

### C

Bibliothèque requise : [cJSON](https://github.com/DaveGamble/cJSON). Compilation : `gcc main.c -lcjson`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <cjson/cJSON.h>

static const cJSON *find_by_id(const cJSON *node, const char *id) {
    const cJSON *current = cJSON_GetObjectItemCaseSensitive(node, "id");
    if (cJSON_IsString(current) && strcmp(current->valuestring, id) == 0) {
        return node;
    }

    const cJSON *value;
    cJSON_ArrayForEach(value, node) {
        if (!cJSON_IsArray(value)) continue;
        const cJSON *child;
        cJSON_ArrayForEach(child, value) {
            const cJSON *found = find_by_id(child, id);
            if (found) return found;
        }
    }
    return NULL;
}

static char *read_file(const char *path) {
    FILE *file = fopen(path, "rb");
    if (!file) return NULL;

    fseek(file, 0, SEEK_END);
    long size = ftell(file);
    rewind(file);

    char *text = size >= 0 ? malloc((size_t)size + 1) : NULL;
    if (text) {
        size_t length = fread(text, 1, (size_t)size, file);
        text[length] = '\0';
    }
    fclose(file);
    return text;
}

int main(void) {
    char *text = read_file("burundi-geo.json");
    if (!text) return 1;

    cJSON *data = cJSON_Parse(text);
    free(text);
    if (!data) return 1;

    const cJSON *item = find_by_id(data, "BH-C01-Z01");
    const cJSON *name = cJSON_GetObjectItemCaseSensitive(item, "name");
    if (cJSON_IsString(name)) puts(name->valuestring);

    cJSON_Delete(data);
    return 0;
}
```

---

## Format

- **Fichier** : `burundi-geo.json`
- **Format** : JSON
- **Encodage** : UTF-8

---

## Contribuer

Les corrections sont les bienvenues.

1. Pour signaler une erreur, ouvrez une *issue* en indiquant l'`id` concerné et la correction proposée.
2. Pour proposer une modification, créez une branche puis une *pull request*.
3. Ne modifiez et ne réutilisez jamais un `id` publié. Un nouvel élément reçoit le numéro libre suivant.
4. Après chaque ajout ou suppression, mettez à jour les `stats` de la province et les tableaux de ce README.
5. Vérifiez que le fichier JSON reste valide et encodé en UTF-8.

---

## Sources

Les zones et les collines ou quartiers ont été vérifiés avec les limites administratives du Burundi (version v02) publiées sur [HDX](https://data.humdata.org/dataset/cod-ab-bdi), source : IGEBU et OCHA, sous licence Creative Commons Attribution for Intergovernmental Organisations. Les aires protégées et les entrées « Champ de Tir » ne sont pas incluses. Certaines orthographes peuvent différer de cette source.

---

## Licence

Données ouvertes. Libre d'utilisation pour tout projet personnel, éducatif ou commercial.
