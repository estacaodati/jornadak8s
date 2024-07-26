# Jornada Kubernetes

Repositório para praticar Kubernetes e aprender mais sobre como utilizar os recursos integrados a ele. Este repositório faz parte de uma playlist lançada no canal 🚂 [@estacaodati](https://www.youtube.com/@estacaodati)

---
### **DIA 0 - Entrando no mundo do AKS - Criação do cluster**

[![Dia0](imagens/Dia0.png)](https://youtu.be/PsHF4HR0Mcg "Aula 0 da Jornada Kubernetes")

### Criação do Cluster

```hcl
terraform {
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = "3.112.0"
    }
  }
}

provider "azurerm" {
  # Configuration options
  features {}
}

module "aks_lab" {
  source                = "git::https://github.com/estacaodati/terraform-module-kube"
  env                   = "dev"
  project_name          = "trilha-aks"
  node_pool_name        = "default"
  client_id             = ""
  client_secret         = ""
  k8s_node_vm_size      = "Standard_B2s"
  k8s_node_count        = 1
  k8s_node_os_disk_size = 30
  enable_prometheus     = false
  enable_grafana        = false
}
```

### Gerar as credenciais para se conectar ao cluster
```
az aks get-credentials --resource-group $(terraform output -raw rg_name) --name $(terraform output -raw cluster_name)
```

---
### **DIA 1 - Salvando o gameplay - Azure storage - blob, files e disks**

[![Dia0](imagens/Dia1.png)](https://youtu.be/0-r7Dg9epKY "Aula 0 da Jornada Kubernetes")

Seguindo com a configuração do nosso cluster, foi adicionado o arquivo de **outputs.tf**:

```hcl
output "rg_name" {
  value = module.aks_lab.resource_group_name
}

output "cluster_name" {
  value = module.aks_lab.kubernetes_cluster_name
}
```

# Comandos usados no vídeo:

### Gerar app registration
```
az ad sp create-for-rbac --name <rbac-name> --role Contributor --scopes /subscriptions/<sub>
```

### Coletar e configurar as credenciais
```
az aks get-credentials --resource-group $(terraform output -raw rg_name) --name $(terraform output -raw cluster_name) --overwrite-existing
```

### Habilitar o blob driver
```
az aks update --enable-blob-driver --resource-group $(terraform output -raw rg_name) --name $(terraform output -raw cluster_name)
```

### Arquivos YAML
[Dia 1 - Arquivos YAML](jornadak8s/tree/main/dia%201/yaml)

---
Este repositório faz parte da sequência de videos lançados pelo canal [Estação da TI](https://www.youtube.com/@estacaodati)
