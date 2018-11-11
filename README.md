# Investigating the performance of Moodle database in cloud computing

## Command used to execute Pgbench:

$ pgbench -t 100 -c 5 -S bdpgbench -U postgres -n 

$ pgbench -c 20 -j 2 -T 120 moodle -U moodle -n >> exitpgbench.txt

$ drop table pgbench_accounts, pgbench_branches, pgbench_history, pgbench_tellers;

$ pgbench -c 40 -j 2 -T 120 moodle -U moodle -n >> exitpgbench.txt

$ pgbench -c 80 -j 2 -T 120 moodle -U moodle -n >> exitpgbench.txt

$ pgbench -c 160 -j 2 -T 120 moodle -U moodle -n >> exitpgbench.txt

$ pgbench -c 320 -j 2 -T 120 moodle -U moodle -n >> exitpgbench.txt



A explicação do comando é a seguinte:

    -t -> número de transações
    -c -> significa o número de clientes conectados, não deve superar o número de conexões disponíveis no seu postgresql.conf
    -S -> determina o tipo de transação, SELECT only


## Command used to execute Sysbench:

O comando abaixo gera 100.000 linhas nas 24 tabelas dentro do banco sbtest.

$ sysbench
--db-driver=pgsql
--oltp-table-size=100000
--oltp-tables-count=24
--threads=1
--pgsql-host=127.0.0.1
--pgsql-port=5432
--pgsql-user=sbtest
--pgsql-password=password
--pgsql-db=sbtest
/usr/share/sysbench/tests/include/oltp_legacy/parallel_prepare.lua
run

Em seguida, é gerado cargas de testes. O comando gerará a carga de trabalho, que consta no script da penúltima linha, em 100.000 linhas de 24 tabelas com 64 threads de trabalho por 60 segundos no host 127.0.0.1. A cada 2 segundos, o sysbench irá reportar as estatísticas.

$ sysbench
--db-driver=pgsql
--report-interval=2
--oltp-table-size=100000
--oltp-tables-count=24
--threads=64
--time=60
--pgsql-host=127.0.0.1
--pgsql-port=5432
--pgsql-user=sbtest
--pgsql-password=password
--pgsql-db=sbtest
/usr/share/sysbench/tests/include/oltp_legacy/oltp.lua
run >> exitsysbench.txt
