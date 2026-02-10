## 🗄️ Projeto Físico de Banco de Dados

Após a fase de **planejamento**, que foi a criação/confecção dos diagramas DER/Lógico, passamos para a fase inicial de **execução** do desenvolvimento do sistema: a criação do **Projeto Físico de Banco de Dados**, apresentado neste repositório.

É nesta etapa que traduzimos os diagramas para a linguagem real de programação (**SQL**). Aqui, definimos o que vamos guardar e *como* vamos guardar (as regras de negócio), isto é:

* Qual será o limite de caracteres de um nome;
* Qual campo é numérico ou texto;
* Quais regras de segurança impedem dados inválidos.
* As relações entre as tabelas

### 🎯 Importância
Aprender o projeto físico é fundamental porque o banco de dados é o **alicerce** do seu sistema/aplicação. Um código SQL bem feito (com restrições e tipagem correta) impede que o sistema aceite e-mails inválidos, CPFs errados ou notas negativas, facilitando muito o trabalho de quem desenvolve o Back-end e garantindo a **integridade da informação**.
