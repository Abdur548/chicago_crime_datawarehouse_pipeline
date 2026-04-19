# CS-404 Big Data Analytics — Assignment 02
## Data Source Selection & Ingestion Pipeline

**Group 8 | BDA Spring 2026 | Ms. Zahida Kausar**

---

## Project Overview

This repository contains the first milestone of the CS-404 Big Data Analytics final project. The goal of this milestone is to select a real-world dataset suitable for a data warehousing project, build a fully automated ingestion pipeline into Hadoop HDFS, and produce a comprehensive data profiling report.

**Dataset:** Chicago Crimes (2001–Present)  
**Source:** [City of Chicago Open Data Portal](https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-Present/ijzp-q8t2)  
**Records:** 7,846,809 rows × 22 columns  
**File Size:** ~1.77 GB  

---

## Repository Structure

```
Group8_A2_BDA/
├── ingest.py                          # Automated HDFS ingestion pipeline
├── profiling_report.py                # Profiling script (generates the PDF)
├── profiling_report.pdf               # Data profiling report (Task 3)
├── hdfs_screenshot.png                # HDFS directory structure screenshot
├── requirements.txt                   # Python dependencies
└── README.md                          # This file
```

---

## Environment Setup

### Prerequisites
- Ubuntu (WSL2 on Windows 10/11)
- Java 11 (OpenJDK)
- Apache Hadoop 3.3.6 (pseudo-distributed mode)
- Python 3.10+

### Environment Variables
Ensure the following are set in your `~/.bashrc`:
```bash
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export HADOOP_HOME=$HOME/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

### Install Python Dependencies
```bash
pip install pandas matplotlib seaborn reportlab chardet --break-system-packages
```

---

## How to Run ingest.py

### Step 1 — Start Hadoop
```bash
start-dfs.sh
start-yarn.sh
```

Verify all daemons are running:
```bash
jps
# Expected: NameNode, DataNode, SecondaryNameNode, ResourceManager, NodeManager
```

### Step 2 — Configure the Script
Open `ingest.py` and update line 34 with the path to your local CSV:
```python
LOCAL_FILE = "/path/to/Crimes_-_2001_to_Present.csv"
```

### Step 3 — Run the Pipeline
```bash
python3 ingest.py
```

The script will automatically:
1. **Load** — locate and checksum the dataset
2. **Validate** — check extension, size, encoding, row count, and schema
3. **Upload** — push the CSV to HDFS at `/warehouse/raw/chicago_crimes/year=2026/month=04/`
4. **Organize** — verify HDFS structure and write a metadata provenance file
5. **Log** — record all steps to `ingest.log`

### Step 4 — Verify on HDFS
```bash
hdfs dfs -ls -R /warehouse/raw/chicago_crimes
```

NameNode Web UI:
```
http://localhost:9870
```

---

## How to Generate the Profiling Report

Place `profiling_report.py` in the same directory as the CSV and run:
```bash
python3 profiling_report.py
```

Output: `profiling_report.pdf` — generated in ~5–10 minutes on the full dataset.

---

## Group Members

| Name | Role |
|------|------|
| Syed Muhammad Abdur Rahman | Pipeline development, profiling report |
| Anas | Hadoop setup, HDFS configuration, dataset ingestion |
 
**Submitted to:** Ms. Zahida Kausar

---

## Submission Details

- **Assignment:** CS-404 BDA Assignment 02 — Data Source Selection & Ingestion Pipeline
- **Submission File:** `Group8_A2_BDA.zip`
