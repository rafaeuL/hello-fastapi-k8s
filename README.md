
# Projeto hello-fastapi-k8s

Este repositório tem como objetivo demonstrar o ciclo completo de uma aplicação Python containerizada utilizando Docker e Kubernetes, com o Kind para criação de clusters locais e o ArgoCD para automação da entrega contínua.

---

## Pré-requisitos

- Python 3.8 ou superior  
- Docker  
- Kind  
- kubectl  

---

## Clonando o Repositório

```bash
git clone https://github.com/rafaeuL/hello-fastapi-k8s.git
cd hello-fastapi-k8s
```

---

## Criando o Ambiente Virtual (Linux)

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

---

## Criando o Cluster com Kind

```bash
kind create cluster --config kind.yaml --name hello-fastapi-k8s
```

---

## Construindo e Publicando a Imagem Docker

Altere a tag se necessário:

```bash
docker build -t rafaeuL/hello-fastapi-k8s:latest .
docker push rafaeuL/hello-fastapi-k8s:latest
```

> É necessário estar logado com sua conta no Docker Hub.

---

## Aplicando os Manifests do Kubernetes

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

Verifique se os recursos estão em execução:

```bash
kubectl get po
kubectl get svc
```

---

## Instalando e Configurando o ArgoCD

Crie o namespace do ArgoCD:

```bash
kubectl create namespace argocd
```

Aplique os manifests de instalação:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Acesse a interface do ArgoCD:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Abra no navegador: [https://localhost:8080](https://localhost:8080)

### Credenciais iniciais:

- **Usuário:** `admin`
- **Senha:** execute o comando abaixo para recuperar:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

A aplicação deve ser configurada no ArgoCD conforme o tutorial do projeto.

---

## Executando os Testes

Para rodar os testes automatizados:

```bash
pytest
```

---

## Modificações Realizadas no Código

Durante o desenvolvimento, algumas alterações foram feitas para garantir o correto funcionamento da aplicação. Abaixo estão as principais:

### `app/main.py`

- Adicionado bloco condicional para permitir execução direta com `python app/main.py` usando Uvicorn.
- Corrigido o endpoint `/square/x`, que anteriormente realizava uma soma. Agora calcula corretamente o quadrado de um número.

### `tests/test_main.py`

- Ajustado o teste da rota `/` para refletir a nova mensagem de resposta, alterada para demonstração ao professor.

---

 ** Professor nao consegui realizar atividade hoje, deu pau na internet da minha vm, meu note não tem entada pra cabo de rede, fiz o que deu (basicamente nada), em casa eu consegui realizar os passospelo menos.
