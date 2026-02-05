# Ansible Role: Nexus Repository Manager

<br>

<p align="center">
  <img src="https://raw.githubusercontent.com/ettory-automation/skill-icons/main/icons/nexus-sonatype.svg" width="250"/>&nbsp;&nbsp;&nbsp;
  <img src="https://raw.githubusercontent.com/ettory-automation/skill-icons/main/icons/Ansible.svg" width="80"/>&nbsp;&nbsp;&nbsp;
  <img src="https://raw.githubusercontent.com/ettory-automation/skill-icons/main/icons/jinja2-simple.svg" width="80"/>&nbsp;&nbsp;&nbsp;
</p>

<br>

Installs and manages [Sonatype Nexus Repository Manager](https://www.sonatype.com/nexus-repository-sonatype) on Linux hosts. Includes installation, directories setup, systemd service, and health checks via API.

<br>

## ✅ Requirements

- Linux OS (Ubuntu/Debian/CentOS/RHEL)
- Ansible >= 3.0
- Python 3
- SSH access with sudo privileges

<br>

## ⚙️ Variables

### Defaults (`roles/nexus/defaults/main.yml`)

| Variable | Default | Description |
| :------ | :------ | :---------- |
| `nexus_version` | `3.88.0-08` | Nexus version to install |
| `nexus_user` | `nexus` | System user to run Nexus |
| `nexus_base_dir` | `/opt/sonatype` | Base installation directory |
| `nexus_install_dir` | `{{ nexus_base_dir }}/nexus-{{ nexus_version }}` | Directory for the specific version |
| `nexus_current_link` | `{{ nexus_base_dir }}/nexus` | Symbolic link to current Nexus |
| `nexus_data_dir` | `/data/sonatype/sonatype-work` | Data/work directory |
| `nexus_port` | `8081` | TCP port Nexus listens on |
| `nexus_java_opts` | `-Xms1200M -Xmx1200M` | Java memory options |
| `nexus_download_url` | `https://download.sonatype.com/nexus/3/nexus-{{ nexus_version }}-linux-x86_64.tar.gz` | Nexus download URL |
| `nexus_archive` | `nexus-{{ nexus_version }}-linux-x86_64.tar.gz` | Downloaded archive filename |
| `nexus_tmp_dir` | `/home/ansible/tmp` | Temporary directory for downloads/extraction |

> You can override variables in `vars/main.yml` if needed.

<br>

## 📐 Task Structure

- `install.yml` – Create user, directories, download/extract Nexus, install systemd service.  
- `systemd.yml` – Configure and enable the systemd service.  
- `healthcheck.yml` – Verify Nexus responds via REST API (`/service/rest/v1/status`).  
- `rollback.yml` – Optional rollback tasks in case of failed upgrade or installation.

<br>

## ⚡ Usage

Example playbook (`playbooks/nexus.yml`):

```yaml
---
- name: Install Nexus Repository Manager
  hosts: nexus
  roles:
    - nexus
...
```

<br>

## 🗒️ Notes / Best Practices

* Role is idempotent: re-running will not break existing installation.
* Health check retries can be tuned with retries and delay in healthcheck.yml.
* Use a dedicated system user (nexus_user) with limited privileges.
* Ensure Java memory settings (nexus_java_opts) are suitable for your environment.
