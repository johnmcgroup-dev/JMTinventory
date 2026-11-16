# Local Development Workflow

## Required tools

Install these on the Windows development computer:

- Git for Windows.
- Node.js 22 LTS.
- pnpm (via Corepack).
- Visual Studio Code.
- Docker Desktop (for local PostgreSQL and Redis containers).
- Android Studio when beginning Android emulator/device work.

Do not install or configure production credentials on the development machine until needed.

## Clone

Open PowerShell:

```powershell
mkdir C:\dev -ErrorAction SilentlyContinue
cd C:\dev
git clone https://github.com/johnmcgroup-dev/JMTinventory.git
cd JMTinventory
git switch global-platform-foundation
code .
```

## Verify tools

```powershell
git --version
node --version
corepack --version
pnpm --version
```

If Corepack does not provide pnpm, enable it with:

```powershell
corepack enable
corepack prepare pnpm@9.15.5 --activate
```

## Daily Git workflow

```powershell
git status
git pull --rebase origin global-platform-foundation
# Make and test changes
git add .
git commit -m "feat: describe the change"
git push -u origin global-platform-foundation
```

Never commit `.env` files, database dumps containing real data, credentials, certificates or private keys. Use `.env.example` with placeholder values.

## Local services

PostgreSQL and Redis will be added through Docker Compose when the first runnable API/database slice is implemented. Do not expose database ports to the public internet.

## Important boundary

GitHub changes do not automatically appear in the local working folder. Run `git pull` to receive remote commits. Local changes do not reach GitHub until committed and pushed.
