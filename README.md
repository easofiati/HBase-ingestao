[![Hbase](https://hbase.apache.org/images/hbase_logo_with_orca_large.png)](https://hbase.apache.org/)

# Ingestão de Dados

Nesse tutorial, de uma maneira simples e objetiva irei mostrar uma alternativa de ingestão de dados no HBase, através do utilitário ImportTsv, que se utiliza do modo Bulk Load. Utilitário esse que citei no tutorial [Hbase - Coprocessor](https://github.com/easofiati/HBase-coprocessor).
Primeiramente, contextualizando a situação, iremos fazer a ingestão de um arquivo CSV do HDFS para o HBASE. Para esse tutorial utilizei a VM da Cloudera, não precisando assim realizer nenhum tipo de instalação.
Bom, chega de papo e vamos por a mão na massa.
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
Pronto! Arquivo importado no HBase, não vou entrar no mérito de explicar exatamente a síntaxe acima com cada um de seus parâmetros, até mesmo porque tentei deixá-la o mais simples possível, porém no final deste artigo deixo o link de referência da própria Apache, onde consta toda a documentação. Existe outra alternativa neste mesmo utilitário, que seria gerar o arquivo do HBase e somente depois carregar o arquivo incremental, muda-se poucos parâmetro, mas isso é assunto para uma outra hora, entretanto deixarei também nos links alguns artigos sobre isso para que possam explorar essa alternativa. Por fim, espero ter contribúido para gerar novas possibilidades de se realizar ingestão de dados no HBase e com isso, você tenha possibilidades de fazer testes com diversas opções e descobrir qual a melhor para cada caso.
Abaixo segue alguns links de referência:
- [http://hbase.apache.org/0.94/book/ops_mgt.html#importtsv] (http://hbase.apache.org/0.94/book/ops_mgt.html#importtsv)
- [https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/mapreduce/ImportTsv.html] (https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/mapreduce/ImportTsv.html)
- [https://blog.cloudera.com/blog/2013/09/how-to-use-hbase-bulk-loading-and-why/] (https://blog.cloudera.com/blog/2013/09/how-to-use-hbase-bulk-loading-and-why/)
