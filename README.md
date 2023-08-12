# Case Banco de Dados Sistema de Vendas

No projeto a seguir vamos conferir como criar um banco de dados no SGBD PostgreSQL para um sistema de vendas de uma loja.


## 1. Informações necessárias para a montagem do Banco de Dados

A seguir descrevemos os Requisitos do cliente e algumas informações obtidas pela equipe de desenvolvimento durante a Reunião de brainstorming realizada para o projeto.

1. Preciso de um banco de dados para controlar as vendas diária dos
produtos da minha empresa, também precisarei saber se meus
vendedores tem dependentes para poder mandar brindes em
ocasiões especiais.

2. Aqui trabalhamos tanto em loja própria, quanto em loja virtual.

3. Quais informações precisa contar na sua venda?
• Data
• Quantidade
• Valor unitário e total
• Clientes (nome, sexo, estado, idade)

4. Em relação aos vendedores, Eles recebem comissão?
• Sim, recebem

5. Em relação aos dependentes, alguma informação específica
deseja ter?
• Sim, o código da Escola que Ele estuda.

6. Qual seria esse código?
• Código do INEP

7. Em relação aos produtos que vendem, alguma característica que
desejam controlar?
• Nosso produto tem uma classificação por tipo de produto,
tipo A, B...

8 Mais alguma necessidade que deseja nos informar?
• Normalmente, nossos clientes tem uma classificação com
base nas compras que já fizeram, se puderem adicionar...

## 2. Desenvolvimento do Modelo de Entidade e Relacionamento (MER)

Com base nas informações obtidas, foi realizado o desenvolvimento do 
Do Modelo conceitual e o Modelo de Entidade e Relacionamento (MER).

Pelo que coletamos do cliente, no nosso banco de dados, teremos
4 entidades
• Vendas
• Vendedores
• Dependentes
• Produto

Tabela Vendas (tbven)
• cdven (pk)
• dtven
• cdcli
• nmcli
• agecli
• clacli
• sxcli
• cidcli
• estcli
• paiscli
• qtven
• vruven
• vrtven
• canal
• stven
• deleted
• cdvdd* (fk)
• cdpro* (fk)

Tabela Vendedores (tbvdd)
• cdvdd (pk)
• nmvdd
• sxvdd
• perccomissao
• matfunc

Tabela Produto (tbpro)
• cdpro (pk)
• nmpro
• tppro
• undpro
• slpro
• stpro

## 3. Criando o Banco de Dados

Após as informações obtidas e desenvolvimento dos Modelos Conceitual e Lógico da Entidade e Relacionamento (MER), iniciamos o desenvolvimento do Banco de Dados.

```sql
CREATE DATABASE db_da_sistema_vendas
```

## 3.1 Criando a tabela Tbpro - Produto

```sql
CREATE TABLE IF NOT EXISTS tbpro (
	cdpro integer PRIMARY KEY,
	nmpro varchar(100),
	tppro varchar(100),
	undpro integer,
	slpro integer,
	stpro integer
)
```

## 3.2 Criando a tabela tbven_item - Item de venda

```sql
CREATE TABLE IF NOT EXISTS tbven_item (
	cdvenitem integer primary key,
	qtven numeric,
	CHECK (qtven >= 0),
	vluven numeric,
	CHECK (vluven >= 0),
	vltven numeric,
	CHECK (vltven >= 0), 
	tppro varchar(100),
	cdpro int references tbpro(cdpro)
)
```

## 3.3  Criando a tabela tbvdd- Vendedor

```sql
CREATE TABLE IF NOT EXISTS tbvdd (
	cdvdd int primary key,
	nmvdd varchar(100),
	sxvdd varchar(10),
	perccomissao numeric,
	CHECK (perccomissao >= 0),
	CHECK (perccomissao <= 1),
	matfunc int
)
```

## 3.4  Criando a tabela tbven - Venda

```sql
CREATE TABLE IF NOT EXISTS tbven (
	cdven int primary key,
	dtven date,
	cdcli int, 
	nmcli varchar(100),
	clacli varchar(50),
	sxcli varchar(10),
	cidcli varchar(50),
	estcli varchar(50),
	paiscli varchar(50),
	canal varchar(50),
	stven varchar(20),
	deleted varchar(3),
	cdvdd int references tbvdd(cdvdd),
	cdpro int references tbpro(cdpro),
	cdvenitem int references tbven_item(cdvenitem)
)
```

## 3.5  Criando a tabela tbdep - Dependentes

```sql
CREATE TABLE IF NOT EXISTS tbdep (
	cddep int primary key,
	nmdep varchar(100),
	dtnasc date,
	sxdep varchar(10),
	inepescola int,
	cdvdd int references tbvdd(cdvdd)
)
```


## 4 Inserindo dados nas Tabelas

Agora com o banco de dados criado, podemos inserir as infromações no sistema por diversas formas, como por exemplo
• Documentos e registros em sistemas em ambiente visual/software;
• Através de Formulários e planilhas;
• Entrevistas e Questionários e levantamentos;
• Planilhas, entre outros;

