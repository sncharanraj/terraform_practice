# Terraform Infrastructure as Code (IaC) – Docker Container Provisioning

## 📌 Objective

Provision a local Docker container using Terraform to demonstrate Infrastructure as Code (IaC).

---

## 🛠 Tools Used

- Terraform
- Docker (Docker Desktop)
- Docker Provider (kreuzwerker/docker)

---

## 📂 Project Structure

```
terraform-docker-project/
│
├── main.tf
├── terraform.tfstate
├── terraform.tfstate.backup
└── README.md
```

---

## ⚙️ main.tf Configuration

```hcl
terraform {
  required_version = ">= 1.5.0"

  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = ">= 3.0.0"
    }
  }
}

provider "docker" {
  host = "unix:///var/run/docker.sock"
}

resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = false
}

resource "docker_container" "nginx_container" {
  name  = "terraform-nginx"
  image = docker_image.nginx.image_id

  ports {
    internal = 80
    external = 8085
  }

  restart = "unless-stopped"
}
```

---

## 🚀 Steps Executed

### 1️⃣ Initialize Terraform

```bash
terraform init
```

Downloads required providers.

---

### 2️⃣ Preview Infrastructure Plan

```bash
terraform plan
```

Shows what Terraform will create before applying.


![](Screenshots/plan.png)
---

### 3️⃣ Apply Configuration

```bash
terraform apply
```

Type `yes` to confirm.

**Result:**

- Pulled nginx image
- Created container
- Mapped port 8085 → 80

**Accessed via:**

```
http://localhost:8085
```

---

### 4️⃣ Verify Resources

```bash
terraform state list
docker ps
```

Confirmed container is running.


![](Screenshots/state.png)
---

### 5️⃣ Destroy Infrastructure

```bash
terraform destroy
```

Container successfully removed.


![](Screenshots/dest1.png)

![](Screenshots/dest2.png)
---

## 📚 Key Concepts Learned

- Infrastructure as Code (IaC)
- Terraform provider usage
- Terraform state file management
- Resource provisioning and destruction
- Docker integration with Terraform

---

## ✅ Outcome

Successfully provisioned and destroyed a Docker container using Terraform, demonstrating Infrastructure as Code principles.
