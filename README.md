# Nutanix Cloud Platform (NCP) Hands-on Lab

A MkDocs-based lab workbook for the Nutanix Cloud Platform Bootcamp. The source docs live in `lab-workbook/docs/` and are built into a static site using [MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

## Repository Structure

```
ncp/
├── lab-workbook/
│   ├── docs/               # Markdown source files
│   │   ├── stylesheets/    # Custom CSS (nx.css)
│   │   └── images/         # Images used in docs
│   ├── site/               # Built static output (git-ignored)
│   ├── mkdocs.yml          # MkDocs configuration
│   ├── requirements.txt    # Python dependencies
│   ├── Dockerfile          # Docker image (nginx serving built site)
│   └── vercel.json         # Vercel deployment config
├── course-setup/           # Terraform infrastructure for course environment
└── README.md
```

## Prerequisites

- Python 3.8+ (or [uv](https://github.com/astral-sh/uv) for fast installs)
- Docker (optional, for container-based serving)
- Node.js (optional, for Vercel CLI)

---

## Run Locally

### Option 1 — Python (recommended for development)

Install dependencies and start the dev server with live reload:

```bash
cd lab-workbook
pip install -r requirements.txt
mkdocs serve
```

Open [http://localhost:8000](/http://localhost:8000). Pages reload automatically on save.

### Option 2 — Serve pre-built site

If the `site/` folder already exists (after a build):

```bash
cd lab-workbook
python3 -m http.server 8000 --directory site
```

---

## Build

Generate the static site into `lab-workbook/site/`:

```bash
cd lab-workbook
pip install -r requirements.txt
mkdocs build
```

The output in `site/` is self-contained and can be served by any static file server.

---

## Run with Docker

Build and run using the included Dockerfile (serves the pre-built site via nginx):

```bash
# Build the static site first
cd lab-workbook
pip install -r requirements.txt
mkdocs build

# Build Docker image
docker build -t ncp-hol .

# Run container
docker run -p 8080:80 ncp-hol
```

Open [http://localhost:8080](/http://localhost:8080).

> The Dockerfile copies the `site/` directory into an nginx image. You must run `mkdocs build` before `docker build`.

---

## Deploy to Vercel

### Automatic deploys (recommended)

The project is connected to GitHub. Vercel automatically:
- Builds and deploys a **preview** on every push to any branch
- Deploys to **production** on every push to `main`

No manual steps needed after pushing.

### Manual deploy via CLI

```bash
# Install Vercel CLI (one-time)
npm install -g vercel

# Preview deploy
cd lab-workbook
vercel

# Production deploy
vercel --prod
```

### How Vercel builds the project

Configured in `lab-workbook/vercel.json`:

```json
{
  "framework": null,
  "buildCommand": "uv pip install --system -r requirements.txt && uv run mkdocs build",
  "outputDirectory": "site",
  "installCommand": ""
}
```

Vercel installs dependencies via `uv`, runs `mkdocs build`, and serves the `site/` output as a static site.

---

## Contributing

### Editing docs

All content lives in `lab-workbook/docs/` as Markdown files. The site navigation is defined in `lab-workbook/mkdocs.yml` under the `nav:` key.

```
lab-workbook/docs/
├── index.md
├── whatisncp.md
├── technologyoverview.md
├── storageconfiguration.md
├── networkconfiguration.md
├── deployingworkloads.md
├── managingworkloads.md
├── dataprotection.md
└── extras/
    └── environmentdetails.md
```

### Adding a new page

1. Create a new `.md` file in `lab-workbook/docs/`
2. Add it to the `nav:` section in `lab-workbook/mkdocs.yml`
3. Run `mkdocs serve` to preview locally

### Styling

Custom styles are in `lab-workbook/docs/stylesheets/nx.css`.

### Workflow

1. Fork or branch from `main`
2. Make changes and preview with `mkdocs serve`
3. Push your branch — Vercel will create a preview deployment automatically
4. Open a pull request against `main`
5. On merge, Vercel deploys to production

### What NOT to commit

The `.gitignore` blocks sensitive and generated files:

- `lab-workbook/site/` — built output, regenerated on every deploy
- `*.tfvars` — Terraform variable files that may contain secrets
- `*.tfstate*` — Terraform state files
- `.env`, `*.pem`, `*.key`, `kubeconfig*` — credentials and secrets
