# 🛠️ Playbook MLOps Universel

> Un guide de production pragmatique, sans blabla théorique, directement actionnable pour structurer n'importe quel projet d'IA (tabulaire, vision, NLP, séries temporelles, RL, LLM / RAG).

---

## 💡 C'est quoi ce repo ?

La plupart des projets de Machine Learning souffrent des mêmes travers : des notebooks exploratoires bricolés qui finissent en prod, zéro reproductibilité, des déploiements manuels risqués et des modèles qui dérivent silencieusement sans monitoring.

Ce repo fournit **[`MLOPS.md`](file:///c:/Users/rayen/OneDrive/Desktop/MDs/MLOPS.md)**, un playbook complet conçu par et pour des praticiens. Il remplit 3 rôles au quotidien :

1. **Un standard de structuration & d'architecture pour l'équipe** : structure de repo saine, conventions de code, pipelines modulaires, gestion propre des dépendances et choix de stack technique.
2. **Une checklist opérationnelle de mise en production** : Quality gates automatisés, critères d'acceptation stricts (Definition of Done), stratégies de rollback en < 5 min et runbooks d'incident.
3. **Un fichier de contexte pour agents de code IA** : pensé pour être ingéré directement par **Cursor** (`.cursorrules`), **Claude Code** (`CLAUDE.md`) ou **GitHub Copilot** afin d'éviter qu'ils n'introduisent du code spaghetti ou des anti-patterns.

---

## ⚡ Quickstart (Comment l'utiliser)

### 1. Copier le playbook dans ton projet
Dépose simplement [`MLOPS.md`](file:///c:/Users/rayen/OneDrive/Desktop/MDs/MLOPS.md) à la racine de ton repository.

### 2. Remplir la Project Card
Ouvre [`MLOPS.md`](file:///c:/Users/rayen/OneDrive/Desktop/MDs/MLOPS.md#L47-L69) et complète le tableau de variables (**Section 0**) :
- Métrique business visée & métrique ML cible
- SLOs de latence (ex: `p99 < 100ms`) et budget d'inférence
- Niveau de maturité cible (🟢, 🟡 ou 🔴)
- Baseline de départ et hypothèses de travail

### 3. Appliquer les règles adaptées à ton échelle
Inutile de monter une usine à gaz pour un POC. Le playbook est gradué en 3 niveaux :
- 🟢 **Essentiel (Solo / POC)** : Repo structuré, seeds fixées, tracking local (MLflow), validation de schéma de données, tests unitaires et CI basique.
- 🟡 **Standard (Équipe / Prod)** : Pipeline DAG orchestré, Model Registry à stages, Quality gates bloquants, déploiements progressifs (Canary/Shadow), monitoring de data drift.
- 🔴 **Avancé (Scale / Réglementé)** : Feature store partagé, Continuous Training (CT) automatique, inférence distribuée/quantifiée, auditabilité stricte, gouvernance EU AI Act / RGPD et FinOps.

### 4. Piloter les agents IA
Pointe ton assistant de code vers la **Section 20 (`Règles pour agents IA`)**. Il y trouvera les conventions de nommage, les commandes `make` canoniques et la liste des interdits (ne jamais commiter de secrets, interdiction de modifier les golden datasets, zéro logique de production dans les notebooks).

---

## 📦 Ce que contient `MLOPS.md`

| Section | Ce qu'on y traite concrètement |
|---|---|
| **0. Project Card** | Fiche d'identité paramétrable (`{{SLO_LATENCE}}`, `{{METRIQUE_CIBLE}}`, etc.). |
| **1. Principes directeurs** | Les 10 règles non négociables du MLOps en production. |
| **2. Cycle de vie & Maturité** | Workflow end-to-end (diagramme Mermaid) et grille de passage des niveaux 0 à 3. |
| **3. Cadrage** | Traduction problème métier → métrique ML, mesure de baseline et critères Go/No-Go. |
| **4. Structure du Repo & Conventions** | Arborescence standard, `pyproject.toml` (`uv`), `Makefile` et pre-commit hooks. |
| **5. Données & Anti-leakage** | Data contracts, validation (Pandera), splits déterministes et audit anti-leakage. |
| **6. Expérimentation** | Tracking systématique, gestion des configs (Hydra), HPO (Optuna) et seeds universelles. |
| **7. Entraînement & Pipelines** | DAGs orchestrés, checkpoints résilients, mixed precision et gestion des coûts GPU. |
| **8. Évaluation & Quality Gates** | Évaluation par slices, calibration, explicabilité (SHAP) et tests bloquants en CI. |
| **9. Model Registry & Packaging** | Cycle de vie (`staging` → `prod`), formats d'export (ONNX, TorchScript), conteneurisation. |
| **10. CI / CD / CT** | Pyramide de tests ML, pipelines d'intégration continue et triggers de réentraînement. |
| **11. Déploiement & Serving** | Batch vs temps réel (FastAPI), Shadow / Canary releases, optimisation (INT8, vLLM). |
| **12. Monitoring & Observabilité** | Les 4 couches (système, data quality, data drift, concept drift), alertes et feedback loop. |
| **13. Infrastructure & Coûts** | Infrastructure as Code (Terraform), instances Spot, auto-scaling et FinOps. |
| **14. Sécurité & Gouvernance** | Scan de vulnérabilités (Trivy), anonymisation, conformité RGPD / EU AI Act. |
| **15. Extension LLMOps / GenAI** | RAG (chunking, retrieval, rerank), prompts versionnés, LLM-as-judge, guardrails et coûts par token. |
| **16. Organisation & Rituels** | Matrice RACI, revues d'expériences, Architecture Decision Records (ADR). |
| **17. Checklists Finales** | Production Readiness Review (PRR), Go/No-Go release, checklist post-mortem. |
| **18. 18 Anti-Patterns** | Tableau terrain : *Piège → Symptôme → Correctif immédiat*. |
| **19. Stack de Référence** | Matrice d'outils recommandés par brique et par niveau de maturité (🟢/🟡/🔴). |
| **20. Règles pour Agents IA** | Directives impératives pour assistants de code (Cursor, Claude, Copilot). |
| **Annexes (Templates prêts à copier)** | Model Card, Experiment Report, ADR, train.yaml, Makefile, workflow CI GitHub Actions, Runbook d'incident, Data Contract. |

---

## 🧱 Structure de Repo Standard

L'arborescence recommandée dans le playbook pour séparer nettement la logique métier, la config et l'infrastructure :

```text
{{NOM_PROJET}}/
├── .github/workflows/       # CI (tests), CT (training auto), CD (déploiement)
├── configs/                 # Configs d'entraînement et serving (YAML / Hydra)
│   ├── train.yaml
│   └── serve.yaml
├── data/                    # Contrats de données, golden datasets de référence
│   └── data_contract.yaml
├── docker/                  # Dockerfile.train et Dockerfile.serve
├── docs/                    # Documentation, cadrage, Model Card, ADRs
├── src/{{NOM_PROJET}}/      # Package Python principal (data, model, training, serving, monitoring)
├── tests/                   # Tests unitaires, tests de données, tests de qualité modèle
├── Makefile                 # Commandes canoniques : make lint, make test, make train, make serve
├── pyproject.toml           # Dépendances (uv), ruff, mypy, pytest
└── MLOPS.md                 # Le playbook de référence
```

---

## 🛠️ Commandes Usuelles du Makefile

Chaque projet adoptant ce playbook expose une interface unifiée :

```bash
make install        # Synchronise l'environnement virtuel avec uv et installe les hooks pre-commit
make lint           # Vérifie le code avec ruff et mypy (mode strict)
make test           # Lance la suite complète de tests (unit, data, model)
make validate-data  # Valide les données d'entrée par rapport au Data Contract
make train          # Lance le pipeline d'entraînement avec la configuration par défaut
make serve          # Démarre l'API locale FastAPI sur le port 8000
```

---

## 📄 Licence

Ce playbook est distribué sous licence MIT. Réutilisable, modifiable et adaptable librement pour vos projets persos, d'équipe ou d'entreprise.
