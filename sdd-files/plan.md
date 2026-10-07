# Arquitetura e Decisões

## Stack Tecnológica

- **Linguagem:** Python 3.11.
  - Escolhida pela simplicidade, legibilidade e facilidade de manutenção do código.

- **Framework:** FastAPI.
  - Facilita a criação de APIs REST, a validação de dados e a implementação de testes para as rotas.

- **Servidor:** Uvicorn.
  - Escolhido pela integração com FastAPI, simplicidade de configuração e suporte à execução assíncrona.

- **Banco de dados:** SQLite.
  - Banco de dados relacional leve, de fácil utilização e que não exige um servidor separado, simplificando a persistência dos dados.

- **Testes automatizados:** pytest.
  - Ferramenta integrada ao ecossistema Python, que facilita a criação, organização e execução de testes automatizados.

## Estrutura de Arquivos a Gerar

- **`main.py`:** configuração do FastAPI e definição das rotas da API.
- **`models.py`:** definição das classes Bilhete e Relatório.
- **`service.py`:** implementação das regras de negócio, incluindo cálculo de tarifas, abertura e cancelamento de bilhetes e validação de placas.
- **`banco.py`:** gerenciamento da conexão e persistência dos dados no SQLite.
- **`testes.py`:** testes automatizados utilizando pytest.
