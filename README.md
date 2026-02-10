## 🗄️ Projeto Físico de Banco de Dados

Após a fase de **planejamento**, que foi a criação/confecção dos diagramas DER/Lógico, passamos para a fase inicial de **execução** do desenvolvimento do sistema: a criação do **Projeto Físico de Banco de Dados**, apresentado neste repositório.

É nesta etapa que traduzimos os diagramas para a linguagem real de programação (**SQL**). Aqui, definimos o que vamos guardar e *como* vamos guardar (as regras de negócio), isto é:

* Qual será o limite de caracteres de um nome;
* Qual campo é numérico ou texto;
* Quais regras de segurança impedem dados inválidos.
* As relações entre as tabelas

### 🎯 Importância
Aprender o projeto físico é fundamental porque o banco de dados é o **alicerce** do seu sistema/aplicação. Um código SQL bem feito (com restrições e tipagem correta) impede que o sistema aceite e-mails inválidos, CPFs errados ou notas negativas, facilitando muito o trabalho de quem desenvolve o Back-end e garantindo a **integridade da informação**.


## O que tem neste arquivo .sql

No script você encontrará as tabelas principais do sistema escolar (modelo simplificado para entrega):

* usuario — dados de login e identificação (email, cpf, senha_hash, tipo);

* turma — turmas existentes;

* disciplina — disciplinas do curso;

* responsavel — responsáveis (ligados a usuario);

* professor — professores (ligados a usuario);

* estudante — dados dos alunos (chaque turma e responsável);

* professor_turma — tabela associativa (N:N) entre professor e turma;

* acompanhamento — notas / faltas / ocorrências por disciplina e período;

* falta_justificada — justificativas de faltas (texto, por enquanto);

* evento_calendario — eventos da turma (provas, reuniões, etc).

## Principais decisões e justificativas 

* CPF sem máscara no banco: armazenamos apenas números (ex.: 12345678900). A máscara (. e -) que vem normalmente no cpf, a gente pensou em tratar isso no back-end, quando o usuário informar a gente pega o cpf 123.456.789-00 e transforma só em digitos pro banco -> 12345678900.

* E-mail como login: escolhemos o e-mail como identificador para login (é mais comum e prático). O CPF permanece como dado cadastral e único.

* Senha como senha_hash: Não armazenamos senhas em texto puro por segurança. Já fiz (jefferson) um projeto que tinha login e usei essa lib para o back-end (Python/Flask) que gera um hash utilizando a biblioteca werkzeug.security (generate_password_hash) e salva apenas esse valor no banco.
  Quando o usuário faz login, o sistema utiliza check_password_hash para comparar a senha digitada com o hash armazenado.
  Após a validação, o sistema pode gerar um token JWT para manter o usuário autenticado.

* IDs automáticos: usamos SERIAL (ou IDENTITY) para gerar identificadores numéricos automáticos nas PKs.

* Tabelas de especialização (professor / responsavel): cada professor e responsável é também um usuario. Mantemos id_usuario como UNIQUE em professor/responsavel para garantir 1:1 (um usuário só pode ser cadastrado como professor uma vez).

* Chaves estrangeiras e ações ON DELETE:

  estudante → ON DELETE RESTRICT para impedir apagar turma quando há alunos (depende do requisito).

  falta_justificada.id_estudante → ON DELETE CASCADE: se o aluno for removido, não faz sentido manter justificativas.

  falta_justificada.id_responsavel → ON DELETE RESTRICT (escolhemos RESTRICT para preservar histórico e evitar perda não      intencional).

  professor_turma e evento_calendario → ON DELETE CASCADE em turma/professor quando faz sentido apagar registros              dependentes.

* Tabela acompanhamento: formatada para registrar notas (0–100), faltas e ocorrências. Inclui:

  tipo_acompanhamento (NOTA / FALTA / OCORRENCIA);

  periodo_avaliacao (1BIM, 2BIM, 3BIM, 4BIM, RECUPERACAO, FINAL);

  valor numeric(5,2) com CHECK para garantir 0 ≤ valor ≤ 100 quando for NOTA;

  descricao para ocorrências (mas pode ser usada pra notas e faltas também).

  Possui FK para professor, estudante e disciplina.

* Justificativas de falta: por enquanto apenas texto (documento_url contém texto explicativo). Em produção, a aplicação fará upload do PDF e gravará o caminho/URL no banco (não guardamos o arquivo dentro do banco por padrão).

Escolhas para a entrega (por que simplificamos)

Arquivo/Upload: nesta entrega guardamos apenas texto/caminho de arquivo no banco (campo documento_url). Acreditamos que isso é suficiente para demonstrar modelagem.

* Tudo em uma tabela acompanhamento: 
por rapidez e simplicidade, usamos uma tabela polimórfica (nota/falta/ocorrência).      

Quando for codar de verdade, a gente planeja fazer tabelas diferentes para separar nota, falta e ocorrencia, isso facilita relatórios e regras mais complexas posteriormente.
