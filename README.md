# Querying Wearable Heart Rate Data with Amazon Athena 🩺☁️

This project is part of my coursework in the **Stanford Data Ocean Precision Medicine Program**. It demonstrates how to use **Amazon Athena** and **Python (PyAthena)** to query heart rate data collected from Fitbit devices in a biomedical research study.

---

## Tools Used

- Amazon Athena (serverless, interactive query service)
- AWS S3 for storage
- PyAthena (Python SQL connector)
- Pandas for data handling and display

---

## Query Example

Using PyAthena, I queried heart rate data for a participant (`pid = a6bui4n`) from the 2022 dataset collected via Fitbit devices.

```python
import pyathena
import pandas as pd

conn = pyathena.connect(
    s3_staging_dir="s3://athena-output-351869726285/",
    region_name='us-east-1',
    encryption_option='SSE_S3'
)

query = "SELECT * FROM sid_00000.hr_minute_a6bui4n WHERE year='2022' AND device='fitbit' LIMIT 10"
df = pd.read_sql(query, conn)
print(df.head(10))







