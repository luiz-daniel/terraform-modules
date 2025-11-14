# Terraform Modules Monorepo

Repositório centralizado de módulos reutilizáveis do Terraform para infraestrutura AWS.

## 📋 Índice de Módulos

### [Lambda](./lambda/)
Módulo para criação e configuração de funções AWS Lambda.

### [DynamoDB](./dynamodb/)
Módulo para criação e configuração de tabelas DynamoDB.

## 🚀 Como Usar

### Em seus projetos

Para usar estes módulos em seus projetos Terraform, referencie-os via Git:

```hcl
module "lambda" {
  source = "git::https://github.com/luiz-daniel/terraform-modules.git//lambda?ref=v1.0.0"
  
  # suas variáveis aqui
}
```

### Exemplo: Projeto Lambda em Go

```hcl
# main.tf do seu projeto
terraform {
  required_version = ">= 1.6.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

module "my_lambda" {
  source = "git::https://github.com/luiz-daniel/terraform-modules.git//lambda?ref=v1.0.0"
  
  # Configure as variáveis do módulo
  function_name = "my-go-lambda"
  handler       = "bootstrap"
  runtime       = "provided.al2"
  
  # Outras configurações...
}
```

### Usando a Pipeline em Outros Projetos

Para reutilizar a pipeline de validação do Terraform em seus projetos:

```yaml
# .github/workflows/terraform.yml no seu projeto
name: Terraform CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  terraform:
    uses: luiz-daniel/terraform-modules/.github/workflows/pipeline.yml@main
    with:
      terraform_version: '1.6.0'
      working_directory: './terraform'  # ajuste conforme necessário
```

## 📦 Padrão de Versionamento

Este projeto segue o [Semantic Versioning](https://semver.org/lang/pt-BR/):

- **MAJOR** (v1.0.0): Mudanças incompatíveis com versões anteriores
- **MINOR** (v0.1.0): Novas funcionalidades mantendo compatibilidade
- **PATCH** (v0.0.1): Correções de bugs e melhorias mantendo compatibilidade

### Tags e Releases

- Cada release é taggeada com a versão: `v1.0.0`, `v1.1.0`, etc.
- Use a tag específica na referência do módulo para garantir estabilidade
- A branch `main` sempre contém a versão estável mais recente
- Consulte o [CHANGELOG.md](./CHANGELOG.md) para histórico completo

### Recomendações

- **Produção**: Sempre use uma tag específica (`ref=v1.0.0`)
- **Desenvolvimento**: Pode usar `ref=main` para testar as últimas mudanças
- **Pin de versão**: Atualize as versões conscientemente após revisar o changelog

## 🔧 Desenvolvimento

### Estrutura de cada módulo

```
module-name/
├── main.tf       # Recursos principais
├── variables.tf  # Variáveis de entrada
├── outputs.tf    # Outputs do módulo
└── README.md     # Documentação específica
```

### CI/CD

A pipeline automaticamente executa em cada push ou PR:

1. **Detecção de mudanças**: Identifica quais módulos foram alterados
2. **Terraform fmt**: Valida formatação do código
3. **Terraform validate**: Valida sintaxe e configuração
4. **TFLint**: Análise estática de boas práticas

### Contribuindo

1. Crie uma branch para sua feature/fix
2. Faça commits seguindo [Conventional Commits](https://www.conventionalcommits.org/pt-br/)
3. Abra um Pull Request
4. Aguarde a pipeline passar e a revisão de código
5. Após merge, uma nova tag será criada se necessário

## 📝 Licença

Este projeto é de uso interno.

## 📞 Suporte

Para dúvidas ou problemas, abra uma issue no repositório.