# 📡 Dimensionnement et Planification d'un Réseau GSM 900 sous Atoll

---

## 📋 Description du Projet

Ce projet porte sur le **dimensionnement et la planification d'un réseau GSM 900 MHz** pour la zone urbaine de **Sidi Bennour** (Maroc), en combinant une approche théorique rigoureuse et une simulation pratique sous le logiciel **Atoll** (Forsk).

La problématique centrale : concevoir une architecture réseau optimale assurant une couverture radio continue et de qualité sur l'ensemble de la ville (8,587 km²), tout en minimisant les interférences et en rationalisant les coûts d'infrastructure.

---

## 🗂️ Structure du Projet

```
projet-gsm-atoll/
│
├── rapport_Atoll.pdf          # Rapport complet du projet
├── README.md                  # Ce fichier
│
├── partie_theorique/          # Calculs et modélisation
│   ├── bilan_liaison.md
│   ├── modele_propagation.md
│   └── dimensionnement_hex.md
│
└── simulation_atoll/          # Fichiers de simulation
    ├── PLANIFICATION_GSM900_SB.atoll
    └── exports/               # Cartes et captures d'écran
```

---

## 🌍 Zone d'Étude

| Paramètre | Valeur |
|---|---|
| Ville | Sidi Bennour, région Casablanca-Settat, Maroc |
| Superficie totale | 8,587 km² |
| Altitude moyenne | ≈ 185 m |
| Type de terrain | Urbain de densité moyenne |
| Hauteur des bâtiments | R+1 à R+4 |
| Activités principales | Commercial, administratif, résidentiel, agricole |

---

## 🔧 Technologies et Outils

- **Technologie radio :** GSM 900 MHz (2G)
- **Logiciel de simulation :** Atoll (Forsk) — version GSM Urban
- **Modèle de propagation :** Okumura-Hata (milieu urbain)
- **Système de coordonnées :** WGS 84 / UTM zone 29N

### Pourquoi le GSM 900 MHz ?

- **Pénétration indoor supérieure** grâce à une longueur d'onde plus grande qu'à 1800/2100 MHz
- **Portée cellulaire adaptée** à une agglomération de taille moyenne
- **Coût de déploiement maîtrisé** par rapport aux technologies 4G/5G

---

## 📐 Méthodologie

### 1. Modèle de Propagation — Okumura-Hata

La formule utilisée pour les pertes en milieu urbain :

```
Lp(dB) = 69,55 + 26,16·log₁₀(f) − 13,82·log₁₀(Hb) − a(Hm) + [44,9 − 6,55·log₁₀(Hb)]·log₁₀(d)
```

Avec les paramètres suivants :

| Paramètre | Valeur | Justification |
|---|---|---|
| f | 900 MHz | Bande GSM du projet |
| Hb | 25 m | Hauteur typique antenne BTS |
| Hm | 1,5 m | Hauteur standard terminal mobile (3GPP) |
| a(Hm) | ≈ 0,00 dB | Facteur de correction calculé |

**Formule simplifiée finale :**

```
Lp(dB) = 127,50 + 35,74·log₁₀(d)
```

---

### 2. Bilan de Liaison (Link Budget)

| Composante | Valeur |
|---|---|
| Puissance d'émission Ptx | 43 dBm (20 W) |
| Gain d'antenne Gant | 17 dBi (sectorielle 65°) |
| Pertes câbles Lcâbles | 3 dB |
| **PIRE calculée** | **57 dBm** |
| Sensibilité mobile | −104 dBm |
| Marge de fading | 7 dB |
| Marge de pénétration indoor | 12 dB |
| Marge d'interférence | 3 dB |
| **Marges totales** | **22 dB** |
| **MAPL** | **139 dB** |

---

### 3. Calcul du Rayon de Cellule

- **Rayon théorique (Okumura-Hata) :** 2,097 km
- **Facteur correctif urbain :** 0,38 (pertes bâti, chevauchement handover, recommandations ITU-R P.1411)
- **Rayon réel retenu : R = 0,80 km**

---

### 4. Dimensionnement Hexagonal

