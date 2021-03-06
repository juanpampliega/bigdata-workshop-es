# ETL: US stocks analysis (BATCH)

## How to run our app

```bash
docker exec -it master bash
cd /app/python/us-stock-analysis

spark-submit \
  --master 'spark://master:7077' \
  --jars /app/postgresql-42.1.4.jar \
  src/batch/etl_steps.py \
  /dataset/stocks-small \
  /dataset/yahoo-symbols-201709.csv \
  /dataset/output.parquet


pyspark \
  --master 'spark://master:7077' \
  --jars /app/postgresql-42.1.4.jar
```

## More examples

```bash
spark-submit \
  --master 'spark://master:7077' \
  src/examples/first_example.py

spark-submit \
  --master 'spark://master:7077' \
  --jars /app/postgresql-42.1.4.jar \
  src/examples/postgres_example.py
```

# ETL: US stocks analysis (STREAM)

### Start fake gen
```bash
docker exec -it worker1 bash

cd /app/python/us-stock-analysis/

# generate stream data
python src/stream/fake_stock_price_generator.py kafka:9092 stocks 2017-11-11T10:00:00Z
```

### Process using Spark Structured Stream API
[Structured Streaming + Kafka Integration Guide](https://spark.apache.org/docs/latest/structured-streaming-kafka-integration.html#deploying)

```bash
spark-submit \
  --master 'spark://master:7077' \
  --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.5 \
  --jars /app/postgresql-42.1.4.jar \
  src/stream/etl_stream.py \
  kafka:9092 stocks


```

# Create a Project using `venv`

```bash
mkdir project1
cd project1

# Create virtualenv
python3 -m venv venv
source venv/bin/activate

# Upgrade pip & Install deps
pip install --upgrade pip
pip install -r requirements.txt

charm .
```
