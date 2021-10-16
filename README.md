# Projeto Lakehouse (SnowFlake)
### Projeto final do Bootcamp Data Lakehouse da [triggo.ai](https://learning.triggo.ai)

<img src="https://images.unsplash.com/photo-1541420937988-702d78cb9fa1?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8bGFrZWhvdXNlfGVufDB8fDB8fA%3D%3D&ixlib=rb-1.2.1&w=1000&q=80" width="500">

Fonte de dados: [dataset covid19](https://github.com/wcota/covid19br)  

Veja mais detalhes do [Data Lakehouse aqui:](https://anderson-paulucci.medium.com/big-data-com-data-lakehouse-cbd7b4300a81)

## 1) Criando o warehouse "LakeHouse" no SlowFlake: 
   
CREATE WAREHOUSE LakeHouse
WITH WAREHOUSE_SIZE = 'MEDIUM' 
WAREHOUSE_TYPE = 'STANDARD' 
AUTO_SUSPEND = 600 
AUTO_RESUME = TRUE 
MIN_CLUSTER_COUNT = 1 
MAX_CLUSTER_COUNT = 1 
SCALING_POLICY = 'STANDARD';
![Fig1](https://github.com/vsvasconcelos/projSnowFlake/blob/main/fig1.png)

## 2) Criando o database "covid_cidades" e a tabela "Casos":   

CREATE DATABASE covid_cidades;   

CREATE TABLE CASOS
("EPI_WEEK" INTEGER,
"DATE" DATE,
"COUNTRY" STRING,
"STATE" STRING,
"CITY" STRING,                                              
"IBGEID" INTEGER,
"NEWDEATHS" INTEGER,
"DEATHS" INTEGER,                                             
"NEWCASES" INTEGER,
"TOTALCASES" INTEGER,                                                
"DEATHS_PER_100K_INHABITANTS" DOUBLE,                                               
"TOTALCASES_PER_100K_INHABITANTS" DOUBLE,                                                
"DEATHS_BY_TOTALCASES" DOUBLE,                                                
"_SOURCE" STRING,                                             
"LAST_INFO_DATE" DATE);

![Fig2](https://github.com/vsvasconcelos/projSnowFlake/blob/main/fig2.png)

## 3) Carregando o [dataset](https://github.com/wcota/covid19br) na tabela "Casos" no SnowFlake: 
Obs.: Os dados foram carregados zipados (.gz com 43,5MB) pois descompactados (138,4MB) o SnowFlake acusa "limite excedido". 

CREATE FILE FORMAT "COVID_CIDADES"."PUBLIC".cvs_file_format TYPE = 'CSV' COMPRESSION = 'AUTO' FIELD_DELIMITER = ',' RECORD_DELIMITER = '\n' SKIP_HEADER = 1 FIELD_OPTIONALLY_ENCLOSED_BY = 'NONE' TRIM_SPACE = FALSE ERROR_ON_COLUMN_COUNT_MISMATCH = TRUE ESCAPE = 'NONE' ESCAPE_UNENCLOSED_FIELD = '\134' DATE_FORMAT = 'AUTO' TIMESTAMP_FORMAT = 'AUTO' NULL_IF = ('\\N');

![Fig3](https://github.com/vsvasconcelos/projSnowFlake/blob/main/fig3.png)

Delalhe: rows parsed = 1336186, rows loaded = 1336186     

## 4) Criando um Warehouse para Analytics "Analytics_lakehouse"   

CREATE WAREHOUSE Analytics_lakehouse WITH WAREHOUSE_SIZE = 'XLARGE' WAREHOUSE_TYPE = 'STANDARD' AUTO_SUSPEND = 600 AUTO_RESUME = TRUE MIN_CLUSTER_COUNT = 1 MAX_CLUSTER_COUNT = 1 SCALING_POLICY = 'STANDARD';   

## 5) Criar um clone do Database para DEV;   
create table casos_dev clone casos;

![Fig5](https://github.com/vsvasconcelos/projSnowFlake/blob/main/fig5.png)

## 6) Executando consultas que permitam identificar a variação de número de casos de covid e mortes na linha do tempo   

As consultas foram executadas no Jupyter Notebook "snowflow.ipynb" deste repositório.   

![Fig4](https://github.com/vsvasconcelos/projSnowFlake/blob/main/fig4.png)