Dessa forma, para testar o banco e tabelas criadas, vamos popular algumas tabelas para praticar algumas funções INSERT.

## 4.1 Inserindo dados na tabela VENDEDOR função INSERT (DML)

```sql
INSERT INTO tbvdd (cdvdd, nmvdd, sxvdd, perccomissao, matfunc) VALUES (0001, 'LUIZ SILVA', 'masculino', 0.05, 0001);
INSERT INTO tbvdd (cdvdd, nmvdd, sxvdd, perccomissao, matfunc) VALUES (0002, 'LAIS GABRIELLE', 'feminino', 0.05, 0002);
INSERT INTO tbvdd (cdvdd, nmvdd, sxvdd, perccomissao, matfunc) VALUES (0003, 'BRUNA POLICARPO', 'feminino', 0.05, 0003);
INSERT INTO tbvdd (cdvdd, nmvdd, sxvdd, perccomissao, matfunc) VALUES (0004, 'BHRENDA BASTOS', 'feminino', 0.05, 0004);
INSERT INTO tbvdd (cdvdd, nmvdd, sxvdd, perccomissao, matfunc) VALUES (0005, 'SERGIO BRAGA', 'masculino', 0.05, 0005);
```

## 4.2 Inserindo dados na tabela DEPENDENTES DE VENDEDOR função INSERT (DML)

```sql
INSERT INTO tbdep (cddep,nmdep,dtnasc,sxdep,inepescola,cdvdd) VALUES (0001,'MARJORIE POLICARPO',TIMESTAMP '2020-11-25','feminino',23268093,0003);
```

## 4.3 Inserindo dados na tabela PRODUTO - função INSERT (DML)

```sql
INSERT INTO tbpro(cdpro,nmpro,tppro,undpro,slpro,stpro) VALUES (0001,'BOLSA FEMININA TIRACOLO MODELO SB-TPA-2015','TIPO A',10,150,150);
INSERT INTO tbpro(cdpro,nmpro,tppro,undpro,slpro,stpro) VALUES (0002,'BOLSA FEMININA PRAIA PALHA MODELO SB-TPA-2233','TIPO A',10,120,120);
```

## 4.4  Inserindo dados na tabela ITEM DE VENDA - função INSERT (DML)

```sql
INSERT INTO tbven_item(cdvenitem,qtven,vluven,vltven,tppro,cdpro) VALUES (000000000001,1,150,150,'TIPO A',0001);
INSERT INTO tbven_item(cdvenitem,qtven,vluven,vltven,tppro,cdpro) VALUES (000000000002,1,120,120,'TIPO A',0002);
```

## 4.5  Inserindo dados na tabela VENDA função INSERT (DML)

```sql
INSERT INTO tbven(cdven,dtven,cdcli,nmcli,clacli,sxcli,cidcli,estcli,paiscli,canal,stven,deleted,cdvdd,cdpro,cdvenitem) VALUES (000000000001, TIMESTAMP '2023-04-29 18:55:06',0000001,'MARIA CLARA SOUZA','A','feminino','FORTALEZA','CE','BRASIL','LOJA FISICA FOR01','CONCLUIDA','NAO',0003,0001,000000000001);
INSERT INTO tbven(cdven,dtven,cdcli,nmcli,clacli,sxcli,cidcli,estcli,paiscli,canal,stven,deleted,cdvdd,cdpro,cdvenitem) VALUES (000000000002, TIMESTAMP '2023-04-29 19:30:06',0000002,'FERNANDA AMORIM BASTOS','A','feminino','FORTALEZA','CE','BRASIL','LOJA FISICA FOR01','CONCLUIDA','NAO',0003,0002,000000000002);
```

## 5. Consultando as tabelas após popular os dados

Agora com as tabelas populadas no Banco de Dados, vamos utilizar as funções SELECT para consultar algumas tabelas publicadas.

## 5.1 Consultando a tabela VENDEDOR - função SELECT (DQL)

```sql
SELECT * FROM tbvdd;
```

## 5.2 Consultando a tabela DEPENDENTES DE VENDEDOR - função SELECT (DQL)

```sql
SELECT * FROM tbdep;
```

## 5.3 Consultando a tabela PRODUTO - função SELECT (DQL)

```sql
SELECT * FROM tbpro;
```

## 5.4 Consultando a tabela ITEM DE VENDA - função SELECT (DQL)

```sql
SELECT * FROM tbven_item;
```

## 5.5 Consultando a tabela VENDA - função SELECT (DQL)

```sql
SELECT * FROM tbven;
```

## 6. Considerações Finais

Bom, após todo o desenvolvimento apresentado acima, concluímos o desenvolvimento do banco de dados e estrutura de tabelas de acordo com as necessidades e exigências do cliente. 

Para mais informações ou detalhes, entre em contato atráves do [e-mail](engluizpolicarpo)

**Luiz Policarpo**



