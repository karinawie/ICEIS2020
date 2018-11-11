# Investigating the performance of Moodle database in cloud computing

## Command used to execute Pgbench:

$ pgbench -i -s 10 moodle -U moodle

$ pgbench -c 20 -j 2 -T 120 moodle -U moodle -n >> exitpgbench.txt

$ drop table pgbench_accounts, pgbench_branches, pgbench_history, pgbench_tellers;

// increasing clients to 40, 80, 160, 280.

    -t -> número de transações
    -c -> significa o número de clientes conectados, não deve superar o número de conexões disponíveis no seu postgresql.conf
    -S -> determina o tipo de transação, SELECT only


## Command used to execute Sysbench:

//CONFERIR $ sysbench --db-driver=pgsql --oltp-table-size=100000 --oltp-tables-count=24 --pgsql-host=127.0.0.1 --pgsql-port=5432 --pgsql-user=sbtest --pgsql-password=sbtest --pgsql-db=moodle /usr/share/sysbench/tests/include/oltp_legacy/parallel_prepare.lua cleanup

$ sysbench --db-driver=pgsql --oltp-table-size=100000 --oltp-tables-count=24 --pgsql-host=127.0.0.1 --pgsql-port=5432 --pgsql-user=sbtest --pgsql-password=sbtest --pgsql-db=moodle /usr/share/sysbench/tests/include/oltp_legacy/parallel_prepare.lua run

$ sysbench --db-driver=pgsql --oltp-table-size=100000 --oltp-tables-count=24 --threads=20 --pgsql-host=127.0.0.1 --pgsql-port=5432 --pgsql-user=sbtest --pgsql-password=sbtest --pgsql-db=moodle /usr/share/sysbench/tests/include/oltp_legacy/oltp.lua run>> exitsysbench.txt 

//increasing threads to: 40, 80, 160, 250.

Em seguida, é gerado cargas de testes. O comando gerará a carga de trabalho, que consta no script da penúltima linha, em 100.000 linhas de 24 tabelas com 64 threads de trabalho por 60 segundos no host 127.0.0.1. A cada 2 segundos, o sysbench irá reportar as estatísticas.
