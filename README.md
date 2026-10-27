# 🛡️ Sistema Integrado de Monitoramento Urbano e Detecção de Ameaças (YOLOv11 + FastAPI)

[![Python](https://img.shields.io/badge/Python-3.11%20%7C%203.12-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100%2B-009688.svg)](https://fastapi.tiangolo.com/)
[![YOLOv11](https://img.shields.io/badge/YOLO-v11-00FFFF.svg)](https://docs.ultralytics.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📌 Visão Geral do Projeto

Este projeto consiste em um microsserviço de Visão Computacional para **segurança pública e monitoramento de ambientes urbanos/residenciais**. O sistema processa fluxos de imagens enviadas via requisição HTTP `POST` e aplica uma pipeline de **duplo modelo YOLO** para realizar a detecção simultânea de tráfego/pedestres e ameaças críticas de segurança (armas de fogo).

### 🎯 Principais Funcionalidades
- **Monitoramento de Tráfego e Mobilidade:** Detecção de classes urbanas padrão (`person`, `car`, `motorcycle`, `bicycle`, `bus`, `truck`) usando modelo YOLO predefinido.
- **Detecção de Ameaças Críticas:** Identificação precisa de armas de fogo (`guns`/`armas`) utilizando um modelo YOLOv11 afinado (*fine-tuned*) com dataset especializado do Roboflow.
- **Interface de API RESTful:** Processamento em tempo real de imagens via FastAPI, retornando saídas estruturadas em formato JSON com métricas de confiança e coordenadas delimitadoras (*bounding boxes*).

---

## 🏗️ Arquitetura do Microsserviço

A pipeline de inferência processa os dados de forma isolada e combinada conforme o diagrama abaixo:

                                          ┌────────────────────────┐
                                          │   Cliente / Sistema    │
                                          │   (Envio da Imagem)    │
                                          └───────────┬────────────┘
                                                      │ HTTP POST (/detect)
                                                      ▼
                                          ┌────────────────────────┐
                                          │    API FastAPI App     │
                                          │   (Processamento)      │
                                          └─────┬────────────┬─────┘
                                                │            │
                              ┌─────────────────┘            └─────────────────┐
                              ▼                                                ▼
                    ┌───────────────────────────┐            ┌───────────────────────────┐
                    │     Modelo Base YOLO      │            │   Modelo Fine-Tuned YOLO  │
                    │  (Tráfego/Pedestres/COCO) │            │  (Ameaças / Armas - Best) │
                    └─────────────┬─────────────┘            └─────────────┬─────────────┘
                                  │                                        │
                                  └─────────────────┬──────────────────────┘
                                           Detecções Combinadas
                                                    ▼
                                        ┌────────────────────────┐
                                        │   Retorno JSON Final   │
                                        │  (Status, Box, Conf)   │
                                        └────────────────────────┘


---

## 📊 Dataset e Treinamento

- **Dataset de Armas (Roboflow):** O modelo especializado foi treinado e validado utilizando imagens anotadas da plataforma Roboflow.
- **Acesso ao Dataset:** [Clique aqui para acessar o Dataset no Roboflow]([https://universe.roboflow.com/](https://universe.roboflow.com/dfyv7/guns-detection-security-camera-o62yq))

---

## 🚀 Guia de Instalação e Execução Local

### 1. Pré-requisitos
- Python 3.11 ou 3.12 instalado.
- Git instalado.

### 2. Clonar o Repositório
```bash
git clone [https://github.com/Willemann0511/ProjetoFinalModulo2.git]
cd ProjetoFinalModulo2
```

### 3. Instalar Dependências
Recomenda-se criar um ambiente virtual antes de instalar as dependências:

```bash
# Ativar no Windows:
venv\Scripts\activate

# Ativar no Linux/macOS:
source venv/bin/activate

# Instalar os pacotes
pip install -r requirements.txt
```

### 4. Executar a API FastAPI
Com o ambiente ativo e os modelos (yolo11n.pt e best.pt) na pasta raiz, inicie o servidor com Uvicorn:

```bash
uvicorn main:app --reload
O servidor estará acessível em: http://127.0.0.1:8000
```
🧪 Como Testar a API
Opção 1: Via Documentação Interativa (Swagger UI)

Com o servidor ativo, abra o navegador e acesse: http://127.0.0.1:8000/docs

Expanda o endpoint POST /detect.

Clique em Try it out.

Faça o upload de uma imagem no campo file.

Clique em Execute e visualize a resposta JSON estruturada.

Opção 2: Exemplo de Resposta JSON da API
```bash
{
  "status": "success",
  "detections": [
    {
      "class_name": "person",
      "confidence": 0.89,
      "box": {
        "x_min": 120,
        "y_min": 80,
        "x_max": 350,
        "y_max": 600
      }
    },
    {
      "class_name": "gun",
      "confidence": 0.94,
      "box": {
        "x_min": 210,
        "y_min": 150,
        "x_max": 280,
        "y_max": 220
      }
    }
  ]
}

```

🛠️ Metodologia Ágil e Gerenciamento (Trello)
O planejamento e a execução de todo o ciclo de vida deste projeto foram gerenciados utilizando a metodologia Kanban no Trello, estruturado nas colunas: Backlog, To Do, Doing, Blocked, Review e Done.

📋 Quadro Público do Trello: [Clique aqui para acessar o Quadro do Trello](https://trello.com/b/6TLqKxu0/quadro-projeto-final)

🌿 Estrutura de Versionamento (GitFlow)
O repositório foi organizado e mantido seguindo o padrão de ramificação GitFlow:
```bash
main: Código estável e pronto para produção.

develop: Branch de integração para desenvolvimento.

feature/*: Branches específicas para novas funcionalidades (ex: feature/yolo-finetuning, feature/api-setup).
```
📹 Vídeo de Apresentação e Demonstração
🔗 Link do Vídeo Demonstrativo: Assistir no YouTube / Google Drive

