
# ✨ Real-Time Ride-Sharing Analytics with Apache Spark 🚖

Welcome to a real-time analytics engine for ride-sharing platforms, powered by **Apache Spark Structured Streaming**! This project simulates incoming trip data, processes it live, and helps uncover trends and performance insights for drivers.

---

## ⚙️ Requirements

- Python 3.7+
- Apache Spark 3.x
- PySpark (`pip install pyspark`)
- Two terminals: one for simulation, one for streaming jobs

---

## 📂 Project Layout

```
.
├── chk_task1/              # Checkpointing for task 1
├── chk_task2/              # Checkpointing for task 2
├── chk_task3/              # Checkpointing for task 3
├── output_task1/           # Parsed ride data
├── output_task2/           # Driver-level aggregates
├── output_task3/           # Time-window trends
├── data_generator.py       # Real-time JSON data simulator
├── task1.py                # JSON ingestion & parsing
├── task2.py                # Real-time aggregations
├── task3.py                # Sliding window analysis
└── README.md
```

---

## 🧪 Sample Data Format

Streaming data is structured in JSON like this:

```json
{
  "trip_id": "T7890",
  "driver_id": "D21",
  "distance_km": 10.2,
  "fare_amount": 31.75,
  "timestamp": "2025-04-01T18:02:15"
}
```

---

## 🚀 Getting Started

### ✅ Step 1: Run the Data Generator (Terminal 1)

```bash
python data_generator.py
```

This simulates live data and sends it to `localhost:9999`.

### ✅ Step 2: Open a New Terminal for the Tasks (Terminal 2)

Run each task below in its own terminal while the generator is active.

---

## 📌 Task 1 — Ingest & Parse Trip Data

**Goal**: Read live JSON data, parse it, and view records.

```bash
python task1.py
```

✔️ Output is printed to the console and optionally saved to `output_task1/`  
🧾 Fields: `trip_id`, `driver_id`, `distance_km`, `fare_amount`, `timestamp`

---

## 📊 Task 2 — Aggregations by Driver

**Goal**: Compute each driver's total earnings and average trip length.

```bash
python task2.py
```

📈 Outputs:
- **Console**: Real-time aggregates per driver
- **CSV files**: Written to `output_task2/`

🧮 Grouped by: `driver_id`

---

## 🪟 Task 3 — Time-Window Trend Analysis

**Goal**: Analyze fare trends over time using 5-minute sliding windows (1-min slide).

```bash
python task3.py
```

📊 Results:
- Sum of fare amounts per sliding window
- Saved to: `output_task3/` as CSV

---

## 🔍 Behind the Scenes: Approach & Logic

1. **Simulation** — Ride data is generated every second by `data_generator.py`
2. **Streaming Read** — Spark ingests it via socket connection
3. **Schema Enforcement** — JSON is parsed using a defined schema
4. **Transformations**:
   - Grouped aggregations (Task 2)
   - Time-window analytics (Task 3)
5. **Output** — Results written to CSVs and printed to console

---

## 💡 Sample Output

### Task 1 Output
```
T1234,D12,12.5,28.0,2025-04-01 18:00:00
```

### Task 2 Aggregation
```
+--------+-----------+-------------+
|driver_id|total_fare|avg_distance |
+--------+-----------+-------------+
|D12     |84.75      |14.25        |
```

### Task 3 Sliding Window
```
window_start, window_end, total_fare
2025-04-01 17:50:00, 2025-04-01 17:55:00, 132.50
```

---

## 📎 Notes

- ✅ Ensure port `9999` is open before running anything
- 🗑️ Clear checkpoints if restarting jobs
- 📁 All outputs are in their respective `output_task*/` folders

---

## 👋 Final Thoughts

This project is a great hands-on example of how to use **Apache Spark** for real-time data engineering and analytics. You’ll get exposure to:

- Stream ingestion
- JSON parsing
- Windowed computations
- Real-time aggregations

---

## 🙋 Support

For questions, reach out to your instructor or TA.



---

## 🧭 How It Works – Approach

1. **Data Simulation**  
   - `data_generator.py` sends real-time ride events to `localhost:9999` in JSON format.
2. **Streaming Engine**  
   - Spark reads and processes the data using Structured Streaming.
3. **Task 1**: Parses raw JSON and prints the structured output.
4. **Task 2**: Groups data by `driver_id` to compute driver stats in real time.
5. **Task 3**: Applies a sliding window to analyze trends in fare earnings over time.
6. **Output**: Results are streamed to the console and saved as CSVs.

---

## 📖 Explanation

### 🔌 Streaming Input
Spark uses a socket stream (`format("socket")`) to continuously read JSON data being pushed by the Python simulator.

### 🧾 JSON Schema
Data is parsed using `from_json()` and a custom schema matching the fields: `trip_id`, `driver_id`, `distance_km`, `fare_amount`, and `timestamp`.

### 📈 Real-Time Processing
- **Task 2** groups records by `driver_id` to calculate total and average values.
- **Task 3** performs a sliding window aggregation using:
  ```python
  window(col("event_time"), "5 minutes", "1 minute")
  ```

### 📤 Output Modes
- Console (`format("console")`) for real-time view
- CSV files (`format("csv")`) for persistent storage

---

## 📊 Sample Results

### ✅ Parsed Records (Task 1)
```
trip_id, driver_id, distance_km, fare_amount, timestamp
T1005, D34, 15.6, 42.0, 2025-04-01 18:15:00
```

### ✅ Aggregations (Task 2)
```
+----------+-----------+--------------+
|driver_id |total_fare |avg_distance  |
+----------+-----------+--------------+
|D34       |125.75     |12.67         |
```

### ✅ Time-Window Trends (Task 3)
```
window_start           window_end             total_fare
2025-04-01 18:00:00    2025-04-01 18:05:00     198.25
2025-04-01 18:01:00    2025-04-01 18:06:00     210.40
```

---

