# Data Ingestion API

A simple Node.js (Express) API system for submitting data ingestion requests and checking their status.  
It processes IDs in batches asynchronously, respects rate limits, and prioritizes jobs by priority and submission time.

---

## Features

- **POST `/ingest`**: Submit a list of IDs and a priority. Returns a unique `ingestion_id`.
- **GET `/status/:ingestion_id`**: Check the status of your ingestion request and its batches.
- **Batch Processing**: Processes up to 3 IDs per batch, one batch every 5 seconds.
- **Priority Handling**: Higher priority jobs are processed first; ties are broken by submission time.
- **In-memory storage**: All data is stored in memory for simplicity.

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v14 or higher recommended)
- [npm](https://www.npmjs.com/)

### Installation

1. Clone the repository or copy the code.
2. Install dependencies:
    ```bash
    npm install
    ```
3. Start the server:
    ```bash
    npm start
    ```
    The server will run on [http://localhost:5001](http://localhost:5001) by default.

---

## API Endpoints

### 1. Ingestion API

- **POST `/ingest`**
- **Body Example:**
    ```json
    {
      "ids": [1, 2, 3, 4, 5],
      "priority": "HIGH"
    }
    ```
- **Response Example:**
    ```json
    {
      "ingestion_id": "your-uuid-here"
    }
    ```

### 2. Status API

- **GET `/status/:ingestion_id`**
- **Response Example:**
    ```json
    {
      "ingestion_id": "your-uuid-here",
      "status": "triggered",
      "batches": [
        { "batch_id": "uuid1", "ids": [1, 2, 3], "status": "completed" },
        { "batch_id": "uuid2", "ids": [4, 5], "status": "triggered" }
      ]
    }
    ```

---

## Testing with Thunder Client

1. **POST /ingest**
    - Method: `POST`
    - URL: `http://localhost:5001/ingest`
    - Body (JSON):
      ```json
      {
        "ids": [1, 2, 3, 4, 5],
        "priority": "HIGH"
      }
      ```
2. **GET /status/:ingestion_id**
    - Method: `GET`
    - URL: `http://localhost:5001/status/<ingestion_id>`
    - Replace `<ingestion_id>` with the value from the POST response.

---

## Design Choices

- **In-memory storage** for simplicity and demonstration.
- **Batching**: Each batch contains up to 3 IDs.
- **Rate limiting**: Only one batch is processed every 5 seconds.
- **Priority queue**: Batches are sorted by priority and submission time.
- **Async processing**: Uses an infinite async loop to process batches in the background.

---

## Notes

- This is a demo project. All data is lost when the server restarts.
- No real external API calls are made; data fetching is simulated with a delay.
