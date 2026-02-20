# Luna – Manhattan My Way

## Application Walkthrough

[![Watch Demo](https://img.youtube.com/vi/v3rjbjH0VNo/maxresdefault.jpg)](https://www.youtube.com/watch?v=v3rjbjH0VNo)

---

This is a full-stack web application designed to help New Yorkers make smarter, greener, and less crowded mobility decisions.

---

## Team Members

| Name               | Role                |
| ------------------ | ------------------- |
| HSUAN-YU TAN       | Back-End Code Lead  |
| Zhaofang He        | Maintenance Lead    |
| Neasa Ní Fhátharta | Data Lead           |
| Martynas Kapocius  | Coordination Lead   |
| Prakhar Dayal      | Front-End Code Lead |

## Tech Stack

- **Frontend**: Next.js (React framework), Mapbox GL JS, TypeScript
- **Backend API**: Next.js API Routes, Node.js
- **Machine Learning**: scikit-learn, XGBoost (Python Flask microservice)
- **Deployment**: Docker + GitLab CI/CD on Linux server
- **Infrastructure**: Docker Compose, nginx

## Data Access

**Note:** Large data files (MTA subway data, taxi data, etc.) are provided via external links to avoid repository size issues.

- **Non-UCD users:** [OneDrive Link](https://1drv.ms/u/c/8d3d08b3430e0636/EdUH4bKPSSxJjh0gCPrHzXYBkHWbARjVnDuLgMxHRidYfw)
- **UCD users:** [SharePoint Link](https://ucd.sharepoint.com/:f:/s/Team9-ResearchPracticum/EvPx4NfivI5Bu1NMOzEvmOEB7gtJQxPcXbWLRkZa8mz_kw?e=zCLaud)

Available data files:
- `MTA_Subway_Hourly_Ridership_Manhattan_2024.csv` (1.2GB)
- `combined_yellow_tripdata.parquet` (830MB)
- `manhattan_taxi_2024_clean.parquet` (731MB)
- Monthly taxi data files (48-62MB each)
- `zone_hourly_summary.csv` (120MB)

## Quick Start with Automation

For the complete project setup with automatic code quality and maintenance:

```bash
# One-time setup (includes git hooks and dependencies)
npm run setup

# Daily development
git add .
git commit -m "feat: your changes"  # Automatic code formatting and checks

# When you need to clean up Docker cache
npm run cleanup

# Start the project
docker-compose up -d
```

**For detailed automation features, see [AUTOMATION-GUIDE.md](docs/AUTOMATION-GUIDE.md)**

# ML API Integration Guide

This document explains how to call the ML API in different environments.

## Development Environment

### ➤ **Option 1. Running Flask app directly**

If you are running the Flask app directly on your local machine, fetch the API using: http://127.0.0.1:5000/predict-all

### ➤ **Option 2. Running the entire project with Docker**

If you are running the project via `docker`, fetch the API using: http://localhost:5000/predict-all

## Production Environment

In production, always fetch: /api/ml/predict-all

This endpoint is **proxied by Nginx or the Next.js API route to the ML server**. You do not need to call the ML server directly.

## Project Structure

```
comp47360_team9/
├── README.md                    # Project overview and setup instructions
├── package.json                 # Root package.json for project metadata and scripts
├── package-lock.json            # Dependency lock file
├── docker-compose.yml           # Development Docker configuration
├── docker-compose.prod.yml      # Production Docker configuration
├── nginx.conf                   # Nginx configuration for production
├── .gitignore                   # Git ignore rules
├── .dockerignore                # Docker ignore rules
├── .pre-commit-config.yaml      # Pre-commit hooks configuration
├── .gitlab-ci.yml               # GitLab CI/CD configuration
├── gitlab-ci-alternatives.yml   # Alternative CI/CD configurations
├── .gitlab-ci-fixed.yml         # Fixed CI/CD configuration

# Frontend Application
├── webapp/                      # Next.js application
│   ├── src/
│   │   ├── app/                 # Next.js App Router
│   │   │   ├── layout.tsx       # Root layout component
│   │   │   ├── page.tsx         # Home page
│   │   │   ├── globals.css      # Global styles
│   │   │   ├── api/             # API route handlers
│   │   │   │   ├── bikes/       # Bike sharing data endpoint
│   │   │   │   ├── contact/     # Contact form endpoint
│   │   │   │   ├── directions/  # Routing directions endpoint
│   │   │   │   ├── EV-charging/ # EV charging stations endpoint
│   │   │   │   ├── manhattan/   # Manhattan data endpoint (includes busyness)
│   │   │   │   ├── parks/       # Parks data endpoint
│   │   │   │   ├── token/       # Token validation endpoint
│   │   │   │   ├── validation/  # Data validation endpoint
│   │   │   │   └── weather/     # Weather data endpoint
│   │   │   ├── contact/         # Contact page
│   │   │   ├── map/             # Interactive map page
│   │   │   └── containers/      # Page-level container components
│   │   ├── components/          # Reusable UI components
│   │   ├── hooks/               # Custom React hooks
│   │   ├── utils/               # Utility functions
│   │   ├── assets/              # Images and static assets
│   │   ├── constants/           # App constants and icons
│   │   ├── contexts/            # React contexts
│   │   ├── lib/                 # Library utilities
│   │   ├── middleware/          # Next.js middleware
│   │   ├── tests/               # Test files
│   │   └── types/               # TypeScript type definitions
│   ├── public/                  # Static files served by Next.js
│   │   ├── data/                # JSON data files for zones and maps
│   │   └── *.svg, *.ico         # Icons and favicons
│   ├── Dockerfile               # Docker configuration for webapp
│   ├── package.json             # Frontend dependencies
│   ├── next.config.ts           # Next.js configuration
│   ├── tailwind.config.js       # Tailwind CSS configuration
│   ├── postcss.config.mjs       # PostCSS configuration
│   ├── eslint.config.mjs        # ESLint configuration
│   ├── jest.config.js           # Jest testing configuration
│   ├── jest.setup.js            # Jest setup configuration
│   └── tsconfig.json            # TypeScript configuration

# Machine Learning Service
├── ml/                          # Python Flask ML API service
│   ├── app.py                   # Flask API server with prediction endpoints
│   ├── requirements.txt         # Python dependencies
│   ├── Dockerfile               # Docker configuration for ML service
│   ├── __init__.py              # Python package initialization
│   ├── app_py_documentation.md  # API documentation
│   ├── xgboost_taxi_model.joblib              # Trained XGBoost taxi model
│   ├── subway_ridership_model_xgboost_final.joblib  # Trained subway ridership model
│   ├── manhattan_taxi_zones.csv                 # NYC taxi zone reference data
│   ├── subway_stations.csv                      # Subway stations data
│   ├── station_to_zone_mapping.csv              # Station to zone mapping
│   ├── zone_hourly_busyness_stats.csv           # Taxi zone busyness statistics
│   ├── zone_subway_busyness_stats.csv           # Subway zone busyness statistics
│   ├── required_features.json                   # Required features for ML models
│   ├── utils/                   # Utility modules
│   │   ├── __init__.py
│   │   ├── request_tracker.py   # Request tracking functionality
│   │   └── tests/               # Utility tests
│   └── tests/                   # ML service tests
│       ├── conftest.py          # Test configuration
│       ├── test_basic.py        # Basic functionality tests
│       ├── test_geometry.py     # Geometry-related tests
│       ├── test_health.py       # Health check tests
│       ├── test_helpers.py      # Helper function tests
│       ├── test_predict_all.py  # Prediction endpoint tests
│       ├── test_predict_auth.py # Authentication tests
│       ├── test_predict_endpoint.py # Endpoint tests
│       ├── test_root.py         # Root endpoint tests
│       └── test_routes_basic.py # Route tests

# Data and Analysis
├── data/                        # Data files and analysis
│   ├── manhattan-subway/        # Subway data and analysis
│   │   ├── data/
│   │   │   ├── raw/             # Raw data files
│   │   │   │   ├── mta/         # MTA subway data
│   │   │   │   └── weather_data.csv
│   │   │   └── processed/       # Processed data
│   │   │       ├── analysis/    # Analysis results
│   │   │       ├── integration/ # Integrated datasets
│   │   │       ├── modeling/    # ML modeling data
│   │   │       └── models/      # Trained models
│   │   ├── notebooks/           # Jupyter notebooks for analysis
│   │   ├── results/             # Analysis results and plots
│   │   ├── README.md            # Subway data documentation
│   │   └── requirements.txt     # Analysis dependencies
│   ├── taxi/                    # Taxi data and analysis
│   ├── taxi_locations/          # Taxi zone location data
│   └── weather/                 # Weather data

# Deployment & Operations
├── scripts/                     # Deployment and utility scripts
│   ├── setup/                   # Setup scripts
│   │   ├── configure-firewall.sh
│   │   ├── install-gitlab-runner-linux.sh
│   │   ├── setup-https-acme-simple.sh
│   │   ├── setup-nginx.sh
│   │   ├── setup-server.sh
│   │   └── setup-ssh-keys.sh
│   ├── archive/                 # Archived scripts
│   ├── windows/                 # Windows batch scripts
│   ├── setup-git-hooks.sh       # Git hooks setup
│   ├── start-project.sh         # Project startup script
│   ├── test-complete-system.sh  # System testing
│   ├── test-ml-api.sh           # ML API testing
│   ├── test-request-tracking.sh # Request tracking tests
│   ├── diagnose-project.sh      # Project diagnostics
│   ├── maintenance.sh           # Maintenance utilities
│   ├── scheduled-maintenance.sh # Scheduled maintenance
│   └── cleanup-scripts.sh       # Cleanup utilities
├── nginx/                       # Nginx configuration files
│   └── nginx.conf               # Nginx server block configuration

# Documentation
└── docs/                        # Project documentation
    ├── git_workflow.md          # Git branching strategy
    ├── AUTOMATION-GUIDE.md      # Automation features guide
    ├── DOCKER-SETUP-GUIDE.md    # Docker setup instructions
    └── SCRIPTS-ANALYSIS.md      # Scripts analysis
```

## How to Run the Project
### Local Development
#### Quick Start with Docker
```bash
docker-compose down
docker-compose up --build
```

### Production Deployment
The project uses GitLab CI/CD for automated deployment to a Linux server.
#### Deployment URLs
- **Webapp**: http://137.43.49.26 
- **Flask**: http://137.43.49.26/api/ml/ and http://137.43.49.26/api/ml/predict-all

## Features

- **Interactive Map**: Mapbox GL JS integration for NYC navigation
- **Route Planning**: Multi-modal transportation options with OSRM routing
- **Real-time Data**: Weather, bike availability, EV charging stations
- **ML Predictions**: Crowd density and busyness forecasting for both taxi and subway
- **Responsive Design**: Mobile-first approach
- **Data Integration**: Comprehensive integration of subway, taxi, and weather data
- **Spatial Analysis**: Geographic analysis and zone mapping

## Git Branching Model

We use a simple Git branching strategy based on `main`, `develop`, and `feature/*`.

- `main`: Stable and deployable version.
- `develop`: Integration branch for all features.
- `feature/*`: One branch per feature (created from `develop`, merged back into `develop`).
- `hotfix/*`: For urgent fixes (from `main`, merged into `main` and `develop`).

See `docs/git_workflow.md` for full explanation and diagram.

## Available Scripts

The project includes several npm scripts for development and maintenance:

- `npm run setup` - Complete project setup
- `npm run maintenance` - Run maintenance tasks
- `npm run health-check` - Check system health
- `npm run cleanup` - Clean up Docker cache
- `npm run security-scan` - Run security scans
- `npm run diagnose` - Diagnose project issues
- `npm run test-api` - Test ML API endpoints
