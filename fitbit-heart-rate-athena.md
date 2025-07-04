# Querying Wearable Heart Rate Data with Amazon Athena 🩺☁️

This section demonstrates how to query minute-by-minute heart rate data collected from Fitbit devices, using Amazon Athena and PyAthena, part of the Stanford Data Ocean Precision Medicine program.

---

## Setup & Connection

We use **PyAthena** to connect to Athena, which queries data stored in AWS S3. Athena is serverless, so no infrastructure management is needed — just SQL and Python.

```python
import pyathena
import pandas as pd

conn = pyathena.connect(
    s3_staging_dir="s3://athena-output-351869726285/", 
    region_name='us-east-1', 
    encryption_option='SSE_S3'
)




Querying Sample Heart Rate Data
Here is a query retrieving 10 rows of heart rate data for a single participant (pid = a6bui4n) from the study with ID 00000 for the year 2022, device fitbit:

query = """
SELECT * 
FROM sid_00000.hr_minute_a6bui4n 
WHERE year = '2022' AND device = 'fitbit' 
LIMIT 10
"""

df = pd.read_sql(query, conn)
df.head(10)



| datetime | heartrate | sid   | pid     | device | year | month | day |
| -------- | --------- | ----- | ------- | ------ | ---- | ----- | --- |
| 06:41:57 | 70.0      | 00000 | aw4exxk | fitbit | 2022 | 11    | 17  |
| 05:57:09 | 70.0      | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:00:01 | 70.0      | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:03:13 | 70.0      | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:05:35 | 72.7      | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:06:00 | 135.3     | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:07:00 | 123.8     | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:08:06 | 95.3      | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:09:01 | 72.1      | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |
| 06:10:01 | 70.0      | 00000 | aw4exxk | fitbit | 2022 | 12    | 4   |




What the Columns Mean
datetime: Time the heart rate was recorded (HH:MM:SS)

heartrate: Heart beats per minute (bpm)

sid: Study ID (tracks the study source)

pid: Participant ID (unique participant identifier)

device: Device used to record data (here, Fitbit)

year, month, day: Date of recording



Why This Matters
This granular, minute-level heart rate data provides insights into participant physiology in real-time. When combined with genomic data (such as from the 1000 Genomes Project), it enables powerful multi-dimensional analyses for precision medicine and personalized health.

