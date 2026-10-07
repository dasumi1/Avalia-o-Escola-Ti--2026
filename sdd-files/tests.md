| **#** | **Cenário** | **Tipo** |
|---|---|---|
| T1 | Abrir bilhete com placa válida retorna 201, id, entrada e status "aberto" | feliz |
| T2 | Abrir bilhete com entrada fora do padrão ISO-8601 retorna 422 `{"erro": "entrada_invalida"}` | borda |
| T3 | Encerrar bilhete aberto após 30 minutos retorna 200 e valor de 300 centavos | feliz |
| T4 | Encerrar bilhete já encerrado retorna 409 `{"erro": "bilhete_ja_encerrado"}` | borda |
| T5 | Listar bilhetes ativos retorna 200 com os bilhetes abertos, mais recentes primeiro | feliz |
| T6 | Listar bilhetes ativos quando não existem bilhetes abertos retorna 200 e array vazio | borda |
| T7 | Gerar relatório diário com bilhetes encerrados retorna 200 com total, faturamento e tempo médio | feliz |
| T8 | Consultar relatório diário com data inválida retorna 422 `{"erro": "data_invalida"}` | borda |
| T9 | Cancelar bilhete aberto retorna 200 com status "cancelado", sem cobrança | feliz |
| T10 | Cancelar bilhete já encerrado retorna 409 `{"erro": "bilhete_nao_aberto"}` | borda |
| T11 | Consultar histórico de placa cadastrada retorna 200 com todos os bilhetes, mais recentes primeiro | feliz |
| T12 | Consultar histórico de placa sem registros retorna 200 e array vazio | borda |
| T13 | Encerrar bilhete com permanência dentro da tolerância retorna valor de 0 centavos | feliz |
| T14 | Encerrar bilhete 1 minuto acima da tolerância cobra uma fração integral de 300 centavos, considerando tolerância de 0 minutos | borda |
| T15 | Abrir novo bilhete após encerrar o anterior da mesma placa retorna 201 | feliz |
| T16 | Abrir bilhete para placa que já possui bilhete aberto retorna 409 `{"erro": "bilhete_em_aberto"}` | borda |
| T17 | Gerar relatório por período retorna 200 com total, faturamento e tempo médio dos bilhetes encerrados no intervalo | feliz |
| T18 | Gerar relatório por período sem bilhetes encerrados retorna 200 com totais zerados | borda |
| T19 | Verificar placa com bilhete aberto retorna 200, `possui_bilhete_aberto: true` e `bilhete_id` | feliz |
| T20 | Verificar placa sem bilhete aberto retorna 200 com `possui_bilhete_aberto: false` | borda |
| T21 | Abrir bilhete com placa válida `ABC1D23` retorna 201 | feliz |
| T22 | Abrir bilhete com placa de tamanho inválido retorna 422 `{"erro": "placa_invalida"}` | borda |