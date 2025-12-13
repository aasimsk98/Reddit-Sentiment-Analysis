# Reddit Trend Analysis (Local Deployment)
Real-time sentiment analysis pipeline for Reddit data using locally deployed Kafka via Docker, MongoDB storage, and interactive dashboards.

> **Branch Info:**
> - `main` - Uses Confluent Cloud (managed Kafka)
> - `ApacheKafka` - Uses locally deployed Kafka via Docker

## Table of Contents
- [Project Goal](#project-goal)
- [Live Dashboard](#live-dashboard)
- [Requirements](#requirements)
- [Setup Instructions](#setup-instructions)
- [Running the Pipeline](#running-the-pipeline)
- [Methodology](#methodology)
- [Project Structure](#project-structure)
- [Docker Commands](#docker-commands)
- [Troubleshooting](#troubleshooting)
- [Future Enhancements](#future-enhancements)

## Project Goal

This project builds an end-to-end data pipeline that:
- Fetches Reddit posts and comments in real-time from multiple subreddits
- Streams data through locally deployed Apache Kafka using Docker
- Performs sentiment analysis using NLTK VADER
- Stores processed data in MongoDB with automatic indexing
- Visualizes trends through an interactive Streamlit dashboard with live search capabilities

## Live Dashboard
**Access the dashboard here:** (https://reddit-trend-analysis.streamlit.app/)

No installation required! View real-time Reddit sentiment analysis directly in your browser.

## Requirements

### Software
- Python 3.10+
- Docker Desktop
- MongoDB Atlas account and get credentials
- Reddit API credentials

## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/Basim592003/Reddit-Trend-Analysis.git
cd Reddit-Trend-Analysis
git checkout ApacheKafka  # Switch to local deployment branch
```

### 2. Install Docker Desktop
Download and install from [docker.com](https://www.docker.com/products/docker-desktop/)

### 3. Configure Environment Variables
Create `.env` file in root directory:
```
REDDIT_CLIENT_ID=your_client_id
REDDIT_CLIENT_SECRET=your_client_secret
REDDIT_USER_AGENT=your_user_agent

KAFKA_BOOTSTRAP_SERVERS=kafka:9092

MONGO_CONNECTION_STRING=your_mongo_connection_string
```

### 4. Configure Subreddits
Edit `producer/config.yaml` to specify subreddits to monitor:
```yaml
subreddits:
  - technology
  - news
  - worldnews
batch_size: 3
posts_per_subreddit: 10
comments_per_post: 5
cycle_delay: 300
```

## Run Everything with Docker
```bash
docker-compose up -d
```
This starts Kafka, producer, and consumer all together.

## Methodology

### Data Collection
- Reddit API (PRAW) fetches posts and comments from configured subreddits
- Multi-threaded producer handles rate limiting and continuous streaming
- Data serialized to JSON and published to Kafka topics

### Stream Processing
- Apache Kafka runs locally via Docker (KRaft mode, no Zookeeper)
- Consumer processes messages in real-time
- Duplicate detection via MongoDB unique indexes

### Sentiment Analysis
- NLTK VADER analyzer computes compound sentiment scores
- Range: -1 (negative) to +1 (positive)
- Scores stored with original post/comment data

### Storage
- MongoDB Atlas stores processed data
- Indexed fields: post_id, subreddit, timestamp, score
- TTL indexes for automatic data cleanup

### Visualization
- Streamlit dashboard provides real-time analytics
- Metrics: sentiment trends, top posts, subreddit comparisons
- Interactive filters and time-based analysis
