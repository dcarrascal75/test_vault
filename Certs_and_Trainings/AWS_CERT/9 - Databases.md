
- Relacionales (SQL / OLTP) 
- NoSQL: DynamoDB, elasticsche, Neptune, 
- Almacenaje de objetos: S3, Glacier
- Warehouse: RedShift, Athena, EMR
- Search: Opensearch
- Grafos: Neptune
- Ledger: Quantum (QLDB)
- Time Series: TimeStream

#### Comparativa de las Bases de Datos en AWS

| **Servicio de Base de Datos** | **Tipo**                       | **Casos de Uso Principales**                                              | **Modelo de Datos**                                     | **Escalabilidad**                             | **Gestión Automática**                             | **Compatibilidad**                             |
| ----------------------------- | ------------------------------ | ------------------------------------------------------------------------- | ------------------------------------------------------- | --------------------------------------------- | -------------------------------------------------- | ---------------------------------------------- |
| **Amazon RDS**                | Relacional                     | Aplicaciones empresariales, ERP, CRM                                      | Tablas y relaciones SQL                                 | Escalado vertical y horizontal (lecturas)     | Backups, parcheo, y replicación                    | MySQL, PostgreSQL, MariaDB, Oracle, SQL Server |
| **Amazon Aurora**             | Relacional                     | Aplicaciones empresariales, SaaS, juegos                                  | Tablas y relaciones SQL                                 | Escalado automático, alta disponibilidad      | Backups, parcheo, y replicación                    | MySQL, PostgreSQL                              |
| **Amazon DynamoDB**           | NoSQL (key-value y documentos) | Aplicaciones móviles, IoT, gaming, e-commerce                             | Tablas con ítems de key-value y documentos JSON         | Escalado automático y global                  | Backups, replicación, y administración del tráfico | N/A (API basada en JSON y key-value)           |
| **Amazon Redshift**           | Almacén de datos               | Análisis de grandes volúmenes de datos, BI                                | Tablas relacionales optimizadas para consultas          | Escalado automático y nodos de almacenamiento | Backups, parcheo, y copias de seguridad            | SQL compatible con PostgreSQL                  |
| **Amazon ElastiCache**        | Cache en memoria               | Aceleración de aplicaciones, sesiones de usuario, análisis en tiempo real | Key-value en memoria                                    | Escalado horizontal (sharding)                | Backups, recuperación, y replicación               | Redis, Memcached                               |
| **Amazon DocumentDB**         | NoSQL (documentos)             | Aplicaciones web y móviles, catálogos, gestión de contenido               | Documentos JSON                                         | Escalado automático                           | Backups, recuperación, y replicación               | API compatible con MongoDB                     |
| **Amazon Neptune**            | Base de datos de grafos        | Redes sociales, recomendaciones, detección de fraudes                     | Grafos (Propiedad y RDF)                                | Escalado automático                           | Backups, recuperación, y replicación               | Gremlin, SPARQL                                |
| **Amazon Timestream**         | Serie temporal                 | IoT, DevOps, análisis de eventos                                          | Datos de series temporales                              | Escalado automático                           | Gestión de retención y consulta                    | N/A (API optimizada para series temporales)    |
| **Amazon Keyspaces**          | NoSQL (wide column)            | Aplicaciones con grandes volúmenes de datos, IoT, gaming                  | Tablas con columnas anchas (modelo similar a Cassandra) | Escalado automático                           | Backups, recuperación, y replicación               | Compatible con Apache Cassandra                |


#### RDS

Posgre, MySQL, Oracle, SQL server, DB2, Maria
EBS volumes
Auto escalado, read replicas, MultiAZ
IAM, SG, KMS, SSL in transit
Backup automatico configurable hasta 35 dias
Snapshot manual que no se borra

#### Aurora: 

Posgre - mysql

==Los **datos** se almacenan **en 6 replicas en 3 AZ** --> High avail.==

ojo, los datos, las instancias son otra cosa

##### Por Qué Ocurre Esto

