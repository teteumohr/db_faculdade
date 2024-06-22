# Fases do Projeto

- [x]  Levantamento dos Requisitos
- [x]  Identificação de Entidades e Relacionamentos
- [x]  Diagrama E-R: Cardinalidades
- [x]  Diagrama E-R: Eliminando N
- [x]  Dicionário de Dados
- [x]  Modelo Lógico
- [x]  Normalização
- [x]  Ajustes no ML e DD
- [x]  Alteração para o MySQL
- [x]  Implementação
- [x]  Testes Básicos

# Regras do Negócio

- Alunos possuem um código de identificação (RA)
- Um aluno só pode estar matriculado em um curso por vez
- Cursos são compostos por disciplinas
- Cada disciplina terá no máximo 30 alunos por turma
- As disciplinas podem ser obrigatórias ou optativas, dependendo do curso
- As disciplinas pertencem a departamentos específicos
- Cada disciplina possui um código de identificação
- Um Histórico Escolar traz todas as disciplinas cursadas por um aluno, incluindo nota final, frequência e período do curso realizado.
- Professores podem ser cadastrados mesmo sem lecionar disciplinas
- Existem 40 professores trabalhando na escola.
- Cada professor irá lecionar no máximo 4 disciplinas diferentes
- Cada professor é vinculado a um departamento
- Professores são identificados por um código de professor
- Alunos podem trancar matrícula disciplina no semestre
- Em cada semestre , cada aluno pode se matricular em no máximo 9 disciplinas
- O aluno só pode ser reprovado no máximo 3 vezes na mesma disciplina
- A faculdade terá no máximo 3.000 alunos matriculados simultaneamente, em 10 cursos distintos
- Entram 300 alunos novos por ano
- Existem 90 disciplinas no total disponíveis

# Identificando as Entidades

- Aluno
- Professor
- Disciplina
- Curso
- Departamento

# Identificando os Relacionamentos

- Aluno está Matriculado em Curso
- Aluno Cursa Disciplina
- Aluno Realizou Disciplina
- Disciplina Pertence a Curso
- Professor Ministra Disciplina
- Professor Pertence a Departamento
- Departamento é Responsável por Disciplina
- Departamento Controla Curso
- Disciplina Depende de Disciplina

# Identificando os Atributos - Aluno

- Número de Matrícula (RA) (PK)
- Nome
- Sobrenome
- CPF
- *Telefone
- Filiação
    - Pai
    - Mãe
- Endereço ( Atributo composto )
    - Rua
    - Número
    - Bairro
    - CEP
    - Cidade
    - Estado
- Código do Curso
- Aluno - cont.
    - Status
    - Filiação
    - Sexo
    - Contato
    - Cod_Turma

# Identificando os Atributos - Professor

- Código do Professor (PK)
- Nome
- Sobrenome
- Código do Departamento
- Status

# Identificando os Atributos - Disciplina

- Código da Disciplina (PK)
- Nome da Disciplina
- Descrição Curricular
- Código do Departamento
- Número de Alunos
- Carga_Horária
- Histórico
    - Cod_Histórico
    - Nota
    - Média
    - Frequência
    - Período_Realização
    - RA
    - Cod_DIsciplina
- Turma
    - Cod_Turma
    - Período
    - Cod_Curso
    - Num_Alunos
    - Data_Início
    - Data_Fim
    - Turno

# Identificando os Atributos - Curso

- Código do Curso (PK)
- Nome do Curso
- Código do Departamento

# Identificando os Atributos - Departamento

- Código do Departamento (PK)
- Nome do Departamento

# Esboço:

![teste.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3f953c80-ebbe-4646-9227-f6994639a4ca/42dcf8de-b661-48f6-9a8b-5eee5679ecb5/teste.png)

# Dicionário de Dados: Entidades

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/3f953c80-ebbe-4646-9227-f6994639a4ca/546e1bb1-5ede-4468-97ee-816258dad5af/Untitled.png)

