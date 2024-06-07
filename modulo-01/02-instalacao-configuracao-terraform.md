### Instalação e Configuração

---

#### 1. Instalação do Terraform em Diferentes Sistemas Operacionais

**1.1. Windows**

**Passo a Passo:**

1. **Download:**
   - Acesse o site oficial do [Terraform](https://www.terraform.io/downloads).
   - Baixe o arquivo ZIP para Windows.

2. **Extrair:**
   - Extraia o conteúdo do ZIP em um diretório, por exemplo, `C:\Terraform`.

3. **Adicionar ao PATH:**
   - Abra o **Painel de Controle** > **Sistema** > **Configurações Avançadas do Sistema** > **Variáveis de Ambiente**.
   - Encontre a variável **Path** em **Variáveis do Sistema** e clique em **Editar**.
   - Adicione o caminho para o diretório onde o Terraform foi extraído, por exemplo, `C:\Terraform`.

4. **Verificar a Instalação:**
   - Abra o **Prompt de Comando** ou **PowerShell** e execute `terraform --version` para verificar se o Terraform está instalado corretamente.

**Recursos Adicionais:**
- **Documentação:** [Terraform para Windows](https://www.terraform.io/docs/cli/install/windows.html)

---

**1.2. Linux**

**Passo a Passo:**

1. **Download:**
   - Acesse o site oficial do [Terraform](https://www.terraform.io/downloads).
   - Baixe o arquivo ZIP para Linux.

2. **Extrair e Instalar:**
   - Extraia o arquivo ZIP:
     ```bash
     unzip terraform_<versão>_linux_amd64.zip
     ```
   - Mova o binário para `/usr/local/bin`:
     ```bash
     sudo mv terraform /usr/local/bin/
     ```
   - Altere as permissões, se necessário:
     ```bash
     sudo chmod +x /usr/local/bin/terraform
     ```

3. **Verificar a Instalação:**
   - Abra o terminal e execute `terraform --version` para verificar a instalação.

**Instalação Alternativa via Gerenciadores de Pacotes:**
- **Ubuntu/Debian:**
  ```bash
  sudo apt-get update
  sudo apt-get install -y terraform
  ```
- **CentOS/Fedora:**
  ```bash
  sudo yum install -y terraform
  ```

**Recursos Adicionais:**
- **Documentação:** [Terraform para Linux](https://www.terraform.io/docs/cli/install/linux.html)

---

**1.3. macOS**

**Passo a Passo:**

1. **Download:**
   - Acesse o site oficial do [Terraform](https://www.terraform.io/downloads).
   - Baixe o arquivo ZIP para macOS.

2. **Extrair e Instalar:**
   - Extraia o arquivo ZIP:
     ```bash
     unzip terraform_<versão>_darwin_amd64.zip
     ```
   - Mova o binário para `/usr/local/bin`:
     ```bash
     sudo mv terraform /usr/local/bin/
     ```

3. **Verificar a Instalação:**
   - Abra o terminal e execute `terraform --version` para verificar a instalação.

**Instalação via Homebrew:**
- **Instalação Simples:**
  ```bash
  brew tap hashicorp/tap
  brew install hashicorp/tap/terraform
  ```
- **Atualização:**
  ```bash
  brew update
  brew upgrade hashicorp/tap/terraform
  ```

**Recursos Adicionais:**
- **Documentação:** [Terraform para macOS](https://www.terraform.io/docs/cli/install/mac.html)

---

#### 2. Configuração Inicial e Primeiros Passos

**2.1. Criação de Diretório de Trabalho**

1. **Criar um Diretório de Projeto:**
   - Crie um novo diretório para o seu projeto Terraform:
     ```bash
     mkdir my-terraform-project
     cd my-terraform-project
     ```

2. **Estrutura Básica do Diretório:**
   - Dentro do diretório de projeto, crie arquivos básicos como `main.tf` para começar.

**2.2. Configuração do Terraform CLI**

1. **Configurando um Provedor:**
   - Abra o `main.tf` e adicione a configuração de um provedor, por exemplo, AWS:
     ```hcl
     provider "aws" {
       region = "us-east-1"
     }
     ```

2. **Inicialização do Projeto:**
   - Execute o comando `terraform init` para inicializar o projeto. Isso baixa os plugins do provedor e prepara o ambiente.
     ```bash
     terraform init
     ```

**2.3. Verificação do Setup**

1. **Verificar a Instalação do Provedor:**
   - Após a inicialização, você pode verificar quais provedores estão configurados:
     ```bash
     terraform providers
     ```

2. **Verificar a Versão:**
   - Verifique a versão do Terraform para garantir que a instalação está correta:
     ```bash
     terraform version
     ```

**2.4. Comandos Básicos de Configuração**

1. **Terraform Init:**
   - Inicializa o projeto e prepara o ambiente, baixando plugins necessários:
     ```bash
     terraform init
     ```

2. **Terraform Plan:**
   - Gera um plano de execução mostrando as mudanças que serão feitas na infraestrutura:
     ```bash
     terraform plan
     ```

3. **Terraform Apply:**
   - Aplica as mudanças descritas no plano e provisiona os recursos:
     ```bash
     terraform apply
     ```

4. **Terraform Destroy:**
   - Remove todos os recursos gerenciados pelo Terraform:
     ```bash
     terraform destroy
     ```

**Exemplo de `main.tf` Inicial:**
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleInstance"
  }
}
```

**Executando o Primeiro Projeto:**
1. **Inicie o projeto:**
   ```bash
   terraform init
   ```
2. **Planeje a configuração:**
   ```bash
   terraform plan
   ```
3. **Aplique a configuração:**
   ```bash
   terraform apply
   ```
4. **Destrua a infraestrutura se necessário:**
   ```bash
   terraform destroy
   ```

---

### Recursos Adicionais
- **Documentação Oficial do Terraform:** [Terraform Documentation](https://www.terraform.io/docs)
- **Tutoriais e Exemplos:** [HashiCorp Learn](https://learn.hashicorp.com/terraform)
- **Comunidade e Fóruns:** [Terraform Discuss](https://discuss.hashicorp.com/c/terraform-core/)

