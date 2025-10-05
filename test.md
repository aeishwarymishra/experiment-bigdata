flowchart LR
A[User Interface (React / Angular)] --> B[App Server (Spring Boot / Django)]
B --> C[(RDBMS: PostgreSQL / Oracle)]
B --> D[Cache (Redis)]
C --> E[Backup / Replication Node]
