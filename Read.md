# FastAPI Meta Ads API Integração

Este repositório contém um exemplo de como criar uma API utilizando o **FastAPI** para integrar com a **Meta Ads API** (antigo Facebook Ads API). A API criada será capaz de puxar dados de anúncios do Meta (Facebook) para análise e integração com outras ferramentas ou plataformas.

## 🚀 Começando

Este guia irá destrinchar todo o processo de criação de uma API com FastAPI que pode acessar os dados da Meta Ads API. Você precisará de uma conta no Facebook e configurar um aplicativo para acessar a API do Facebook.

### Pré-requisitos

Antes de começar, você precisa ter os seguintes itens instalados em sua máquina:

1. **Python 3.7 ou superior**
2. **Git** (se você deseja clonar o repositório)
3. **Facebook Developer Account** - você pode criar sua conta aqui: [Facebook for Developers](https://developers.facebook.com/)
4. **Token de Acesso do Facebook** - Para acessar os dados do Meta Ads, você precisa criar um token de acesso na plataforma do Facebook Developers.
5. **FastAPI** e **Uvicorn** para rodar o servidor da API.
   
### Instalação

#### 1. Clonando o Repositório
```
git clone https://github.com/SEU_USUARIO/fastapi-meta-ads-api.git
cd fastapi-meta-ads-api
```

#### 2. Instalando Dependências
Crie e ative um ambiente virtual (opcional, mas recomendado): 
```
python -m venv venv
source venv/bin/activate  # Para Linux/Mac
venv\Scripts\activate     # Para Windows
```
Instale as dependências necessárias:
```
pip install -r requirements.txt
```

Se não tiver um arquivo requirements.txt, instale as dependências manualmente com:
```
pip install fastapi uvicorn requests python-dotenv
```

 - **FastAPI**: Framework para criação da API.
 - **Uvicorn**: Servidor ASGI para rodar a aplicação FastAPI.
 - **Requests**: Biblioteca para fazer requisições HTTP.
 - **python-dotenv**: Para carregar variáveis de ambiente de um arquivo .env.

#### 3. Acesse o Facebook Developers, crie um aplicativo e gere um Access Token.

Você precisará do ID do seu app, ID da conta de anúncios e Access Token. 
Mantenha esses dados em um arquivo .env para garantir segurança:

Crie um arquivo .env na raiz do projeto e adicione as seguintes variáveis:
```
FACEBOOK_APP_ID=seu_app_id
FACEBOOK_APP_SECRET=seu_app_secret
ACCESS_TOKEN=seu_token_de_acesso
AD_ACCOUNT_ID=sua_conta_de_anuncios_id
```
#### 4. Estrutura do Projeto no VsCode/Pyenv/Pycharm.
```
/fastapi-meta-ads-api
├── .env
├── main.py
├── requirements.txt
└── README.md
```

#### 5. Escrevendo o Código da API
Arquivo main.py
Aqui está o código básico para criar a API com FastAPI e fazer uma requisição para a Meta Ads API:
```
import os
from fastapi import FastAPI
import requests
from dotenv import load_dotenv

# Carrega as variáveis do arquivo .env
load_dotenv()

app = FastAPI()

# Variáveis de ambiente
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
AD_ACCOUNT_ID = os.getenv("AD_ACCOUNT_ID")

# URL base da API do Facebook Ads
BASE_URL = "https://graph.facebook.com/v17.0"

# Endpoint para pegar os dados de campanhas
@app.get("/ads/campaigns")
async def get_campaigns():
    url = f"{BASE_URL}/{AD_ACCOUNT_ID}/campaigns"
    params = {
        "access_token": ACCESS_TOKEN,
        "fields": "id,name,status",
    }
    
    response = requests.get(url, params=params)
    
    if response.status_code == 200:
        return response.json()
    else:
        return {"error": "Falha ao recuperar dados", "status_code": response.status_code}
```

## 📝 Explicação do Código

Este código foi feito para criar uma API simples que se conecta ao **Meta Ads API** (a API do Facebook para anúncios). Vamos explicar o que cada parte do código faz, de forma simples:

- **Variáveis de ambiente**:
  - Para proteger informações sensíveis, como o **Access Token** (um código que autoriza nosso aplicativo a acessar os dados do Facebook), utilizamos a biblioteca `python-dotenv`.
  - Com ela, carregamos essas informações a partir de um arquivo chamado `.env`. Esse arquivo contém variáveis como o **ID do aplicativo** e o **token de acesso** do Facebook, que são necessários para fazer as requisições para a API.
  - Isso é importante porque, ao usar variáveis de ambiente, evitamos que informações sensíveis fiquem visíveis no código e ajudem a manter a segurança.

- **FastAPI**:
  - Usamos o **FastAPI**, que é uma ferramenta para criar APIs, ou seja, maneiras de permitir que diferentes sistemas se comuniquem entre si.
  - Criamos um "ponto de acesso" na nossa API chamado `/ads/campaigns`. Isso significa que, quando alguém acessar essa URL, a API vai buscar e retornar os dados das campanhas de anúncios que estão na conta de anúncios do Facebook.
  - Em termos simples, a FastAPI facilita a criação de uma interface entre nosso código e o mundo exterior (ou seja, outras pessoas ou sistemas que querem acessar os dados).

- **Requisição HTTP**:
  - A API do Meta Ads precisa ser "consultada" para obter os dados. Para isso, usamos a biblioteca `requests`.
  - A `requests` faz uma "pergunta" para a Meta Ads API e pede informações sobre as campanhas de anúncios. Quando a Meta Ads API responde com os dados, o código devolve essas informações para quem fez a solicitação à nossa API.
  - Em outras palavras, estamos enviando uma solicitação para o Facebook e recebendo os dados das campanhas de anúncios como resposta.



#### 6. Rodando a API
Com o código pronto, você pode rodar a aplicação com o comando:
```
uvicorn main:app --reload
```
A API estará rodando localmente em *http://127.0.0.1:8000.*

## 📚 Links Úteis

- [Documentação do FastAPI](https://fastapi.tiangolo.com/)
- [Meta Ads API Docs](https://developers.facebook.com/docs/marketing-api/)
- [Facebook for Developers](https://developers.facebook.com/)
- [Uvicorn Docs](https://www.uvicorn.org/)
