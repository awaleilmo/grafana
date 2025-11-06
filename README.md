# 🧠 Custom Grafana Build Guide (Ubuntu)

This guide explains how to **install, build, customize, and deploy Grafana from source** on Ubuntu.

---

## ⚙️ 1. Clone the Grafana Repository

```bash
git clone https://github.com/grafana/grafana.git
cd grafana
```

---

## 📦 2. Install Dependencies

Make sure Node.js, Yarn, and Go are installed.

```bash
sudo apt update
sudo apt install -y nodejs npm golang make git
corepack enable
corepack prepare yarn@stable --activate
```

Verify versions:

```bash
node -v
yarn -v
go version
```

---

## 🧰 3. Install Grafana Dependencies

```bash
yarn install
```

If you get permission errors:

```bash
sudo chown -R $USER:$USER ~/grafana
yarn install
```

---

## 🧩 4. Build the Frontend

```bash
yarn run build
```

---

## 🧪 5. Run Grafana (Development Mode)

To run Grafana directly after build:

```bash
./bin/linux-amd64/grafana server
```

Then open in browser:
```
http://localhost:3000
```
Default login: **admin / admin**

---

## 🚀 6. Deploying Grafana in Production

### Copy binary to system path
```bash
sudo cp -r bin/linux-amd64 /usr/share/grafana
sudo mkdir -p /etc/grafana /var/lib/grafana /var/log/grafana
```

### Create a systemd service

```bash
sudo nano /etc/systemd/system/grafana.service
```

Paste the following content:

```ini
[Unit]
Description=Grafana Custom Server
After=network.target

[Service]
Type=simple
User=grafana
Group=grafana
ExecStart=/usr/share/grafana/grafana server   --config=/etc/grafana/grafana.ini   --homepath=/usr/share/grafana
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Then:

```bash
sudo systemctl daemon-reload
sudo systemctl enable grafana
sudo systemctl start grafana
sudo systemctl status grafana
```

Access from browser again at:

```
http://your-server-ip:3000
```

---

## 🎨 7. Customizing Logo or UI

Edit frontend assets in:
```
packages/grafana-ui/src/components
public/img/
```
Then rebuild:
```bash
yarn run build
```

Restart Grafana to apply changes.

---

## 🧹 8. Troubleshooting

**Error:** “Could not find config defaults”  
➡ Make sure to run Grafana with `--homepath` or from project root.

**Error:** Permission denied on `.yarn/cache`  
➡ Run `sudo chown -R $USER:$USER .` inside the Grafana folder.

**Warning:** Running as root is not recommended  
➡ Always create a dedicated user for production (e.g., `grafana`).

---

✅ **You now have a fully working and customizable Grafana build!**
