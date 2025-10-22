# ai-music-peerstream
Emphasizes AI + peer-to-peer streaming.
# P2P-AI Music PoC

A **Proof-of-Concept (PoC)** for a legal, AI-powered, peer-to-peer music platform for building start-up.  
This PoC runs on **AWS** and demonstrates artist uploads, AI tagging, P2P playback, and royalty logging.

---

## Features

- Artist uploads tracks with rights verification  
- AI tagging & embedding via SageMaker or local models  
- Search by genre, mood, or similarity  
- P2P playback in-browser using WebTorrent, with S3/CloudFront fallback  
- Play events logged for royalty calculation  
- Backend services containerized and deployable on ECS Fargate  
- AWS Infra managed via Terraform (S3, DynamoDB, Cognito, ECS, ALB, CloudFront, OpenSearch)

---

## Repo Structure
p2p-ai-music-poc/
├─ README.md
├─ .gitignore
├─ infra/ # Terraform AWS infra
├─ backend/ # Node.js upload service + worker
└─ frontend/ # React + WebTorrent frontend


---

## Quick Start (Local Development)

### Prerequisites

- Node.js 18+
- Python 3.10+
- Terraform 1.3+
- Docker
- AWS CLI configured with credentials

---

### Backend

``bash
cd backend
npm install
export TRACKS_BUCKET=<your-s3-bucket>
export TRACKS_TABLE=<your-dynamodb-table>
export PROCESSING_QUEUE_URL=<your-sqs-queue>
node server.js

Or using Docker:
docker build -t p2p-upload-service .
docker run -e AWS_REGION=us-east-1 \
           -e TRACKS_BUCKET=<bucket> \
           -e TRACKS_TABLE=<table> \
           -p 3000:3000 p2p-upload-service

           
### Frontend
cd frontend
npm install
npm run start

Open your browser at http://localhost:3000
Search or play demo tracks via P2P WebTorrent or fallback S3 URL

### Terraform Deployment (AWS Infra)
cd infra
terraform init
terraform apply -var='aws_region=us-east-1' -auto-approve
Outputs include S3 bucket, DynamoDB tables, Cognito info, CloudFront URL
Replace local env vars with Terraform outputs


### Demo Tracks (Placeholders)
Use royalty-free audio for testing:
demo/track-01_sunset-loop.mp3 (30s loop)
demo/track-02_city-beat.mp3 (45s loop)
demo/track-03_acoustic-guitar.mp3 (60s loop)
Upload via /upload/init → /upload/complete endpoints.


### PoC Workflow
Artist logs in (Cognito) → uploads track via presigned URL
Lambda/ECS worker processes track → AI tags + embeddings → DynamoDB + OpenSearch
Frontend queries tracks → search, discovery, P2P playback
Play events logged in DynamoDB → royalty aggregator job calculates payouts

###Notes
Ensure only cleared tracks are uploaded
For large audio/ML workloads, run ECS tasks with GPU instances or use SageMaker
WebTorrent requires peer availability; fallback is S3/CloudFront
CORS must be enabled on S3 for browser playback

### License
This PoC is for demonstration purposes only. Do not use copyrighted tracks without permission.

---

