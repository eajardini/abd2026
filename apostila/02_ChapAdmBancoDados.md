# Administrador de Banco de Dados (DBA)

 ## Introdução
 
O trabalho do DBA contribui para a operação efetiva de todos os sistemas que rodam com o SGBD Postgres. O DBA dá suporte técnico para todos e espera-se que se torne fluente em todas as questões técnicas que aparecem no SGBD.

O DBA é responsável pelo seguinte:

*  Dia-a-dia do banco de dados;  
*  Instalação e atualização do software;  
*  Otimização do desempenho;  
*  Estratégias de cópia e recuperação;  
*  Reunião com desenvolvedores.

## USUÁRIO COM DIREITOS DE DBA NO POSTGRE

O PostgreSQL possui o usuário **postgres** que é automaticamente criado na instalação e possui direitos de administrador

:exclamation: <mark>Usuário postgres: é o proprietário do dicionário de dados de todos os bancos de dados do SGBD.<mark> 

## FERRAMENTAS DE ADMINISTRAÇÃO (FA)

Gerenciar banco de dados (BD) não é tarefa fácil. Normalmente, o dia a dia de um BD ocorre sem grandes mudanças ou surpresas, porém, quando algo sai da rotina e o BD é afetado por uma queda ou grande lentidão, é necessário que o DBA utilize alguma ferramenta para poder fazer acesso ao SGBD e tentar garantir a normalidade do BD.

Em se tratando do Postgre, existe uma divisão no que refere-se às ferramentas de administração (FA): 

* **Modo linha ou texto**: esse tipo de ferramenta é caracterizado pela ausência de interface gráfica. O *psql* é a FA do modo texto. A vantagem que se tem sobre este tipo de FA é sua presença certa em qualquer sistema operacional (SO) na qual o Postgres esteja instalado.

* **Modo gráfico**: uma ferramenta de administração deste tipo é o *PgAdmin*. Normalmente, também vem instalada na maioria dos SOs que o SGBD instalado. 

Como já usamos o PgAdmin, vamos conhecer um pouco o *psql*.

### Chamando o psql
Para chamarmos o psql, basta digitar no prompt do SO o comando: 

`psql -h localhost -U postgres`

Após estar conectado ao psql, pode-se obter ajudar por meio dos comandos: 

*  **\h** :que mostra ajuda a respeito dos comando SQL. 

<br>
Exemplo:   

`\h alter table;`
<br>

* **`\?`** : que mostra ajuda a respeito dos comando do próprio psql. Conhecidos como comando do tipo ”`\`" (contrabarra).

#### Realizando tarefas no psql com os comando do tipo `”\”`.

1. \l :trazer a lista dos databases existentes.
```sql
\l
```

2.  \c : conectar em um database específico.
```sql
\c postgres
\c abds5
```

3. \i: importar um script no formato sql.
```sql
\c postgres
drop database abds5;
\i /caminho do arquivo/00_criabaseBD_Postgres.sql
```

4. \d: listar os objetos como tabelas, sequencias, etc do database em que está conectado.
```sql
\d

Variações:

\d tabela: descreve a estrutura de uma tabela.
\dt :lista somente os objetos do tipo tabela.
\dv : lista as visões do database.
\di : lista os índices do database.
\df : lista as funções desenvolvidas pelos programadores no database.
\sf nome_da_função: mostra a definição de uma função desenvolvida pelo programador.
\db nome_da_função: lista todas as tablespaces disponíveis.

```

## TAREFAS DO DBA

Vimos alguns exemplos de como podemos utilizar o *psql* para realizar atividades do DBA dentro do SGBD Postgre. A seguir serão apresentadas algumas tarefas desempenhas por esse profissional.

### Conhecendo a versão do Postgres
Para sabendo a versão do SGBD que estamos trabalhando de dentro do psql digite:
```sql
SELECT version();
```

Na linha de comando do Sistema Operacional, digite:
```sql
psql --version
```

### Tempo em que o sgbd está no ar
Para saber quanto tempo o SGBD Postgres está no ar, digite a consulta a seguir:
```sql
SELECT date_trunc('second',
     current_timestamp - pg_postmaster_start_time())
       as uptime;
```

### Criando um Banco de Dados
Um banco de dados é um objeto do SGBD que armazena as tabelas, visões, gatilhos, sequences, etc.  
Para criar um novo banco de dados, utiliza-se o comando:
```sql
CREATE DATABASE nome_do_banco_de_dados [parâmetros];
```

em que os parâmetros podem ser:  
* **OWNER usuário**: é possível informar qual usuário do servidor será responsável pelo banco de dados.  
* **ENCODING valor**: esse argumento é responsável por indicar qual o conjunto de caracteres que o banco de dados irá usar. Para poder utilizar acentos da língua portuguesa, utilize o ENCODING *utf-8*.  
* **TABLESPACE nome**: indica em qual tablespace o BD será armazenado.  
* **CONNECTION LIMIT valor**: indica a quantidade máxima de usuários que poderão se  conectar simultaneamente ao BD. O limite máximo só é observado para usuários que não sejam administradores, dessa forma, para o usuário postgres, não existe limite de conexão. O valor padrão é -1 indicando que não há limite de conexão simultânea.

#### Exemplos
1. Criando o banco de dados escola com *encoding utf-8*:
```sql
CREATE DATABASE escola encoding 'utf-8';
```

2. Criando o banco de dados sisvenda para o usuário usersisvenda:
```sql
-- Com o usuário postgres faça:
create user usersisvenda WITH ENCRYPTED PASSWORD '123456';

CREATE DATABASE sisvenda encoding 'utf-8' owner usersisvenda;

-- Abra outro psql desta forma:
psql -h localhost -U usersisvenda -d sisvenda
```

3. Criando um banco de dados com limite de acesso a um usuário por vez. Lembrando que o usuário *postgres* não possui esse limite:

```sql
CREATE DATABASE dblimite connection limit 1 owner usersisvenda;

-- Em outro psql tente se conectar (vai dar certo):
psql -h localhost -U usersisvenda -d dblimite

-- Novamente, em outro psql tente se conectar (o limite de conexão vai segurar):
psql -h localhost -U usersisvenda -d dblimite

-- Para aumentar as conexões, faça:

ALTER DATABASE dblimite connection limit -1;

-- Agora tente se conectar novamente:
psql -h localhost -U usersisvenda -d dblimite
```

### Visualizando os Banco de Dados
Para visualizar os banco de dados no psql, digite:  

*\l*

Já por meio de SQL, escreva o comando a seguir:
```sql
select datname from pg_database;
```

### Conectando a um Banco de Dados
Para conectarmos a um banco de dados no psql, digite:  
```
\c nome_database
```

Por exemplo, para conectarmos ao database *sisvenda*, fazemos:  
```
\c sisvenda
```
