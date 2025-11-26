# 🎯 AULA 6 - Serverless: Build, Teste e Deploy Multi-Ambiente

> **Objetivo**: Dominar o desenvolvimento, teste e deploy de aplicações serverless usando AWS SAM, implementando pipelines CI/CD completos com canary deployment e estratégias multi-ambiente.

---

## 🎯 Objetivos de Aprendizado

Ao final desta aula, você será capaz de:

- ✅ Compreender arquitetura serverless e seus benefícios
- ✅ Instalar e configurar AWS SAM CLI
- ✅ Criar e gerenciar templates SAM (IaC)
- ✅ Desenvolver funções Lambda com Python
- ✅ Testar localmente com `sam local`
- ✅ Implementar testes unitários e de integração
- ✅ Configurar múltiplos ambientes (dev/staging/prod)
- ✅ Criar pipeline CI/CD completo com GitHub Actions
- ✅ Implementar canary deployment com auto rollback
- ✅ Monitorar aplicações com CloudWatch
- ✅ Calcular e otimizar custos serverless

---

## 📁 Estrutura dos Arquivos

```
fiap-dclt-aula06/
├── README.md                           # Este arquivo
├── VIDEO-6.1-PASSO-A-PASSO.md         # Vídeo 1: Build e Deploy
├── VIDEO-6.2-PASSO-A-PASSO.md         # Vídeo 2: Deploy Multi-Ambiente
├── template.yaml                       # SAM template (Lambda + API Gateway)
├── samconfig.toml                      # Configurações multi-ambiente
├── src/
│   ├── handlers.py                     # Funções Lambda (hello, health, info)
│   └── requirements.txt                # Dependências Python
└── .github/workflows/
    ├── sam-pipeline.yml                # Pipeline simples (Vídeo 6.1)
    └── sam-pipeline-multi-env.yml      # Pipeline multi-ambiente (Vídeo 6.2)
```

---

## 🚨 Troubleshooting

### Erro 1: SAM CLI não encontrado
```bash
Error: sam: command not found
```
**Causa**: SAM CLI não instalado ou não está no PATH

**Solução**:
```bash
# macOS
brew install aws-sam-cli

# Linux
pip install aws-sam-cli

# Verificar
sam --version
```

---

### Erro 2: Credenciais AWS não configuradas
```bash
Error: Unable to locate credentials
```
**Causa**: AWS credentials não configuradas

**Solução**:
```bash
# Configurar credenciais
aws configure

# Ou usar variáveis de ambiente
export AWS_ACCESS_KEY_ID=your_key
export AWS_SECRET_ACCESS_KEY=your_secret
export AWS_DEFAULT_REGION=us-east-1

# Verificar
aws sts get-caller-identity
```

---

### Erro 3: S3 bucket não existe
```bash
Error: Failed to create changeset: S3 bucket does not exist
```
**Causa**: Bucket SAM não criado

**Solução**:
```bash
# Usar --guided na primeira vez
sam deploy --guided

# SAM criará o bucket automaticamente
# Ou criar manualmente:
aws s3 mb s3://fiap-sam-deployments-dev --region us-east-1
```

---

### Erro 4: Lambda timeout
```bash
Error: Task timed out after 3.00 seconds
```
**Causa**: Função Lambda excedeu timeout configurado

**Solução**:
```yaml
# Aumentar timeout no template.yaml
Globals:
  Function:
    Timeout: 30  # Aumentar de 3 para 30 segundos
```

---

### Erro 5: GitHub Actions falha com credenciais
```bash
Error: Credentials could not be loaded
```
**Causa**: Secrets não configurados ou expirados

**Solução**:
1. Verificar se os 3 secrets existem no GitHub
2. Atualizar credenciais do Learner Lab (expiram a cada sessão)
3. Verificar nomes: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`

---

## 🧹 Comandos de Limpeza

```bash
# Deletar stacks
aws cloudformation delete-stack --stack-name fiap-serverless-dev
aws cloudformation delete-stack --stack-name fiap-serverless-staging
aws cloudformation delete-stack --stack-name fiap-serverless-prod

# Verificar
aws cloudformation list-stacks --stack-status-filter DELETE_COMPLETE

# Limpar local
rm -rf .aws-sam/
```

---

## 📝 Licença

Este material é parte do curso FIAP Pós Tech - DevOps e Arquitetura Cloud.

---

**Disciplina**: CI/CD
**Aula**: 6 - Serverless
