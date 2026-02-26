# Frappe CRM Local Setup — macOS (Apple Silicon)

## Environment

- OS: macOS (Apple Silicon / arm64)
- Shell: zsh
- Bench location: `~/frappe-bench`
- Site: `crm.localhost`

## Installed Versions

| Component | Version |
|---|---|
| Frappe Framework | v15.101.1 (branch: version-15) |
| Frappe CRM | v1.59.2 (branch: main) |
| MariaDB | 10.6.25 (keg-only via Homebrew) |
| Redis | 8.6.1 |
| Python | 3.11.14 (via uv) |
| Node.js | 24.13.1 (via Homebrew node@24) |
| Yarn | 1.22.22 |
| bench CLI | 5.29.1 (via uv tool) |

## Credentials

| | Value |
|---|---|
| Site admin username | `Administrator` |
| Site admin password | `admin` |
| MariaDB root password | `frappe123` |

## Access URLs

| Mode | URL |
|---|---|
| Normal | http://crm.localhost:8000/crm |
| Frontend dev (Vite hot-reload) | http://crm.localhost:8080 |

## Key Paths

| Item | Path |
|---|---|
| Bench root | `~/frappe-bench` |
| Frappe app | `~/frappe-bench/apps/frappe` |
| CRM app | `~/frappe-bench/apps/crm` |
| CRM Vue frontend | `~/frappe-bench/apps/crm/frontend` |
| Site config | `~/frappe-bench/sites/crm.localhost/site_config.json` |
| Site logs | `~/frappe-bench/sites/crm.localhost/logs` |
| Bench logs | `~/frappe-bench/logs` |

## Shell PATH (added to ~/.zshrc)

```zsh
export PATH="/opt/homebrew/opt/mariadb@10.6/bin:/opt/homebrew/opt/node@24/bin:/Users/hi/.local/bin:$PATH"
export LDFLAGS="-L/opt/homebrew/opt/mariadb@10.6/lib"
export CPPFLAGS="-I/opt/homebrew/opt/mariadb@10.6/include"
```

## Starting CRM

```bash
cd ~/frappe-bench
bench start
```

Open http://crm.localhost:8000/crm — login: `Administrator` / `admin`

## Frontend Dev Mode (hot-reload)

```bash
cd ~/frappe-bench/apps/crm
yarn dev
```

Open http://crm.localhost:8080

## Services (auto-start on login via Homebrew)

```bash
# Check status
brew services list | grep -E "mariadb|redis"

# Start manually if needed
brew services start mariadb@10.6
brew services start redis

# Stop
brew services stop mariadb@10.6
brew services stop redis
```

## Useful bench Commands

```bash
# Run from ~/frappe-bench

bench start                                      # Start all services
bench --site crm.localhost list-apps             # List installed apps
bench --site crm.localhost migrate               # Run DB migrations
bench --site crm.localhost clear-cache           # Clear cache
bench --site crm.localhost console               # Python REPL
bench get-app <appname>                          # Install a new app
bench update --patch                             # Apply patches only
bench restart                                    # Restart workers
```

## Notes

- Frappe v16 was skipped — it now requires Python 3.14 which is unstable
- Node v24 works with Frappe v15 frontend build (no issues found)
- MariaDB uses `mysql_native_password` plugin for root (not unix_socket)
- uv manages Python; bench CLI installed as a uv tool
- MariaDB is keg-only — binary at `/opt/homebrew/opt/mariadb@10.6/bin/mysql`
