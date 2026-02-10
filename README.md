# MiniLog: Gestão de Distribuição Matriz-CD

## 📋 Sobre o Projeto
Este projeto consiste em uma aplicação web desenvolvida para a disciplina de **Programação WEB**, sob orientação do **Prof. Dr. Luiz Carlos Camargo**. 

O objetivo é gerenciar a logística de distribuição de mercadorias entre uma unidade Matriz e seus Centros de Distribuição (CDs) cadastrados, garantindo a integridade dos dados e o rastreio eficiente das movimentações de estoque.

## 🏗️ Domínio e Escopo
O sistema foca no processo de suprimento interno, onde a Matriz atua como o HUB central de estoque.

### Funcionalidades Principais:
* **Gestão de Unidades (CRUD):** Cadastro completo dos Centros de Distribuição (Localização, capacidade, responsável).
* **Distribuição de Pedidos (Transação):** Processo crítico que registra a saída do item da Matriz e a entrada no CD de destino, garantindo que não haja duplicidade ou perda de registros (Princípio ACID).
* **Controle de Inventário:** Visualização em tempo real do saldo de produtos em cada unidade.
* **Autenticação:** Acesso restrito via login com geração de tokens para segurança das operações.

## 🛠️ Tecnologias Utilizadas
A escolha do stack tecnológico baseia-se na robustez e nos requisitos da disciplina:

* **Linguagem:** Java (Orientação a Objetos)
* **Arquitetura:** REST com padrão MV*
* **Persistência de Dados:** PostgreSQL (Relacional)
* **Controle de Versão:** Git
* **Conceitos Aplicados:** * Inversão de Controle (IoC) e Injeção de Dependência (ID).
    * Padrões de Projeto (Singleton para conexão com DB, Strategy para regras de frete).
    * Desenvolvimento orientado a testes (TDD).

## ⚖️ Justificativa das Tecnologias
* **Java:** Escolhido pela forte tipagem e suporte nativo a padrões de projeto (Design Patterns) e serviços de mensageria, essenciais para sistemas logísticos.
* **PostgreSQL:** Utilizado para garantir a **Persistência Não Volátil** e a segurança em transações complexas de transferência de mercadorias, onde a falha de uma etapa deve anular toda a operação para evitar erros de estoque.

## 📅 Plano de Trabalho (Check-points)
Conforme o cronograma quinzenal:
- [x] Definição do Domínio e Repositório (Check-point 1)
- [ ] Modelagem de Dados e Arquitetura REST
- [ ] Implementação do CRUD de CDs
- [ ] Implementação da Transação de Pedidos
- [ ] Testes Unitários e Deploy

## 👥 Integrantes
* **Nome do Aluno 1** - [GitHub Profile Link]
* **Nome do Aluno 2 (se houver)** - [GitHub Profile Link]
