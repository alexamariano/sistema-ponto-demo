# ⏱️ System Tracker & Financial Reconciliation API

> Aplicação web full-stack e autônoma para registro de ponto, conciliação matemática automatizada e disparo diário de relatórios financeiros em nuvem.

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.0-000000?style=for-the-badge&logo=flask&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Supabase-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Render](https://img.shields.io/badge/Render-Cloud-46E3B7?style=for-the-badge&logo=render&logoColor=white)

---

## 🎯 Contexto & Problema de Negócio

Em operações com múltiplos projetos e equipes alocadas em campo ou regime híbrido, o controle manual de jornadas via planilhas descentralizadas gera:
- **Inconsistências matemáticas** no cálculo de horas trabalhadas e diárias.
- **Atrasos na consolidação financeira** para fechamento de folha de pagamento e prestação de contas com clientes.
- **Gargalos operacionais** causados por retrabalho do RH e da gestão.

Esta solução foi desenvolvida para solucionar essa dor, centralizando o registro via interface web e automatizando todo o pipeline de processamento de dados e envio de relatórios.

---

## 🏗️ Arquitetura do Sistema

```text
[ Colaborador / Celular ] ──(HTTPS)──> [ Flask Web App @ Render ]
                                               │
                                       (Persistência SQL)
                                               ▼
                                    [ Supabase PostgreSQL ]
                                               │
                                  (Fechamento Agendado / Cron)
                                               ▼
                                  [ Engine Pandas + openpyxl ]
                                               │
                                     (API Resend - SSL/443)
                                               ▼
                                  [ E-mail do Gestor (.xlsx) ]
```

---

## ✨ Principais Funcionalidades

1. **Interface de Marcação Simplificada (`/`):**
   - Registro instantâneo de **INÍCIO** e **FIM** de jornada associado ao ID do funcionário, projeto e atividade.
   - Espelho de ponto em tempo real para consulta do próprio colaborador.

2. **Auditoria em Tempo Real (`/admin`):**
   - Dashboard administrativo para acompanhamento ao vivo de todas as batidas armazenadas na nuvem.

3. **Motor Agendado de Fechamento (`APScheduler`):**
   - Execução diária automática às 18:30 (Fuso de Brasília).
   - Leitura do banco PostgreSQL, separação de jornadas diurnas/noturnas, cálculo de horas exatas e aplicação das taxas/diárias específicas de cada contrato.

4. **Fechamento On-Demand (`/api/fechar_agora`):**
   - Rota de controle para antecipação imediata da planilha consolidada antes do horário agendado.

5. **Envio de Relatório via API (`Resend HTTP API`):**
   - Geração e anexação automática da planilha Excel formatada (`.xlsx`) com sumário financeiro por projeto e envio direto para a diretoria.

---

## 🛠️ Tech Stack

- **Linguagem:** Python 3.10+
- **Framework Web:** Flask / Gunicorn
- **Banco de Dados:** PostgreSQL (Supabase) com suporte local a SQLite3
- **Manipulação de Dados & Excel:** Pandas / openpyxl
- **Automação & Agendamento:** APScheduler
- **Envio de E-mail:** Resend HTTP API
- **Hospedagem & Nuvem:** Render Platform

---

## 🚀 Como Executar o Projeto Localmente

### Pré-requisitos
- Python 3.10 ou superior instalado
- Git instalado

### Passo a Passo

1. **Clonar o repositório:**
   ```bash
   git clone [https://github.com/alexamariano/sistema-ponto-demo.git](https://github.com/alexamariano/sistema-ponto-demo.git)
   cd sistema-ponto-demo
   ```

2. **Criar e ativar o ambiente virtual:**
   ```bash
   python -m venv venv
   # No Windows:
   venv\Scripts\activate
   # No Linux/Mac:
   source venv/bin/activate
   ```

3. **Instalar dependências:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configurar variáveis de ambiente:**
   Crie um arquivo `.env` baseado no `.env.example`:
   ```env
   RESEND_API_KEY=sua_chave_resend
   EMAIL_DESTINATARIO=seu_email@exemplo.com
   ```

5. **Executar a aplicação:**
   ```bash
   python app.py
   ```
   Acesse no seu navegador: `http://localhost:5000`

---

## 🔒 Segurança e LGPD

Os dados exibidos neste repositório e na versão de demonstração (nomes de funcionários, tabelas de custos e códigos de projetos) foram **100% anonimizados** para preservação de sigilo corporativo e conformidade com a LGPD.
