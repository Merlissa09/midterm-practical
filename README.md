# Acme Support Portal - Vulnerability Exploitation Lab

## Tool Requirements

Install these before you start:

1. Docker (required)
2. A terminal
3. A web browser
4. Burp Suite Community Edition (recommended for evidence/screenshots)
5. `curl` (recommended, usually preinstalled on macOS/Linux)

## Environment Setup (Step-by-Step)

Follow these steps in order.

### 1) Get the Lab Files

You will receive two files from your instructor:

- `acme-support.tar` — the lab Docker image
- `docker-compose.yml` — the container configuration

Place both files in the same folder on your machine (e.g. `acme-lab/`).

### 2) Start Docker Daemon / Docker Engine

You must have Docker running before using `docker compose`.

macOS:

1. Open Docker Desktop from Applications.
2. Wait until it shows "Docker Desktop is running".

Windows:

1. Open Docker Desktop from Start Menu.
2. Wait until it shows "Engine running".

Linux (systemd):

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

If your user is not in the `docker` group (Linux), either:

- Run Docker commands with `sudo`, or
- Add your user to the group:

```bash
sudo usermod -aG docker $USER
```

Then log out/in.

### 3) Verify Docker Is Running

Run:

```bash
docker version
docker compose version
docker info
```

If these fail, Docker is not fully running yet.

### 4) Load the Lab Image

From the folder containing your lab files:

```bash
docker load -i acme-support.tar
```

You should see output like `Loaded image: acme-support:latest`.

### 5) Start the Lab

```bash
docker compose up
```

Keep this terminal open while the lab is running.

### 6) Open the Lab in Your Browser

- http://localhost:8090/
- http://localhost:8090/status
- http://localhost:8090/hints

### 7) Stopping and Restarting

Stop:

```bash
Ctrl + C
docker compose down
```

Restart:

```bash
docker compose up
```

### 8) Resetting Your Instance (If Needed)

This generates a new instance identity and new flag values:

```bash
docker compose down
rm -rf labdata
docker compose up
```

On Windows PowerShell:

```powershell
docker compose down
Remove-Item -Recurse -Force .\labdata
docker compose up
```

## Objective

You are assessing an intentionally vulnerable web application running locally via Docker.

Your goal is to complete all four required attacks in sequence and retrieve the final flag in the format `FLAG{...}`.

The application's hint system at `/hints` will guide you through each step. You must unlock each stage by proving you completed it before you can progress to the next.

## Rules

- This environment is for training only.
- Run it locally on a private network only.
- Do not deploy it publicly.
- Do not attack systems outside this lab.

## Required Attack Sequence

You must complete all four attacks in order. Each step builds on the previous one.

1. **Information Disclosure** — Extract a secret token from the application's response headers
2. **API Discovery** — Find and enumerate hidden API documentation
3. **Path Traversal** — Read a sensitive internal file by exploiting an unsafe file endpoint
4. **XXE (XML External Entity)** — Use a malicious XML payload to read a protected server-side file and retrieve the final flag

## What to Submit

Submit one ZIP file containing:

```text
flag.txt
instance_id.txt
writeup.md (or PDF)
evidence/
```

## Writeup Format (Required)

Include the following sections in `writeup.md`. Keep responses concise — bullet points are fine.

### Attack Summary Table

| Attack # | Vulnerability Type     | Endpoint(s) Targeted | Tool Used | What Was Gained |
| -------- | ---------------------- | -------------------- | --------- | --------------- |
| 1        | Information Disclosure |                      |           |                 |
| 2        | API Discovery          |                      |           |                 |
| 3        | Path Traversal         |                      |           |                 |
| 4        | XXE                    |                      |           |                 |

### Exploitation Chain Narrative

For each step, briefly answer the prompts below in 2–4 bullet points.

Step 1 - Information Disclosure:

- What endpoint you targeted
- What value you found and where it appeared

Step 2 - API Discovery:

- How you located the API documentation
- What endpoints it listed

Step 3 - Path Traversal:

- What request you crafted
- What file you read and what it revealed

Step 4 - XXE and Flag Retrieval:

- How you crafted your XML payload
- How you retrieved the resolved content
- The final `FLAG{...}` you obtained

## Evidence Requirements (Mandatory)

For each of the four attacks, provide:

- One request artifact
- One response artifact

Minimum total: 8 evidence artifacts.

Accepted evidence types:

- Burp Repeater screenshot with full request visible
- Burp exported request/response
- `curl` command plus output
- Browser screenshot plus matching network request details

File naming format:

```text
evidence/attack1_request.png
evidence/attack1_response.png
evidence/attack2_request.png
evidence/attack2_response.png
evidence/attack3_request.png
evidence/attack3_response.png
evidence/attack4_request.png
evidence/attack4_response.png
```

## Instance Verification (Required)

At the end of your writeup, include:

- Exact contents of `labdata/instance_id.txt`
- Your final `FLAG{...}`

## Grading Rubric (100 points)

- 4 required attacks completed: 50 points
- Evidence quality (8 required artifacts): 30 points
- Chain explanation clarity: 20 points

Deductions:

- Fewer than 4 attacks completed: maximum 59%
- Correct flag but weak/missing evidence: maximum 69%
- Missing request/response evidence pair: -10 per missing pair

## Troubleshooting

`docker: command not found`:
Docker is not installed correctly or your terminal needs a restart.

`Cannot connect to the Docker daemon`:
Docker Desktop/Engine is not running.

`Error response from daemon: No such image`:
Run `docker load -i acme-support.tar` before running `docker compose up`.

`port is already allocated` or `address already in use` for `8090`:
Stop the process using port 8090, then rerun.

Container starts but app not reachable:
Check the logs in the terminal running `docker compose up`. Confirm the URL is exactly `http://localhost:8090`.
