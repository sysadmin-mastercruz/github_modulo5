# 🛒 AZURE API

API REST em Flask para simular um sistema de compras sustentáveis com cálculo de impacto ambiental. Permite gerir produtos, supermercados e encomendas, carregando dados a partir de ficheiros CSV e expondo funcionalidades através de endpoints HTTP.

---

## 📚 Índice

- [Visão Geral](#visão-geral)
- [Funcionalidades](#funcionalidades)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Instalação](#instalação)
- [Execução Local](#execução-local)
- [Endpoints da API](#endpoints-da-api)
- [Exemplos de Uso](#exemplos-de-uso)
- [Erros Comuns](#erros-comuns)
- [Deploy no Azure](#deploy-no-azure)
- [Licença](#licença)
- [Autor](#autor)

---

## 👀 Visão Geral

Este projeto fornece uma API RESTful completa com:

- Gestão de produtos e supermercados
- Encomendas com cálculo de impacto ambiental
- Carga de dados dinâmica via CSV
- Preparado para deploy em ambientes cloud (ex: Azure VM)

---

## ✅ Funcionalidades

- 📦 `GET /api/produtos`: listar produtos disponíveis
- 🏪 `GET /api/supermercados`: listar supermercados
- 🧾 `POST /api/encomendas`: registar encomendas
- 🌍 `GET /api/impacto`: calcular impacto ambiental agregado
- 🧠 Lógica de negócio modularizada
- 🔌 API sem interface gráfica (JSON-only)

---

## 📁 Estrutura do Projeto

```
api-azure/
├── app/
│   ├── api_routes.py         # Rotas da API
│   ├── __init__.py           # Criação da aplicação Flask
│   ├── logic/                # Lógica de domínio (Produto, Encomenda, etc.)
│   └── utils/                # Ferramentas de carregamento de dados
├── run.py                    # Ponto de entrada da aplicação
├── requirements.txt          # Bibliotecas necessárias
├── teste_api.py              # Script para testes locais
├── .env                      # Variáveis de ambiente
└── README.md                 # Documentação do projeto
```

---

## ⚙️ Instalação

### Pré-requisitos

- Python 3.10+
- pip
- flask
- python-dotenv
- azure-storage-blob


### Setup local

```bash
git clone <repo-url>
cd projeto5-api-azure
sudo apt update && upgrade -y
sudo apt install python3.12-venv -y
python3 -m venv .venv
source .venv/bin/activate  # Ou .\venv\Scripts\activate no Windows
pip install -r requirements.txt
```

---

## ▶️ Execução Local

```bash
python run.py
```

A aplicação fica disponível por padrão em:

```
http://localhost:5000/
```

Num ambiente VM:
```
http://<IP_DA_VM>:5000/
```

> Verifica que o `run.py` tem `host='0.0.0.0'` para aceitar ligações externas.

---

## 📌 Endpoints da API

| Método | Rota               | Descrição                              |
|--------|--------------------|----------------------------------------|
| GET    | `/`                | Página HTML com links de teste         |
| GET    | `/api/produtos`    | Lista todos os produtos                |
| GET    | `/api/supermercados` | Lista os supermercados               |
| POST   | `/api/encomendas`  | Cria uma nova encomenda                |
| GET    | `/api/impacto`     | Mostra impacto ambiental acumulado     |

---

## 🧪 Exemplos de Uso

### Criar Encomenda (POST)

Se for utilizado o comando curl, o input seria:
```bash
curl -X POST http://localhost:5000/api/encomendas \
     -H "Content-Type: application/json" \
     -d '{
           "supermercado": "Pingo Doce",
           "produtos": [
             {"nome": "Banana", "quantidade": 3},
             {"nome": "Maçã", "quantidade": 2}
           ]
         }'
```
Testar com o postman:
```bash
Abrir o postman e dar import do ficheiro teste_postman.json
```

Testar com script python:
```bash
cd ~/projeto5/api-azure
python3 teste_endpoints.py
```

### Obter Produtos

```bash
curl http://localhost:5000/api/produtos
```

Relembrando que caso se use uma VM é necessário alterar o acesso para http://<IP_DA_VM>:5000/

---

## ⚠️ Erros Comuns

| Código | Causa                                                   |
|--------|----------------------------------------------------------|
| 404    | Caminho inválido ou supermercado não encontrado          |
| 400    | Dados mal formatados (JSON inválido, campos ausentes)    |
| 500    | Erro interno no servidor (verifica a lógica do código)   |

---

## ☁️ Deploy no Azure

### Na Azure VM

1. Verificar se a app corre com:
   ```python
   app.run(debug=True, host="0.0.0.0")
   ```

2. Abrir no **Grupo de Segurança de Rede** no portal Azure, as **Definições de Rede** no menu lateral e adicionar na funcionalidade **Criar regra de portas** as permissões de acesso da porta 5000, digitando-a na opção **Intervalos de portas de destino**

3. Aceder à aplicação via:
   ```
   http://<IP_PÚBLICO_DA_VM>:5000/
   ```

---

