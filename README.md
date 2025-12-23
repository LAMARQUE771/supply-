# 📦 Prévision de la demande et Stock de sécurité  
**Dataset : Supply Chain Logistics Problem (Kaggle)**

## 🎯 Objectif
Réaliser une **prévision simple de la demande** et calculer un **stock de sécurité** à partir des données historiques, selon une approche basique utilisée en supply chain.

---

## 📊 Données utilisées
- Source : *Order List* (fichier Excel Kaggle)
- Variable de demande : **Weight**
- Les valeurs manquantes sont exclues du calcul.

---

## 📈 Méthode de prévision
La prévision est réalisée à l’aide de la **moyenne historique** :

\[
\text{Prévision} = \frac{1}{n} \sum_{i=1}^{n} \text{Demande}_i
\]

Cette méthode est adaptée lorsque :
- la demande est relativement stable,
- il n’y a pas de saisonnalité marquée,
- l’objectif est une estimation simple et robuste.

---

## 🛡️ Stock de sécurité
Le stock de sécurité est calculé avec la formule classique :

\[
SS = Z \times \sigma \times \sqrt{LT}
\]

Avec :
- \( Z = 1{,}65 \) (niveau de service 95 %)
- \( \sigma \) : écart-type de la demande
- \( LT = 2 \) périodes (lead time)

---

## 📦 Stock cible
Le stock cible correspond au stock nécessaire pour couvrir la demande pendant le délai d’approvisionnement :

\[
Stock\ cible = (\text{Prévision} \times LT) + SS
\]

---

## 🧮 Implémentation Python (Kaggle)

```python
import numpy as np

d = orders["Weight"].dropna()

forecast = d.mean()
sigma = d.std(ddof=1)

Z = 1.65 # Niveau de service 95 %
lead_time = 2 # Délai d'approvisionnement (périodes)

safety_stock = Z * sigma * np.sqrt(lead_time)
target_stock = forecast * lead_time + safety_stock

print("Prévision =", round(forecast, 2))
print("Stock de sécurité =", round(safety_stock, 2))
print("Stock cible =", round(target_stock, 2))
