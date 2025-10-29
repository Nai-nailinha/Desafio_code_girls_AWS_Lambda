# Criação do arquivo lambda_function.py
lambda_code = """import json
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
"""

lambda_path = Path("/mnt/data/lambda_function.py")
lambda_path.write_text(lambda_code, encoding="utf-8")

lambda_path
