🚀 AI Agent on Google Cloud Run using n8n + Gemini

This project demonstrates how to deploy a self-hosted n8n instance on Google Cloud Run, connect it with Google Gemini, and build a serverless AI Agent workflow powered by an LLM.

It includes:

- Containerized n8n deployment
- Cloud SQL (PostgreSQL) integration
- Gemini-powered AI Agent workflow
- Secure secrets using Secret Manager
- Public Chat UI workflow
- Optional tool integrations 

Components

- Cloud Run → hosts n8n (serverless)
- Cloud SQL (Postgres) → stores n8n workflows + data
- Artifact Registry → stores Docker image for n8n
- Secret Manager → holds sensitive credentials
- Gemini API → LLM powering the agent
- n8n Workflows → chat agent logic

<img width="975" height="473" alt="image" src="https://github.com/user-attachments/assets/efae9b52-a1a6-4ed7-a21a-ebae051c7238" />



🚀 Deploy to Cloud Run
1. Build & Push Image
gcloud builds submit --tag us-central1-docker.pkg.dev/<PROJECT-ID>/n8n-repo/n8n

2. Deploy
gcloud run deploy n8n \
  --image=us-central1-docker.pkg.dev/<PROJECT-ID>/n8n-repo/n8n \
  --region=us-central1 \
  --platform=managed \
  --allow-unauthenticated \ 
  --add-cloudsql-instances=<PROJECT-ID>:us-central1:n8n-sql \
  --set-env-vars DB_TYPE=postgresdb \
  --set-env-vars DB_POSTGRESDB_HOST=/cloudsql/<PROJECT-ID>:us-central1:n8n-sql \
  --set-env-vars DB_POSTGRESDB_DATABASE=n8n \
  --set-env-vars DB_POSTGRESDB_USER=n8n_user \
  --set-env-vars DB_POSTGRESDB_PASSWORD_SECRET=n8n-db-password \
  --set-env-vars N8N_HOST=0.0.0.0 \
  --set-env-vars N8N_PORT=8080 

🤖 AI Agent Workflow (Inside n8n):

Nodes Used
Chat Trigger → receives messages
Google Gemini Chat Model → generates responses
AI Agent Node → orchestrates the logic
Chat Message → replies to user
