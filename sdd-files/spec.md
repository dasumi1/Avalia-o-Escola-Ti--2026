# Contrato da API

- **Base URL:** `http://localhost:8005`
- **Porta obrigatória:** `8005`

## UC1 — Abrir Bilhete

**Endpoint:** `POST /bilhetes`

**Body:**
```json
{
  "placa": "ABC1D23"
}
```

A placa deve conter 7 caracteres alfanuméricos, em maiúsculas.

**Resposta — 201:**
```json
{
  "id": 1,
  "placa": "ABC1D23",
  "entrada": "<ISO-8601 com fuso -03:00>",
  "status": "aberto"
}
```

**Regras:**
- O body aceita o campo `entrada` opcional, no formato ISO-8601 com fuso horário.
- Quando informado, o bilhete deve ser aberto no instante especificado, em vez do horário atual.
- Esse campo permite testar frações e teto de cobrança sem depender da passagem do tempo real.
- Formato de entrada inválido retorna `422`:

```json
{
  "erro": "entrada_invalida"
}
```

## UC2 — Encerrar Bilhete

**Endpoint:** `POST /bilhetes/{id}/encerramento`

**Resposta — 200:**
```json
{
  "id": 1,
  "placa": "ABC1D23",
  "entrada": "...",
  "saida": "...",
  "minutos": 95,
  "valor_centavos": 1250
}
```

### Regras de Tarifação

- A cobrança ocorre por frações de `FRACAO_MINUTOS`, arredondando para cima.
- Uma fração exata corresponde à cobrança de uma fração.
- Um minuto adicional inicia a cobrança da próxima fração.
- A tarifa de uma hora completa corresponde a `TARIFA_HORA_CENTAVOS`.
- O valor de cada fração é calculado por:

  `TARIFA_HORA_CENTAVOS ÷ (60 ÷ FRACAO_MINUTOS)`

- Cobrar **300 centavos por fração de 30 minutos**.
- Aplicar o teto de `TETO_DIARIO_CENTAVOS`.
- Cobrar no máximo **7000 centavos por bilhete**.
- Qualquer permanência positiva gera cobrança.
- Os valores monetários devem ser representados em centavos inteiros.
- A API nunca deve retornar valores monetários em ponto flutuante.

## UC3 — Listar Bilhetes Ativos

**Endpoint:** `GET /bilhetes/ativos`

**Resposta — 200:**

Retornar um array contendo todos os bilhetes com status `aberto`, ordenados dos mais recentes para os mais antigos.

## UC4 — Relatório Diário

**Endpoint:** `GET /relatorios/diario?data=AAAA-MM-DD`

**Resposta — 200:**
```json
{
  "data": "2026-10-05",
  "total_bilhetes": 12,
  "faturamento_centavos": 8400,
  "tempo_medio_minutos": 47
}
```

**Regras:**
- Considerar apenas os bilhetes encerrados no dia informado.
- `tempo_medio_minutos` deve considerar apenas os bilhetes encerrados no dia.
- Arredondar a média de minutos com a regra de **0,5 para cima**.
- Somar os valores cobrados dos bilhetes encerrados.
- Respeitar a tarifa de **300 centavos por fração de 30 minutos**.
- Respeitar o teto de **7000 centavos por bilhete**.

## UC5 — Cancelar Bilhete

**Endpoint:** `POST /bilhetes/{id}/cancelamento`

**Resposta — 200:**

Retornar o bilhete com status `cancelado`.

**Regras:**
- Apenas bilhetes com status `aberto` podem ser cancelados.
- O cancelamento não gera cobrança.
- Não gerar os campos `saida` nem `valor_centavos`.

## UC6 — Histórico por Placa

**Endpoint:** `GET /bilhetes?placa=ABC1D23`

**Resposta — 200:**

Retornar um array com todos os bilhetes associados à placa, independentemente do status.

**Regras:**
- Ordenar os bilhetes dos mais recentes para os mais antigos.
- Caso a placa nunca tenha estacionado, retornar um array vazio.

## UC7 — Tolerância Gratuita

**Regras:**
- Os primeiros `TOLERANCIA_MINUTOS` de um bilhete são gratuitos.
- Se a duração for menor ou igual à tolerância, retornar `valor_centavos: 0`.
- Se a duração ultrapassar a tolerância, mesmo por apenas um minuto, cobrar o valor integral desde o primeiro minuto.
- O tempo de tolerância não deve ser descontado da duração utilizada no cálculo.

## UC8 — Uma Vaga por Placa

**Endpoint:** `POST /bilhetes`

**Regra:**

Uma placa não pode possuir mais de um bilhete aberto simultaneamente.

**Resposta em caso de conflito — 409:**
```json
{
  "erro": "bilhete_em_aberto"
}
```

Após o encerramento ou cancelamento do bilhete, a placa poderá abrir um novo bilhete.

## UC9 — Relatório por Período

**Endpoint:** `GET /relatorios/periodo?inicio=2026-10-01&fim=2026-10-07`

**Resposta — 200:**
```json
{
  "inicio": "2026-10-01",
  "fim": "2026-10-07",
  "total_bilhetes": 50,
  "faturamento_centavos": 35000,
  "tempo_medio_minutos": 62
}
```

**Regras:**
- Considerar a data de encerramento dos bilhetes, e não a data de abertura.
- Calcular o total de bilhetes encerrados no período informado.
- Somar os valores cobrados pelos bilhetes encerrados no período.
- Calcular o tempo médio de permanência dos bilhetes encerrados no período.
- Respeitar a tarifa de **300 centavos por fração de 30 minutos**.
- Respeitar o teto de **7000 centavos por bilhete**.

## UC10 — Verificar Bilhete Aberto por Placa

**Endpoint:** `GET /bilhetes/ativos/ABC1D23`

**Resposta — 200:**
```json
{
  "placa": "ABC1D23",
  "possui_bilhete_aberto": true,
  "bilhete_id": 1
}
```

**Regras:**
- Verificar se a placa informada possui um bilhete ativo.
- Considerar ativo apenas o bilhete com status `aberto`.

## UC11 — Validar Placa

**Regras:**
- A placa deve possuir exatamente **7 caracteres**.
- Todos os caracteres devem ser alfanuméricos.
- As letras devem estar em maiúsculas.
- Rejeitar placas com tamanho ou caracteres inválidos.

**Resposta para placa inválida — 422:**
```json
{
  "erro": "placa_invalida"
}
```