- **Separación de Datos e Instancias**: El almacenamiento de datos en múltiples AZs garantiza la durabilidad y disponibilidad de los datos, independientemente de dónde se ejecuten las instancias de base de datos. Esto significa que los datos permanecen seguros y accesibles incluso si una AZ falla.

- **Flexibilidad Operacional**: Al permitir que las instancias de base de datos (primarias y read replicas) se configuren en diferentes AZs, Aurora proporciona flexibilidad en la arquitectura y capacidad de recuperación ante fallos.

##### Resumen

- **Datos en Múltiples AZs**: Amazon Aurora almacena datos en seis réplicas distribuidas en tres Availability Zones por defecto para garantizar alta disponibilidad y durabilidad.

- **Instancias en Una AZ Específica**: Al crear una instancia de Aurora, se especifica una AZ para la instancia primaria. Se pueden configurar read replicas en otras AZs para mejorar el rendimiento de lectura y la disponibilidad.

Aurora Global: hasta 16 read instances por region 


**DynamoDB:** Es serverless, propietaria de AWS, NoSQL
capacidad de backups de retencion automatica 35 dias
tambien backups on demanda

se usa como serverless distriburted cache. Pequeños documentos.

**S3**:

key / value store for objects
Max object size 5TB. Escala infinito.
Tier: S3 standard, Infreq. access, S3 intelligent, S3 Glacier 
Politicas de lyfecycle
Versioning, Encription, Replication...
IAM, Bucket policies, ACL...


**DocumentDB** 

DocumentDB **≠** DynamoDB

Ambas son NoSQL pero Dynamo es serverless y Document no!


columnas:

| Base de Datos     | Tipo de Base de Datos | Modelo de Datos | Escalabilidad | Replicación Multi-AZ | Replicación Multi-Region | Escrituras Atómicas | Escalabilidad Automática | Transacciones ACID | Índices Globales Secundarios | Tipo de Datos Soportados | Casos de Uso Comunes     | Herramientas de Consulta | Integración con Servicios AWS |
| ----------------- | --------------------- | --------------- | ------------- | -------------------- | ------------------------ | ------------------- | ------------------------ | ------------------ | ---------------------------- | ------------------------ | ------------------------ | ------------------------ | ----------------------------- |
| Amazon RDS        | Relacional            | Tabular         | Vertical      | Sí                   | Sí                       | Sí                  | Sí                       | Sí                 | Sí                           | Estructurados            | Aplicaciones de Negocio  | SQL                      | Sí                            |
| Amazon DynamoDB   | NoSQL                 | Documento       | Horizontal    | No                   | No                       | Sí                  | Sí                       | Sí                 | Sí                           | Semi-estructurados       | Aplicaciones Web         | API                      | Sí                            |
| Amazon Redshift   | Almacén de Datos      | Tabular         | Horizontal    | No                   | No                       | No                  | No                       | Sí                 | No                           | Estructurados            | Análisis de Datos        | SQL                      | Sí                            |
| Amazon Aurora     | Relacional            | Tabular         | Horizontal    | Sí                   | Sí                       | Sí                  | Sí                       | Sí                 | Sí                           | Estructurados            | Aplicaciones de Negocio  | SQL                      | Sí                            |
| Amazon Neptune    | Grafo                 | Grafo           | Horizontal    | No                   | Sí                       | No                  | Sí                       | Sí                 | No                           | Semi-estructurados       | Redes Sociales           | Gremlin (traversal)      | Sí                            |
| Amazon DocumentDB | Documento             | Documento       | Horizontal    | No                   | Sí                       | Sí                  | Sí                       | Sí                 | No                           | Semi-estructurados       | Gestión de Contenido     | SQL                      | Sí                            |
| Amazon QLDB       | Contabilidad          | Tabular         | Horizontal    | No                   | No                       | Sí                  | Sí                       | Sí                 | No                           | Estructurados            | Contabilidad y Auditoría | PartiQL (SQL-like)       | Sí                            |

**QLDB**:

Serverless, para registros (recording), inmutable.
Sirve para llevar registros de cambios. Es criptograficamente inmutable

p.ef transacciones financieras
Se parece al Blockchain.

