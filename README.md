# Lista de Exercícios - Banco de Dados

## Objetivo da aula
Praticar criação de banco de dados, esquemas e tabelas com comandos SQL.

## Instruções gerais
1. Use SQL padrão (adapte pequenos detalhes ao SGBD usado em sala).
2. Escreva os comandos em um arquivo `.sql` separado por exercício.
3. Execute cada exercício e valide se os objetos foram criados corretamente.
4. Sempre nomeie objetos de forma clara e consistente.

---

## Bloco 1 - Aquecimento (fundamentos)

### Exercício 1 - Criar banco de dados
Crie um banco chamado `academico`.

Requisitos:
- O banco deve ser criado com sucesso.
- Verifique se ele aparece na lista de bancos do SGBD.

### Exercício 2 - Criar esquemas
No banco `academico`, crie os esquemas:
- `cadastro`
- `financeiro`
- `biblioteca`

Requisitos:
- Listar os esquemas para comprovar criação.

### Exercício 3 - Tabela simples de alunos
No esquema `cadastro`, crie a tabela `aluno` com os campos:
- `id_aluno` (inteiro, chave primária)
- `nome` (texto, obrigatório)
- `email` (texto, único, obrigatório)
- `data_nascimento` (data)
- `ativo` (booleano, padrão verdadeiro)

Requisitos:
- Definir PK, `NOT NULL`, `UNIQUE` e `DEFAULT` corretamente.

### Exercício 4 - Tabela de cursos
No esquema `cadastro`, crie a tabela `curso` com os campos:
- `id_curso` (inteiro, chave primária)
- `nome_curso` (texto, obrigatório, único)
- `carga_horaria` (inteiro, obrigatório)

Requisitos:
- Criar uma regra para impedir `carga_horaria <= 0`.

---

## Bloco 2 - Relacionamentos e integridade

### Exercício 5 - Matrículas (relação N:N)
Crie a tabela `matricula` no esquema `cadastro` para relacionar aluno e curso.

Campos mínimos:
- `id_aluno`
- `id_curso`
- `data_matricula`
- `status_matricula` (ex.: `ATIVA`, `TRANCADA`, `CANCELADA`)

Requisitos:
- Chave primária composta (`id_aluno`, `id_curso`).
- Chaves estrangeiras para `aluno` e `curso`.
- `status_matricula` obrigatório.

### Exercício 6 - Tabela de mensalidades
No esquema `financeiro`, crie a tabela `mensalidade`:
- `id_mensalidade` (inteiro, chave primária)
- `id_aluno` (inteiro, obrigatório)
- `competencia` (texto curto, obrigatório; exemplo: `2026-04`)
- `valor` (decimal, obrigatório)
- `data_vencimento` (data, obrigatória)
- `pago` (booleano, padrão falso)

Requisitos:
- FK para `cadastro.aluno`.
- Regra para impedir `valor <= 0`.

### Exercício 7 - Regras de exclusão/atualização
Ajuste as FKs criadas:
- Em `cadastro.matricula`, ao excluir um aluno, excluir suas matrículas automaticamente.
- Em `financeiro.mensalidade`, ao excluir um aluno, não permitir exclusão se houver mensalidades.

Requisitos:
- Definir ações apropriadas (`ON DELETE CASCADE`, `RESTRICT`/`NO ACTION`, etc.).
- Justificar em comentário SQL por que escolheu cada ação.

### Exercício 8 - Índices básicos
Crie índices para melhorar consultas frequentes:
- Índice no `nome` de `cadastro.aluno`.
- Índice no `id_aluno` de `financeiro.mensalidade`.
- Índice composto em `cadastro.matricula` considerando consultas por `id_curso` e `status_matricula`.

Requisitos:
- Nomear índices seguindo padrão consistente.

---

## Bloco 3 - Evolução do esquema

### Exercício 9 - Alterações de tabela
Faça as alterações abaixo sem recriar tabelas:
- Adicionar coluna `telefone` em `cadastro.aluno`.
- Renomear coluna `nome_curso` para `nome` em `cadastro.curso`.
- Alterar tamanho/tipo de um campo textual para suportar valores maiores.

Requisitos:
- Usar `ALTER TABLE`.
- Validar estrutura final com comando de descrição da tabela.

### Exercício 10 - Tabela de autores e livros
No esquema `biblioteca`, crie:
- `autor` (`id_autor` PK, `nome` obrigatório)
- `livro` (`id_livro` PK, `titulo` obrigatório, `ano_publicacao`)
- `livro_autor` (tabela associativa com PK composta)

Requisitos:
- Relacionamento N:N entre livro e autor.
- Chaves estrangeiras corretas.

### Exercício 11 - Restrições adicionais
Implemente restrições:
- Em `biblioteca.livro`, impedir `ano_publicacao` menor que 1500 e maior que o ano atual.
- Em `cadastro.aluno`, garantir que `email` contenha `@` (restrição simples).

Requisitos:
- Usar `CHECK` quando possível no seu SGBD.

### Exercício 12 - Script completo e organizado
Organize um único script SQL contendo:
1. Criação do banco (se aplicável ao seu SGBD)
2. Criação de esquemas
3. Criação de tabelas
4. Alterações (`ALTER TABLE`)
5. Índices

Requisitos:
- Ordem correta para evitar erros de dependência.
- Comentários breves separando cada etapa.

---

## Desafios extras (se houver tempo)

### Desafio A
Crie uma tabela `professor` em `cadastro` e relacione professor com curso (1:N).

### Desafio B
Adicione uma tabela `emprestimo` em `biblioteca` relacionada a `aluno` e `livro`, com:
- data de empréstimo
- data prevista de devolução
- data de devolução real

### Desafio C
Crie uma visão (view) que mostre aluno + curso + status da matrícula.

---

## Critérios de correção sugeridos
- Comandos executam sem erro.
- PK, FK, `NOT NULL`, `UNIQUE`, `CHECK` e `DEFAULT` aplicados corretamente.
- Nomes claros e padronizados.
- Script organizado e legível.
- Uso correto de relacionamentos entre tabelas.
