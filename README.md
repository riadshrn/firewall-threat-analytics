<p align="center">
  <img src="images/logo-github.png" alt="Firewall Threat Analytics" width="480"/>
</p>

<h1 align="center">SISE-OPSIE 2026 — Firewall Threat Analytics</h1>

<p align="center">
  Application Streamlit interactive pour l'analyse des logs Firewall Iptables (cloud + on-prem).<br/>
  Visualisation • Rules/Ports • IP Threats • ML Anomalies • LLM Analyst
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Streamlit-1.32+-red?logo=streamlit&logoColor=white"/>
  <img src="https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white"/>
  <img src="https://img.shields.io/badge/LLM-Mistral-orange?logo=openai&logoColor=white"/>
  <img src="https://img.shields.io/badge/ML-IsolationForest-green?logo=scikit-learn&logoColor=white"/>
</p>

---

## 📸 Aperçu de l'application

### 📊 Overview — KPIs & Timeline
> Vue d'ensemble : total événements, IP sources, ratio ALLOW/DENY, protocoles, top règles et timeline horaire.

<p align="center">
  <img src="images/1.png" alt="Overview" width="100%"/>
</p>

---

### 📋 Rules & Ports — Top règles & Sankey
> Top 5 TCP, Top 10 UDP, et diagramme Sankey interactif Rule ID → Port DST → Action.

<p align="center">
  <img src="images/2.png" alt="Rules and Ports" width="100%"/>
</p>

---

### 🌐 IP Analysis — Top sources & hors plan
> Top 5 IP émettrices, top 10 ports <1024 autorisés, tableau des IP hors plan d'adressage avec triage IA.

<p align="center">
  <img src="images/3.png" alt="IP Analysis" width="100%"/>
</p>

---

### 🗺️ Carte géographique — GeoIP
> Origine des connexions géolocalisées (ip-api), avec arcs de connexion sources → destinations.

<p align="center">
  <img src="images/4.png" alt="GeoIP Map" width="100%"/>
</p>

---

### 🤖 ML / Anomalies — IsolationForest
> Détection d'anomalies non supervisée, top 20 IP suspectes, scatter score, parallel coordinates.

<p align="center">
  <img src="images/5.png" alt="ML Anomalies" width="100%"/>
</p>

---

### 🌍 Globe tactique — IP suspecte + Timeline
> Globe 3D terminator jour/nuit, timeline par heure, contexte calendrier (jours fériés FR), derniers événements.

<p align="center">
  <img src="images/6.png" alt="Globe Tactique" width="100%"/>
</p>

---

### 🧠 Hypothèse politique firewall (IA)
> Analyse défensive par IP : inférence de la politique firewall observée à partir des logs, sans procédure offensive.

<p align="center">
  <img src="images/7.png" alt="Policy Inference" width="100%"/>
</p>

---

### 📝 LLM Report — Rapport d'incident
> Rapport d'incident structuré par IP (Mistral API ou template offline) : comportement, hypothèse, recommandations.

<p align="center">
  <img src="images/8.png" alt="LLM Report" width="100%"/>
</p>

---

## 📁 Structure du projet

```
opsie-app/
├── app.py                  # Application Streamlit principale
├── src/
│   ├── loader.py           # Chargement & parsing données (Parquet / CSV / raw_log)
│   ├── ml.py               # Features engineering + IsolationForest
│   └── llm.py              # Rapports IA (Mistral API ou template offline)
├── images/
│   └── logo.png            # Logo sidebar
├── requirements.txt
├── Dockerfile
└── README.md
```

---

## 🚀 Lancement local

```bash
# 1. Installer les dépendances
pip install -r requirements.txt

# 2. (Optionnel) Clé Mistral pour les rapports IA
export MISTRAL_API_KEY=votre_clé_ici

# 3. Lancer l'application
streamlit run app.py

# 4. Ouvrir → http://localhost:8501
```

Placez vos fichiers Parquet dans `data/` ou uploadez-les directement via l'interface.

---

## 🐳 Docker

### Build & Run

```bash
# Build de l'image
docker build -t opsie-app .

# Lancement avec données montées
docker run -p 8501:8501 \
  -v $(pwd)/data:/data \
  opsie-app

# Avec clé Mistral (optionnel)
docker run -p 8501:8501 \
  -v $(pwd)/data:/data \
  -e MISTRAL_API_KEY=votre_clé_ici \
  opsie-app
```

### Depuis Docker Desktop

1. `docker build -t opsie-app .`
2. **Images** → `opsie-app` → **Run**
3. Ports : `8501:8501` | Volume : dossier `data/` → `/data`
4. Ouvrir → [http://localhost:8501](http://localhost:8501)

---

## 📊 Formats de données supportés

| Format | Description |
|--------|-------------|
| `logs_export.parquet` | Colonnes structurées : `date, ip_src, ip_dst, protocol, src_port, dst_port, rule_id, action, in_interface, out_interface` |
| `clean_logs_nov2025_feb2026.parquet` | Colonne `raw_log` semi-structurée séparée par `;` |
| `.csv` | Même colonnes que le format structuré (fallback) |

> La colonne `fw` (numéro firewall) est automatiquement ignorée.

---

## 🧩 Fonctionnalités

| Onglet | Contenu |
|--------|---------|
| **📊 Overview** | KPIs (événements, IP, ports, ratio), PERMIT/DENY, protocoles, top règles, timeline horaire |
| **📋 Rules & Ports** | Top 5 TCP / Top 10 UDP, Sankey interactif rule_id → dst_port → action |
| **🌐 IP Analysis** | Bubble plot, top 5 émetteurs, top 10 ports <1024, IPs hors-plan, carte GeoIP, datatable |
| **🤖 ML / Anomalies** | IsolationForest (contamination réglable), top 20 suspects, globe 3D, timeline, analyse IA |
| **📝 LLM Report** | Rapport d'incident par IP (Mistral API ou template heuristique offline) |

---

## 🔑 Variable d'environnement

| Variable | Description |
|----------|-------------|
| `MISTRAL_API_KEY` | Clé API Mistral pour activer le mode LLM (rapports IA). Sans clé, un template offline est utilisé. |

---

## 🛠️ Dépendances principales

```
streamlit>=1.32.0
pandas>=2.0.0
numpy>=1.24.0
plotly>=5.18.0
scikit-learn>=1.3.0
pyarrow>=14.0.0
mistralai>=1.0.0
requests>=2.31.0
```

---

## 👥 Projet universitaire

**SISE-OPSIE 2026** — Université Lyon 2  
Encadrants : Ricco RAKOTOMALALA, David PIERROT
