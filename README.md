# Kafka Stock Market Project

A real-time stock market data streaming application using Apache Kafka to produce and consume stock market messages. This project demonstrates how to build a scalable event-driven architecture for financial data processing.

## Project Overview

This project consists of two main components:

- **Kafka Producer** (`kafka-producer.ipynp`): Generates stock market data and publishes it to a Kafka topic
- **Kafka Consumer** (`kafka-consumer.ipynp`): Subscribes to stock market messages and processes them in real-time

Data is persisted to AWS S3 for long-term storage and analysis.

## Architecture

### System Diagram

![Architecture Diagram](Architecture.jpg)

*Replace `./images/architecture-diagram.png` with the path to your architecture image*

```
Stock Market Dataset
        ↓
SDK Boto3 (Stock Market App Simulation)
        ↓
    Producer ──→ Kafka (Amazon EC2) ──→ Consumer
                                            ├─→ Amazon S3
                                            ├─→ AWS Glue Data Catalog
                                            ├─→ Web Crawler
                                            └─→ Amazon Athena (SQL Query)
```

### Components Overview

The architecture demonstrates a complete real-time data pipeline:

- **Data Source**: Stock market dataset processed through Python SDK
- **Kafka Broker**: Runs on Amazon EC2 for centralized message distribution
- **Producer**: Pushes real-time stock events to Kafka topics
- **Consumer**: Receives and processes events, storing data in multiple destinations
- **Data Storage**: Amazon S3 acts as the data lake
- **Data Catalog**: AWS Glue catalogs and organizes the data
- **Data Discovery**: Web Crawler indexes and discovers data patterns
- **Query Engine**: Amazon Athena enables SQL queries on stored data

## System Requirements

### Prerequisites

Before running this project, ensure you have the following installed:

- Python 3.9+
- pip (Python package manager)
- Apache Kafka (or access to a Kafka broker)
- AWS account and credentials configured (for S3 access)

## Installation

### Step 1: Clone the Repository

1. Clone the repository:
```bash
git clone <repository-url>
cd KAFKA-STOCK-MARKET-PROJECT
```

### Step 2: Install Dependencies

2. Install required Python dependencies:
```bash
pip install -r requirements.txt
```

The main dependencies include:
- `kafka-python` - Kafka client for Python
- `pandas` - Data manipulation and analysis
- `boto3` - AWS SDK for Python (S3 access)
- `python-dotenv` - Environment variable management

3. Set up AWS credentials in your environment or use the AWS CLI:
```bash
aws configure
```

## Configuration

Update the following in the notebooks before running:

- **Bootstrap servers**: Replace `13.53.133.33:9092` with your Kafka broker address
- **AWS S3 bucket**: Update the S3 path in the consumer to match your bucket name
- **Topic name**: Default topic is `demotest` (configurable in both producer and consumer)

## Usage

### Running the Producer

The producer reads stock data from `indexProcessed.csv` and sends it to Kafka:

1. Open `kafka-producer.ipynb` in Jupyter Notebook
2. Execute the cells to start the producer
3. The producer will continuously sample data and send it to the Kafka topic every second

```python
producer = KafkaProducer(
    bootstrap_servers=['13.53.133.33:9092'],
    value_serializer=lambda x: dumps(x).encode('utf-8')
)
```

### Running the Consumer

The consumer subscribes to the Kafka topic and processes incoming messages:

1. Open `kafka-consumer.ipynb` in Jupyter Notebook
2. Execute the cells to start the consumer
3. Messages will be consumed, stored in S3, and displayed in real-time

```python
consumer = KafkaConsumer(
    'demotest',
    bootstrap_servers=['13.53.133.33:9092'],
    value_deserializer=lambda x: loads(x.decode('utf-8'))
)
```

## Project Structure

```
KAFKA-STOCK-MARKET-PROJECT/
├── scripts/
│   ├── kafka-producer.ipynb      # Producer notebook
│   ├── kafka-consumer.ipynb      # Consumer notebook
│   └── indexProcessed.csv        # Input stock data
├── README.md                      # This file
├── kafka-stock-market-project.pem # SSH key (keep secure)
└── .gitignore
```

## Data Flow

1. **Producer**: Reads CSV file → Samples random records → Sends to Kafka topic
2. **Kafka Broker**: Receives and distributes messages
3. **Consumer**: Subscribes to topic → Receives messages → Stores in S3 → Displays output

## Key Features

- Real-time stock data streaming using Apache Kafka
- Automatic data persistence to AWS S3
- Scalable architecture supporting multiple consumers
- JSON serialization for cross-platform compatibility
- Continuous monitoring with configurable intervals

## Environment Variables

Create a `.env` file in the project root (optional):

```
KAFKA_BOOTSTRAP_SERVERS=13.53.133.33:9092
KAFKA_TOPIC=demotest
AWS_S3_BUCKET=your-bucket-name
```

## Troubleshooting

### Connection Issues

Verify that your Kafka broker is running and accessible at the specified bootstrap server address.

### AWS S3 Errors

Ensure your AWS credentials are properly configured and have S3 permissions.

### Serialization Errors

Confirm that the data format in your CSV matches the expected JSON structure.

## Performance Considerations

- Adjust the sleep interval in the producer to control message frequency
- Consider batch processing in the consumer for high-throughput scenarios
- Monitor S3 costs when storing large volumes of data
- Use Kafka partitioning for better parallelism with multiple consumers

## Next Steps

- Implement error handling and retry logic
- Add data validation and schema enforcement
- Create monitoring and alerting for producer/consumer health
- Scale with multiple consumer instances
- Implement a database for analytical queries

## Support

For issues or questions, refer to the official documentation:
- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Kafka-Python Documentation](https://kafka-python.readthedocs.io/)
- [AWS S3 Documentation](https://docs.aws.amazon.com/s3/)