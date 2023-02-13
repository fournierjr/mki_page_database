# mki_page_database
CREATE TABLE colaboradores (
  matricula SERIAL PRIMARY KEY NOT NULL,
  nome VARCHAR(30),
  salario FLOAT,
  cargo VARCHAR(20)
);

CREATE TABLE projetos (
  id_projeto SERIAL PRIMARY KEY NOT NULL,
  nome_projeto VARCHAR(30),
  prazo_projeto VARCHAR(10),
  orcamento_projeto FLOAT
);

CREATE TABLE clientes (
  cnpj INT PRIMARY KEY NOT NULL,
  nome_cliente VARCHAR(30),
  email_cliente VARCHAR(30),
  fone_cliente VARCHAR(13),
  contato_cliente VARCHAR(30)
);

INSERT INTO clientes (cnpj, nome_cliente, email_cliente, fone_cliente, contato_cliente) VALUES
(1, 'Carrier Ltda', 'carrier@gmail.com', '2134556789', 'Carlos Souza'),
(2, 'Express Ltda', 'express@gmail.com', '2132789056', 'João Mendes'),
(3, 'Bentô Ltda', 'bento@gmail.com', '2134789021', 'Henrique Sá'),
(4, 'Spring Ltda', 'spring@gmail.com', '2138903455', 'Sérgio Lopes'),
(5, 'Moura Ltda', 'moura@gmail.com', '2137654839', 'Tomas Moura');

INSERT INTO colaboradores (matricula, nome, salario, cargo) VALUES
(1, 'Maria Clara', 1800, 'Tech Jr'),
(2, 'Leila Pinheiro', 3700, 'Desenvolvedor Jr'),
(3, 'Cristian Peres', 6700, 'Desenvolvedor Pleno'),
(4, 'Carla Dias', 4800, 'Analista Jr'),
(5, 'Denis Pereira', 7600, 'Analista Senior'),
(6, 'Francisco Cruz', 8000, 'Gerente Projeto I'),
(7, 'Ana Paula', 9000, 'Gerente Projeto II');

INSERT INTO projetos (id_projeto, nome_projeto, prazo_projeto, orcamento_projeto) VALUES
(1, 'Ampliação Base Dados', '24', 30000),
(2, 'Migração de Tecnologia', '16', 20000),
(3, 'Percepção de Padrões', '20', 45000),
(4, 'Gerenciamento de Crise', '36', 40000);

CREATE TABLE colaboradores_projetos (
	fk_matricula INT,
  fk_id_projeto INT,
  CONSTRAINT colaboradores_projetos_pk PRIMARY KEY (fk_matricula, fk_id_projeto),
  CONSTRAINT matricula_fk FOREIGN KEY (fk_matricula) REFERENCES colaboradores (matricula),
  CONSTRAINT id_projeto_fk FOREIGN KEY (fk_id_projeto) REFERENCES projetos (id_projeto)
); 

CREATE TABLE projetos_clientes (
	fk_id_projeto INT,
	fk_cnpj INT,
  	CONSTRAINT projetos_clientes_pk PRIMARY KEY (fk_id_projeto, fk_cnpj),
  	CONSTRAINT id_projeto_fk FOREIGN KEY (fk_id_projeto) REFERENCES projetos (id_projeto),
  	CONSTRAINT cnpj_fk FOREIGN KEY (fk_cnpj) REFERENCES clientes (cnpj)
); 

INSERT INTO colaboradores_projetos (fk_matricula, fk_id_projeto)
VALUES (1, 1), (2, 1), (3,3), (4,4), (5,3), (6,2), (7,3);

INSERT INTO projetos_clientes (fk_id_projeto, fk_cnpj)
VALUES (4, 1), (3, 2), (2,3), (1,4);

-- consulta 01 - Exibir os dados dos clientes que não tem projetos contratados
SELECT * FROM clientes
LEFT JOIN projetos
ON id_projeto = clientes.cnpj
WHERE projetos.id_projeto IS NULL;

-- consulta 02 - Exibir os dados dos clientes, independente de terem contratado projeto
SELECT * FROM clientes
LEFT JOIN projetos_clientes
ON clientes.cnpj = projetos_clientes.fk_cnpj;

-- consulta 03 - exibir o nome dos colaboradores e os nomes dos projetos/clientes a eles vinculados
SELECT colaboradores.nome, projetos.nome_projeto, clientes.nome_cliente 
from colaboradores
LEFT JOIN colaboradores_projetos
ON colaboradores_projetos.fk_matricula = colaboradores.matricula
LEFT JOIN projetos_clientes
ON projetos_clientes.fk_id_projeto = colaboradores_projetos.fk_id_projeto
LEFT JOIN clientes
ON projetos_clientes.fk_cnpj = clientes.cnpj
LEFT JOIN projetos
ON projetos.id_projeto = projetos_clientes.fk_id_projeto;
