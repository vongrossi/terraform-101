### Conceitos Básicos

Esta seção introduz os conceitos fundamentais do Terraform, proporcionando uma base sólida para a criação e gerenciamento de infraestrutura como código.

---

#### Providers

**1. O que são Providers?**

- **Definição:**
  - **Providers** são plugins que permitem ao Terraform interagir com APIs de diferentes serviços e plataformas para gerenciar recursos.
  - Cada provider é responsável por entender a API do serviço e traduzir as configurações Terraform em chamadas para a API.

- **Exemplos de Providers:**
  - **Nuvem:** AWS, Azure, GCP.
  - **On-Premises:** VMware, OpenStack.
  - **Outros Serviços:** GitHub, Datadog.

**2. Configuração de Providers:**

- **Sintaxe Básica:**
  ```hcl
  provider "aws" {
    region = "us-east-1"
  }
  ```

- **Configuração com Variáveis:**
  ```hcl
  variable "aws_region" {
    default = "us-east-1"
  }

  provider "aws" {
    region = var.aws_region
  }
  ```

- **Configuração Múltipla:**
  - **Exemplo: Configurar múltiplas contas ou regiões:**
    ```hcl
    provider "aws" {
      alias  = "primary"
      region = "us-east-1"
    }

    provider "aws" {
      alias  = "secondary"
      region = "us-west-2"
    }
    ```

- **Recursos:**
  - [Terraform Providers](https://registry.terraform.io/browse/providers)

---

#### Recursos (Resources)

**1. O que são Recursos?**

- **Definição:**
  - **Recursos** são os blocos fundamentais que descrevem os componentes da infraestrutura, como instâncias de VM, redes, balanceadores de carga, etc.
  - Cada recurso possui um tipo, como `aws_instance`, e propriedades específicas.

**2. Definindo Recursos:**

- **Sintaxe Básica:**
  ```hcl
  resource "aws_instance" "example" {
    ami           = "ami-0c55b159cbfafe1f0"
    instance_type = "t2.micro"

    tags = {
      Name = "ExampleInstance"
    }
  }
  ```

- **Referência e Dependências:**
  - Recursos podem se referir uns aos outros criando dependências.
  - **Exemplo:** Dependência explícita usando `depends_on`:
    ```hcl
    resource "aws_instance" "example" {
      ami           = "ami-0c55b159cbfafe1f0"
      instance_type = "t2.micro"
    }

    resource "aws_eip" "example_ip" {
      instance = aws_instance.example.id
      depends_on = [aws_instance.example]
    }
    ```

- **Recursos Dinâmicos:**
  - Criando múltiplos recursos usando `count` e `for_each`.
  - **Exemplo com `count`:**
    ```hcl
    resource "aws_instance" "example" {
      count         = 2
      ami           = "ami-0c55b159cbfafe1f0"
      instance_type = "t2.micro"
    }
    ```

  - **Exemplo com `for_each`:**
    ```hcl
    resource "aws_instance" "example" {
      for_each      = var.instances
      ami           = each.value.ami
      instance_type = each.value.instance_type
    }
    ```

**3. Propriedades Comuns:**

- **`depends_on`:** Define dependências explícitas.
- **`count` e `for_each`:** Controla a criação de múltiplas instâncias de um recurso.
- **`lifecycle`:** Controla o ciclo de vida de um recurso, como criar antes de destruir.

- **Recursos:**
  - [Documentação de Recursos](https://www.terraform.io/docs/language/resources/syntax.html)

---

#### Variáveis

**1. O que são Variáveis?**

- **Definição:**
  - **Variáveis** permitem a parametrização de configurações, facilitando a reutilização e customização.
  - Elas podem ser definidas para diferentes tipos de dados e valores padrão.

**2. Tipos de Variáveis:**

- **String:**
  ```hcl
  variable "instance_type" {
    type    = string
    default = "t2.micro"
  }
  ```

- **Number:**
  ```hcl
  variable "instance_count" {
    type    = number
    default = 1
  }
  ```

- **Boolean:**
  ```hcl
  variable "enable_monitoring" {
    type    = bool
    default = true
  }
  ```

- **List:**
  ```hcl
  variable "availability_zones" {
    type = list(string)
    default = ["us-east-1a", "us-east-1b"]
  }
  ```

- **Map:**
  ```hcl
  variable "tags" {
    type = map(string)
    default = {
      Name = "ExampleInstance"
      Environment = "dev"
    }
  }
  ```

- **Object:**
  ```hcl
  variable "instance_config" {
    type = object({
      ami = string
      instance_type = string
    })
  }
  ```

**3. Definindo e Usando Variáveis:**

- **Definição no Arquivo de Variáveis:**
  - **Arquivo:** `variables.tf`
  ```hcl
  variable "aws_region" {
    description = "The AWS region to deploy in"
    type        = string
    default     = "us-east-1"
  }
  ```

- **Passando Valores de Variáveis:**
  - **Linha de Comando:**
    ```bash
    terraform apply -var="aws_region=us-west-2"
    ```
  - **Arquivo `.tfvars`:**
    - **Arquivo:** `terraform.tfvars`
    ```hcl
    aws_region = "us-west-2"
    ```
  - **Variáveis de Ambiente:**
    ```bash
    export TF_VAR_aws_region="us-west-2"
    ```

**4. Sensibilidade e Segurança:**

- **Variáveis Sensíveis:**
  - Use o atributo `sensitive` para ocultar valores sensíveis:
    ```hcl
    variable "db_password" {
      type      = string
      sensitive = true
    }
    ```

- **Recursos:**
  - [Documentação de Variáveis](https://www.terraform.io/docs/language/values/variables.html)

---

#### Outputs

**1. O que são Outputs?**

- **Definição:**
  - **Outputs** são valores que o Terraform exibe após a aplicação de um plano. Eles podem ser usados para referenciar informações importantes ou compartilhá-las entre diferentes configurações.

**2. Configurando Outputs:**

- **Sintaxe Básica:**
  ```hcl
  output "instance_id" {
    value = aws_instance.example.id
  }
  ```

- **Referência em Outros Módulos:**
  - Outputs podem ser referenciados por outros módulos para encadear configurações.
  - **Exemplo:**
    ```hcl
    module "vpc" {
      source = "./vpc"
    }

    output "vpc_id" {
      value = module.vpc.vpc_id
    }
    ```

**3. Propriedades de Outputs:**

- **`value`:** O valor a ser exibido ou compartilhado.
- **`description`:** Descrição do output.
- **`sensitive`:** Indica se o output é sensível e deve ser ocultado.

**4. Usos Comuns:**

- **Compartilhamento de Dados:**
  - Outputs podem ser utilizados para compartilhar dados entre módulos, como IDs de recursos ou informações de configuração.

- **Debug e Informações:**
  - Outputs fornecem uma maneira conveniente de exibir informações sobre os recursos provisionados.

- **Exemplo Avançado:**
  ```hcl
  output "instance_public_ip" {
    value       = aws_instance.example.public_ip
    description = "The public IP address of the instance"
  }
  ```

- **Recursos:**
  - [Documentação de Outputs](https://www.terraform.io/docs/language/values/outputs.html)

---

### Recursos Adicionais
- **Documentação Oficial do Terraform:** [Terraform Documentation](https://www.terraform.io/docs)
- **Tutoriais e Exemplos:** [HashiCorp Learn](https://learn.hashicorp.com/terraform)
- **Comunidade e Fóruns:** [Terraform Discuss](https://discuss.hashicorp.com/c/terraform-core/)

