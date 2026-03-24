--Exercício sobre Trigger

Desenvolva um gatilho para monitorar a alteração dos endereços dos clientes. Toda vez que um cliente tiver seu endereço alterado por meio de um comando UPDATE, a alteração deve ser registrada por meio de um INSERT em uma tabela de log. Assim, na função do Trigger deve haver um comando INSERT e o evento do Trigger deve ser BEFORE UPDATE.
A estrutura da tabela sera:...

```
CREATE OR REPLACE FUNCTION registra_endereco()  RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO  log_clientes
    	     VALUES (DEFAULT, old.codigo_cliente, old.endereco, new.endereco, current_date);   
    
    RETURN NEW;
END;
$$
LANGUAGE plpgsql;

CREATE TRIGGER trg_bloquear_exclusao_cliente
BEFORE UPDATE 
ON cliente
FOR EACH ROW
EXECUTE FUNCTION registra_endereco();


```

2) Desenvolva um trigger que evite a venda de um produto cujo estoque seja menor que a quantidade vendida.
Porém, caso haja estoque, deverá ser dado baixa no item no estoque. O trigger deverá ser criado sob a tabela item_pedido. Toda vez que um registro for inserido nela, antes da inserção (BEFORE), o trigger deverá verificar se existe estoque suficiente na tabela produto. Você deverá criar uma variável na função que receberá a quantidade atual em estoque (tabela produto) por meio de um select into.
Em seguinda, deverá ser comparada a quantidade a ser vendida (variável NEW) com a quantidade em estoque (obtida pelo select into).
Caso aquela seja menor ou igual a quantidade em estoque, será efetuada a baixa no estoque,
caso contrário será gerado um erro com o comando Raise Exception impossibilitando a operação.	



```
CREATE OR REPLACE   FUNCTION f_BaixaEstoque() returns TRIGGER 
as $$
DECLARE 
	qtdeEstoque produto.quantidade%type;

BEGIN
    SELECT quantidade INTO qtdeEstoque
	FROM produto
	WHERE codigo_produto = new.codigo_produto;

    IF (qtdeEstoque >= new.quantidade) THEN
        UPDATE produto
        SET quantidade = quantidade - new.quantidade
        WHERE codigo_produto = new.codigo_produto;
    ELSE
         raise 'Quantidade insuficiente de produto'
            using ERRCODE = 'ER003';        
    END IF;

    return new;
END;
$$
LANGUAGE plpgsql;

CREATE TRIGGER tr_baixa_estoque
BEFORE INSERT
ON item_pedido 
FOR EACH ROW
EXECUTE PROCEDURE f_BaixaEstoque();

select * from produto;
select * from item_pedido;
-- Vai dar certo
INSERT INTO item_pedido
    VALUES (121, 25, 5, (SELECT valor_venda
                            FROM produto
                           WHERE codigo_produto = 25), 
                           (SELECT valor_custo
                            FROM produto
                           WHERE codigo_produto = 25) );

-- O trigger não vai deixar dar baixa no estoque
INSERT INTO item_pedido
    VALUES (121, 31, 15, (select valor_venda
                            FROM produto
                           WHERE codigo_produto = 31), 
                           (select valor_custo
                            FROM produto
                           WHERE codigo_produto = 31) );
    select * from produto;
    select * from item_pedido;
```


(ENADE) Em um Banco de Dados PostgreSQL, Joana precisa criar uma trigger para inserir dados na tabela de auditoria chamada AGENTE_AUDIT todas as vezes que um registro da tabela AGENTE for efetivamente excluído. Para isso, considerando que a função "agente_removido()" já esteja implementada, Joana utilizará o comando:  
A. CREATE TRIGGER audit_agente AFTER DELETE ON agente_audit FOR EACH STATEMENT EXECUTE PROCEDURE agente_removido();  
B. CREATE TRIGGER audit_agente AFTER EXCLUDE ON agente FOR EACH ROW EXECUTE PROCEDURE agente_removido();  
C. CREATE EVENT TRIGGER audit_agente AFTER DELETED ON agente FOR EACH ROW EXECUTE PROCEDURE agente_removido();  
**D.[X]**  CREATE TRIGGER audit_agente AFTER DELETE ON agente FOR EACH ROW EXECUTE PROCEDURE agente_removido();  
E. CREATE EVENT TRIGGER audit_agente AFTER DELETE ON agente_audit FOR EACH STATEMENT EXECUTE PROCEDURE agente_removido();  
