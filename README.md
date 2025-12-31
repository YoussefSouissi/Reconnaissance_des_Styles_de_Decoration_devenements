# **Reconnaissance des Styles de Décoration d'Événements - Documentation du Projet**

## **Présentation du Projet**

### **Objectif**

Développer un système de classification d'images basé sur l'IA qui détecte automatiquement les styles de décoration à partir d'images téléchargées par les utilisateurs. Ce système aidera les organisateurs d'événements sur notre plateforme à identifier rapidement leur esthétique préférée.

### **Contexte Business**

Notre plateforme de gestion d'événements sert deux types d'utilisateurs principaux :

- **Clients** : Créent et gèrent des événements, ont besoin de trouver des prestataires correspondant à leurs préférences de style
- **Prestataires** : Offrent des services d'événements (traiteur, DJ, décoration, lieux, etc.)

Le système IA analyse les images d'inspiration téléchargées par les clients et retourne automatiquement le style de décoration détecté, permettant une meilleure expérience utilisateur et un matching plus efficace.

---
## **Lien du dataset**
https://www.kaggle.com/datasets/souissiyoussef/event-decoration-styles-dataset

---

## **Architecture du Système - Deux Composants Indépendants**


# **PARTIE 1 : Détection de Style par Patterns Visuels**

**Priorité : 🔴 HAUTE - C'est le cœur du système**

## **1.1 Principe de Fonctionnement**

### **Comment le système détecte le style**

Le système utilise un réseau de neurones convolutif (CNN) qui apprend à reconnaître les styles en analysant les **patterns visuels** dans les images.

**IMPORTANT : Le système NE SE BASE PAS principalement sur les couleurs pour détecter le style !**

### **Ce que le modèle analyse**

| **Éléments Analysés** | **Exemples** |
| --- | --- |
| **Formes & Géométrie** | Lignes épurées vs courbes ornées, symétrie vs asymétrie |
| **Textures** | Bois brut, métal lisse, tissus luxueux, matériaux naturels |
| **Composition Spatiale** | Organisation des éléments, densité de la décoration |
| **Éléments Décoratifs** | Type d'objets présents (chandeliers, plantes, mobilier) |
| **Matériaux** | Macramé, verre, acier, bois, tissus |
| **Style de Mobilier** | Moderne, vintage, industriel, classique |
| **Éclairage** | Type de luminaires, ambiance lumineuse |

### **Pourquoi les couleurs seules ne suffisent pas**

**Exemple concret** : Imaginez deux événements avec exactement les mêmes couleurs (or + blanc + crème) :

- **Événement 1 - Style Royal** : Chandeliers en cristal, nappes en soie, mobilier Louis XV, symétrie parfaite
- **Événement 2 - Style Bohème** : Macramé doré, tissus naturels, coussins au sol, arrangement libre et désordonné

**→ Mêmes couleurs, mais styles complètement différents !**

Le modèle doit donc apprendre à reconnaître les **patterns visuels caractéristiques** de chaque style, pas seulement les couleurs utilisées.

---

## **1.2 Architecture Technique**

### **Modèle : MobileNetV2 avec Transfer Learning**

**Qu'est-ce que le Transfer Learning ?**
Au lieu de partir de zéro, nous utilisons un modèle pré-entraîné (MobileNetV2) qui a déjà appris à reconnaître des milliers de concepts visuels. Nous adaptons ensuite ce modèle pour reconnaître nos 7 styles de décoration.

**Avantages :**

- Entraînement plus rapide (heures au lieu de jours/semaines)
- Meilleure précision avec moins de données
- Modèle léger et rapide (adapté au déploiement mobile)

### **Spécifications du Modèle**

- **Input** : Images RGB de 224×224 pixels
- **Output** : 7 probabilités (une pour chaque style)
- **Taille** : 15-20 MB (modèle compact)
- **Temps d'inférence** : 100-300 millisecondes par image
- **Précision attendue** : 85-90%

---

## **1.3 Dataset : Interior Design Styles**

### **Source du Dataset**

- **Nom** : Interior Design Styles by Stepan Yarullin
- **Source** : Kaggle
- **Taille** : ~10,000 images (environ 1,000 images par style)
- **Qualité** : Photographie professionnelle de haute qualité provenant de Houzz.com
- **Format** : Images JPG organisées par style

### **Pourquoi des images d'intérieur fonctionnent pour les événements ?**

Bien que le dataset contienne des photos d'intérieurs (salons, chambres, salles à manger) et non des événements, les **principes de décoration sont universels** :

- Les palettes de couleurs utilisées
- Les styles de mobilier
- Les matériaux et textures
- Les patterns de composition
- L'ambiance générale créée

**Ces éléments se traduisent directement dans la décoration de lieux d'événements.**

---

## **1.4 Les 7 Styles à Classifier**

Voici les 7 styles de décoration que notre système peut reconnaître :

### **1. Bohème**

**Caractéristiques visuelles clés :**

- Asymétrie et arrangement libre
- Superposition d'éléments et de textures
- Matériaux naturels et artisanaux
- Ambiance décontractée et chaleureuse

