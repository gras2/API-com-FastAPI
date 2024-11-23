### 1. **Instalação de Dependências e Ambiente de Desenvolvimento**

Antes de começarmos a escrever o código, precisamos garantir que tudo esteja configurado corretamente no seu ambiente de desenvolvimento. Para isso, usamos ferramentas como **Python** e bibliotecas como **FastAPI** e **requests**.

**Passo 1: Instalar Python e Bibliotecas**

- **Python** é a linguagem que vamos usar para escrever nosso código. É necessário instalar a versão 3.7 ou superior. Se você ainda não tem o Python, pode baixar e instalar [aqui](https://www.python.org/downloads/).
  
- **FastAPI**: é um framework que facilita a criação de APIs (que são como portas de entrada para interagir com um sistema). Vamos usar o FastAPI para criar uma API que se comunica com a Meta Ads API.

- **requests**: é uma biblioteca que facilita a comunicação com APIs. Ela permite que você faça pedidos de dados de outras plataformas (como o Facebook, no caso da Meta Ads).

- **python-dotenv**: é uma biblioteca que ajuda a manter dados sensíveis (como seu **Access Token**) seguros, carregando esses dados a partir de um arquivo `.env` ao invés de deixá-los diretamente no código.

Para instalar as dependências, você pode usar o seguinte comando:

```bash
pip install fastapi uvicorn requests python-dotenv
```

### 2. **Estrutura do Código**

O código que vamos escrever terá algumas partes essenciais:

1. **Carregamento das variáveis de ambiente**: Vamos usar informações sensíveis, como **Access Token** e **ID da conta de anúncios**. Essas informações são carregadas de um arquivo `.env`.

2. **Criação da API**: Usaremos o FastAPI para criar um ponto de entrada (`/ads/campaigns`), onde os dados dos anúncios serão recuperados.

3. **Integração com a Meta Ads API**: A API do Facebook (Meta Ads API) precisa ser chamada para puxar os dados da conta de anúncios.

### 3. **Passo a Passo do Código**

Agora, vamos olhar cada parte do código, explicando como funciona e por que é importante:

#### Arquivo `.env`

O arquivo `.env` contém as variáveis de ambiente com dados sensíveis, como o **Access Token** e o **ID da conta de anúncios**. Este arquivo deve estar na raiz do seu projeto e nunca deve ser compartilhado publicamente. O `.env` pode ter a seguinte aparência:

```env
FACEBOOK_APP_ID=seu_app_id
FACEBOOK_APP_SECRET=seu_app_secret
ACCESS_TOKEN=seu_access_token
AD_ACCOUNT_ID=sua_conta_de_anuncios_id
```

**Explicação**:
- **FACEBOOK_APP_ID**: É o identificador do seu aplicativo no Facebook Developer.
- **FACEBOOK_APP_SECRET**: É a chave secreta associada ao seu aplicativo.
- **ACCESS_TOKEN**: É o token gerado para autorizar sua aplicação a acessar os dados da sua conta de anúncios.
- **AD_ACCOUNT_ID**: É o ID da conta de anúncios que você deseja acessar.

O arquivo `.env` ajuda a manter esses dados em segredo e facilita o gerenciamento em diferentes ambientes (como desenvolvimento e produção).

#### Carregando as Variáveis de Ambiente no Código

Agora que temos o arquivo `.env` com nossas variáveis de ambiente, precisamos carregá-las para o código. Isso é feito com a biblioteca `python-dotenv`:

```python
import os
from dotenv import load_dotenv

# Carrega as variáveis do arquivo .env
load_dotenv()

ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
AD_ACCOUNT_ID = os.getenv("AD_ACCOUNT_ID")
```

**Explicação**:
- `load_dotenv()`: Essa função carrega as variáveis do arquivo `.env` para o ambiente, tornando-as acessíveis no código.
- `os.getenv("ACCESS_TOKEN")`: Aqui, estamos pegando o valor da variável `ACCESS_TOKEN` do arquivo `.env` e atribuindo à variável `ACCESS_TOKEN` no código.

Essa abordagem é importante porque mantém os dados sensíveis (como o token de acesso) fora do código, o que é uma boa prática de segurança.

#### Criando a API com FastAPI

Agora, vamos usar o **FastAPI** para criar uma API simples. A FastAPI nos ajuda a definir endpoints — ou seja, URLs para os quais podemos fazer requisições.

```python
from fastapi import FastAPI
import requests

app = FastAPI()
```

**Explicação**:
- `FastAPI()`: Aqui estamos criando uma instância da aplicação FastAPI, que vai nos permitir criar endpoints.

Agora, vamos criar o primeiro endpoint para acessar as campanhas de anúncios:

```python
@app.get("/ads/campaigns")
async def get_campaigns():
    url = f"https://graph.facebook.com/v17.0/{AD_ACCOUNT_ID}/campaigns"
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

**Explicação**:
- **`@app.get("/ads/campaigns")`**: Isso cria um endpoint na URL `/ads/campaigns`. Ou seja, quando você acessar essa URL, o código será executado.
- **`async def get_campaigns()`**: Define uma função assíncrona (para maior performance) que será chamada quando alguém acessar o endpoint `/ads/campaigns`.
- **URL da Meta Ads API**: A variável `url` contém o endereço para a API do Facebook, com o ID da conta de anúncios (`AD_ACCOUNT_ID`) para buscar informações.
- **Parâmetros da Requisição**: No dicionário `params`, estamos passando o **Access Token** (autorizando o acesso) e os **campos** que queremos retornar (`id`, `name`, e `status` das campanhas de anúncios).
- **Requisição HTTP**: A função `requests.get()` faz uma requisição GET à Meta Ads API com os parâmetros definidos.
- **Tratamento da Resposta**: Se a resposta for bem-sucedida (código 200), retornamos os dados das campanhas em formato JSON. Se não for bem-sucedida, retornamos uma mensagem de erro.

#### Rodando a API

Depois de criar o código, basta rodar a API com o seguinte comando:

```bash
uvicorn main:app --reload
```

**Explicação**:
- **Uvicorn** é um servidor que executa a aplicação FastAPI.
- O comando `--reload` faz com que a API recarregue automaticamente sempre que fizermos alterações no código.

A API estará disponível em `http://127.0.0.1:8000/`, e você pode acessá-la através do navegador ou de ferramentas como **Postman** para testar.

### 4. **Como Alguém Sem Conhecimento de Programação Pode Fazer Isso?**

Aqui está um passo a passo para alguém sem experiência em programação conseguir seguir esse processo:

1. **Instalar o Python**: Baixe e instale o Python do site oficial.
2. **Criar uma Conta de Desenvolvedor no Facebook**: Acesse [Facebook for Developers](https://developers.facebook.com/) e crie uma conta.
3. **Criar um App no Facebook Developer**: Siga o tutorial no site do Facebook para criar um app e obter o **Access Token**.
4. **Baixar o Código**: Você pode baixar o código (ou copiar) de um repositório no GitHub, como o deste exemplo.
5. **Instalar as Bibliotecas**: Abra o terminal ou prompt de comando, navegue até a pasta do projeto e digite o comando `pip install -r requirements.txt` para instalar as bibliotecas necessárias.
6. **Editar o Arquivo `.env`**: Abra o arquivo `.env` e coloque seu **Access Token** e outros dados que você obteve do Facebook Developer.
7. **Rodar a API**: Execute o comando `uvicorn main:app --reload` para rodar a API.
8. **Testar a API**: Acesse `http://127.0.0.1:8000/ads/campaigns` no navegador ou use ferramentas como Postman para ver os dados sendo retornados.

### 5. **Integração Prática com a Meta Ads API**

Para integrar com a **Meta Ads API** na prática, siga esses passos:

1. **Criação do Aplicativo no Facebook Developer**: No painel do Facebook for Developers, crie um novo aplicativo. Durante o processo, você vai precisar gerar um **Access Token** e fornecer permissões.
2. **Configuração da Conta de Anúncios**: Certifique-se de que a conta de anúncios do Facebook esteja vinculada ao aplicativo para que ele possa acessar os dados.
3. **Testar as Chamadas à API**: Use o endpoint `/ads/campaigns` que criamos no código para buscar as campanhas. O Facebook vai retornar os dados conforme solicitado.

---

Esse guia é projetado para que até mesmo pessoas sem experiência em programação possam seguir passo a passo e implementar a integração com a Meta Ads API.
