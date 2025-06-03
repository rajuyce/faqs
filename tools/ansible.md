# 📘 Ansible Real-Time Scenario Interview Questions & Answers

This document covers real-world Ansible interview questions with answers tailored for DevOps and Cloud professionals.

---

### 1. How would you manage secrets and sensitive data in Ansible?

- Use `ansible-vault` to encrypt files or strings. Example: `ansible-vault encrypt secrets.yml`.
- Store secrets outside Git and reference them using `vars_files`.
- Use dynamic lookup plugins for integration with Vault, AWS Secrets Manager, or SOPS.
- In CI/CD, inject secrets as environment variables and reference them using `lookup('env', 'SECRET_NAME')`.

---

### 2. You have multiple environments (dev, staging, prod). How do you handle inventory and variable segregation?

- Use directory-based inventory structure: `inventories/dev`, `inventories/staging`, etc.
- Use group variables (`group_vars`, `host_vars`) per environment.
- Load vars using `ansible-playbook -i inventories/dev/hosts site.yml`.

---

### 3. How do you ensure idempotency in Ansible playbooks?

- Use Ansible modules (e.g., `file`, `yum`, `copy`) that are inherently idempotent.
- Avoid using shell/command unless absolutely necessary.
- Test repeatedly to confirm no changes are reported on subsequent runs.

---

### 4. How do you run a specific task or play for a specific host from a large playbook?

- Use `--limit <hostname>` to target specific hosts.
- Use tags for specific tasks and `--tags` to execute them.
- Example: `ansible-playbook site.yml --limit web01 --tags "restart"`

---

### 5. How do you handle failed tasks and retries in Ansible?

- Use `retries` and `delay` with `until` loops.
- Example:
  ```yaml
  - name: wait for app to be up
    uri:
      url: http://localhost:8080
    register: result
    until: result.status == 200
    retries: 5
    delay: 10
  ```
- Use `ignore_errors: true` to continue on non-critical failures.

---

### 6. How do you use dynamic inventory in a cloud environment like AWS?

- Use the `aws_ec2` plugin in `ansible.cfg`.
- Configure `aws_ec2.yaml` to filter tags or regions.
- Run: `ansible-inventory -i aws_ec2.yaml --list`

---

### 7. You need to execute a task only when a file is modified. How do you achieve this?

- Use `stat` module to check file properties.
- Use `when` condition with checksum comparison.
- Or use `notify` and `handlers` to respond to changes.

---

### 8. How do you implement role-based architecture in Ansible?

- Create reusable roles: `roles/webserver`, `roles/db`, etc.
- Use `include_role` or `roles:` in playbooks.
- Maintain `defaults`, `vars`, `tasks`, `handlers`, and `templates` inside each role.

---

### 9. How do you troubleshoot a failed playbook run?

- Use `-vvv` for verbose logging.
- Inspect `register` outputs.
- Use `debug` module to print variable values.
- Check `ansible.log` or CI/CD logs if applicable.

---

### 10. How would you ensure Ansible playbooks are tested in CI/CD pipelines?

- Use tools like `ansible-lint` and `yamllint`.
- Perform syntax checks: `ansible-playbook --syntax-check`.
- Use Molecule for unit testing roles.
- Run test environments using Docker or ephemeral VMs.

---

### 11. You need to deploy different config files for each environment. How do you manage templating and variable precedence?

- Use `with_items` and `templates/` directory.
- Create `vars/dev.yml`, `vars/prod.yml` and load based on inventory.
- Use Jinja2 templating in `template` module.

---

### 12. How would you use Ansible in a hybrid (cloud + on-prem) environment?

- Use multiple inventory sources: static + dynamic.
- Use conditional logic (`when`) to apply different roles/tasks.
- Tag infrastructure using common naming schemes.

---

### 13. How do you handle secrets management using Vault or AWS Secrets Manager?

- Use `hashi_vault` or `aws_secret` lookup plugins.
- Example: `lookup('aws_secret', 'my/secret/key')`
- Avoid committing secrets in plaintext by using CI/CD to inject them.

---

### 14. You need to patch 100s of servers during a window. How do you control this in Ansible?

- Use `serial` to roll out changes in batches.
- Example:
  ```yaml
  - hosts: all
    serial: 10
    tasks:
      - name: apply patches
        yum:
          name: "*"
          state: latest
  ```
- Monitor with `--forks` and batch logs.

---

### 15. How do you run certain tasks with sudo and others without?

- Use `become: true` only for privileged tasks.
- Set `become_user` per task if needed.
- Avoid using `become: true` at the play level unless required globally.

---

### 16. How do you handle dependencies between Ansible roles?

- Define dependencies in `meta/main.yml` inside a role.
- Example:
  ```yaml
  dependencies:
    - { role: common }
  ```

---

### 17. You're running against hundreds of nodes and it's slow. How do you optimize performance?

- Use `--forks` to increase parallelism.
- Avoid unnecessary `gather_facts`.
- Use SSH pipelining and persistent connections (`ssh_args` in `ansible.cfg`).

---

### 18. How do you detect what will change before applying a playbook?

- Use `--check` mode to perform dry-run.
- Combine with `--diff` to show file/template changes.
- Note: not all modules fully support `check`.

---

### 19. How do you write OS-specific tasks in Ansible?

- Use `ansible_os_family` or `ansible_distribution` facts.
- Example:
  ```yaml
  - name: install package
    yum:
      name: httpd
    when: ansible_os_family == 'RedHat'
  ```

---

### 20. How do you support multiple teams managing their own Ansible code in the same repo?

- Use a monorepo structure with role separation.
- Isolate inventories, vars, and roles by team folders.
- Use automation to lint and validate changes with GitHub Actions or GitLab CI.

---

> 📌 **Pro Tip:** Use `collections`, `requirements.yml`, and Molecule to modularize and test reusable code.
