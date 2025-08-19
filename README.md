# Bardos & Bruxas — Sistema de Gestão de Motoclube

Aplicação para gerenciamento do **motoclube Bardos & Bruxas**, construída com **.NET 8**, **PostgreSQL**, **RabbitMQ** e **Angular 19 (PrimeNG)**.  
O objetivo é **aprender backend moderno** e ao mesmo tempo disponibilizar uma solução útil para o clube, cobrindo: membros, mensalidades, consumo no bar, eventos, fluxo de caixa e relatórios.

---

## 🎯 MVP (Primeira Entrega)

Objetivo: rodar algo útil rápido, cobrindo os pilares principais.

### **Membership**
- Cadastro de membros  
- Plano/mensalidade (valor fixo)  
- Comanda mensal (lançar consumo)  

### **POS/Bar**
- Cadastro de produtos  
- Venda vinculada ao membro (lança na comanda)  
- Estoque simples (diminui ao vender)  

### **Billing**
- Fechar mês: gera cobrança = mensalidade + consumo  
- Status: Aberta, Paga, Atrasada  

### **Conta**
- Receber eventos de pagamento e venda  
- Registrar entradas no fluxo de caixa  

---

## 📅 Planejamento em Sprints

### **Sprint 1 — Estrutura & Primeiros Endpoints (1-2 semanas)**
- Criar solution com módulos base (Api, Domain, Application, Infra)  
- Configurar Postgres + EF Core + Migrations  
- Entidades iniciais: `Membro`, `Plano`, `Produto`  
- Endpoints: CRUD Membro e Produto  
- Subir tudo no `docker-compose` (Api + Postgres)  

**Aprendizado:** controllers, EF Core, migrations, camadas  

---

### **Sprint 2 — Comandas & Vendas (1-2 semanas)**
- Criar `ComandaMensal`  
- Endpoint: lançar consumo de membro  
- Estoque: baixa produto ao vender  
- Fechamento mensal gera cobrança  

**Aprendizado:** regras de negócio, transações, testes de integração  

---

### **Sprint 3 — Billing & Conta (1-2 semanas)**
- Criar `Cobranca` (Billing)  
- Integrar pagamento (manual, sem gateway por enquanto)  
- RabbitMQ: publicar eventos `PagamentoEfetuado`, `VendaRegistrada`  
- Conta consome eventos → cria `Movimento`  

**Aprendizado:** RabbitMQ, background services, consistência  

---

### **Sprint 4 — Eventos (1-2 semanas)**
- Criar `Evento` (cadastro + datas)  
- Vendas vinculadas ao evento  
- Relatório P&L básico: receita – despesas cadastradas  

**Aprendizado:** multi-contextos, relatórios  

---

### **Sprint 5 — Extras**
- Empréstimo a membros (bloqueio se inadimplente)  
- Abertura/fechamento de caixa  
- Relatórios: inadimplência, consumo por produto, fluxo de caixa  
- CI/CD (GitHub Actions ou Azure DevOps → Azure)  

---

## 🔮 Futuro (fase 2)

- **Totem de autoatendimento / PDV app** → frontend consumindo os mesmos endpoints de venda  
- **Controle avançado de estoque**: entrada por compra, transferências para evento, ajuste por inventário  
- **Autenticação**: JWT + roles (Admin, Caixa, Membro)  
- **Observabilidade**: logs estruturados, métricas  

---

## 📖 Como estudar junto

Cada sprint foca em um tema de backend:

- **Sprint 1** → API + EF Core  
- **Sprint 2** → Domínio + Regras de Negócio  
- **Sprint 3** → Mensageria + Consistência  
- **Sprint 4** → Eventos + Relatórios  
- **Sprint 5** → Segurança + CI/CD  

📌 Ao final de cada sprint:  
- Documentar no README o que foi implementado e aprendido  
- Disponibilizar versão para teste com Docker Compose + Frontend demo  

---

## 🚀 Como rodar o projeto

### Pré-requisitos
- [.NET 8 SDK](https://dotnet.microsoft.com/)  
- [Node.js 20+](https://nodejs.org/)  
- [Angular CLI 19](https://angular.dev/)  
- [Docker](https://www.docker.com/)  

### Backend (API + DB + MQ)
1. Subir infraestrutura:
   ```bash
   docker compose up -d
   ```
   Isso sobe:
   - PostgreSQL (porta 5432)
   - RabbitMQ (porta 5672 + painel em http://localhost:15672)

2. Rodar a API:
   ```bash
   dotnet run --project BnB.Api
   ```
   API disponível em: [http://localhost:5000/swagger](http://localhost:5000/swagger)  

### Frontend (Angular + PrimeNG)
1. Instalar dependências:
   ```bash
   cd bnb-admin
   npm install
   ```

2. Rodar o app:
   ```bash
   ng serve --open
   ```
   App disponível em: [http://localhost:4200](http://localhost:4200)

---

## 📂 Estrutura (resumida)

```
BardosBruxas.sln
BnB.Api/                     -> Web API (Controllers, Swagger)
BnB.CrossCutting.Ioc/        -> Injeção de dependência, configs comuns
Modules/
  Membership/                -> Membros, planos, comandas
  Pos/                       -> Produtos, vendas, estoque
  Billing/                   -> Cobranças, pagamentos
  Conta/                     -> Fluxo de caixa
  Eventos/                   -> Cadastro e P&L de eventos
BnB.Tests/                   -> Testes de unidade
BnB.IntegrationTests/        -> Testes de integração
docker-compose.yml           -> Infra (Postgres + RabbitMQ)
bnb-admin/                   -> Frontend Angular (PrimeNG)
```

