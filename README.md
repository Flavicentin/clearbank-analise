# ClearBank - Análise Financeira com Python

## Descrição do Projeto

Este projeto foi desenvolvido como desafio final do módulo de Python aplicado à análise de dados. O notebook simula o cenário de uma fintech (ClearBank) que precisa processar mensalmente um arquivo CSV de transações bancárias contendo dados inconsistentes — campos vazios, valores inválidos, datas mal formatadas e registros duplicados.

O script lê o arquivo `transacoes.csv` com o módulo nativo `csv` (sem uso de pandas), valida e limpa cada linha, agrupa as transações válidas por mês, calcula métricas financeiras, identifica o período analisado, sinaliza movimentações suspeitas (valores acima de R$ 10.000,00) e exporta o resultado consolidado em um arquivo JSON, além de exibir um relatório formatado no terminal em padrão monetário brasileiro (R$ 1.234,56).

## Como Executar

1. Abra o notebook `desafio-final.ipynb` no [Google Colab](https://colab.research.google.com/) ou no Jupyter Notebook local (Python 3.10+).
2. Certifique-se de que o arquivo `transacoes.csv` está no mesmo diretório do notebook (ou no ambiente do Colab).
3. Execute todas as células em ordem, de cima para baixo (`Ambiente de execução → Executar tudo` no Colab, ou `Run All` no Jupyter).
4. Ao final da execução, o relatório será exibido no terminal e o arquivo `relatorio.json` será gerado no mesmo diretório.

Nenhuma biblioteca externa é necessária — o projeto usa apenas módulos nativos do Python (`csv`, `json`, `datetime`).

## O que o Notebook Gera como Saída

- **Resumo da validação**, impresso logo após a leitura do CSV: total de linhas lidas, linhas válidas e linhas inválidas.

- **Relatório formatado no terminal** (`exibir_relatorio`), contendo:
  - Total de transações válidas e inválidas.
  - Período analisado (data mais antiga → data mais recente, com a quantidade de dias entre elas).
  - Resumo mensal, para cada mês presente nos dados: quantidade de transações, total de crédito, total de débito, saldo, valor médio, maior e menor valor.
  - Lista de transações suspeitas (valor acima de R$ 10.000,00), com id, cliente, data e valor — ou o aviso "Nenhuma transação suspeita encontrada." caso não haja nenhuma.

- **Arquivo `relatorio.json`** (`salvar_json`), com a estrutura:

```json
{
  "gerado_em": "2026-08-10 00:42:35",
  "total_transacoes_validas": 45,
  "total_transacoes_invalidas": 5,
  "periodo_analisado": {
    "data_mais_antiga": "2026-01-05",
    "data_mais_recente": "2026-05-30",
    "dias_entre": 145
  },
  "resumo_mensal": {
    "2026-01": {
      "quantidade": 9,
      "total_credito": 5778.39,
      "total_debito": 5413.35,
      "saldo": 365.04,
      "media": 1243.53,
      "maior_valor": 2271.41,
      "menor_valor": 179.32
    }
  },
  "transacoes_suspeitas": [
    {
      "id": 12,
      "cliente_id": "CLI005",
      "data": "2026-02-08",
      "valor": 15000.0
    }
  ]
}
```

## Estrutura do Código

O notebook está organizado em funções com responsabilidades separadas:

| Função | Responsabilidade |
|---|---|
| `ler_transacoes()` | Lê o CSV com `csv.DictReader`, valida cada linha e retorna a lista de transações válidas junto com o resumo da leitura (total, válidas, inválidas). Trata `FileNotFoundError`. |
| `valida_id`, `valida_cliente_id`, `valida_data`, `valida_tipo`, `valida_valor` | Funções individuais de validação por campo, cada uma retornando o valor já convertido (ou `False` se inválido). |
| `validar_transacao()` | Aplica todas as regras de `VALIDA_REGRAS` a uma linha e retorna o registro limpo (ou `None`, se qualquer campo for inválido). |
| `gerar_relatorio()` | Calcula o período analisado, agrupa as transações por mês, calcula as métricas financeiras e identifica transações suspeitas (`LIMITE_SUSPEITO = 10000`). |
| `exibir_relatorio()` / `formatar_moeda()` | Formatam e imprimem o relatório no terminal, com valores monetários no padrão brasileiro. |
| `salvar_json()` | Salva o resultado da análise em `relatorio.json`. |

## Observações

- Os requisitos opcionais (análise com pandas e visualização com matplotlib) não foram implementados nesta versão.