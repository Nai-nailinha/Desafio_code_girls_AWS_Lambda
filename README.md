# ☁️ Executando Tarefas Automatizadas com AWS Lambda e S3

## 🧩 Descrição do Desafio

Este laboratório tem como objetivo consolidar os conhecimentos em tarefas automatizadas com Lambda Function e Amazon S3.
A proposta é documentar todo o processo, demonstrando domínio dos conceitos aprendidos e capacidade de aplicação prática em um ambiente serverless.

## 🎯 Objetivos de Aprendizagem

* Ao concluir o desafio, você será capaz de:
* Aplicar conceitos de automação e computação sem servidor (serverless);
* Integrar AWS Lambda e S3 para executar funções automaticamente em resposta a eventos;
* Documentar processos otécnicos de forma clara e estruturada;
* Utilizar o GitHub como ferramenta de versionamento e compartilhamento técnico.

## 🧠 Conceitos-Chave

🔹 AWS Lambda

* Serviço de computação serverless da AWS.
*Permite executar código sem precisar gerenciar servidores, pagando apenas pelo tempo de execução.

## Características principais:

* Escalabilidade automática
* Suporte a múltiplas linguagens (Python, Node.js, Go, etc.)
* Cobrança baseada em uso real
* Integração com eventos (S3, DynamoDB, CloudWatch, etc.)

🔹 Amazon S3

Serviço de armazenamento de objetos.
Ideal para guardar arquivos, logs, imagens, backups e servir como gatilho (trigger) para automações.

Exemplo de uso conjunto:

Toda vez que um arquivo é enviado ao bucket S3, o Lambda é acionado para processá-lo, redimensionar uma imagem, converter formato ou enviar logs para outro serviço.

## ⚙️ Estrutura Prática do Projeto
🔸 Passo 1 — Criar bucket S3

* Crie um bucket (ex: bootcamp-lambda-automatizacao) para receber arquivos de upload.

🔸 Passo 2 — Criar função Lambda

* Configure uma função Lambda com runtime Python 3.9 (ou Node.js, se preferir) e permissões de leitura/escrita no S3.

Exemplo (Python):
```
import json
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']

    print(f"Arquivo recebido: {key} no bucket {bucket}")

    # Exemplo de ação: copiar o arquivo para outra pasta
    copy_source = {'Bucket': bucket, 'Key': key}
    s3.copy_object(CopySource=copy_source, Bucket=bucket, Key=f"processados/{key}")

    return {
        'statusCode': 200,
        'body': json.dumps('Processamento concluído com sucesso!')
    }
```

🔸 Passo 3 — Configurar evento de trigger no S3

* No console do S3 → Propriedades → Eventos → Criar evento
* Ação: s3:ObjectCreated:*
* Destino: a função Lambda criada anteriormente.

🔸 Passo 4 — Testar

* Envie um arquivo pro bucket e verifique os logs no CloudWatch.
* Você verá o Lambda executando automaticamente, confirmando a automação.

## ☕ Analogia pra fixar

Pensa no S3 como o entregador e no Lambda como o funcionário multitarefa:

📦 O S3 entrega o pacote\
🧠 O Lambda abre, processa e arquiva\
🔥 Tudo isso sem chefe(servidor) e sem hora extra!

📁 Estrutura de Repositório Sugerida
```lambda-s3-automation/
│
├── lambda_function.py
├── README.md
```

