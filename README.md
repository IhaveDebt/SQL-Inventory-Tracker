# inventory.py
import psycopg2

DB_PARAMS = {
    "dbname": "inventory_db",
    "user": "postgres",
    "password": "password",
    "host": "localhost",
    "port": 5432
}

def add_item(name, quantity):
    conn = psycopg2.connect(**DB_PARAMS)
    cur = conn.cursor()
    cur.execute("INSERT INTO inventory(name, quantity) VALUES (%s, %s);", (name, quantity))
    conn.commit()
    cur.close()
    conn.close()
    print(f"Item {name} added with quantity {quantity}")

def update_stock(name, change):
    conn = psycopg2.connect(**DB_PARAMS)
    cur = conn.cursor()
    cur.execute("UPDATE inventory SET quantity = quantity + %s WHERE name=%s;", (change, name))
    conn.commit()
    cur.close()
    conn.close()
    print(f"Stock updated for {name}")

if __name__ == "__main__":
    add_item("Laptop", 5)
    update_stock("Laptop", -1)
