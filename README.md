# 🏥 Sistema de Gestão Hospitalar e Inteligência de Dados (SQL)

![Status](https://shields.io)
![SGBD](https://shields.io)
![Área](https://shields.io)

## 📌 Contextualização

A gestão hospitalar moderna enfrenta o desafio diário de equilibrar a qualidade assistencial com a sustentabilidade financeira. Hospitais e clínicas geram um volume massivo de dados operacionais a cada atendimento, internação e prescrição médica. Sem ferramentas adequadas de análise, essas informações ficam isoladas e perdem valor estratégico.

Este projeto simula um ambiente real de **Business Intelligence (BI) e Análise de Dados** aplicado à saúde. Ele resolve 10 situações-problema críticas levantadas por setores estratégicos de um hospital — como **Financeiro, Farmácia, Faturamento, Infraestrutura, Auditoria e Regulação** — transformando registros brutos em indicadores de desempenho (KPIs) para tomada de decisão.

---

## 🎯 Objetivo Educacional

Este repositório foi estruturado com foco prático e didático para consolidar competências essenciais em bancos de dados relacionais:

* **Modelagem Relacional:** Estruturação de entidades com integridade referencial (`PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `DEFAULT`) utilizando o motor `InnoDB` no MySQL.
* **Tradução de Regras de Negócio:** Converter solicitações de áreas não técnicas em consultas SQL precisas.
* **Relacionamento de Entidades:** Aplicar junções (`INNER JOIN`) entre múltiplas tabelas normalizadas.
* **Análise Estatística e Agregação:** Utilizar funções de agrupamento (`SUM`, `AVG`, `COUNT`, `ROUND`) para cálculo de ticket médio, faturamento e volumetria.
* **Segmentação de Dados:** Dominar filtros avançados utilizando operadores lógicos (`OR`), listas (`IN`) e busca por padrões textuais (`LIKE` com wildcards `%`).

---

## ⚙️ Como Funciona na Prática

O fluxo de dados do sistema reflete a jornada completa do paciente dentro da unidade hospitalar:

1. **Cadastro e Convênio (`pacientes`):** O paciente dá entrada no sistema com seus dados pessoais (`nome_paciente`, `cpf`, `data_nascimento`, `telefone_paciente`) e vínculo de plano de saúde (`convenio`).
2. **Atendimento Clínico (`consultas` e `medicos`):** O paciente passa por consulta (`data_consulta`, `status_consulta`, `valor_consulta`) com um especialista registrado (`nome_medico`, `crm`, `especialidade`).
3. **Conduta Terapêutica (`prescricoes`):** Caso necessário, o médico emite receitas vinculadas à consulta contendo `medicamento`, `dosagem` e `quantidade`.
4. **Internação Hospitalar (`internacoes` e `quartos`):** Em casos de maior complexidade, o paciente é alocado em um leito específico (`numero_quarto`, `andar`, `tipo_quarto`, `status_quarto`), registrando `data_entrada`, `data_saida` e `valor_diaria`.

---

## 🗄️ Modelagem do Banco de Dados (DER)

O diagrama abaixo ilustra os relacionamentos entre as **6 tabelas** do sistema no MySQL Workbench:

![Diagrama Entidade-Relacionamento (DER)](./imagens/der-gestao-hospitalar.png<img width="1203" height="737" alt="Captura de tela 2026-11-19 112324" src="https://github.com/user-attachments/assets/a5d91143-0973-43b4-89e5-4ef9c28baa67" />
)
*(Nota: Salve a primeira imagem como `der-gestao-hospitalar.png` dentro de uma pasta `imagens` no seu repositório).*

---

## 🛠️ Como Criar e Configurar o Banco de Dados (DDL)

O banco foi desenvolvido para **MySQL** utilizando `ENGINE=InnoDB` para garantir integridade referencial. Execute o script completo abaixo para criar toda a estrutura:

```sql
CREATE DATABASE IF NOT EXISTS gestao_hospitalar;
USE gestao_hospitalar;

-- 1. Tabela de Pacientes
CREATE TABLE IF NOT EXISTS pacientes (
    id_paciente INT AUTO_INCREMENT PRIMARY KEY,
    nome_paciente VARCHAR(100) NOT NULL,
    cpf CHAR(14) NOT NULL UNIQUE,
    data_nascimento DATE NOT NULL,
    telefone_paciente CHAR(15),
    convenio CHAR(50) NOT NULL
) ENGINE=InnoDB;

-- 2. Tabela de Médicos
CREATE TABLE IF NOT EXISTS medicos (
    id_medico INT AUTO_INCREMENT PRIMARY KEY,
    nome_medico CHAR(100) NOT NULL,
    crm CHAR(12) NOT NULL UNIQUE,
    especialidade CHAR(50) NOT NULL
) ENGINE=InnoDB;

-- 3. Tabela de Quartos
CREATE TABLE IF NOT EXISTS quartos (
    id_quarto INT AUTO_INCREMENT PRIMARY KEY,
    numero_quarto CHAR(3) NOT NULL UNIQUE,
    andar CHAR(3) NOT NULL,
    tipo_quarto CHAR(30) NOT NULL,
    status_quarto CHAR(20) NOT NULL DEFAULT 'Livre'
) ENGINE=InnoDB;

-- 4. Tabela de Consultas
CREATE TABLE IF NOT EXISTS consultas (
    id_consulta INT AUTO_INCREMENT PRIMARY KEY,
    data_consulta DATETIME NOT NULL,
    status_consulta CHAR(20) NOT NULL,
    valor_consulta DECIMAL(10, 2) NOT NULL,
    id_paciente INT NOT NULL,
    id_medico INT NOT NULL,
    FOREIGN KEY (id_paciente) REFERENCES pacientes(id_paciente),
    FOREIGN KEY (id_medico) REFERENCES medicos(id_medico)
) ENGINE=InnoDB;

-- 5. Tabela de Internações
CREATE TABLE IF NOT EXISTS internacoes (
    id_internacao INT AUTO_INCREMENT PRIMARY KEY,
    id_paciente INT NOT NULL,
    id_quarto INT NOT NULL,
    data_entrada DATE NOT NULL,
    data_saida DATE,
    valor_diaria DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (id_paciente) REFERENCES pacientes(id_paciente),
    FOREIGN KEY (id_quarto) REFERENCES quartos(id_quarto)
) ENGINE=InnoDB;

-- 6. Tabela de Prescrições
CREATE TABLE IF NOT EXISTS prescricoes (
    id_prescricao INT AUTO_INCREMENT PRIMARY KEY,
    id_consulta INT NOT NULL,
    medicamento CHAR(100) NOT NULL,
    dosagem CHAR(50) NOT NULL,
    quantidade CHAR(30) NOT NULL,
    FOREIGN KEY (id_consulta) REFERENCES consultas(id_consulta)
) ENGINE=InnoDB;
```

### 📸 Evidências da Criação das Tabelas (MySQL Workbench)

<details>
<summary>Clique para visualizar os prints dos scripts DDL</summary>

* **Tabela `pacientes`:**
  ![Script Tabela Pacientes](./imagens/create-pacientes.png)

* **Tabela `medicos`:**
  ![Script Tabela Médicos](./imagens/create-medicos.png)

* **Tabela `quartos`:**
  ![Script Tabela Quartos](./imagens/create-quartos.png)

* **Tabela `prescricoes`:**
  ![Script Tabela Prescrições](./imagens/create-prescricoes.png)

</details>

---

## 📊 Consultas SQL e Regras de Negócio

Cada script deste repositório responde a uma demanda específica da operação hospitalar:

### 1. Faturamento Total de Consultas por Convênio
* **Setor Solicitante:** Financeiro.
* **Regra de Negócio:** Somar o valor arrecadado em consultas para operadoras que possuem `"Saúde"` no nome (com alternativa comentada para busca exata via `= 'Unimed'`).
* **Recursos SQL:** `INNER JOIN`, `SUM()`, `LIKE '%Saúde%'`, `GROUP BY`.

![Exemplo Consulta 01](./imagens/consulta-01.png)

### 2. Média do Valor de Consulta por Especialidade Médica
* **Setor Solicitante:** Coordenação Clínica.
* **Regra de Negócio:** Calcular o ticket médio das consultas realizadas especificamente em especialidades ligadas à Cardiologia (`LIKE '%Cardio%'`).
* **Recursos SQL:** `INNER JOIN`, `ROUND(AVG(), 2)`, `LIKE`, `GROUP BY`.

### 3. Valor Médio das Diárias por Tipo de Quarto
* **Setor Solicitante:** Gerência de Infraestrutura Hospitalar.
* **Regra de Negócio:** Analisar o custo médio das diárias praticadas em internações nas acomodações `"Apartamento"` ou `"UTI"`.
* **Recursos SQL:** `INNER JOIN`, `ROUND(AVG(), 2)`, operador lógico `OR` combinado com `LIKE`.

### 4. Total de Consultas Realizadas por Médico
* **Setor Solicitante:** Diretoria de Atendimento.
* **Regra de Negócio:** Listar o faturamento acumulado por profissional considerando exclusivamente consultas com status `"Realizada"` ou `"Finalizada"`, ordenando do maior para o menor valor.
* **Recursos SQL:** `INNER JOIN`, `SUM()`, operador `IN`, `GROUP BY`, `ORDER BY DESC`.

### 5. Soma de Diárias por Paciente com Sobrenome "Silva"
* **Setor Solicitante:** Setor de Cobrança.
* **Regra de Negócio:** Consolidar o valor total de diárias de internação acumuladas por pacientes que possuem `"Silva"` no nome.
* **Recursos SQL:** `INNER JOIN`, `SUM()`, `LIKE '%Silva%'`, `GROUP BY`, `ORDER BY DESC`.

### 6. Contagem de Prescrições por Status da Consulta
* **Setor Solicitante:** Farmácia Hospitalar.
* **Regra de Negócio:** Contabilizar o volume de medicamentos prescritos agrupados pelo status da consulta, filtrando apenas receitas contendo `"Dipirona"` ou `"Paracetamol"`.
* **Recursos SQL:** `INNER JOIN`, `COUNT()`, `LIKE` com `OR`, `GROUP BY`, `ORDER BY DESC`.

### 7. Média de Valor de Consultas por Convênio e Especialidade
* **Setor Solicitante:** Auditoria e Negociação Contratual.
* **Regra de Negócio:** Cruzar dados de três tabelas para calcular o valor médio pago por consulta nas redes `"Unimed"`, segmentado por especialidade médica.
* **Recursos SQL:** Duplo `INNER JOIN`, `ROUND(AVG(), 2)`, `LIKE '%Unimed%'`, ordenação composta (`ASC` e `DESC`).

### 8. Média de Diária de Internação por Convênio em Quartos de UTI
* **Setor Solicitante:** Regulação Hospitalar.
* **Regra de Negócio:** Calcular o custo médio diário de internação por operadora de saúde considerando unicamente leitos classificados exatamente como `"UTI"`.
* **Recursos SQL:** Duplo `INNER JOIN`, `ROUND(AVG(), 2)`, filtro exato (`= 'UTI'`), `GROUP BY`, `ORDER BY DESC`.

### 9. Medicamentos Prescritos por Médico em Consultas Realizadas
* **Setor Solicitante:** Comissão de Farmácia e Terapêutica.
* **Regra de Negócio:** Medir a quantidade total de itens prescritos por cada médico nas consultas efetivamente concluídas (`status_consulta = 'Realizada'`).
* **Recursos SQL:** Duplo `INNER JOIN`, `COUNT()`, `GROUP BY`, `ORDER BY DESC`.

### 10. Valor Total e Média de Consultas por Paciente
* **Setor Solicitante:** Atendimento ao Cliente.
* **Regra de Negócio:** Gerar um extrato financeiro contendo o gasto total e a média por atendimento para pacientes cujos nomes iniciam com a letra `"M"`.
* **Recursos SQL:** `INNER JOIN`, `SUM()`, `ROUND(AVG(), 2)`, `LIKE 'M%'`, `GROUP BY`, `ORDER BY DESC`.

---

## 🚀 Como Usar

1. Clone este repositório para sua máquina local:
   ```bash
2. Crie uma pasta chamada imagens na raiz do projeto e salve as capturas de tela com os nomes indicados no Markdown acima.
3. Abra o MySQL Workbench (ou outro cliente MySQL de sua preferência).
4. Execute o script DDL da seção Como Criar e Configurar o Banco de Dados.
5. Popule as tabelas com dados fictícios (`INSERT INTO`) e execute as consultas de `01` a `10` para visualizar os relatórios.

👨‍💻 Autor

Desenvolvido por [Kauê Ferreira Gonçalves de Abreu]
• GitHub: kaue-ferreira652
• E-mail: 10kaue981@gmail.com

📄 Licença

Este projeto foi desenvolvido para fins educacionais e de portfólio, estando livre para estudo, adaptação e compartilhamento.