![Completa.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3f953c80-ebbe-4646-9227-f6994639a4ca/a48056e5-ae14-4f67-a85e-2c4529425b3a/Completa.png)

# Normalização:

### Primeira normalização:

| Entidade  | Atributo antigo    | Atributo novo  |
| --------- | ------------------ | -------------- |
| Histórico | Período_Realização | Data_Inicio    |
                                   Data_Fim       |
---------------------------------------------------
| Alunos    | Contato            | WhatsApp       |
                                   E-mail         |
---------------------------------------------------
|           | Telefone           | Telefone_Res   |
                                   Telefone_Cel   |

### Segunda normalização:

Na tabela de Alunos, eliminamos os dois campos de telefones e adicionamos as seguintes tabelas no lugar:

Telefones_Aluno

| Restrições | Atributo            | Tipo de Dados e Comprim. |
| ---------- | ------------------- | ------------------------ |
| PK         | Cod_Telefones_Aluno | numeric(4)               |
| FK         | RA | numeric(4)     |                          |
| FK         | Cod_Tipo_Telefone   | numeric(4)               |
|            | Num_Telefone        | varchar(11)              |

                      Tipo_Telefone
| Restrições | Atributo          | Tipo de Dados e Comprim. |
|----------- | ----------------- | ------------------------ |
| PK         | Cod_Tipo_Telefone | numeric(4)               |
|            | Tipo_Telefone     | varchar(8)               |

Na tabela de Alunos, eliminamos os campos relacionados a endereço e adicionamos as seguintes tabelas no lugar:

Endereco_Aluno

| Restrições | Atributo            | Tipo de Dados e Comprim. |
| ---------- | ------------------- | ------------------------ |
| PK         | Cod_Endereco_Aluno  | numeric(4)               |
| FK         | RA                  | numeric(8)               |
| FK         | Cod_Tipo_Logradouro | numeric(10)              |
|            | Nome_Rua            | varchar(30)              |
|            | Num_Rua             | numeric(5)               |
|            | Complemento         | varchar(10)              |
|            | CEP                 | varchar(8)               |

Tipo_Logradouro

| Restrições | Atributo            | Tipo de Dados e Comprim. |
| ---------- | ------------------- | ------------------------ |
| PK         | Cod_Tipo_Logradouro | numeric(4)               |
|            | Tipo_Logradouro     | varchar(11)              |

### Terceira normalização:

Tabela esta padronizada.

# Ajustes no ML e DD

![D1 - Entidades.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3f953c80-ebbe-4646-9227-f6994639a4ca/67ad9b66-c9cd-4c75-b41b-f8fcd4d143d1/D1_-_Entidades.png)

![D2 - Atributos.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/3f953c80-ebbe-4646-9227-f6994639a4ca/a14dcd7a-0b8e-4cf0-9ba1-e9e0e15820d8/D2_-_Atributos.png)

# Alteração para MySQL

O banco de dados foi criado para funcionar no MySQL, conforme isso ele precisou de algumas alterações como:

1. **Remoção da largura de exibição dos inteiros**:
    - A largura de exibição dos inteiros (`INT(2)`, `INT(4)`, etc.) foi removida, pois esta funcionalidade está obsoleta e será removida em futuras versões do MySQL. Apenas `INT` foi utilizado.
2. **Adição de colunas `AUTO_INCREMENT` para chaves primárias**:
    - Algumas tabelas receberam a adição da palavra-chave `AUTO_INCREMENT` para colunas de chave primária para facilitar a inserção automática de valores únicos.
3. **Correção dos tipos de dados**:
    - O tipo `TINYINT` foi utilizado para `Status_Professor` e `Status_Aluno` em vez de `BOOLEAN`.
    - O tipo `VARCHAR` foi ajustado conforme necessário.
4. **Adição e correção de `CONSTRAINTS`**:
    - `FOREIGN KEY CONSTRAINTS` foram adicionadas para manter a integridade referencial entre as tabelas.
    - A nomeação das `CONSTRAINTS` foi padronizada para facilitar a compreensão e manutenção.
