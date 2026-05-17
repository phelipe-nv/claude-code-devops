## 0. Preparação

- [x] 0.1 Carregar a skill de Terraform executando `/gerador-terraform`

## 1. Módulo Terraform — `modules/vpc`

- [x] 1.1 Criar `terraform/modules/vpc/variables.tf` com inputs: nome, região e ip_range da VPC
- [x] 1.2 Criar `terraform/modules/vpc/main.tf` com recurso `digitalocean_vpc`
- [x] 1.3 Criar `terraform/modules/vpc/outputs.tf` expondo `vpc_id` e `vpc_urn`

## 2. Módulo Terraform — `modules/doks`

- [x] 2.1 Criar `terraform/modules/doks/variables.tf` com inputs: região, nome do cluster, tamanho e quantidade de nodes, e `vpc_id`
- [x] 2.2 Criar `terraform/modules/doks/main.tf` com recurso `digitalocean_kubernetes_cluster` associando o cluster à VPC via `vpc_uuid = var.vpc_id` (node pool fixo, sem autoscaler, versão via data source `digitalocean_kubernetes_versions`)
- [x] 2.3 Criar `terraform/modules/doks/outputs.tf` expondo `cluster_id`, `cluster_endpoint` e `kubeconfig`

## 3. Ambiente Terraform — `envs/dev`

- [x] 3.1 Criar `terraform/envs/dev/versions.tf` com provider `digitalocean/digitalocean ~> 2.0` usando `DIGITALOCEAN_TOKEN` via variável de ambiente
- [x] 3.2 Criar `terraform/envs/dev/variables.tf` declarando as variáveis do ambiente `dev`
- [x] 3.3 Criar `terraform/envs/dev/main.tf` chamando `module "vpc"` (`source = "../../modules/vpc"`) e `module "doks"` (`source = "../../modules/doks"`), passando `module.vpc.vpc_id` como input para o módulo doks
- [x] 3.4 Criar `terraform/envs/dev/outputs.tf` re-exportando outputs dos módulos vpc e doks
- [x] 3.5 Criar `terraform/envs/dev/terraform.tfvars` com os valores concretos do ambiente dev
- [x] 3.6 Adicionar ao `.gitignore`: `.terraform/`, `*.tfstate`, `*.tfstate.backup` e `terraform.tfvars`
- [ ] 3.7 Executar `terraform init` e `terraform apply` em `terraform/envs/dev/` e verificar que o cluster é criado com 2 nodes `Ready`

## 4. Ajuste no manifest Kubernetes — remoção do PVC do Postgres

- [x] 4.1 Remover o objeto `kind: PersistentVolumeClaim` de `k8s/kube-news.yml`
- [x] 4.2 Remover o bloco `volumeMounts` e o bloco `volumes` do Deployment do Postgres em `k8s/kube-news.yml`

