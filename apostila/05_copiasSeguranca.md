# Cópias de Segurança por meio de backup

## Introdução

A realização de cópias de segurança, denominadas de backup, é uma tarefa essencial no dia a dia do DBA. Uma vez que o DBA é o responsável pelo Banco de Dados, qualquer problema que venha ocorrer neste,
é de responsabilidade do DBA. Ele é o responsável por deixar o banco sempre em produção e se ocorrer  algum problema de perda de informação, ele deve providenciar e recuperar os dados o mais rápido possível.

Existem diversos fatores que podem causar perdas de dados, alguns deles são:  
  1. Comandos mal utilizados como drop, delete, update, etc....
  2. Usuários mal-intencionados
  3. Roubos de dados
  4. Vírus
  5. Falha no hardware, etc

## TIPOS DE BACKUPS

Existem várias formas de realizar *backup*. A mais simples é dar shutdown no banco e copiar todos os arquivos necessários com o banco *Offline*. Isso seria um **Cold _backup_**. Embora seja uma das formas mais fáceis para posterior *restore/recover*, não é a mais recomendada, pois existem bases que trabalham 24/7 ou seja, parar um banco de dados é difícil. A outra forma de fazer backup é o **Hot _backup_**,  realizado com o banco de dados em operação.

Basicamente, existem três tipos de backups que podem ser feitos em um SGBD, são eles:

  - **Backups Completos**: este tipo **consiste na cópia de todos os arquivos para a mídia de backup**. Se os dados sendo copiados nunca mudam, cada backup completo será igual aos outros, ou seja, todos os arquivos serão copiados novamente. Isso ocorre devido o fato que um backup completo não verifica se o arquivo foi alterado desde o último backup; copia tudo indiscriminadamente para a mídia de backup, tendo modificações ou não. Esta é a razão pela qual os backups completos não são feitos o tempo todo. Por este motivo os backups incrementais foram criados
    
  - **Backups Incrementais**: ao contrário dos backups completos, o processo de backup incremental primeiro verifica se o horário de alteração de um arquivo no disco rígido é mais recente que o horário de seu último backup. Se não for, o arquivo não foi modificado desde o último backup e pode ser ignorado desta vez. Assim, no **backup incremental, só será feita a cópia de segurança dos arquivos que toram modificados desde o último backup completo**. Por outro lado, se a data de modificação é mais recente que a data do último backup, o arquivo foi modificado e deve ter seu backup feito. Os backups incrementais são usados em conjunto com um backup completo frequente (ex.: um backup completo semanal, com incrementais diários).
    A vantagem principal em usar backups incrementais é que rodam mais rápido que os backups completos. A principal desvantagem dos backups incrementais é que para restaurar um determinado arquivo, pode ser necessário procurar em um ou mais backups incrementais até encontrar o arquivo. Para restaurar um sistema de arquivo completo, é necessário restaurar o último backup completo e todos os backups incrementais subsequentes. Numa tentativa de diminuir a necessidade de procurar em todos os backups incrementais, foi implementada uma tática ligeiramente diferente. Esta é conhecida como backup diferencial.

    - **Backups Diferenciais**: são similares aos backups incrementais, pois ambos podem fazer backup somente de arquivos modificados. No entanto, **os backups diferenciais são acumulativos**, em outras palavras, no caso de um backup diferencial, uma vez que um arquivo foi modificado, este continua a ser incluso em todos os backups diferenciais (obviamente, até o próximo backup completo). Isto significa que cada backup diferencial contém todos os arquivos modificados desde o último backup completo, possibilitando executar uma restauração completa somente com o último backup completo e o último backup diferencial. Assim como a estratégia utilizada nos backups incrementais, os backups diferenciais normalmente seguem a mesma tática: um único backup completo periódico seguido de backups diferenciais mais frequentes. O efeito de usar backups diferenciais desta maneira é que estes tendem a crescer um pouco ao longo do tempo (assumindo que arquivos diferentes foram modificados entre os backups completos). Isto posiciona os backups diferenciais em algum ponto entre os backups incrementais e os completos em termos de velocidade e utilização da mídia de backup, enquanto geralmente oferecem restaurações completas e de arquivos mais rápidas (devido o menor número de backups onde procurar e restaurar).
   
## FORMAS DE BACKUPS NO POSTGRE