5. **Remoção de colunas desnecessárias**:
    - Colunas que especificavam larguras que não eram necessárias foram removidas ou ajustadas.

Estas alterações garantem que o código funcione corretamente no MySQL, seguindo as práticas recomendadas para a definição de tabelas e chaves estrangeiras.

# Código do MySQL Físico:

CREATE TABLE Departamento (
    Cod_Departamento INT PRIMARY KEY AUTO_INCREMENT,
    Nome_Departamento VARCHAR(20) NOT NULL
);

CREATE TABLE Professor (
    Cod_Professor INT PRIMARY KEY AUTO_INCREMENT,
    Nome_Professor VARCHAR(20) NOT NULL,
    Sobrenome_Professor VARCHAR(50) NOT NULL,
    Status_Professor TINYINT,
    Cod_Departamento INT,
    CONSTRAINT fk_Codigo_Departamento FOREIGN KEY(Cod_Departamento) REFERENCES Departamento(Cod_Departamento)
);

CREATE TABLE Curso (
    Cod_Curso INT PRIMARY KEY AUTO_INCREMENT,
    Nome_Curso VARCHAR(30),
    Cod_Departamento INT,
    CONSTRAINT fk_Cod_Departamento FOREIGN KEY(Cod_Departamento) REFERENCES Departamento(Cod_Departamento)
);

CREATE TABLE Turma (
    Cod_Turma INT PRIMARY KEY AUTO_INCREMENT,
    Cod_Curso INT,
    Periodo VARCHAR(8),
    Num_Alunos INT,
    Data_Inicio DATE,
    Data_Fim DATE,
    CONSTRAINT fk_Cod_Curso FOREIGN KEY(Cod_Curso) REFERENCES Curso(Cod_Curso)
);

CREATE TABLE Disciplina (
    Cod_Disciplina INT PRIMARY KEY AUTO_INCREMENT,
    Cod_Disciplina_Depende INT NULL, /* Auto-relacionamento */
    Nome_Disciplina VARCHAR(30),
    Cod_Departamento INT NOT NULL,
    Carga_Horaria INT NOT NULL,
    Descrição VARCHAR(80),
    Num_Alunos INT NOT NULL,
    CONSTRAINT fk_Cod_Departamento_Disciplina FOREIGN KEY(Cod_Departamento) REFERENCES Departamento(Cod_Departamento),
    CONSTRAINT fk_Cod_Disciplina FOREIGN KEY(Cod_Disciplina_Depende) REFERENCES Disciplina(Cod_Disciplina)
);

CREATE TABLE Prof_Disciplina (
    Cod_Professor INT NOT NULL,
    Cod_Disciplina INT NOT NULL,
    PRIMARY KEY (Cod_Professor, Cod_Disciplina),
    CONSTRAINT fk_Cod_Professor_Prof FOREIGN KEY(Cod_Professor) REFERENCES Professor(Cod_Professor),
    CONSTRAINT fk_Cod_Disciplina_Prof FOREIGN KEY(Cod_Disciplina) REFERENCES Disciplina(Cod_Disciplina)
);

CREATE TABLE Curso_Disciplina (
    Cod_Curso INT NOT NULL,
    Cod_Disciplina INT NOT NULL,
    PRIMARY KEY(Cod_Curso, Cod_Disciplina),
    CONSTRAINT fk_Cod_Curso_Disci FOREIGN KEY(Cod_Curso) REFERENCES Curso(Cod_Curso),
    CONSTRAINT fk_Cod_Disciplina_Disci FOREIGN KEY(Cod_Disciplina) REFERENCES Disciplina(Cod_Disciplina)
);

