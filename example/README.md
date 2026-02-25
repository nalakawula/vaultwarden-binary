# Deploy Vaultwarden Without Docker

This is an example on how to deploy Vaultwarden without Docker. We will use Vaultwarden binary with systemd service.

## Directory Structure

```bash
/opt/vaultwarden
|-- .env
|-- bin
|   `-- vaultwarden
|-- lib
    |-- data
    `-- web-vault
```

## .env File

Refer to [.env.template](https://github.com/dani-garcia/vaultwarden/blob/main/.env.template)

```bash
ROCKET_ADDRESS=0.0.0.0
ROCKET_PORT=8080
DOMAIN=https://your-domain.tld
LOG_LEVEL=error
ORG_EVENTS_ENABLED=true
EVENTS_DAYS_RETAIN=7

# https://github.com/dani-garcia/vaultwarden/wiki/Enabling-admin-page#using-argon2
ADMIN_TOKEN='please-fill-it'
ADMIN_RATELIMIT_SECONDS=300
ADMIN_RATELIMIT_MAX_BURST=3
ADMIN_SESSION_LIFETIME=20

# Behind cloudflare proxy
# IP_HEADER=CF-Connecting-IP

SIGNUPS_ALLOWED=false
SIGNUPS_VERIFY=true
SIGNUPS_DOMAINS_WHITELIST=your-domain.tld

# https://github.com/dani-garcia/vaultwarden/wiki/SMTP-Configuration
SMTP_HOST=""
SMTP_FROM=""
SMTP_FROM_NAME=""
SMTP_SECURITY=starttls
SMTP_PORT=587
SMTP_USERNAME=""
SMTP_PASSWORD=""

# https://github.com/dani-garcia/vaultwarden/wiki/Enabling-Mobile-Client-push-notification
PUSH_ENABLED=true
PUSH_INSTALLATION_ID=""
PUSH_INSTALLATION_KEY=""

# I am using PostgresSQL instead of sqlite
DATABASE_URL=postgresql://db_user:db_pass@db_host:5432/db_name

DATA_FOLDER=data
WEB_VAULT_ENABLED=true
WEB_VAULT_FOLDER=web-vault/
```

## Systemd Service

Refer to [vaultwarden/wiki](https://github.com/dani-garcia/vaultwarden/wiki/Setup-as-a-systemd-service)

```bash
sudo nano /etc/systemd/system/vaultwarden.service
```

```ini
[Unit]
Description=Vaultwarden
Documentation=https://github.com/dani-garcia/vaultwarden

# In this example I use PostgreSQL instead of sqlite
After=network.target postgresql.service
Requires=postgresql.service


[Service]
# The user/group vaultwarden is run under. the working directory (see below) should allow write and read access to this user/group
User=your-user
Group=your-user
# Use an environment file for configuration.
EnvironmentFile=/opt/vaultwarden/.env
# The location of the compiled binary
ExecStart=/opt/vaultwarden/bin/vaultwarden
# Set reasonable connection and process limits
LimitNOFILE=1048576
LimitNPROC=64
# Isolate vaultwarden from the rest of the system
PrivateTmp=true
PrivateDevices=true
ProtectHome=true
ProtectSystem=strict
# Only allow writes to the following directory and set it to the working directory (user and password data are stored here)
WorkingDirectory=/opt/vaultwarden/lib
ReadWritePaths=/opt/vaultwarden/lib
```

To make systemd aware of your new file or any changes you made, run

```bash
$ sudo systemctl daemon-reload
```

To start this "service", run

```bash
$ sudo systemctl start vaultwarden.service
```

To enable autostart, run

```bash
$ sudo systemctl enable vaultwarden.service
```

In the same way you can `stop`, `restart` and `disable` the service.
