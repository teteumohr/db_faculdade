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

![Esboço](https://github.com/teteumohr/db_faculdade/assets/17818209/50d11025-32d5-440c-aeb9-76ad16716ed0)


# Dicionário de Dados: Entidades

![D-Entidades](https://github.com/teteumohr/db_faculdade/assets/17818209/5047c76c-4d90-4721-84ae-3e6a71606af8)

![D-Atributos](https://github.com/teteumohr/db_faculdade/assets/17818209/4019f4c2-3f3e-46ac-98f2-15438f327aa6)


# Normalização:

### Primeira normalização:

| Entidade  | Atributo antigo    | Atributo novo |
| --------- | ------------------ |-------------- |
| Histórico | Período_Realização | Data_Inicio   |
|           |                    | Data_Fim      |
| Alunos    | Contato            | WhatsApp      |
|           |                    | E-mail        |
|           | Telefone           | Telefone_Res  |
|           |                    | Telefone_Cel  |

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

![D-ML-Entidades](https://github.com/teteumohr/db_faculdade/assets/17818209/6f290725-8f8d-4ebe-977f-5753a7161870)


![D-ML-Atributos](https://github.com/teteumohr/db_faculdade/assets/17818209/c968f3f0-f7b0-450d-a50b-e4c071a94ff9)


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

# Arquivo do MySQL Físico:

Esta nos arquivos.