CREATE TABLE Aluno (
    RA INT PRIMARY KEY AUTO_INCREMENT,
    Nome_Aluno VARCHAR(20) NOT NULL,
    Sobrenome_Aluno VARCHAR(20) NOT NULL,
    CPF VARCHAR(11) NOT NULL,
    Status_Aluno TINYINT NOT NULL,
    Cod_Turma INT,
    Sexo VARCHAR(1),
    Cod_Curso INT,
    Nome_Pai VARCHAR(50) NOT NULL,
    Nome_Mae VARCHAR(50) NOT NULL,
    Email VARCHAR(50) NOT NULL,
    Whatsapp VARCHAR(20) NOT NULL,
    CONSTRAINT fk_Cod_Turma_Aluno FOREIGN KEY(Cod_Turma) REFERENCES Turma(Cod_Turma),
    CONSTRAINT fk_Cod_Curso_Aluno FOREIGN KEY(Cod_Curso) REFERENCES Curso(Cod_Curso)
);

CREATE TABLE Aluno_Disc (
    RA INT NOT NULL,
    Cod_Disciplina INT NOT NULL,
    PRIMARY KEY (RA, Cod_Disciplina),
    CONSTRAINT fk_RA_Aluno FOREIGN KEY (RA) REFERENCES Aluno(RA),
    CONSTRAINT fk_Cod_Disciplina_Aluno FOREIGN KEY (Cod_Disciplina) REFERENCES Disciplina(Cod_Disciplina)
);

CREATE TABLE Historico (
    Cod_Historico INT PRIMARY KEY AUTO_INCREMENT,
    RA INT NOT NULL,
    Data_Inicio DATE NOT NULL,
    Data_Final DATE,
    CONSTRAINT fk_Cod_RA FOREIGN KEY (RA) REFERENCES Aluno(RA)
);

CREATE TABLE Disc_Hist (
    Cod_Historico INT NOT NULL,
    Cod_Disciplina INT NOT NULL,
    Nota Decimal,
    Frequência INT,
    PRIMARY KEY (Cod_Historico, Cod_Disciplina),
    CONSTRAINT fk_Cod_Historico FOREIGN KEY (Cod_Historico) REFERENCES Historico(Cod_Historico),
    CONSTRAINT fk_Cod_Disciplina_Hist FOREIGN KEY (Cod_Disciplina) REFERENCES Disciplina(Cod_Disciplina)
);

CREATE TABLE Tipo_Telefone(
    Cod_Tipo_Telefone INT PRIMARY KEY AUTO_INCREMENT,
    Tipo_Telefone VARCHAR(8)
);

CREATE TABLE Telefones_Aluno (
    Cod_Telefones_Aluno INT PRIMARY KEY AUTO_INCREMENT,
    RA INT NOT NULL,
    Cod_Tipo_Telefone INT NOT NULL,
    Num_Telefone VARCHAR(20) NOT NULL,
    CONSTRAINT fk_Cod_RA_Tel FOREIGN KEY (RA) REFERENCES Aluno(RA),
    CONSTRAINT fk_Cod_Tipo_Telefone FOREIGN KEY(Cod_Tipo_Telefone) REFERENCES Tipo_Telefone(Cod_Tipo_Telefone)
);

CREATE TABLE Tipo_Logradouro (
    Cod_Tipo_Logradouro INT PRIMARY KEY AUTO_INCREMENT,
    Tipo_Logradouro VARCHAR(11)
);

CREATE TABLE Endereco_Aluno (
    Cod_Endereco_Aluno INT PRIMARY KEY AUTO_INCREMENT,
    RA INT NOT NULL,
    Cod_Tipo_Logradouro INT NOT NULL,
    Nome_Rua VARCHAR(50) NOT NULL,
    Num_Rua INT NOT NULL,
    Complemento VARCHAR(20) NULL,
    CEP VARCHAR(8) NOT NULL,
    CONSTRAINT fk_Cod_RA_End FOREIGN KEY(RA) REFERENCES Aluno(RA),
    CONSTRAINT fk_Cod_Tipo_Logradouro FOREIGN KEY(Cod_Tipo_Logradouro) REFERENCES Tipo_Logradouro(Cod_Tipo_Logradouro)
);
