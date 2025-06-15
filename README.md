# 🔐 Système d'Alarme avec Arduino, Clavier, LCD et Capteur de Mouvement

Ce projet Arduino met en œuvre un **système d'alarme intelligent** basé sur un **capteur de mouvement (PIR)**, un **clavier matriciel**, un **afficheur LCD I2C** et un **buzzer**. L'utilisateur peut activer ou désactiver l'alarme à l'aide d'un **code secret** via le clavier.

---

## 🎯 Objectif du Projet

- Détecter les intrusions avec un capteur de mouvement.
- Activer une **séquence d'alerte progressive** (bips, puis sirène).
- Utiliser un **mot de passe** pour activer/désactiver le système.
- Afficher les états et instructions via un écran **LCD 16x2 I2C**.
- Réagir en temps réel grâce à `millis()` (évite l'utilisation de `delay()` bloquant).

---

## 🧰 Matériel Nécessaire

| Composant           | Quantité |
|---------------------|----------|
| Arduino UNO         | 1        |
| Capteur PIR (mouvement) | 1        |
| Clavier 4x4         | 1        |
| Écran LCD 16x2 I2C  | 1        |
| Buzzer              | 1        |
| LED (statut)        | 1        |
| Résistances         | Selon besoin |
| Fils de connexion   | Plusieurs |
| Breadboard          | 1        |

---

## 🔌 Connexions

| Élément              | Pin Arduino |
|----------------------|-------------|
| Capteur PIR          | D8          |
| LED                  | D9          |
| Buzzer               | D10         |
| Clavier 4x4 Lignes   | D4 → D7     |
| Clavier 4x4 Colonnes | D0 → D3     |
| LCD I2C              | SDA/SCL (A4/A5 sur UNO) |

---

## ⚙️ Fonctionnement

1. **Désactivation par défaut** : Le système démarre désactivé.
2. **Code d'accès** : L'utilisateur entre le code (`1234`) pour activer l'alarme.
3. **Activation** :
   - Attente de 15 secondes (temps pour quitter la pièce).
   - Ensuite, détection active.
4. **Détection** :
   - Si mouvement détecté → Mode ALERTE : bips pendant 10 secondes.
   - Si aucun code saisi → Passage en **Mode SIRENE** (buzzer continu).
5. **Désactivation** :
   - Entrée correcte du code arrête immédiatement toute alarme.
   - LED d’état mise à jour, écran LCD actualisé.

---

## 📟 Interface utilisateur

- **Clavier 4x4** : pour entrer le code.
  - `*` → Effacer le code saisi.
  - `#` → Valider le code.
- **LCD 16x2** :
  - Affiche si l’alarme est activée ou non.
  - Affiche les chiffres saisis.
  - Affiche `CORRECT` ou `INCORRECT` après validation.

---

## 🧠 Logique logicielle

- Code bien structuré avec :
  - Utilisation de `millis()` pour éviter les blocages.
  - Séparation claire en états (`désactivée`, `attente`, `alerte`, `sirène`).
  - Gestion dynamique du clavier et affichage LCD.
- Mot de passe géré via la bibliothèque `Password.h`.

---

## 🛠️ Bibliothèques utilisées

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Keypad.h>
#include <Password.h>
