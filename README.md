![Kali-Codespace](https://g.top4top.io/p_3536nulms0.png)

# Tutorial

This tutorial shows how to set up a Kali Linux container in GitHub Codespaces for pentesting using terminal and web-based tools.

---

## 1. Update & Upgrade System

```bash
sudo apt update && sudo apt upgrade -y
````

---

## 2. Create Project Directory (Optional)

```bash
mkdir -p ~/kali-codespace
cd ~/kali-codespace
```

---

## 3. Create Dockerfile

```bash
echo 'FROM kalilinux/kali-rolling

RUN apt update && apt install -y \
    git curl wget python3 python3-pip \
    ca-certificates build-essential sudo \
    && apt clean

RUN useradd -ms /bin/bash rosemary && echo "rosemary:kali" | chpasswd && adduser rosemary sudo
RUN echo "rosemary ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER rosemary
WORKDIR /home/rosemary

CMD ["/bin/bash"]' > Dockerfile
```

**Notes:** Root password for `rosemary` is `kali`.

---

## 4. Create docker-compose.yml

```bash
echo 'version: "3.8"

services:
  kali:
    build: .
    container_name: kali-cs
    tty: true
    stdin_open: true
    volumes:
      - kali-data:/home/

volumes:
  kali-data:' > docker-compose.yml
```

**Notes:** Volume `kali-data` stores persistent data.

---

## 5. Build Container

```bash
docker compose build
```

---

## 6. Run Container

```bash
docker compose up -d
```

---

## 7. Access Kali Container

```bash
docker exec -it kali-cs /bin/bash
```

---

## Notes

* Codespaces containers stop after idle.
* Save important files in volume (`kali-data`) or repository.
* Default user: `rosemary`, root password: `kali`.

---

## References

* [Kali Linux Docker Images](https://hub.docker.com/r/kalilinux/kali-rolling)
* [GitHub Codespaces Documentation](https://docs.github.com/en/codespaces)
