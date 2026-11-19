# 🏥 Análise de Dados e Gestão Hospitalar (SQL)

Este repositório contém um conjunto de 10 consultas SQL desenvolvidas para resolver problemas reais de negócio e apoiar a tomada de decisão em diferentes setores de um ambiente hospitalar, como financeiro, farmácia, faturamento e regulação.

---

## 📋 Estrutura do Banco de Dados

As consultas operam sobre um modelo relacional hospitalar composto pelas seguintes entidades principais:

* **`pacientes`**: Dados cadastrais e informações de convênio (`id_paciente`, `nome_paciente`, `convenio`).
* **`medicos`**: Registro do corpo clínico e suas áreas de atuação (`id_medico`, `nome_medico`, `especialidade`).
* **`quartos`**: Classificação das acomodações hospitalares (`id_quarto`, `tipo_quarto`).
* **`consultas`**: Histórico de atendimentos, valores e status (`id_consulta`, `id_paciente`, `id_medico`, `valor_consulta`, `status_consulta`).
* **`internacoes`**: Registro de ocupação de leitos e custos diários (`id_paciente`, `id_quarto`, `valor_diaria`).
* **`prescricoes`**: Controle de medicamentos receitados por consulta (`id_prescricao`, `id_consulta`, `medicamento`).

---

## 📊 Consultas e Regras de Negócio

Abaixo está o detalhamento de cada cenário analisado nos scripts:

| # | Título da Consulta | Setor Solicitante | Objetivo e Filtros Aplicados |
| :--- | :--- | :--- | :--- |
| **01** | **Faturamento por Convênio** | Financeiro | Soma o valor total arrecadado em consultas para convênios que contêm `"Saúde"` no nome. |
| **02** | **Média por Especialidade** | Coordenação Clínica | Calcula o ticket médio das consultas voltadas à área de `"Cardio"` (Cardiologia). |
| **03** | **Média de Diárias por Quarto** | Infraestrutura | Avalia o valor médio das diárias de internação em acomodações do tipo `"Apartamento"` ou `"UTI"`. |
| **04** | **Faturamento por Médico** | Diretoria de Atendimento | Lista o faturamento total por médico considerando apenas consultas com status `"Realizada"` ou `"Finalizada"`. |
| **05** | **Diárias por Sobrenome** | Cobrança | Identifica a soma acumulada das diárias de internação para pacientes com o sobrenome `"Silva"`. |
| **06** | **Prescrições por Status** | Farmácia Hospitalar | Conta o total de prescrições de `"Dipirona"` ou `"Paracetamol"` agrupadas pelo status da consulta. |
| **07** | **Média Convênio x Especialidade** | Auditoria | Cruza o valor médio das consultas por especialidade médica para os convênios da rede `"Unimed"`. |
| **08** | **Média de Diária UTI por Convênio** | Regulação | Calcula o custo médio diário de internação por convênio exclusivamente em leitos de `"UTI"`. |
| **09** | **Prescrições por Médico** | Comissão de Farmácia | Contabiliza o volume total de medicamentos prescritos por cada médico em consultas `"Realizadas"`. |
| **10** | **Extrato Financeiro de Pacientes** | Atendimento ao Cliente | Exibe o gasto total e a média por consulta de pacientes cujos nomes iniciam com a letra `"M"`. |

---

## 🛠️ Conceitos SQL Aplicados

* **Junções (`INNER JOIN`)**: Cruzamento de dados entre até três tabelas simultaneamente (ex.: `internacoes`, `pacientes` e `quartos`).
* **Funções de Agregação**: Uso de `SUM()`, `AVG()` e `COUNT()` para consolidação de métricas financeiras e operacionais.
* **Filtros Dinâmicos e Exatos**: Aplicação de `LIKE` (com wildcards `%`), `IN` e operadores lógicos (`OR`) para segmentação de registros.
* **Agrupamento e Ordenação**: Estruturação de relatórios gerenciais com `GROUP BY`, `ORDER BY` e arredondamento numérico com `ROUND()`.

---

## 🚀 Como Utilizar

1. Certifique-se de ter um SGBD relacional configurado (MySQL, PostgreSQL, SQL Server ou SQLite).
2. Crie as tabelas respeitando as chaves primárias (`id_*`) e estrangeiras mencionadas na estrutura.
3. Execute os scripts individualmente para gerar os relatórios gerenciais de cada área.
