# postgres
```yml
  pgsql:
    tty: true
    container_name: pgsql
    build:
      context: ./postgres
      args:
        - DATABASE=XXX #replace with your own database
    ports:
      - 5432:5432
    volumes:
      - /etc/localtime:/etc/localtime
      - ./postgres/data:/var/lib/postgresql/data
      - ./postgres/backupDb:/backupDb
    environment:
      - POSTGRES_DB=XXX
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=XXXXXX
    restart: always
 ```
