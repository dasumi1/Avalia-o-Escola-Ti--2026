# Constitution — Regras Persistentes do Projeto

## 1. Idioma e Padronização
- Todo o código, identificadores, comentários e documentação devem estar em português.
- As respostas JSON devem seguir o padrão `camelCase`.

## 2. API REST
- A API deve seguir os princípios REST.
- As rotas devem utilizar recursos no plural, como `/bilhetes` e `/relatorios`.
- Utilizar métodos HTTP e códigos de status apropriados para cada operação.

## 3. Stack Tecnológica
- **Linguagem:** Python.
- **Framework:** FastAPI.
- **Servidor:** Uvicorn.
- **Banco de dados:** SQLite.
- **Testes automatizados:** pytest.
- Não utilizar dependências adicionais além das previstas na stack.

## 4. Valores Monetários
- Todos os valores monetários devem ser armazenados e calculados em centavos inteiros.
- Na apresentação, os valores monetários devem possuir duas casas decimais, seguindo o formato brasileiro, como `1.234,99`.
- Não utilizar números de ponto flutuante para cálculos financeiros.

## 5. Testes Automatizados
- Utilizar pytest para a execução dos testes.
- Cada regra de negócio deve possuir pelo menos um teste de borda.
- Priorizar o desenvolvimento orientado a testes (TDD).

## 6. Datas
- As datas devem seguir o padrão ISO 8601, no formato `AAAA-MM-DD`.
- Datas e horários dos bilhetes devem seguir o padrão ISO-8601 com fuso -03:00.

## 7. Conformidade com a Especificação
- A implementação deve respeitar os requisitos definidos no `spec.md`.
- Não implementar funcionalidades que não estejam previstas na especificação.