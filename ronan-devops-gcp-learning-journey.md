[28
# Jornada DevOps + GCP — Registro de Aprendizado (Ronan)

Data: 2026-02-13

Este documento consolida os principais aprendizados práticos obtidos durante sua jornada inicial com **Linux (WSL)**, **VS Code**, **Git**, **Docker** e conceitos de **GCP/IAM** usados no dia a dia.

---

## 1) Setup e ambiente

Ferramentas:
- Windows + WSL (Ubuntu)
- VS Code (edição)
- Terminal Linux/WSL (execução)
- Git (versionamento)
- Docker (containerização)
- Browser no Windows (Edge)

Regra prática:
- **VS Code → escrever**
- **Terminal → executar**
- **README.md → documentar (não executa nada)**

---

## 2) Linux essencial

Comandos:
- `pwd` → onde estou
- `ls` / `ls -la` → listar arquivos (inclui ocultos com `-la`)
- `cd <pasta>` → navegar

Pasta padrão do projeto:
- `~/Projects/first-project`

---

## 3) Git essencial

Conceitos:
- Repositório existe após `git init`
- Commit = fotografia do estado do projeto
- Graph do VS Code mostra histórico (versões antigas aparecem porque são parte da linha do tempo)

Fluxo básico:
- `git init`
- `git add .`
- `git commit -m "Initial project structure"`
- `git status`
- `git log --oneline`

---

## 4) HTML (mínimo)

Estrutura validada:
- `<h1>` título principal
- `<p>` parágrafo
- `<ul><li>` lista

Abrir no navegador:
- Live Server no VS Code (Windows abre, WSL sem browser GUI não abre com `xdg-open`)

---

## 5) Docker (primeiro ciclo completo)

Objetivo:
- Servir `index.html` via Nginx em container.

Conceitos:
- Dockerfile = receita
- Image = artefato construído
- Container = imagem em execução

Build:
- `docker build -t first-project .`

Run:
- `docker run -d -p 8080:80 --name first-container first-project`

Verificar:
- `docker ps`

---

## 6) Permissão do Docker (erro e correção)

Erro:
- permission denied ao acessar `/var/run/docker.sock`

Causa:
- usuário sem permissão/grupo docker

Correção:
- `sudo groupadd docker` (se não existir)
- `sudo usermod -aG docker $USER`
- reiniciar sessão (ou `newgrp docker`)
- validar: `docker ps`

---

## 7) Conceito-chave: imutabilidade

Sintoma:
- alterei `index.html`, mas o container não atualizou

Motivo:
- a imagem/container tem cópia “congelada” do arquivo no momento do build

Atualizar (modo simples):
1. `docker stop first-container`
2. `docker rm first-container`
3. `docker build -t first-project .`
4. `docker run -d -p 8080:80 --name first-container first-project`

Próxima evolução:
- bind mount em DEV (sem rebuild)
- CI/CD para rebuild automático

---

## 8) GCP — mentalidade correta

Você já executa no GCP (triggers etc.) e pesquisa durante o processo.

Conceitos que precisam ficar sólidos:

### Build-time vs Runtime Identity
- Trigger só dispara
- Quem executa build: **Cloud Build Service Account**
- Imagem vai para: **Artifact Registry**
- Quem roda app: **Cloud Run Service Account**

Diagnóstico:
- Se deploy ok, mas erro ao acessar banco → problema em **runtime SA do Cloud Run**

Segurança:
- Dar `roles/editor` “resolve”, mas viola **Princípio do Menor Privilégio**.

---

## 9) Próximos passos (curto e eficiente)

Docker:
- bind mount
- `docker logs`, `docker exec`, `docker inspect`
- Docker Compose (2 serviços)

GCP:
- hierarquia (Org/Folder/Project)
- IAM bindings (adicionar/remover)
- ciclo completo: Cloud Build → Artifact Registry → Cloud Run → Cloud Logging

Inglês técnico:
- docs em inglês
- explicar em PT e repetir em EN simples (funcional)

---

## 10) Checklist diário (5–10 min)

- Registrar 1 aprendizado no README
- Entender 1 erro: causa → impacto → correção
- 1 termo em inglês por dia (service account, binding, artifact registry...)

---

Fim.
