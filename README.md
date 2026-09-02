# ConformiteReglementaire
# AuditPro MVP — Préparation de dossier d'audit

MVP du cas pédagogique AuditPro/ACSI : un **workflow orchestré** (pas un agent) qui prépare un dossier de travail sourcé pour un auditeur, avant mission — sans jamais qualifier d'écart ni formuler de conclusion.
Voir `docs/cadrage_mvp.md`, `docs/architecture_code.md`

## Installation

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
```

Dépendances : `pydantic`, `pandas`, `scikit-learn` (RAG en TF-IDF local, aucune clé d'API requise — le MVP tourne entièrement hors-ligne). `pytest` pour les tests.

## Utilisation

```bash
# Depuis la racine du projet
python3 -m auditpro.cli --entity-id E001 --mission-date 2026-08-27

# Écrire le dossier dans un fichier plutôt que sur stdout
python3 -m auditpro.cli --entity-id E001 --mission-date 2026-08-27 --out dossier_E001.md

```

Chaque exécution écrit aussi un journal de trace structuré dans `traces/<trace_id>.json` — le lien entre le dossier produit et sa trace complète est le champ `trace_id` du dossier.

## Lancer les tests

```bash
python3 -m pytest tests/ -v
```

- `tests/unit/test_step1_scope.py` — la résolution de périmètre ne retourne jamais de valeur par défaut sur ambiguïté.
- `tests/unit/test_step2_temporal_filter.py` — le module **critique** : cas limites de dates (veille/jour même/lendemain d'expiration, seuil de signalement).


## Données

Le dossier `donnees/` contient un **jeu de données d'exemple**:

| `donnees_tabulaire/entities.csv` | 
| `donnees_tabulaire/regulations.csv` | 
| `sources_documentaires/*.md` | 
