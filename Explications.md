# terraformAWSLab6

## Schema

![Project schema](img/schemaProjet.png)

## To do

- [x] VPC (Subnets, availability zones)
- [x] Internet Gateway
- [x] Nat Gateway
- [x] Web server (create, sg, user data)
- [x] Route table to internet
- [x] Route table private subnet to nat gateway
- [x] RDS Primary (create and sg)
- [ ] RDS Secondary (Not possible with student account)

## Credentials

- Mettre vos credentials aws: `notepad "$env:USERPROFILE\.aws\credentials"`
- Générer clé SSH: `ssh-keygen -t rsa -b 4096 -f web-server-key`

## Project Structure

```bash
.
├── data.tf
├── main.tf
├── outputs.tf
├── secrets.tfvars
├── terraform.tfstate
├── terraform.tfvars
├── variables.tf
```

## Description des fichiers

- data.tf: définit les sources de données utilisées par Terraform (AMI pour le web server, amazon linux 2 dans ce cas)
- main.tf: déclare les ressources principales (réseau, instances, groupes de sécurité, routes)
- variables.tf: déclare les variables d'entrée (types, valeurs par défaut) -> But pas coder en dur dans main.tf et améliorer la lisibilité du code
- terraform.tfvars: fournit les valeurs concrètes des variables définies dans variables.tf(CIDRs, tailles, noms)
- outputs.tf: expose les sorties affichées après l'exécution de `terraform apply` (IDs des sous-réseaux, id du VPC, endpoint DB RDS et lien cliquable vers le serveur web)
- terraform.tfstate: état de Terraform (Ne pas modifier, contient les ressources créées par Terraform)
- secrets.tfvars: contient `db_password` et `db_username`

## Commands

### Terraform

- `terraform init` - Initialise un répertoire de travail Terraform et télécharge les plugins des fournisseurs requis
- `terraform fmt` - Formate les fichiers de configuration Terraform selon le style standard
- `terraform fmt -check` - Vérifie si les fichiers sont correctement formatés sans les modifier
- `terraform validate` - Valide la syntaxe et la configuration des fichiers Terraform
- `terraform plan` - Affiche les modifications que Terraform apportera à l'infrastructure
- `terraform apply` - Applique les modifications planifiées et crée ou met à jour les ressources
- `terraform destroy` - Supprime toutes les ressources définies dans la configuration Terraform

### SSH

- `ssh -i web-server-key ec2-user@<Public_DNS>`
