# Factures piégées

Instructions invisibles contre un agent multimodal d'extraction, et défenses par comparaison OCR et rendu.

Projet encadré S9 (sujet I8), filière IA & Data, EMSI, 2026/2027.
Encadrante : Pr. Lalla Amina Charaf.
Équipe : Yassir, Mustapha.

> Statut : cadrage validé en séance 2. Le code arrive à partir de la séance 3.

## Le problème

Un agent multimodal qui traite des factures lit toute l'image, y compris un texte trop pâle ou trop petit pour qu'un humain le remarque. Si ce texte contient une instruction (« utilise cet IBAN à la place »), l'agent peut la suivre : c'est une injection de prompt indirecte.

Ce projet mesure cette vulnérabilité sur un agent de factures fictives, puis évalue deux défenses.

## Ce que contient le projet

| Partie | Rôle |
| --- | --- |
| Générateur | Crée des factures fictives (images PNG) et leur vérité terrain en JSON |
| Attaques | Ajoute un texte caché : faible contraste, très petite police, zone peu visible |
| Agent | Agent en trois étapes : lecture de l'image, vérification des champs avec des outils, appel simulé de « préparer le paiement » (validation humaine obligatoire) |
| Défense D1 | Compare l'OCR de l'image normale et d'une version révélée (contraste, binarisation), plus un score d'anomalie visuelle |
| Défense D2 | Sépare données et instructions dans le prompt, puis valide les champs extraits (IBAN, fournisseur, montant) |
| Évaluation | Taux de réussite des attaques, précision sur les factures saines et temps ajouté, pour chaque défense seule et combinée |

Modèles utilisés : Qwen2.5-VL 7B et Llama 3.2 Vision 11B, via Ollama (gratuits, exécutés en local ou sur Colab / Kaggle).

## Structure prévue

```text
src/fp/
  generator/    factures et vérité terrain
  attacks/      techniques d'attaque visuelles
  agent/        agent multi-étapes et outils simulés
  defenses/     D1 (OCR / rendu) et D2 (prompt et validation)
  evaluation/   campagnes et métriques
app/            démo Streamlit
configs/        paramètres des campagnes
data/           jeux d'images (non versionnés directement)
docs/           note de cadrage, fiches d'avancement, rapport
tests/          tests unitaires et d'intégration
```

## Installation

À venir (séance 3). L'objectif est une installation en une commande :

```bash
docker compose up
```

## Planning

| Séance | Dates | Objectif |
| --- | --- | --- |
| 2 | 16–21 nov. 2026 | Jalon 1 : cadrage validé |
| 3 | 30 nov.–5 déc. | Squelette de bout en bout |
| 4 | 7–12 déc. | Jalon 2 : MVP (agent + première série d'images piégées) |
| 5 | 14–19 déc. | Baseline et premiers résultats |
| 6 | 21–26 déc. | Jalon 3 : défenses et comparaison terminées |
| 7 | 28 déc.–2 janv. | Répétition générale |
| Dépôt | 3 janv. 2027 | Code, rapport, vidéo de secours |

## Règles du projet

- Données fictives uniquement : aucune vraie facture, aucun vrai IBAN.
- Aucune clé d'API ni mot de passe dans le dépôt : utiliser un fichier `.env` local (jamais commité).
- Toute attaque reste dans notre environnement de test, jamais sur un service réel ou un tiers.
- L'action de paiement est simulée et n'a aucun effet réel.

## Usage responsable

Ce dépôt est un travail de recherche défensive dans un cadre pédagogique. Les techniques d'attaque servent uniquement à mesurer et à améliorer les défenses.
