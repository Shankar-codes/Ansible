# 📦 Ansible Playbooks — Learning Collection

A structured collection of Ansible playbooks built progressively to cover core automation concepts, from basic connectivity checks to advanced error handling. Each file is numbered to reflect the recommended learning order.

---

## 🗂️ Repository Structure

```
Ansible/
├── 01-playbook.yaml              # Basic playbook & ping
├── 02-install-package-nginx.yaml # Package installation
├── 03-multi-playbook.yaml        # Multiple plays in one file
├── 04-variable.yml               # Play-level variables
├── 05-task-level-variable.yaml   # Task-level variables
├── 06-var-files.yaml             # External variable files
├── 07-var-prompt.yaml            # Interactive variable prompts
├── 08-var-inventory.yaml         # Inventory-defined variables
├── 09-var-args.yaml              # Command-line variable args
├── 10-var-preference.yaml        # Variable precedence
├── 11-data-types.yaml            # Ansible data types
├── 12-conditions.yaml            # Conditional task execution
├── 13-gather-facts.yaml          # Gathering system facts
├── 14-facts.yaml                 # Using facts in playbooks
├── 15-loops.yaml                 # Basic loops
├── 16-loops.yaml                 # Intermediate loops
├── 17-loops.yaml                 # Advanced loops
├── 18-functions.yaml             # Jinja2 filters & functions
├── 19-error-handling.yaml        # Error handling & ignore_errors
├── course.yaml                   # Course-level reference playbook
└── data.yaml                     # Sample data definitions
```

---

## 🚀 Getting Started

### Prerequisites

- **Ansible** installed on the control node (`pip install ansible`)
- SSH access to your managed hosts
- An **inventory file** defining your host groups (e.g., `frontend`, `local`)

### Running a Playbook

```bash
ansible-playbook -i <inventory-file> <playbook>.yaml
```

**Example:**

```bash
ansible-playbook -i inventory.ini 01-playbook.yaml
```

To pass extra variables at runtime:

```bash
ansible-playbook -i inventory.ini 09-var-args.yaml -e "username=shankar"
```

---

## 📚 Concepts Covered

### 1. Basics (`01` – `03`)
- Writing your first playbook with `ansible.builtin.ping` and `ansible.builtin.debug`
- Targeting specific host groups (e.g., `hosts: frontend`)
- Combining multiple plays within a single YAML file

### 2. Variables (`04` – `10`)
- Defining variables at the play level, task level, and in external files
- Collecting input at runtime using `vars_prompt`
- Setting variables via inventory and command-line `-e` flags
- Understanding **variable precedence** (which definition wins when multiple exist)

### 3. Data Types (`11`)
- Working with strings, integers, booleans, lists, and dictionaries in YAML

### 4. Conditions (`12`)
- Using the `when` directive to run tasks conditionally based on variable values or facts

### 5. Facts (`13` – `14`)
- Enabling and disabling `gather_facts`
- Accessing system information (OS, IP, CPU, memory) via `ansible_facts`

### 6. Loops (`15` – `17`)
- Iterating over lists with `loop`
- Looping over dictionaries and complex data structures
- Using `loop_control` for better output labeling

### 7. Functions / Filters (`18`)
- Applying Jinja2 filters (e.g., `upper`, `default`, `join`, `selectattr`)

### 8. Error Handling (`19`)
- Using `ignore_errors: yes` to continue execution after a failure
- Registering task output with `register`
- Conditional recovery — only running a task when a prior one failed (checking `.rc`)

---

## 🔍 Key Playbook Highlights

### `01-playbook.yaml` — Hello, Ansible
```yaml
- name: ping the server
  hosts: frontend
  tasks:
    - name: ping the server
      ansible.builtin.ping:
    - name: check connectivity
      ansible.builtin.debug:
        msg: "Hello from the server"
```

### `19-error-handling.yaml` — Safe User Creation
```yaml
- name: user creation
  hosts: local
  connection: local
  become: yes
  tasks:
    - name: check if user exists
      ansible.builtin.command: id ellamma
      register: user_creation
      ignore_errors: yes
    - name: create user only if absent
      ansible.builtin.command: useradd ellamma
      when: user_creation.rc != 0
```

---

## 🛠️ Tips

- Always test with `--check` (dry-run) before applying changes to production hosts:
  ```bash
  ansible-playbook -i inventory.ini playbook.yaml --check
  ```
- Use `-v`, `-vv`, or `-vvv` flags to increase verbosity for debugging.
- Ensure your inventory correctly defines host groups like `frontend` and `local` referenced across the playbooks.

---

## 👤 Author

**Shankar** — [github.com/Shankar-codes](https://github.com/Shankar-codes)

---

## 📄 License

This repository is intended for educational purposes. Feel free to fork, adapt, and use these playbooks as reference material.
