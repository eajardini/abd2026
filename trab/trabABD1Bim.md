# Trabalho da disciplina Administração de banco de dados
## 1º Bimestre

**Data da entrega**: Verificar no Moodle.  
**Nota:** 2 pontos.  
**Grupo:** 02 alunos (apenas 1 grupo com 3 alunos).  
**Forma de entrega**: Postar no Moodle o repositório Github do trabalho.  

### Descrição:

1. Os grupos deverão criar um banco de dados para as tabelas de um banco de dados exemplo do Postgres disponível em [PostgreSQL Sample Database](https://neon.com/postgresql/postgresql-getting-started/postgresql-sample-database).
2. O tutorial de importação das tabelas encontra-se em [Load PostgreSQL Sample Database](https://neon.com/postgresql/postgresql-getting-started/load-postgresql-sample-database).
3. O banco de dados contém as seguintes tabelas:
  - actor – armazena dados do ator, incluindo nome e sobrenome.
  - film – armazena dados do filme, como título, ano de lançamento, duração, classificação, etc.
  - film_actor – armazena as relações entre filmes e atores.
  - category – armazena os dados das categorias do filme.
  - film_category- armazena as relações entre filmes e categorias.
  - store –contém os dados da loja, incluindo gerente, equipe e endereço.
  - inventory – armazena dados de estoque.
  - rental – armazena dados de aluguel.
  - payment – armazena os pagamentos dos clientes.
  - staff – armazena dados dos funcionários.
  - customer – armazena dados do cliente.
  - address – armazena dados de endereço para funcionários e clientes.
  - city –  armazena nomes de cidades.
  - country – armazena nomes de países. 

### Atividades avaliativas

Após o banco de dados estiver importados, as atividades avaliadas dos grupos serão:  

1. ( -10.0 pontos) Se não importar o banco de dados corretamente.
2. ( -2.0 pontos) Se não criar a visão:  
   - Escolha uma tabela qualquer e crie uma visão sobre ela.
3. ( -2.0 pontos) Se não criar a visão:
   - Escolha duas tabelas com cardinalidade 1:N e crie uma visão que faça uma junção entre elas.
4. ( -3.0 pontos) Se não criar as sequencias e nem inserir dados nas tabelas:
   - Crie duas sequencias para serem usadas em duas tabelas quaisquer e faça pelo menos dois inserts em cada uma delas.
5. ( -2.0 pontos) Se não criar a visão:
   - Crie uma visão que faça uma união (UNION) entre os nomes das tabelas *customer* e *staff*.
   - 
