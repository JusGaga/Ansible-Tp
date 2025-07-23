# Ansible Secure Nginx Deployment & Verification

This project provides Ansible playbooks to **securely deploy Nginx** with best practices (SSL, strict permissions, security headers) and to **verify** the correct operation of your Nginx servers, including HTTP/HTTPS checks and service status.

---

## Features

- Automated installation and configuration of Nginx
- Secure permissions for web root and SSL files
- Self-signed SSL certificate generation (or use your own)
- Advanced HTTP security headers (HSTS, CSP, X-Frame-Options, etc.)
- HTTP to HTTPS redirection
- Automated deployment of a custom index page
- Service and HTTP(S) verification playbook

---

## Prerequisites

- Ansible >= 2.9
- Target hosts: Ubuntu/Debian-based Linux servers
- SSH access to the target servers

---

## Project Structure

```
ansible-config/
  files/
    index.html           # Custom web page
    nginx.conf.j2        # Secure Nginx configuration template (Jinja2)
  inventory.ini          # Define your web_servers group here
  nginx_instal.yml       # Main playbook: installs and configures Nginx securely
  verification_nginx.yml # Playbook: verifies Nginx service and HTTP/HTTPS responses
ssh-keys/                # (Optional) SSH keys for deployment
```

---

## Usage

### 1. **Edit your inventory**

Edit `ansible-config/inventory.ini` to define your `web_servers` group with the target hosts.

### 2. **(Optional) Customize files**

- Replace `files/index.html` with your own web page if desired.
- Place your SSL certificate and key in `files/nginx.crt` and `files/nginx.key` if you do not want a self-signed certificate.

### 3. **Deploy Nginx securely**

```bash
ansible-playbook -i inventory.ini nginx_instal.yml
```

This will:

- Install Nginx
- Remove the default site
- Deploy a hardened configuration (from `nginx.conf.j2`)
- Set up SSL (self-signed or provided)
- Deploy your index.html
- Set strict permissions

### 4. **Verify Nginx deployment**

After deployment, run the verification playbook:

```bash
ansible-playbook -i inventory.ini verification_nginx.yml
```

This will:

- Check if the Nginx service is running
- Test HTTP to HTTPS redirection (expects HTTP 301)
- Test HTTPS response (expects HTTP 200, ignores self-signed certs)
- Display the Nginx version

---

## Security Best Practices Applied

- Nginx runs as a dedicated non-privileged user/group
- Web root and SSL files have strict permissions
- Self-signed SSL certificate is generated if none is provided
- Advanced HTTP security headers (HSTS, CSP, X-Frame-Options, etc.)
- Only safe HTTP methods are allowed
- Access to hidden and sensitive files is denied
- Nginx version is hidden from HTTP responses

---

## Troubleshooting

- If the HTTP check fails with a 301 error, this is expected due to HTTP→HTTPS redirection.
- If the HTTPS check fails, ensure Nginx is running and listening on port 443, and that the SSL certificate is present.
- Use the debug output of the verification playbook for detailed status on each host.

---

## License

MIT

---

If you need further help or want to extend the playbooks, feel free to open an issue or contribute!
