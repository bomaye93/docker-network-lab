# 🧱 Docker Network Lab + Ansible 🧠

> Projet personnel pour simuler une architecture réseau interne avec Docker et Ansible  
> Objectif : maîtriser les bases du DevOps, du réseau, de la conteneurisation, et de l'automatisation

---

## 🚀 Objectif du projet

Ce projet simule une petite infrastructure d’entreprise avec :

- Un **réseau privé Docker isolé**
- Un **serveur DNS Bind9** qui gère un domaine local `lab.local`
- Un **reverse proxy Nginx** qui redirige vers un serveur Apache
- Un **serveur Apache** qui héberge un site HTML
- Des **clients Debian** utilisés pour les tests
- Une **automatisation via Ansible** avec connexion SSH

---

## 🧱 Architecture réseau

```
             +-----------------------+
             |     debian1 (SSH)     |
             |   172.29.1.10         |
             +----------+------------+
                        |
     +------------------|------------------+
     |         Réseau Docker "lan"         |
     +------------------|------------------+
                        |
        +---------------+--------------+
        |     Autres services          |
        |                              |
        |  +---------+   +-----------+ |
        |  | dns     |   | webserver | |
        |  | 1.2     |   | 1.30      | |
        |  +---------+   +-----------+ |
        |                              |
        |     +---------------------+  |
        |     |   nginx-proxy       |  |
        |     |   172.29.1.3        |  |
        |     +---------------------+  |
        +------------------------------+
```

---

## 🧰 Technologies utilisées

| Outil         | Rôle                                       |
|---------------|--------------------------------------------|
| Docker        | Conteneurisation des services              |
| Bind9         | Serveur DNS pour le domaine `lab.local`    |
| Apache (httpd)| Serveur Web pour héberger le site HTML     |
| Nginx         | Reverse proxy HTTP                         |
| Ansible       | Configuration automatisée du client        |
| SSH           | Connexion sécurisée Ansible → container    |

---

## 📦 Contenu du projet

```
docker-network-lab/
├── docker-compose.yml
├── apache/
│   └── index.html
├── dns/
│   ├── named.conf
│   └── db.lab.local
├── nginx/
│   └── default.conf
└── ansible/
    ├── debian1/
    │   └── Dockerfile
    ├── inventory.ini
    └── playbooks/
        └── ping.yml
```

---

## 🛠 Installation

### 1. Cloner le dépôt
```bash
git clone git@github.com:tonpseudo/docker-network-lab.git
cd docker-network-lab
```

### 2. Lancer l’environnement
```bash
docker compose up -d --build
```

---

## ⚡ Utilisation d’Ansible

### 🔹 Connexion test avec ping.yml

```bash
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i ansible/inventory.ini ansible/playbooks/ping.yml
```

✅ Résultat attendu :
```yaml
debian1 | SUCCESS => {
    "ping": "pong"
}
```

---

## 🌍 Accès au site web

Dans `debian1` :
```bash
apt update && apt install -y curl dnsutils
echo "nameserver 172.29.1.2" > /etc/resolv.conf
curl http://web.lab.local
```

🟢 Réponse attendue :
```html
<h1>Bienvenue sur mon intranet Yahya 🔥</h1>
```

---

## 📜 Fichiers importants

### 🔧 `dns/db.lab.local`
```dns
$TTL 604800
@   IN  SOA dns.lab.local. root.lab.local. (
    2 604800 86400 2419200 604800 )
@       IN  NS      dns.lab.local.
dns     IN  A       172.29.1.2
web     IN  A       172.29.1.3
```

### 🔧 `nginx/default.conf`
```nginx
server {
    listen 80;
    server_name web.lab.local;

    location / {
        proxy_pass http://172.29.1.30;
    }
}
```

---

## 🧠 À faire ensuite

- [ ] Playbook pour installer Apache automatiquement
- [ ] Playbook pour configurer `/etc/resolv.conf`
- [ ] Reverse proxy Nginx en HTTPS (Let's Encrypt ou auto-signé)
- [ ] Dashboard web d'état (Netdata, etc.)
- [ ] CI/CD avec GitHub Actions

---

## ✍️ Auteur

**Yahya Diawara**  
🛠 Ingénieur Système en évolution DevOps  
📍 Paris 🇫🇷  
🐙 [github.com/bomaye93](https://github.com/bomaye93)

---

## 🧪 Projet personnel de lab DevOps — Powered by 🔥 Linux | Docker | Ansible
