

# 🍦 AÍ 'SE CREAM – Sistema de Distribuição para Rede de Sorveterias

## 1. Descrição do Projeto

O **AÍ 'SE CREAM** é uma aplicação web desenvolvida para gerenciar a produção e distribuição de sorvetes de uma fábrica central para suas lojas na cidade.

O sistema permite registrar lotes de produção e distribuir quantidades específicas para lojas cadastradas, garantindo controle quantitativo e rastreabilidade das distribuições realizadas.

---

## 2. Domínio do Problema

Uma rede de sorveterias possui uma fábrica central responsável pela produção dos sabores. Após a produção, os lotes precisam ser distribuídos entre as lojas da rede.

Atualmente não existe controle estruturado para:

* Registrar quantidades produzidas
* Controlar quantidade disponível por lote
* Registrar distribuições realizadas
* Garantir que não sejam distribuídas quantidades superiores às disponíveis

O sistema proposto resolve esse problema por meio de controle transacional e persistência estruturada.

---

## 3. Objetivo do Sistema

Desenvolver uma aplicação web que:

* Permita cadastrar lojas e sabores
* Registre lotes de produção
* Realize a distribuição de lotes para lojas
* Garanta consistência de dados
* Controle acesso via autenticação
* Seja publicada em ambiente online

---

# 4. Perfis de Usuário

### ADMIN

* Gerencia lojas
* Gerencia produtos (sabores)
* Visualiza relatórios

### OPERADOR

* Registra lotes de produção
* Realiza distribuições para lojas

---

# 5. Requisitos Funcionais

### RF01 – Autenticação

O sistema deve permitir login com e-mail e senha.

### RF02 – Controle de Acesso

O sistema deve restringir funcionalidades com base no perfil do usuário (ADMIN ou OPERADOR).

### RF03 – CRUD de Loja

O sistema deve permitir cadastrar, listar, editar e excluir lojas.

### RF04 – CRUD de Produto (Sabor)

O sistema deve permitir cadastrar, listar, editar e excluir sabores.

### RF05 – Registro de Lote de Produção

O sistema deve permitir registrar:

* Produto
* Data de produção
* Quantidade produzida
* Quantidade disponível

### RF06 – Distribuição de Lote (Transação Principal)

O sistema deve permitir distribuir uma quantidade de um lote para uma loja.

Regras:

* (RF06.1) A quantidade distribuída não pode ser maior que a disponível
* (RF06.2) A quantidade disponível deve ser atualizada após a distribuição
* (RF06.3) A operação deve ser transacional
* (RF06.4) Deve registrar data e loja destinatária

### RF07 – Consulta de Distribuições

O sistema deve permitir visualizar o histórico de distribuições por:

* Loja
* Produto
* Data

---

# 6. Requisitos Não Funcionais

### RNF01 – Arquitetura

* (RNF01.1) Arquitetura monolítica
* (RNF01.2) Padrão MVC
* (RNF01.3) API RESTful
* (RNF01.4) Separação em camadas:

  * (RNF01.4.1) Controller
  * (RNF01.4.2) Service
  * (RNF01.4.3) Repository

### RNF02 – Segurança

* Autenticação via JWT
* Endpoints protegidos por perfil

### RNF03 – Persistência

* Banco de dados PostgreSQL
* 
### RNF04 – Testes

* Testes unitários com JUnit
* Testes da regra de distribuição

### RNF05 – Deploy

* Aplicação publicada em ambiente online
* Banco de dados externo
* Pipeline CI/CD configurado

### RNF06 – Observabilidade

* Logs estruturados
* Tratamento global de exceções
* Endpoint de health check

### RNF07 – Manutenibilidade

* Versionamento via Git
* README documentado
* Estrutura organizada por camadas

---

# 7. Modelo Conceitual Simplificado

## Entidades Principais

### Usuario

* id
* nome
* email
* senha
* perfil

### Loja

* id
* nome
* bairro
* ativa

### Produto

* id
* nome
* descricao

### LoteProducao

* id
* produto
* dataProducao
* quantidadeProduzida
* quantidadeDisponivel

### Distribuicao

* id
* lote
* loja
* quantidadeDistribuida
* dataDistribuicao

---

# 8. Transação Principal do Sistema

A principal regra de negócio do sistema está na distribuição de lotes, garantindo:

* Consistência de estoque
* Integridade de dados
* Execução atômica da operação

Essa funcionalidade será implementada utilizando controle transacional do Spring.

---

# 9. Tecnologias Utilizadas

## Back-end

* Java 17
* Spring Boot
* Spring Data JPA
* Spring Security
* JWT
* JUnit e Mockito

## Banco de Dados

* PostgreSQL

## Front-end

* HTML5
* CSS3
* JavaScript
* Consumo de API via Fetch

## DevOps

* Git / GitLab
* CI/CD pipeline
