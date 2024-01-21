# Akamai University Compute Demo - Open LLMs
<img src="docs/images/akamai_university.png" alt="Akamai" style="display: block; margin-left: auto; margin-right: auto; width:auto; height:200px">

This repository is a demonstration showcasing how developers can leverage Akamai/Linode cloud platform to deploy open-source Large Language Models (LLMs) and the utilization of open-source technologies to create a ChatGPT-like chatbot experience. Offering a glimpse into the powerful and versatile capabilities of self-hosted LLMs with complete control over data privacy. This demo utilizes open-sourced tools such as Docker, Ollama, Ollama Web UI, and a free-tier ngrok for additional OAuth functionalities, taking less than 15 minutes to set up.

## Features

### 1. **Ease of Deploying Open-Source LLM:**
   - Effortlessly deploy and consume LLMs on Akamai/Linode Cloud with a developer-friendly process

### 2. **Control Data Privacy with Self-Hosted LLM:**
   - Total data privacy: Host LLMs on your infrastructure for complete control, and consumption of internally further-tuned LLMs
   - Compliance and security: Ensure data protection, enhance security, and maintain data sovereignty

### 3 **Future Ready for Langchain Development:**
   - Built for future advancements and lay the groundwork for ever-evolving lanfscape of AI

### 4 **Take Control of AI Integration:**
   - Flexible integration options to empower and enable developers for AI and Business integration.
   - Customizable for specific needs: Tailor AI capabilities to specific project requirements such as offline access

## Prerequisites

- Linode account, can be created via https://login.linode.com/signup 
- Linode Cloud Manager - This demo uses the Cloud Manager UI for VM creation
- Docker
- ngrok account with an authtoken

## Demo Steps

1. Create a new Ubuntu instance using Cloud Manager UI:
   
   Take the opportunity to familiarise with the functionalities of VPC, Firewall, Cloud Regions, and an intuitive Cloud Manager UI designed for enhanced productivity, offering a streamlined experience with reduced complexity for developers.

   This execise only needs to allow port 22 SSH from source IP for package installation; that's the only port you need to open in Firewall. Leave outbound to allow all traffic.

2. SSH into Ubuntu using Terminal:
   ```bash
   ssh root@[ubuntu-ip-address]
   apt update
   ```

3. Install Docker (according to [Docker documentation](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)):
   ```bash
   apt-get update
   apt-get install ca-certificates curl gnupg
   install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   chmod a+r /etc/apt/keyrings/docker.gpg

   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   apt-get update

   apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

4. Install Ollama:
   ```bash
   curl https://ollama.ai/install.sh | sh
   ```

5. Modify Ollama service:
   ```bash
   mkdir -p /etc/systemd/system/ollama.service.d
   echo '[Service]' >>/etc/systemd/system/ollama.service.d/environment.conf
   echo 'Environment="OLLAMA_HOST=0.0.0.0:11434"' >>/etc/systemd/system/ollama.service.d/environment.conf

   systemctl daemon-reload
   systemctl restart ollama
   ```

6. Install Ollama Web UI using Docker:
   ```bash
   docker run -d -p 3000:8080 --add-host=host.docker.internal:host-gateway -v ollama-webui:/app/backend/data --name ollama-webui --restart always ghcr.io/ollama-webui/ollama-webui:main
   ```

7. Register for an [ngrok](https://ngrok.com) account, and obtain your ngrok Authtoken.

8. Install and Configure ngrok:
   ```bash
   snap install ngrok
   ngrok config add-authtoken <your-token-goes-here>
   ```

9. Start ngrok tunnel. Expose port 3000 as it's used by ollama-webui. We can make use of the ngrok built-in security features, i.e. secure tunnel, https, authentication. Use OAuth for simple identity security. You can either allow the entire domain (so you can share the access with co-workers) or limit access to yourself.

   ```bash
   ngrok http 3000 --oauth google --oauth-allow-domain akamai.com
   # Optional -> ngrok http 3000 --oauth google --oauth-allow-email youremail@gmail.com
   ```

10. Once ngrok started, grab the publicly accessible URL under Forwarding row, e.g.: https://5344-2400-8905-00-f03c-94ff-fe24-b6e.ngrok-free.app/c/d11e4b81-3fbf-4abe-b0c3-48131c487272. 

**Continue to explore Ollama Web UI with different open LLMs model and have fun!**

## Disclaimer
The deployment provided in this repository is minimal and intended for demonstration purposes only. For more detailed information, please refer to the open-source tools [Docker](https://docs.docker.com/), [Ollama](https://github.com/jmorganca/ollama), and [Ollama Web UI](https://github.com/ollama-webui/ollama-webui).