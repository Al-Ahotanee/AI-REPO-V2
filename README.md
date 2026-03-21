# 🎓 GSU Faculty Repository — GitHub Codespaces Guide

**Stack:** Flask + SQLite + React 18 + Tailwind CSS + Lucide Icons  
**Hosting:** GitHub Codespaces (Free tier: 60 hrs/month · 15 GB storage)

---

## 📁 Repository Structure

```
your-repo/
├── .devcontainer/
│   └── devcontainer.json   ← Codespace auto-config
├── app.py                  ← Flask backend
├── index.html              ← React SPA (Tailwind + Lucide CDN)
├── requirements.txt        ← Python deps (no gunicorn)
├── uploads/                ← Created automatically on first run
└── repository.db           ← Created automatically on first run
```

---

## 🚀 STEP-BY-STEP DEPLOYMENT

---

### STEP 1 — Create a GitHub Repository

1. Go to **github.com** → click **New** (top-left green button)
2. Fill in:
   - **Repository name:** `gsu-faculty-repo` (or anything you like)
   - **Visibility:** `Private` (recommended) or `Public`
   - ✅ Check **"Add a README file"** (so the repo isn't empty)
3. Click **Create repository**

---

### STEP 2 — Upload Your Files

You have two options:

#### Option A — Upload via GitHub Web UI (Easiest)

1. Open your new repo on GitHub
2. Click **Add file** → **Upload files**
3. Drag and drop ALL these files:
   ```
   app.py
   index.html
   requirements.txt
   .devcontainer/devcontainer.json   ← must be inside .devcontainer/ folder
   ```
4. For the `.devcontainer` folder: click **Add file** → **Create new file** → type `.devcontainer/devcontainer.json` as the filename, then paste the JSON content
5. Click **Commit changes**

#### Option B — Git command line

```bash
git clone https://github.com/YOUR_USERNAME/gsu-faculty-repo.git
cd gsu-faculty-repo

# Copy your files here
cp /path/to/app.py .
cp /path/to/index.html .
cp /path/to/requirements.txt .
mkdir -p .devcontainer
cp /path/to/devcontainer.json .devcontainer/

git add .
git commit -m "Add GSU Faculty Repository"
git push
```

---

### STEP 3 — Launch a Codespace

1. On your GitHub repo page, click the green **`<> Code`** button
2. Click the **Codespaces** tab
3. Click **"Create codespace on main"**

GitHub will now:
- Spin up a virtual Ubuntu machine (takes ~2 minutes first time)
- Install Python 3.11 automatically
- Run `pip install -r requirements.txt` (installs Flask, AI libraries, etc.)
- Start `python app.py` automatically
- Open VS Code in your browser

---

### STEP 4 — Access Your App

Once the Codespace is running:

1. Look at the bottom of VS Code — you'll see a **PORTS** tab
2. Port `5000` will appear with a 🌐 globe icon
3. Click the globe icon → your app opens in a new browser tab

**OR** look for a toast notification:
> *"Your application running on port 5000 is available"* → click **Open in Browser**

The URL will look like:
```
https://YOUR-CODESPACE-NAME-5000.app.github.dev
```

---

### STEP 5 — Make Port 5000 Public (Share with Others)

By default ports are **private** (only you can see it). To share with students/faculty:

1. In VS Code → **PORTS** tab (bottom panel)
2. Right-click on port `5000`
3. Click **Port Visibility** → **Public**

Now anyone with the URL can access it. The `.devcontainer.json` already sets `"visibility": "public"` so this should happen automatically.

---

### STEP 6 — Log In

Open the app URL and log in with the default admin:

| Field | Value |
|---|---|
| Email | `admin@gsu.edu.ng` |
| Password | `Admin@1234` |

> ⚠️ Change this password immediately in **Profile → Change Password**

---

## 🔄 Restarting the App

If the Codespace is stopped (it auto-suspends after 30 min of inactivity):

1. Go to **github.com/codespaces** (or visit your repo → Code → Codespaces)
2. Click **"..."** next to your Codespace → **Resume**
3. The app starts automatically via `postStartCommand`

If it doesn't start automatically:
1. Open the Terminal in VS Code (Ctrl + `` ` ``)
2. Run:
   ```bash
   python app.py
   ```

---

## 💾 Data Persistence

Your data is **persistent within the same Codespace** — `repository.db` and `uploads/` survive restarts.

> ⚠️ If you **delete** the Codespace, all data is lost. To keep your data:
> - Commit `repository.db` to git periodically, OR
> - Download the `uploads/` folder and `repository.db` before deletion

---

## 🌐 Environment Variables (Optional)

The app works with zero configuration. But if you want to customise, create a `.env` approach by setting these in your Codespace terminal:

```bash
export SECRET_KEY="your-super-secret-key-here"
export MAX_MB=100
export JWT_DAYS=14
python app.py
```

Or set them permanently in the Codespace:
1. Go to **github.com/codespaces**
2. Click **"..."** on your Codespace → **Manage secrets**
3. Add `SECRET_KEY` with your value

---

## ⏰ Free Tier Limits

GitHub Codespaces free tier (personal accounts) gives you:

| Resource | Free Allowance |
|---|---|
| Compute time | 60 hours/month |
| Storage | 15 GB |
| Machine type | 2 cores, 4 GB RAM |

**Tips to save hours:**
- Stop your Codespace when not in use: **Code** → Codespaces → **Stop**
- It auto-stops after 30 minutes of browser inactivity
- The 15 GB storage is more than enough for hundreds of documents

---

## 🤖 AI Engine on Codespaces

Unlike Render's free tier, Codespaces **does have enough RAM** to load `sentence-transformers`.

The app will print on startup:
```
[AI] SentenceTransformer loaded ✓
```

This means semantic search is **fully active**. The first startup may take 1–2 minutes to download the model (~90 MB). Subsequent starts are instant.

Check AI status at:
```
GET https://YOUR-CODESPACE-5000.app.github.dev/api/health
```
```json
{
  "status": "ok",
  "ai": "sentence-transformers",
  "documents": 0,
  "users": 1
}
```

---

## 🛠️ Running Manually (Without Auto-Start)

If you want to run manually instead of auto-start, remove `postStartCommand` from `devcontainer.json` and run in terminal:

```bash
# Development mode (with reloader)
python app.py

# Or with Flask CLI
flask --app app run --host=0.0.0.0 --port=5000 --debug
```

---

## 🔧 Troubleshooting

**Port 5000 not showing up:**
- Open terminal → run `python app.py`
- VS Code will prompt you to open in browser

**"Address already in use" error:**
- Another process is using port 5000
- Run: `pkill -f "python app.py"` then `python app.py` again

**`pip install` failed:**
- Run manually in terminal:
  ```bash
  pip install -r requirements.txt
  ```

**App crashes with `ModuleNotFoundError`:**
- Dependencies not installed yet, run:
  ```bash
  pip install flask flask-cors PyJWT bcrypt sentence-transformers PyMuPDF python-docx python-pptx numpy
  ```

**Sentence-transformers model slow to load:**
- First startup downloads ~90 MB model — wait 1–2 minutes
- Subsequent starts load from cache instantly

**Can't see data after resuming Codespace:**
- The database persists — just restart the app: `python app.py`
- If the Codespace was **deleted** (not just stopped), data is gone

---

## 📤 Sharing the App URL

Once the port is set to **Public**, share the URL:
```
https://YOUR-CODESPACE-NAME-5000.app.github.dev
```

This URL is stable as long as your Codespace is **running**. When suspended, the URL returns 502 — resume the Codespace to bring it back.

---

## 🔁 Making It Permanent (Upgrade Path)

When you need 24/7 uptime, migrate to:

| Option | Cost | Notes |
|---|---|---|
| **Render** (paid) | ~$7/month | Add 1 GB disk, works great |
| **Railway.app** | ~$5/month | Similar to Render, simpler setup |
| **Fly.io** | Free tier available | Requires Docker knowledge |
| **VPS (DigitalOcean/Hetzner)** | ~$4–6/month | Full control, persistent storage |

For any of these, the `app.py` and `index.html` need **zero changes** — just update environment variables and use `gunicorn` as the start command.

---

*GSU Faculty Repository · Department of Computer Science · Gombe State University*