**Éléments typiques :** Macramé, plantes suspendues, coussins au sol, tapis ethniques, tissus fluides, lumière tamisée

**Événements compatibles :** Mariage, Expérience Culturelle, Festival, Anniversaire

---

### **2. Royal**

**Caractéristiques visuelles clés :**

- Symétrie stricte et organisation formelle
- Ornements complexes et détails raffinés
- Matériaux luxueux et précieux
- Ambiance élégante et sophistiquée

**Éléments typiques :** Chandeliers en cristal, dorures, tissus en soie, mobilier Louis XV, centres de table hauts et élaborés

**Événements compatibles :** Mariage haut de gamme, Gala, Anniversaire important, Lancement Corporate prestigieux

---

### **3. Minimaliste**

**Caractéristiques visuelles clés :**

- Lignes épurées et géométrie simple
- Espaces négatifs et aération
- Palette de couleurs restreinte
- Ambiance zen et ordonnée

**Éléments typiques :** Surfaces lisses, peu d'objets, formes géométriques pures, mobilier simple et fonctionnel

**Événements compatibles :** Lancement Corporate, Conférence, Exhibition, Lancement Produit

---

### **4. Rustique**

**Caractéristiques visuelles clés :**

- Textures brutes et naturelles
- Matériaux non raffinés
- Look "fait maison" et authentique
- Ambiance champêtre et chaleureuse

**Éléments typiques :** Bois brut, toile de jute, bocaux Mason, fleurs champêtres, tables en bois massif, éléments de ferme

**Événements compatibles :** Mariage champêtre, Anniversaire, Festival, Anniversaire de mariage

---

### **5. Vintage**

**Caractéristiques visuelles clés :**

- Objets rétro et anciens
- Patine du temps visible
- Détails délicats et romantiques
- Ambiance nostalgique

**Éléments typiques :** Meubles antiques, dentelle, couleurs pastel délavées, vaisselle ancienne, cadres dorés vieillis

**Événements compatibles :** Mariage classique, Anniversaire de mariage, Dîner Privé, Gala

---

### **6. Moderne**

**Caractéristiques visuelles clés :**

- Formes géométriques nettes et audacieuses
- Contrastes forts
- Design épuré et contemporain
- Ambiance dynamique et actuelle

**Éléments typiques :** Lignes droites, couleurs vives, mobilier design contemporain, éclairage sculptural, matériaux modernes

**Événements compatibles :** Lancement Corporate, Lancement Produit, Événement Networking, Conférence

---

### **7. Industriel**

**Caractéristiques visuelles clés :**

- Matériaux bruts exposés
- Look urbain et non fini
- Structures apparentes
- Ambiance loft new-yorkais

**Éléments typiques :** Métal, briques apparentes, ampoules Edison, tuyaux visibles, béton, mobilier en acier

**Événements compatibles :** Lancement Produit, Exhibition, Atelier, Événement Networking

---

# **PARTIE 2 : Extraction de Palette de Couleurs**

**Priorité : 🟡 MOYENNE - Information complémentaire pour enrichir l'expérience utilisateur**

## **2.1 Principe de Fonctionnement**

### **Ce qu'est l'extraction de couleurs**

L'extraction de couleurs est un processus **totalement séparé et indépendant** de la classification de style. Il utilise un algorithme de clustering (KMeans) pour identifier les couleurs les plus présentes dans l'image.

### **CLARIFICATION IMPORTANTE**

**Les couleurs NE SONT PAS utilisées pour détecter le style !**

- La **Partie 1** (CNN) détecte le style → basé sur les patterns visuels
- La **Partie 2** (KMeans) extrait les couleurs → information bonus séparée

Ces deux informations sont ensuite **combinées dans la réponse finale** pour donner à l'utilisateur une analyse complète.

### **Pourquoi extraire les couleurs ?**

Même si les couleurs ne sont pas utilisées pour la classification, elles apportent une valeur ajoutée :

1.  **Inspiration palette** : Donner au client une palette de couleurs harmonieuse basée sur son image d'inspiration
2.  **Données enrichies** : Fournir plus d'informations sur le style détecté
3.  **Suggestions créatives** : Proposer des combinaisons de couleurs cohérentes
4. **Matching secondaire** : Permettre aux clients de rechercher aussi par couleurs (fonctionnalité future)

---

## **2.2 Algorithme : KMeans Clustering**

### **Comment ça marche**

**Vue d'ensemble simplifiée :**

1. L'image est composée de milliers de pixels, chaque pixel ayant une couleur (rouge, vert, bleu)
2. L'algorithme KMeans regroupe les pixels similaires en clusters (groupes)
3. Le centre de chaque cluster représente une couleur dominante
4. On extrait les 5 couleurs les plus représentées dans l'image

**Avantages de KMeans :**

- Simple et rapide (50-100ms par image)
- Trouve automatiquement les couleurs les plus fréquentes
- Résultats visuellement satisfaisants
- Nombre de couleurs configurable
