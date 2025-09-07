# Terraform GCP DevOps Project

- **Purpose**: Provision GCP infrastructure for Dev and Prod using Terraform.
- **Branching Strategy**:
  - `feature/*` → dev preview (validate & plan)
  - `main` → central trunk
  - `dev` → plan & apply changes
  - `prod` → plan & apply changes

- **Project Structure**:
  - `.github/workflows/terraform.yml` → CI/CD pipeline
  - `environments/dev` & `environments/prod` → Terraform code per environment
  - `modules/vminstance` → reusable VM instance module

- **Terraform Settings**:
  - Backend: GCS (`terraform-state-hero-gke`)
  - Provider: `google` with project & region variables
  - Input variables:
    - `gcp_project`, `gcp_region1`
    - `machine_type`, `environment`, `business_divsion`

- **GitHub Actions Pipeline**:
  - Triggers: push/pull request to `feature/*`, `dev`, `prod`
  - Steps:
    - Checkout repo
    - Set GCP credentials from secrets
    - Install Terraform
    - Feature branches: validate & plan
    - Dev/Prod: plan & apply

- **GCP Service Account Permissions**:
  - Compute Admin
  - Service Account User
  - Project Viewer
  - Must belong to the same project as `GCP_PROJECT_ID`

- **Workflow**:
  1. Create feature branch → push changes → PR to main
  2. Merge main → dev → plan & apply
  3. Merge dev → prod → plan & apply
