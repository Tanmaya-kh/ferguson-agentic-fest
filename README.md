# Ferguson Agentic Fest · Day 4

**AI Accelerator Program for Ferguson** · the Ferguson RFQ Challenge

This is the common repo for Day 4. Every team pushes its final code here, so everyone can see what the other teams built. Code from Days 1 to 3 will be added here later too.

| Folder | Team |
|---|---|
| [group-1](group-1/) | Anmol Joshi, Saifi Ali, Charan G |
| [group-2](group-2/) | Gopal Agrawal, Prajwal JB, Lakshitha L |
| [group-3](group-3/) | Sanchit Vijay, Pramod B Belagali, Akepati Thulasi, Raghav Rana |

**Deadline: push and open your pull request by the 3:45 hard stop.**

## How to push your code

One teammate does this for the whole team.

### 1. Fork this repo

Click **Fork** (top right of this page), then **Create fork**. You now have your own copy at `github.com/<your-username>/ferguson-agentic-fest`.

### 2. Clone your fork

In the VS Code terminal, outside your kit folder. Replace `<your-username>` with your GitHub name:

```
git clone https://github.com/<your-username>/ferguson-agentic-fest
```

```
cd ferguson-agentic-fest
```

### 3. Copy your code into your team folder

Copy your working kit folder's contents into `group-1`, `group-2` or `group-3`.

**Leave out:** `.env`, `venv`, `out`, `__pycache__`, and any file with a key or password in it.

### 4. Fill in your team's README.md

Your team folder already has a `README.md` with headings. Fill in every one:

- What you built (base, and which agentic arm)
- How to run it
- Architecture (a diagram or a short description from email in to RFQ out)
- Your live Azure link, if it is deployed
- Results, and known limits

The README is judged, so a clear one earns points.

### 5. Check, commit and push

Run these one at a time. First check that no `.env` file is listed:

```
git status
```

```
git add .
```

Use your own group number:

```
git commit -m "Group 1: RFQ flow and agentic arm"
```

```
git push
```

### 6. Open a pull request

On your fork on GitHub, click **Contribute**, then **Open pull request**, then **Create pull request**. One pull request per team.

## No Git on your laptop?

On your fork, open your team folder, click **Add file**, then **Upload files**, and drag your files in. Then follow step 6.

## Never push keys

Do not push `.env`, API keys, App Passwords or any password. This repo is public. The `.gitignore` here blocks `.env`, but always check `git status` before you push. If a key is pushed by mistake, tell your facilitator straight away so it can be revoked.
