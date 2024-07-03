# Data Engineering Project

This project uses a Postgres Docker image with preloaded data.
The Docker image will simulate the backend database.

1. Pull Docker image:
	```
    docker pull lilearningproject/big-star-postgres:latest
    ```

2. Run a container, called "big-start-container", from the pulled image:
    ```
	docker run --platform linux/arm64 --name big-star-container -d -p 5432:5432 lilearningproject/big-star-postgres:latest -c "wal_level=logical"
    ```
    Now you have an instance of the database

3. Access the database using SQL within the container (in the terminal, within Docker):
    ```
	psql -U postgres -d big-star-db
    ```

4. Set a replica identity:
    ```
	ALTER TABLE customers REPLICA IDENTITY DEFAULT;
	ALTER TABLE order_items REPLICA IDENTITY DEFAULT;
	ALTER TABLE orders REPLICA IDENTITY DEFAULT;
	ALTER TABLE products REPLICA IDENTITY DEFAULT;
    ```

5. Create a replication slot:
    ```
	SELECT pg_create_logical_replication_slot('airbyte_slot', 'pgoutput');
    ```

6. Create a publication:
    ```
	CREATE PUBLICATION airbyte_publication FOR TABLE customers, order_items, orders, products;
    ```