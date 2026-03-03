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

*  ** \?** : que mostra ajuda a respeito dos comando do próprio psql. Conhecidos como comando do tipo ”\" (contrabarra).



