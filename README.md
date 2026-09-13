# Stock Analysis API

## API externa: Alpha Vantage

O endpoint `POST /analisar/{ticker}` consulta a [Alpha Vantage](https://www.alphavantage.co/)
para obter dados historicos mensais ajustados de acoes brasileiras. A chave de
acesso pode ser solicitada gratuitamente na [pagina de suporte](https://www.alphavantage.co/support/#api-key).

O projeto utiliza o metodo `TIME_SERIES_MONTHLY_ADJUSTED`:

```text
GET https://www.alphavantage.co/query?function=TIME_SERIES_MONTHLY_ADJUSTED&symbol={TICKER}.SA&apikey={API_KEY}
```

- `function`: seleciona a serie mensal ajustada.
- `symbol`: recebe o ticker informado pelo usuario com o sufixo `.SA`, usado
  para acoes negociadas na B3.
- `apikey`: autentica a requisicao.

A resposta JSON inclui precos mensais, preco ajustado, volume e dividendos,
que sao usados pela regra de analise da aplicacao. A chave gratuita padrao
permite 25 requisicoes por dia; os [termos de uso](https://www.alphavantage.co/terms_of_service/)
da Alpha Vantage se aplicam ao consumo dos dados. A chave nao deve ser exposta
em repositorios publicos.

## Docker

Este componente possui um `Dockerfile` proprio. Execute os comandos abaixo no
PowerShell dentro da pasta `BackEnd`.

### Build da imagem

```powershell
docker build -t puc-rio-backend .
```

### Execucao do container

```powershell
docker run --name puc-rio-backend-container -p 5000:5000 -v "${PWD}\database:/app/database" puc-rio-backend
```

O mapeamento `5000:5000` publica a API em `http://127.0.0.1:5000` e o Swagger
em `http://127.0.0.1:5000/swagger`.

O volume `-v "${PWD}\database:/app/database"` conecta a pasta
`BackEnd/database` ao SQLite do container. Assim, as alteracoes feitas pela
API permanecem no arquivo `database.db` local mesmo se o container for
removido e criado novamente.

API desenvolvida em Flask para análise de ações com base em dados do Alpha Vantage. Realiza a análise de dividendo de ações brasileiras, executa a persistência dos resultados em banco de dados SQLite e disponibiliza as informações para consumo via Front-End.

A API também conta com documentação interativa utilizando Swagger (OpenAPI 3).

---

## 🚀 Funcionalidades

- Analisar um ticker e registrar o resultado da análise de compra (`SIM` ou `NAO`)
- Consultar ações analisadas por status (`SIM` / `NAO`)
- Consultar análises de um ticker específico
- Editar a recomendação de uma ação analisada (`SIM` ou `NAO`)
- Remover análises do banco de dados
- Documentação da API via Swagger UI (OpenAPI 3)

---

## 🛠️ Tecnologias Utilizadas

- Python 3.11.4
- Flask
- Flask-CORS
- SQLAlchemy
- SQLite
- Requests
- Swagger UI (OpenAPI 3)

---

## 📋 Pré-requisitos

- Python 3.11.4 instalado
- Git (opcional, para clonar o repositório)

---

## ⚙️ Instalação e Execução (Ambiente Local)

### 1️⃣ Clonar o repositório
```bash
git clone <URL_DO_REPOSITORIO_BACKEND>
cd <PASTA_DO_BACKEND>
```

### 2️⃣ Criar e ativar ambiente virtual

**Windows (PowerShell):**
```bash
python -m venv venv
.\venv\Scripts\activate
```

**Windows (CMD):**
```bash
python -m venv venv
venv\Scripts\activate
```

### 3️⃣ Instalar dependências
```bash
pip install -r requirements.txt
```

### 4️⃣ Executar a aplicação
```bash
flask --app app/routes run
```

A API estará disponível em:
```
http://localhost:5000
```

---

## 📑 Documentação da API (Swagger)

- Swagger UI:  
  http://localhost:5000/swagger

- Arquivo OpenAPI (YAML):  
  http://localhost:5000/static/openapi.yaml

---

## 🔗 Principais Rotas

- `POST /analisar/{ticker}`  
- `GET /acoes_viaveis/{comprar}`  
- `GET /acoes_especifica/{ticker}`  
- `PUT /editar_acoes_especifica/{ticker}`  
- `DELETE /delete/{ticker}`  

### Exemplo de edição

O corpo JSON da requisição `PUT /editar_acoes_especifica/{ticker}` deve informar a nova recomendação:

```json
{
  "analise": "SIM"
}
```

O campo `analise` aceita os valores `SIM` e `NAO`.

---

## ℹ️ Observações

- A análise utiliza dados da API Alpha Vantage, que pode possuir limite de requisições.
- O banco de dados utilizado é SQLite, armazenado localmente.
- O ambiente virtual (`venv`) não deve ser versionado no repositório.
