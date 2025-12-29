# terraformAWSLab6

## À faire

### Projet de base

- [x] VPC (Sous-réseaux, zones de disponibilité)
- [x] Internet Gateway
- [x] Nat Gateway
- [x] Serveur Web (création, groupe de sécurité, user data)
- [x] Table de routage vers Internet
- [x] Table de routage du sous-réseau privé vers la NAT Gateway
- [x] RDS Primaire (création et groupe de sécurité)
- [ ] RDS Secondaire (Non disponible avec un compte étudiant)

![Schéma du projet](img/schemaProjet.png)

### Workflow GitHub Actions

- [x] Workflow GitHub Actions qui crée toutes les ressources dans les fichiers Terraform -> [terraformApply.yml](.github/workflows/terraformApply.yml)
- [x] Workflow GitHub Actions qui détruit toutes les ressources dans l'état Terraform -> [terraformDestroy.yml](.github/workflows/terraformDestroy.yml)
- [x] Le bucket S3 stocke le fichier `terraform.tfstate`

![Schéma du workflow](img/schemaWorkflow.png)

## Secrets

Créer des secrets GitHub dans Settings -> Secrets and variables -> Actions -> New repository secret

- Identifiants AWS (Dans les détails AWS)
  - AWS_ACCESS_KEY_ID
  - AWS_REGION
  - AWS_SECRET_ACCESS_KEY
  - AWS_SESSION_TOKEN

- Base de données AWS RDS (Vous pouvez choisir)
  - DB_PASSWORD
  - DB_USERNAME
  
- Clé SSH AWS pour l'instance EC2 du serveur Web
  - Générer une clé SSH : `ssh-keygen -t rsa -b 4096 -f web-server-key`
  - SSH_PUBLIC_KEY

- Bucket S3 AWS (Vous pouvez choisir)
  - S3_BACKEND_BUCKET

## Structure du projet

```bash
.
├── .github
│   └── workflows
│       ├── terraformApply.yml
│       └── terraformDestroy.yml
├── data.tf
├── main.tf
├── outputs.tf
├── terraform.tfvars
└── variables.tf
```

- terraformApply.yml : Workflow qui applique automatiquement la configuration Terraform et crée toutes les ressources AWS
- terraformDestroy.yml : Workflow qui détruit automatiquement toutes les ressources AWS gérées par l'état Terraform
- data.tf : Définit les sources de données utilisées par Terraform (AMIs)
- main.tf : Déclare les ressources principales (réseau, instances, groupes de sécurité, routes)
- variables.tf : Déclare les variables d'entrée (types, valeurs par défaut)
- terraform.tfvars : Fournit les valeurs concrètes pour les variables (CIDRs, tailles, noms)
- outputs.tf : Expose les sorties affichées après l'exécution de `terraform apply` (IDs des sous-réseaux et VPC et lien cliquable vers le serveur Web)
- terraform.tfstate : État Terraform (Ne pas modifier manuellement)

## Commandes

### Terraform

- `terraform init` - Initialise un répertoire de travail Terraform et télécharge les plugins des fournisseurs requis
- `terraform fmt` - Formate les fichiers de configuration Terraform selon le style standard
- `terraform fmt -check` - Vérifie si les fichiers sont correctement formatés sans les modifier
- `terraform validate` - Valide la syntaxe et la configuration des fichiers Terraform
- `terraform plan` - Affiche les modifications que Terraform apportera à votre infrastructure
- `terraform apply` - Applique les modifications planifiées et crée ou met à jour les ressources
- `terraform destroy` - Supprime toutes les ressources définies dans la configuration Terraform

### SSH

- `ssh -i web-server-key ec2-user@<Public_DNS>`