Chaque cellule est modélisée comme un hexagone régulier :

```
Shex = 2,598 × R² ≈ 1,663 km²
NBTS = K × (Szone / Shex) = 1,35 × (8,587 / 1,663) ≈ 6,97 → 7 BTS
```

**Résultat : 7 BTS tri-sectoriels = 21 secteurs au total**

---

## 📊 Résultats de Dimensionnement

| Paramètre | Valeur |
|---|---|
| Superficie de la zone | 8,587 km² |
| Technologie | GSM 900 MHz |
| Modèle de propagation | Okumura-Hata (urbain) |
| PIRE | 57 dBm |
| MAPL | 139 dB |
| Rayon de cellule réel | 0,80 km |
| Nombre de BTS | 7 sites tri-sectoriels |
| Nombre de secteurs total | 21 secteurs |
| Plan de fréquences | 4×3 (12 groupes ARFCN) |
| C/I nominal (seuil) | ≥ 9 dB |
| C/I après AFP | > 12 dB |
| Taux de couverture cible | ≥ 90 % |

---

## 🖥️ Simulation sous Atoll

### Étapes réalisées

1. **Configuration du projet** — système de coordonnées WGS 84 / UTM zone 29N
2. **Importation des données géographiques** — zone d'étude (polygone), carte de hauteurs de clutter (SB-HIGHT), carte satellite géoréférencée
3. **Déploiement des 7 sites BTS** — placement automatique via la fonctionnalité *Hexagonal Design* d'Atoll, 20 transmetteurs actifs avec antennes 65° 17dBi 4Tilt 900MHz
4. **Configuration de la propagation** — modèle Okumura-Hata, rayon principal 2 500 m / résolution 10 m, rayon étendu 5 000 m
5. **Planification Automatique des Fréquences (AFP)** — allocation de 20 canaux BCCH distincts (solution n° 2236, C/I > 12 dB)
6. **Relations de voisinage (Handover)** — calcul automatique des relations Co-Site, Adjacentes et de Couverture, symétrie bidirectionnelle activée
7. **Optimisation ACP** — couverture 100 % atteinte, dominance cellulaire portée à 100 % via une unique modification (azimut Site1_2 : 120° → 130°)

### Résultats de l'optimisation ACP

| Objectif | Initial | Final |
|---|---|---|
| Couverture GSM (≥ −85 dBm) | 100 % | 100 % ✅ |
| Dominance cellulaire | 99,94 % | 100 % ✅ |
| Durée d'optimisation | — | 2,98 s |
| Modifications proposées | — | 1 (azimut Site1_2) |

### Carte de couverture finale

La carte finale montre l'intégralité de la zone urbaine couverte avec un signal **≥ −70 dBm** (rouge), largement supérieur au seuil de −85 dBm, confirmant la validité du dimensionnement théorique.

---

## 📡 Plan de Fréquences AFP — Canaux BCCH attribués

| Transmetteur | BCCH | Transmetteur | BCCH |
|---|---|---|---|
| Site0_1 | 54 | Site4_1 | 80 |
| Site1_3 | 115 | Site4_2 | 9 |
| Site2_1 | 48 | Site4_3 | 18 |
| Site2_2 | 28 | Site5_1 | 26 |
| Site3_1 | 103 | Site5_2 | 88 |
| Site3_2 | 41 | Site5_3 | 36 |
| Site3_3 | 6 | Site6_1 | 30 |
| — | — | Site6_2 | 90 |
| — | — | Site6_3 | 1 |

---

## 📚 Références

- Okumura-Hata — modèle de propagation reconnu par l'UIT-R (150–1500 MHz)
- ITU-R P.1411 — pertes de propagation en milieu urbain de densité modérée
- COST 231 — probabilité de couverture 90 % en milieu urbain
- Rappaport, *Wireless Communications*, 2002 — facteur correctif urbain (0,35–0,50)
- Norme 3GPP GSM 900 — paramètres des terminaux mobiles

---

Projet académique — Université Hassan 1er, Settat, Maroc. Tous droits réservés aux auteurs.
