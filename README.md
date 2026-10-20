# NEXUS-GITHUB-BULK-UPLOADER
NEXUS GITHUB BULK UPLOADER X2 Objectif : Uploader un dossier/archive massif sur GitHub en 1 commit. Architecture : Streaming par lots + Gestion du Rate Limit + Arborescence préservée. Auteur : Aissa Mohammedi (DGK) - NEXUS-OPEN-3.0
# 🌌 NEXUS BULK UPLOADER X2

> *Décompresse, fragmente, et domine l'API GitHub. Un seul commit pour des milliers de fichiers.*

[![Version](https://img.shields.io/badge/version-2.0.0-00f3ff?style=for-the-badge)](#)
[![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](#)
[![GitHub API](https://img.shields.io/badge/GitHub_API-v3-181717?style=for-the-badge&logo=github)](#)
[![License](https://img.shields.io/badge/License-NEXUS--OPEN--3.0-ff00ea?style=for-the-badge)](#)

---

## 🛑 LE PROBLÈME

L'interface web de GitHub (et même Git en ligne de commande) a des limites drastiques :
- Impossible de glisser-déposer un dossier de 10 Go.
- L'API Git standard échoue avec des erreurs `422 Unprocessable Entity` ou `413 Payload Too Large` au-delà d'un certain nombre de fichiers.
- Charger une archive massive en RAM (`zipfile.read()`) crash votre machine (OOM - Out of Memory).

## 🚀 LA SOLUTION : NEXUS BULK UPLOADER

Ce script Python contourne toutes ces limites en utilisant l'**API Git Data (Low-Level)** de GitHub. Il ne fait pas un `git push`. Il construit l'arbre Git (Tree) fichier par fichier, directement sur les serveurs de GitHub.

### 🧠 L'Architecture (Le Génie Algorithmique)

1. **Streaming (Générateur Python `yield`)** : Au lieu de charger l'archive entière en mémoire, le script lit un fichier, l'envoie à l'API, et le jette de la RAM. Vous pouvez uploader des archives de 100 Go sans crash.
2. **Fragmentation par Lots (`BATCH_SIZE`)** : Il crée des sous-arbres (Trees) par paquets de 50 fichiers. Cela évite de saturer la limite de taille de requête de l'API GitHub.
3. **Gestion du Rate Limit** : Si GitHub renvoie une erreur `403`, le script intercepte l'erreur, attend 60 secondes, et reprend exactement là où il s'était arrêté.
4. **Un seul commit** : Malgré les milliers de fichiers, tout est fusionné dans un seul `commit` propre sur votre branche `main`.

---

## ⚙️ Installation

Aucune dépendance externe requise. Python 3.9+ suffit.

```bash
git clone https://github.com/Aissamohammedi88/nexus-bulk-uploader.git
cd nexus-bulk-uploader
chmod +x nexus_bulk_uploader.py
