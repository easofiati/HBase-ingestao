[![Hbase](https://hbase.apache.org/images/hbase_logo_with_orca_large.png)](https://hbase.apache.org/)

# Ingestão de Dados

Nesse tutorial, de uma maneira objetiva irei mostrar uma alternativa de ingestão de dados no HBase, através do utilitário ImportTsv, que se utiliza do modo Bulk Load.  
Primeiramente, contextualizando a situação, iremos fazer a ingestão de um arquivos CSV do HDFS para o HBASE. 

1. Fazer download do arquivo [people.csv](https://github.com/easofiati/HBase-Ingestao/blob/master/people.csv)
2. Copiar o arquivo people.csv para o HDFS. Caso não exista a pasta /tmp, então crie-a.
```sh
hdfs dfs -put  /tmp
```
3. Crie a tabela e a família de coluna no HBase.
```sh
--Síntaxe
create '<table_name>', {NAME=>'<name_column_family>'}

--Exemplo
create 'tbl_002', {NAME=>'pessoa'}
```
4. Copiar o arquivo CSV para o HDFS.
```
hdfs dfs -put people.csv /tmp
```
5. Agora iremos importar os arquivos diretamente do arquivo CSV que se encontra no HDFS diretamente para a tabela do HBase.
```sh
hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.separator=',' -Dimporttsv.columns=HBASE_ROW_KEY,person:name,person:email,person:gender,person:country tbl_002 hdfs:///tmp/people.csv
```
Não vou entrar no mérito de explicar exatamente a sintaxe acima com cada um de seus parâmetros, até mesmo porque tentei deixá-la o mais simples possível, porém no final deste artigo deixo o link de referência da própria Apache, onde consta toda a documentação.

Esse tutorial é apenas para mostrar uma alternativa de ingestão de dados, ao qual citei no tutorial [Hbase - Coprocessor](https://github.com/easofiati/HBase-coprocessor). Dentro desse contexto, existem inúmeros materiais para estudo, mas o importante é ter em mente que existem alternativas, ou seja, a melhor alternativa é testar cada uma delas e verificar qual a melhor performance.
