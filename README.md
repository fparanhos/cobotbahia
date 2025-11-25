# DEIA – Diários Eletrônicos com Inteligência Artificial  
**Versão:** 1.0  
**Autor:** Fernando Paranhos – CEPE  
**Arquitetura:** Python (AWS Lambda) • S3 • OpenAI Responses API • Vector Stores  

---

## 📌 Sobre o Projeto

O **DEIA** é uma plataforma de pesquisa inteligente sobre os **Diários Oficiais** que utilizam o Sistema de Diários Oficiais Eletrônicos (SDOE).

A solução usa IA generativa com **RAG** (Retrieval-Augmented Generation) para transformar PDFs densos em respostas claras e fundamentadas, com:

- Referência ao Diário, caderno e página  
- Transparência e rastreabilidade  
- Atualização diária via AWS S3  
- Backend serverless em AWS Lambda  
- Busca contextual profunda via Vector Store  

---

## 🚀 Objetivo

Democratizar o acesso ao conteúdo dos Diários Oficiais, permitindo pesquisa natural, rápida e inteligente, elevando o nível de transparência e a eficiência do serviço público.

---

## 🧠 Funcionalidades Principais

- Chat com IA (RAG)  
- Ingestão automática de PDFs diariamente  
- Painel do moderador (status, logs, ingestão, reindexação)  
- Suporte multiestado (PE, BA, AL e outros)  
- Vector Store dedicado por estado  
- Logs estruturados para auditoria e governança  
- Backend 100% serverless em AWS Lambda  
- Frontend estático hospedado em S3  

---

## 🏗 Arquitetura

```text
+-------------------------+
|      Frontend S3        |
| (HTML + JS + Markdown)  |
+------------+------------+
             |
             v
+-------------------------+
|    API Gateway (REST)   |
+------------+------------+
             |
             v
+-------------------------+
|  AWS Lambda (Python)    |
| - Chat / RAG            |
| - Moderador             |
| - Logs                  |
+------------+------------+
             |
             v
+-------------------------+
|  Vector Store (OpenAI)  |
| - Chunks dos Diários    |
+------------+------------+
             |
             v
+-------------------------+
| AWS S3 (Fonte Oficial)  |
| - PDFs do Diário        |
| - Armazenamento         |
+-------------------------+
````
📂 Estrutura do Repositóriotext
```
deia-server/
│
├── api/
│   ├── lambda_chat.py         # Backend (chat)
│   ├── lambda_ingest.py       # Ingestão diária
│   ├── lambda_moderador.py    # Painel e logs
│   └── requirements.txt
│
├── frontend/
│   ├── index.html             # Chat
│   ├── moderador.html         # Painel
│   ├── css/
│   └── js/
│
├── scripts/
│   ├── inserir_vectorstore.py # Ingestão manual
│   └── baixar_s3.py           # Sync S3
│
├── docs/
│   ├── projeto-deia.md        # Documento técnico completo
│   └── arquitetura.png
│
└── README.md
```


⚙️ Requisitos
- Python 3.11

- Pip + virtualenv (opcional)

- Conta OpenAI com Vector Store ativo

- AWS CLI configurado com permissões para:

- S3

- Lambda

- CloudWatch Logs

- API Gateway

🔧 Instalação (Ambiente Local)
1. Clone o repositório
```
bash
git clone https://gitlab.com/<seu-grupo>/deia.git
cd deia
```
2. Crie ambiente virtual
``
bash
python3.11 -m venv venv
source venv/bin/activate
``
3. Instale dependências
``
bash
pip install -r api/requirements.txt
``
4. Configure sua OPENAI_API_KEY
```
bash
export OPENAI_API_KEY="sua-chave"
```
☁️ Deploy no AWS (Lambda)
1. Gerar pacote
```
bash
cd api
pip install -r requirements.txt -t package/
cp *.py package/
cd package
zip -r deia_lambda.zip .
```
3. Subir para Lambda
```
bash
aws lambda update-function-code \
  --function-name deia-chat \
  --zip-file fileb://deia_lambda.zip
```
📥 Ingestão Diária (Vector Store)
A ingestão ocorre automaticamente via Lambda (cron) lendo:
```
php-template
s3://cepebr-prod/1/cadernos/<ano>/<mes>/<dia>/
```
Para ingestão manual:
```
bash
python3.11 scripts/inserir_vectorstore.py
```
🛡 Painel do Moderador
O painel mostra:

- Status do Vector Store

- Última ingestão

- Número de chunks

- Logs de consulta

- Botões para reprocessamento

A interface é hospedada diretamente em S3.

📊 Logs e Auditoria
Logs são enviados para:

- CloudWatch (execuções Lambda)

S3 (ingestões e consultas)

Exemplo:
```
json
{
  "timestamp": "2025-11-24T13:22:10",
  "estado": "PE",
  "pergunta": "Quais atos de nomeação hoje?",
  "resposta_tokens": 2048
}
```
📘 Documentação Completa
Documentação detalhada:
```
bash
docs/projeto-deia.md
```
📞 Contato
Responsável Técnico:
João Alvarez Analista de Sistemas / Engenharia do DEIA


