# Presto Server Setup / Iceberg Server

This document is for iceberg specific connector.
After all the setup and configuration. Open up your terminal in IntelliJ.

1. Paste the following command into the Terminal:  
`./presto-cli/target/presto-cli-0.293-SNAPSHOT-executable.jar --catalog memory --schema default`
2. This will be presto:default>
3. To use a iceberg specific connector do  **use iceberg.tpch;** in terminal
4. Once in presto:tpch>, do **create schema iceberg.default;**
5. Then verify you are using iceberg tables with **show tables;**
6. You can then run a simple query such as `SELECT * FROM orders WHERE orderkey < 100;`
7. Hit the Shift key Twice and type in `IcebergQueryRunner` and click the Green Play button next to the class name.
8.  This will load up the Presto Server specific with the Iceberg Connector.
9. Then Check your localhost:8080 to check the Server UI
10. Click on the `Finished` box under `Query Details` to show the Queries you ran. 


That's essentially the basics. You can click each Query to gather more data, even check out some Runtime Statistics (which I partly help implement).
