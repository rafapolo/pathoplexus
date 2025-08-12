# Pathoplexus Technology Stack & Frameworks

## Programming Languages

### Primary Languages
1. **Kotlin** - Backend services (Spring Boot)
2. **TypeScript/JavaScript** - Frontend and build tools
3. **Python** - Data processing, CLI tools, and bioinformatics pipelines
4. **Java** - Runtime for Kotlin backend (JVM 21)

## Backend Framework & Technologies

- **Spring Boot 3.5.3** (Kotlin)
- **Exposed ORM** - Database access layer for Kotlin
- **PostgreSQL** - Primary database
- **Flyway** - Database migrations
- **Docker** - Containerization
- **Kubernetes** - Orchestration

## Frontend Framework & Technologies

- **Astro 5.9.2** - Static site generator and web framework
- **React 18** - UI components and interactivity
- **TypeScript** - Type-safe JavaScript
- **Tailwind CSS** - Styling framework
- **Material UI (MUI)** - Component library
- **Vite** - Build tool and dev server
- **Node.js** - JavaScript runtime

## Data Processing & Bioinformatics

- **Python 3.12** - Primary language for data pipelines
- **Snakemake** - Workflow management system
- **Conda/Mamba** - Package and environment management
- **Nextclade** - Sequence analysis and phylogenetic classification
- **BioPython** - Biological sequence analysis
- **Pandas** - Data manipulation
- **NCBI Datasets CLI** - External data ingestion

## Testing & Quality Assurance

- **Vitest** - Frontend unit testing
- **Playwright** - End-to-end browser testing
- **JUnit** - Backend testing (Kotlin/Java)
- **Pytest** - Python testing
- **ESLint/Prettier** - Code formatting and linting
- **KtLint** - Kotlin code formatting

## Infrastructure & DevOps

- **Docker** - All services containerized
- **Kubernetes** - Container orchestration
- **Helm** - Kubernetes package manager
- **MinIO/S3** - Object storage
- **Keycloak** - Authentication and user management
- **ArgoCD** - GitOps deployment
- **LAPIS/SILO** - High-performance sequence search API

## Key Python Libraries & Tools

```python
# Core scientific/bioinformatics stack:
biopython        # Sequence analysis
pandas           # Data manipulation  
snakemake        # Workflow management
nextclade >=3.7.0 # Phylogenetic analysis
ncbi-datasets-cli # External data
psycopg2         # PostgreSQL connector
click            # CLI framework
pyyaml           # Configuration parsing
```

## Key TypeScript/JavaScript Libraries

```json
// Frontend stack:
{
  "astro": "^5.9.2",              // Web framework
  "react": "^18.3.1",             // UI library
  "@mui/material": "~5.14.20",    // Component library
  "@tanstack/react-query": "^4.39.2", // Data fetching
  "tailwindcss": "^3.4.17",       // CSS framework
  "vitest": "3.2.4",              // Testing
  "@playwright/test": "^1.54.1"   // E2E testing
}
```

## Architecture Pattern

- **Microservices** - Separate services for backend, frontend, preprocessing, ingestion
- **Event-driven** - Asynchronous processing pipelines
- **API-first** - RESTful APIs with OpenAPI documentation
- **Container-native** - Everything runs in Docker containers on Kubernetes

This represents a modern, cloud-native genomics platform built with industry-standard tools for scalability, maintainability, and scientific rigor.