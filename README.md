# 🍦 AÍ 'SE CREAM – Sistema de Distribuição para Rede de Sorveterias

---

## 1. Descrição do Projeto

O **AÍ 'SE CREAM** é uma aplicação web baseada em **arquitetura de microserviços**, desenvolvida para gerenciar a produção e distribuição de sorvetes de uma fábrica central para as lojas da marca.

O sistema permite registrar lotes de produção e distribuir quantidades específicas para lojas cadastradas, garantindo controle quantitativo, rastreabilidade e isolamento de responsabilidades entre serviços.

---

## 2. Domínio do Problema

Uma rede de sorveterias possui uma fábrica central responsável pela produção dos sabores. Após a produção, os lotes precisam ser distribuídos entre as lojas da rede.

Atualmente não existe controle estruturado para:

- Registrar quantidades produzidas  
- Controlar quantidade disponível por lote  
- Registrar distribuições realizadas  
- Garantir que não sejam distribuídas quantidades superiores às disponíveis  
- Controlar acesso e responsabilidades por perfil  

O sistema proposto resolve esse problema por meio de serviços independentes, comunicação via API REST e controle de consistência entre serviços.

---

## 3. Objetivo do Sistema

Desenvolver uma aplicação web baseada em **microserviços** que:

- Permita cadastrar lojas e sabores  
- Registre lotes de produção  
- Realize a distribuição de lotes para lojas  
- Garanta consistência de dados entre serviços  
- Controle acesso via autenticação JWT  
- Seja publicada em ambiente online  
- Permita escalabilidade independente por serviço  

---

# 4. Arquitetura do Sistema

O sistema será dividido em microserviços independentes, cada um responsável por um contexto específico.

## 4.1 Microserviços

### 🔐 autenticacao
Responsável por:
- Autenticação de usuários
- Geração e validação de JWT
- Controle de perfil (ADMIN e OPERADOR)

Entidade:
- Usuario

---

### 📦 catalogo
Responsável por:
- CRUD de Loja
- CRUD de Produto (Sabor)

Entidades:
- Loja
- Produto

---

### 🏭 producao
Responsável por:
- Registro de Lotes de Produção
- Controle de quantidade disponível

Entidade:
- LoteProducao

---

### 🚚 distribuicao
Responsável por:
- Registro de Distribuições
- Validação de estoque disponível
- Comunicação com producao para atualização de estoque

Entidade:
- Distribuicao

---

### 🌐 gateway
Responsável por:
- Roteamento das requisições
- Ponto único de entrada da aplicação
- Validação inicial do JWT

---

## 4.2 Comunicação Entre Serviços

- Comunicação síncrona via REST  
- Cada serviço possui API própria  
- Comunicação interna protegida por token  

---

# 5. Perfis de Usuário

## ADMIN
- Gerencia lojas  
- Gerencia produtos (sabores)  
- Visualiza relatórios  

## OPERADOR
- Registra lotes de produção  
- Realiza distribuições para lojas  

---

# 6. Requisitos Funcionais

### RF01 – Autenticação
O sistema deve permitir login com e-mail e senha através do `autenticacao`.

### RF02 – Controle de Acesso
O sistema deve restringir funcionalidades com base no perfil do usuário (ADMIN ou OPERADOR), utilizando JWT.

### RF03 – CRUD de Loja
O `catalogo` deve permitir cadastrar, listar, editar e excluir lojas.

### RF04 – CRUD de Produto (Sabor)
O `catalogo` deve permitir cadastrar, listar, editar e excluir sabores.

### RF05 – Registro de Lote de Produção
O `producao` deve permitir registrar:
- Produto  
- Data de produção  
- Quantidade produzida  
- Quantidade disponível  

### RF06 – Distribuição de Lote (Transação Principal)

O `distribuicao` deve permitir distribuir uma quantidade de um lote para uma loja.

Regras:

- (RF06.1) A quantidade distribuída não pode ser maior que a disponível  
- (RF06.2) O `producao` deve atualizar a quantidade disponível  
- (RF06.3) A operação deve garantir consistência entre serviços  
- (RF06.4) Deve registrar data e loja destinatária  
- (RF06.5) Em caso de falha, deve impedir inconsistência de estoque  

### RF07 – Consulta de Distribuições
O `distribuicao` deve permitir visualizar histórico de distribuições por:
- Loja  
- Produto  
- Data  

---

# 7. Requisitos Não Funcionais

## RNF01 – Arquitetura

- (RNF01.1) Arquitetura baseada em microserviços  
- (RNF01.2) Cada microserviço deve possuir banco de dados próprio  
- (RNF01.3) Comunicação via API RESTful  
- (RNF01.4) Uso de API Gateway como ponto único de entrada  
- (RNF01.5) Separação em camadas internas (Controller, Service, Repository) dentro de cada microserviço  

---

## RNF02 – Segurança

- (RNF02.1) Autenticação via JWT  
- (RNF02.2) Endpoints protegidos por perfil  
- (RNF02.3) Validação de token no Gateway  

---

## RNF03 – Persistência

- Banco de dados PostgreSQL  

---

## RNF04 – Testes

- Testes unitários com JUnit  
- Testes da regra de distribuição  
- Testes de comunicação entre serviços  

---

## RNF05 – Deploy

- Deploy independente por microserviço  
- Banco de dados externo  
- Pipeline CI/CD configurado  

---

## RNF06 – Observabilidade

- Logs estruturados por serviço  
- Tratamento global de exceções  
- Endpoint de health check por microserviço  

---

## RNF07 – Manutenibilidade

- Versionamento via Git  
- README documentado por serviço  
- Estrutura organizada por contexto de negócio  

---

# 8. Modelo Conceitual Distribuído

## autenticacao

### Usuario
- id  
- nome  
- email  
- senha  
- perfil  

---

## catalogo

### Loja
- id  
- nome  
- bairro  
- ativa  

### Produto
- id  
- nome  
- descricao  

---

## producao

### LoteProducao
- id  
- produtoId  
- dataProducao  
- quantidadeProduzida  
- quantidadeDisponivel  

---

## distribuicao

### Distribuicao
- id  
- loteId  
- lojaId  
- quantidadeDistribuida  
- dataDistribuicao  

---

# 9. Transação Principal do Sistema

A principal regra de negócio está na distribuição de lotes.

Em arquitetura de microserviços:

- A validação de estoque ocorre via comunicação com `producao`  
- A atualização de estoque é feita pelo próprio `producao`  
- O `distribuicao` registra a operação  
- Deve haver garantia de consistência entre serviços  

Estratégia adotada:

- Comunicação REST síncrona  
- Tratamento de falhas para evitar inconsistências  

---

# 10. Tecnologias Utilizadas

## Back-end
- Java 17  
- Spring Boot  
- Spring Data JPA  
- Spring Security  
- Spring Cloud Gateway  
- JWT    

## Banco de Dados
- PostgreSQL

## Comunicação
- REST  

## Front-end
- HTML5  
- CSS3  
- JavaScript  
- Consumo via API Gateway  

## DevOps
- Git / GitLab  
- CI/CD pipeline  
