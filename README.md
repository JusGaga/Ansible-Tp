# Ansible Nginx Secure Deployment

This project provides an Ansible playbook to deploy a secure Nginx web server with best security practices, including SSL, strict permissions, and advanced HTTP headers.

## Features

- Automated Nginx installation and configuration
- Dedicated user and group for Nginx
- Secure permissions for web root and SSL files
- Self-signed SSL certificate generation (or use your own)
- Advanced security headers (HSTS, CSP, X-Frame-Options, etc.)
- Restriction of dangerous HTTP methods
- Protection against access to hidden and sensitive files

## Prerequisites

- Ansible >= 2.9
- Target hosts: Ubuntu/Debian-based Linux servers
- SSH access to the target servers

## Usage

1. Clone this repository.
2. Edit `ansible-config/inventory.ini` to define your `web_servers` group.
3. Place your SSH key in `ssh-keys/` if needed.
4. (Optional) Replace `files/index.html` with your own web page.
5. (Optional) Place your SSL certificate and key in `files/nginx.crt` and `files/nginx.key` if you do not want a self-signed certificate.
6. Run the playbook:

```bash
ansible-playbook -i inventory.ini nginx_instal.yml
```

## Security Best Practices Applied

- Nginx runs as a dedicated non-privileged user/group
- Web root and SSL files have strict permissions
- Self-signed SSL certificate is generated if none is provided
- Advanced HTTP security headers (HSTS, CSP, X-Frame-Options, etc.)
- Only safe HTTP methods are allowed
- Access to hidden and sensitive files is denied
- Nginx version is hidden from HTTP responses

## Files Structure

- `ansible-config/nginx_instal.yml`: Main Ansible playbook
- `ansible-config/files/`: Contains templates and static files
- `ansible-config/inventory.ini`: Inventory file for Ansible
- `ssh-keys/`: SSH keys for deployment

## License

MIT
