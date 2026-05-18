# Local AI Email Auto-Responder

A fully local, privacy-preserving AI email auto-responder built with n8n and Ollama. This project intelligently reads incoming emails, processes their intent using a locally hosted Large Language Model (`llama3:8b`), and automatically generates and sends contextually relevant replies—all without your data ever leaving your machine.

## Features

- **100% Local Processing:** No third-party APIs (like OpenAI or Google). Your emails stay entirely private.
- **Automated Workflow Orchestration:** Uses n8n to connect IMAP reading, conditional loop prevention, LLM generation, and SMTP sending into one seamless pipeline.
- **Smart Loop Prevention:** Intelligently avoids replying to its own replies or creating infinite auto-response loops.
- **Thread Continuity:** Replies maintain standard email headers (`Message-ID`) to ensure responses are perfectly threaded in the recipient's inbox.

---

## Setup Instructions

### 1. Prerequisites

- Docker and Docker Compose installed on your system.
- An email account with IMAP and SMTP access enabled (e.g., an App Password if using Gmail).

### 2. Configure Environment Variables

1. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```
2. Open the `.env` file in your preferred text editor and fill in your actual IMAP and SMTP credentials.
   - Replace the `your_email@example.com` and password placeholders with your real details.

### 3. Launch the Services

Start the system using Docker Compose. This will spin up the n8n automation engine, the Ollama inference server, and a lightweight sidecar that automatically downloads the `llama3:8b` model for you.

```bash
docker-compose up -d
```

*Note: The first run may take a few minutes as it downloads the n8n image, the Ollama image, and the `llama3:8b` language model.*

You can verify that both containers are healthy by running:
```bash
docker-compose ps
```

### 4. Import and Activate the Workflow

1. Once the containers are healthy, open your browser and navigate to the n8n dashboard at:
   [http://localhost:5678](http://localhost:5678)
2. Follow the on-screen prompts to set up your owner account for n8n.
3. In the n8n dashboard, click on **Workflows** in the left menu, then click **Add Workflow**.
4. In the top right corner of the workflow canvas, click the **Options menu (three dots)** and select **Import from File**.
5. Select the `workflow.json` file located in the root of this repository.
6. The workflow will appear on your canvas. You may need to double-click the **Email Read (IMAP)** and **Send Email (SMTP)** nodes to quickly select/confirm the credentials you defined in your `.env` file.
7. Toggle the switch in the top right corner from **Inactive** to **Active**.

Your Local AI Email Auto-Responder is now live! Send an email to your configured inbox to test it out.
