## ADDED Requirements

### Requirement: Cluster DOKS provisionado via Terraform na região NYC1
O sistema SHALL ter código Terraform organizado em módulo reutilizável (`terraform/modules/doks/`) e ambiente (`terraform/envs/dev/`) que provisiona o ambiente `dev` com um cluster Kubernetes gerenciado (DOKS) na DigitalOcean, região NYC1, com VPC dedicada, control plane single (sem HA) e node pool fixo de 2 nodes do tipo `s-2vcpu-2gb`. O provider `digitalocean/digitalocean` SHALL estar fixado na versão `~> 2.0` no `versions.tf` do ambiente.

#### Scenario: Cluster criado com terraform apply
- **WHEN** o operador executa `terraform apply` no diretório `terraform/envs/dev/`
- **THEN** o cluster DOKS SHALL ser criado na região NYC1 com 2 nodes do tipo `s-2vcpu-2gb` associados a uma VPC dedicada

#### Scenario: kubectl funcional após apply
- **WHEN** o `terraform apply` conclui com sucesso
- **THEN** o operador SHALL conseguir configurar o `kubectl` e executar `kubectl get nodes` vendo 2 nodes com status `Ready`

#### Scenario: Infraestrutura destruída com terraform destroy
- **WHEN** o operador executa `terraform destroy`
- **THEN** o cluster DOKS, a VPC e o Load Balancer SHALL ser removidos da conta DigitalOcean

### Requirement: VPC dedicada provisionada por módulo próprio
A VPC SHALL ser gerenciada pelo módulo `terraform/modules/vpc/`, separado do módulo `doks`. O módulo `doks` SHALL receber o `vpc_id` como variável de input e associar o cluster via `vpc_uuid`. A VPC não SHALL ser a VPC default da conta DigitalOcean.

#### Scenario: Módulo vpc chamado antes do módulo doks no ambiente dev
- **WHEN** o arquivo `terraform/envs/dev/main.tf` é inspecionado
- **THEN** ele SHALL chamar `module "vpc"` com `source = "../../modules/vpc"` e passar `module.vpc.vpc_id` como input para `module "doks"`

#### Scenario: Cluster associado à VPC do módulo vpc
- **WHEN** o Terraform provisiona os recursos do ambiente `dev`
- **THEN** o cluster DOKS SHALL estar associado à VPC criada pelo módulo `vpc`, identificada por nome que inclui o ambiente (e.g., `kube-news-dev`)

#### Scenario: Cluster não usa VPC default
- **WHEN** o cluster é inspecionado na DigitalOcean após o apply
- **THEN** ele SHALL estar associado a uma VPC cujo nome identifica o ambiente (e.g., `kube-news-dev`) e não à VPC default da conta

### Requirement: Módulo vpc com contrato completo
O módulo `terraform/modules/vpc/` SHALL conter exatamente três arquivos (`main.tf`, `variables.tf`, `outputs.tf`) expondo como outputs ao menos `vpc_id` e `vpc_urn`. O `ip_range` da VPC NÃO SHALL ser configurado explicitamente — a DigitalOcean atribui automaticamente um range válido para evitar conflito com ranges reservados internamente por região.

#### Scenario: Módulo vpc com os três arquivos de contrato
- **WHEN** o diretório `terraform/modules/vpc/` é inspecionado
- **THEN** ele SHALL conter `main.tf`, `variables.tf` e `outputs.tf`, e `outputs.tf` SHALL expor `vpc_id` e `vpc_urn`

#### Scenario: VPC criada sem ip_range explícito
- **WHEN** o recurso `digitalocean_vpc` é inspecionado no código Terraform
- **THEN** ele NÃO SHALL conter o atributo `ip_range` — o range é atribuído automaticamente pela DigitalOcean

### Requirement: Node pool fixo sem autoscaler
O node pool SHALL ter exatamente 2 nodes fixos, sem autoscaling habilitado, definido no código Terraform.

#### Scenario: Pool com 2 nodes fixos após apply
- **WHEN** o cluster é provisionado
- **THEN** o node pool SHALL ter `node_count = 2` com autoscaler desabilitado no recurso Terraform

### Requirement: Token da DigitalOcean não versionado
O token de API da DigitalOcean SHALL ser fornecido via variável de ambiente `DIGITALOCEAN_TOKEN` ou arquivo `terraform.tfvars` excluído do versionamento via `.gitignore`, nunca hardcoded no código Terraform.

#### Scenario: Código Terraform sem token exposto
- **WHEN** o repositório é inspecionado
- **THEN** nenhum arquivo versionado SHALL conter o token de API da DigitalOcean

### Requirement: Módulo doks com contrato completo
O módulo `terraform/modules/doks/` SHALL conter exatamente três arquivos (`main.tf`, `variables.tf`, `outputs.tf`) expondo como outputs ao menos `cluster_id`, `cluster_endpoint` e `kubeconfig`.

#### Scenario: Módulo chamado pelo ambiente dev
- **WHEN** o arquivo `terraform/envs/dev/main.tf` é inspecionado
- **THEN** ele SHALL referenciar o módulo com `source = "../../modules/doks"` passando as variáveis do ambiente

#### Scenario: versions.tf presente no ambiente dev
- **WHEN** o diretório `terraform/envs/dev/` é inspecionado
- **THEN** ele SHALL conter `versions.tf` com o provider `digitalocean/digitalocean` na versão `~> 2.0`
