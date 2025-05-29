# ⚙️ Ansible Real-Time Scenario-Based Interview Questions & Answers

---

### 1. **How would you manage secrets and sensitive data in Ansible?**

Use **Ansible Vault** to encrypt secrets like passwords or keys:
```bash
ansible-vault encrypt secrets.yml
```
To use secrets in playbooks:
```yaml
vars_files:
  - secrets.yml
```
You can also use tools like HashiCorp Vault with dynamic secrets via lookup plugins.

---

### 2. **You have multiple environments (dev, staging, prod). How do you handle inventory and variable segregation?**

Structure your inventory like this:
```
inventory/
├── dev/
│   ├── hosts
│   └── group_vars/
├── staging/
│   ├── hosts
│   └── group_vars/
├── prod/
│   ├── hosts
│   └── group_vars/
```
Use dynamic inventory scripts or environment-specific playbook parameters.

---

### 3. **How do you ensure idempotency in Ansible playbooks?**

- Use Ansible modules instead of raw shell/command.
- Ensure `creates=`, `removes=`, `only_if=` options are used.
- Test playbooks repeatedly to confirm no changes are made when no updates are needed.

---

### 4. **How do you run a specific task or play for a specific host from a large playbook?**

Use the `--limit` flag:
```bash
ansible-playbook site.yml --limit web01
```
Or use `tags` and run specific tagged tasks:
```yaml
- name: Install NGINX
  tags: nginx
```
```bash
ansible-playbook site.yml --tags nginx
```

---

### 5. **How do you handle failed tasks and retries in Ansible?**

- Use `retries` and `delay` in `until` loop:
```yaml
- name: Wait for service
  uri:
    url: http://localhost:8080
  register: result
  until: result.status == 200
  retries: 5
  delay: 10
```
- Use `block`, `rescue`, and `always` for error handling.

---

### 6. **How do you use dynamic inventory in a cloud environment like AWS?**

Use the `aws_ec2` dynamic inventory plugin:
```ini
plugin: aws_ec2
regions:
  - eu-central-1
filters:
  tag:Environment: dev
```
Enable it in `ansible.cfg` and use `ansible-inventory --list` to verify.

---

### 7. **You need to execute a task only when a file is modified. How do you achieve this?**

Use handlers:
```yaml
- name: Update config
  template:
    src: app.conf.j2
    dest: /etc/app.conf
  notify: restart app

handlers:
  - name: restart app
    systemd:
      name: app
      state: restarted
```

---

### 8. **How do you implement role-based architecture in Ansible?**

Create reusable roles:
```
roles/
├── nginx/
│   ├── tasks/main.yml
│   ├── handlers/main.yml
│   ├── templates/
│   └── vars/main.yml
```
Then include them in your playbook:
```yaml
roles:
  - nginx
```

---

### 9. **How do you troubleshoot a failed playbook run?**

- Use `-vvv` verbosity for detailed logs.
- Check for unreachable hosts or permission issues.
- Validate `ansible-lint` for best practices.
- Use `--start-at-task` to resume from a specific point.

---

### 10. **How would you ensure Ansible playbooks are tested in CI/CD pipelines?**

- Use tools like **Molecule** for unit testing roles.
- Run syntax check: `ansible-playbook playbook.yml --syntax-check`
- Use `ansible-lint`, `yamllint` in GitHub Actions/GitLab CI.
- Test in isolated environments using Docker or test VMs.

---
