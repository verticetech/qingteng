---

Qingteng Host Status Collection Documentation
Last Updated: March 4, 2025 | Version: 1.2.0

Title of Collection
`verticetech.qingteng_host` – Ansible Collection for Qingteng Cloud Host Status Monitoring


Description
The Qingteng Host Status Collection provides seamless integration with Qingteng Cloud’s API to monitor Linux host statuses (online/offline) and agent health.

Key Features
- Real-time Host Status Checks: Retrieve agent status (online, offline, disabled) and communication state via API.
- Batch Operations: Query multiple hosts simultaneously with dynamic signature authentication.
- Security-First Design: Built-in SHA1 signature verification and JWT token management.
- Ansible Native Integration: Simplify cloud asset monitoring within existing Ansible workflows.

Target Users:
- DevOps Engineers: Automate compliance checks for host health.
- SysAdmins: Monitor hybrid cloud environments (Qingteng + on-premises).
- Security Teams: Validate agent deployment status for vulnerability management.

Benefits:
- Reduce manual checks by 90% through API-driven automation.
- Ensure compliance with SLAs via programmatic status alerts.


Requirements
Minimum Versions
| Component      | Version | Notes                                  |
|-----------------|---------|----------------------------------------|
| Ansible Core    | ≥2.12   | Required for collection structure.     |
| Python          | ≥3.8    | Compatible with `requests` and `jq`.   |

Dependencies
- Python Libraries:
  - `requests` (≥2.26.0): Handle API calls and SSL/TLS.
  - `jq` (≥1.2.0): Parse JSON responses.
- External Systems:
  - Qingteng Cloud API Endpoint (≥v1.2).
  - Valid Qingteng account with API access permissions.

Prerequisites
1. API Credentials: Store Qingteng username/password securely (e.g., Ansible Vault).
2. Network Access: Ensure Ansible control node can reach `api.qingteng.internal:6000`.


Installation
Via Ansible Galaxy
```bash
ansible-galaxy collection install verticetech.qingteng_host
```

Using `requirements.yml`
```yaml
collections:
  - name: verticetech.qingteng_host
    version: "1.0.2"  # Pin to stable releases
```

Upgrade/Downgrade:
```bash
Upgrade to latest
ansible-galaxy collection install verticetech.qingteng_host --upgrade

Install specific version
ansible-galaxy collection install verticetech.qingteng_host:==1.0.2
```

Post-Installation
1. Configure Authentication:
   ```yaml
   # group_vars/all/vault.yml (encrypted)
   qingteng_api_username: "{{ vault_username }}"
   qingteng_api_password: "{{ vault_password }}"
   ```
2. Validate Connectivity:
   ```bash
   ansible localhost -m verticetech.qingteng_host.get_qingteng_host_status \
     -a "ip_addresses=['10.0.1.100'] hostnames=['web01']"
   ```


Use Cases
1. Batch Host Health Check
Scenario: Validate 500+ hosts’ agent status daily.
Implementation:
```yaml
- name: Check Qingteng Agent Status
  hosts: qingteng_managed
  tasks:
    - name: Get QingTeng Host Status
      verticetech.qingteng_host.get_qingteng_host_status:
        api_url: "{{ qingteng_credentials.api_url  }}"
        username: "{{ qingteng_credentials.username  }}"
        password: "{{ qingteng_credentials.password  }}"
        ip_addresses:
          - "192.168.1.1"            # Example IP1
          - "192.168.1.2"            # Example IP1
        hostnames:
          - "host1.example.com"        # Example hostname 1
          - "host2.example.com"        # Example hostname 1
```

2. Integration with Monitoring Systems
Scenario: Feed agent status into Prometheus/Grafana.
Workflow:
1. Use `get_qingteng_host_status` module to collect data.
2. Export results via `prometheus-exporter` role.

3. Compliance Reporting
Scenario: Generate weekly PDF reports for auditors.
Steps:
- Query all hosts with `agent_status=停用`.
- Render data using Jinja2 templates.

Testing
Tested Environments
| Environment           | Ansible | Python | Qingteng API | Status |
|-----------------------|---------|--------|--------------|--------|
| RHEL 9                | 2.14.3  | 3.9    | v1.1         | ✔️      |

Notes:
- *RHEL 9 requires `python3-jq` manual installation.
- Windows: Use WSL2 for full compatibility.

Known Issues
| Issue ID | Description                     | Workaround                     |
|----------|---------------------------------|--------------------------------|
| #45      | SSL cert verification failures  | Set `validate_certs: false`    |
| #52      | Timezone mismatch in timestamps | Use `TZ=Asia/Shanghai` in playbook |


Contributing
We welcome contributions!

Process
1. Fork the .
2. Submit PRs to `dev` branch with unit tests.
3. Follow .

Community Channels:
- Discussions:
- Bugs/Requests:


Support
Supported Versions
| Collection Version | Ansible Versions | End of Life   |
|--------------------|------------------|---------------|
| 1.0.2              | 2.15             | EOL (Jan 2025)|

Support Channels
- Enterprise Support:  (24/7 SLA).
- Community: GitHub Discussions and Stack Overflow (`qingteng-ansible` tag).


Release Notes and Roadmap
- Changelog:
- Roadmap:


Related Information
- Video Demo:
- Whitepaper:


License Information
- License: Apache 2.0
- Full Text:


Documentation Last Reviewed: March 4, 2025 |
