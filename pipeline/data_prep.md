# Data Preparation


```python
import numpy as np
import pandas as pd

import os
import json
import pymongo
import logging
from dotenv import load_dotenv
```


```python
# Setup logger

logging.basicConfig(
    filename='../logs/data_prep.log',
    level=logging.INFO, 
    format='%(asctime)s - %(levelname)s - %(message)s',
    filemode='w'
)
logger = logging.getLogger(__name__)

logger.info("Data Prep Started")
```


```python
# Load raw json data file

try: 
    with open("../data/forbes_billionaires_list.json", "r") as file:
        data = json.load(file)
    
    logger.info("Loaded raw json data")

except Exception as e:
    print(f"Error: {e}")
    logger.error(f"An error occurred: {e}")

```


```python
# Preview one document
data["data"][0]
```




    {'rank': 1,
     'name': 'Elon Musk',
     'last_name': 'Musk',
     'source': 'Tesla, SpaceX',
     'industries': ['Automotive'],
     'country': 'United States',
     'gender': 'M',
     'age': 54,
     'current_worth': 413046544000,
     'previous_worth': 416338889179,
     'image_url': 'https://specials-images.forbesimg.com/imageserve/62d700cd6094d2c180f269b9/416x416.jpg'}




```python
# Connect to MongoDB

try:

    # Load environment variables for MongoDB credentials
    load_dotenv()
    username = os.getenv("MONGO_USERNAME")
    password = os.getenv('MONGO_PASSWORD')
    
    # Establish MongoDB connection
    uri = f'mongodb+srv://{username}:{password}@cluster0.sssb7.mongodb.net/'
    client = pymongo.MongoClient(uri)
    logger.info("Connected to MongoDB")

    # Create database
    db = client["forbes"]
    logger.info("Created MongoDB database")

    # Create collection
    collection = db["billionaries"]
    logger.info("Created MongoDB collection")

except Exception as e:
    print(f"Error: {e}")
    logger.error(f"An error occurred: {e}")

```


```python
# Pipeline to construct documents and upload to MongoDB

try:
    
    # Extract upper-level information
    title = data.get("title")
    subtitle = data.get("subtitle")
    updated_at = data.get("updated_at")
    logger.info("Extracted source metadata")

    # Extract billionaire rankings data as documents
    billionaires = data.get("data", [])
    logger.info("Extracted billionaires from raw data")

    # Loop through billionaires
    documents = []
    for billionaire in billionaires:

        # Add metadata to each document
        doc = billionaire.copy()
        doc["source_metadata"] = {
            "title": title,
            "subtitle": subtitle,
            "updated_at": updated_at
        }
        
        # Append individual documents to list
        documents.append(doc)
    
    # Clear MongoDB collection if it is populated with documents
    if collection.count_documents({}) != 0:
        collection.delete_many({})
        logger.info("Deleted contents of existed MongoDB collected")
    
    # Insert all documents into MongoDB at once
    if documents:
        collection.insert_many(documents)
    
    logger.info("Inserted documents into MongoDB")

except Exception as e:
    print(f"Error: {e}")
    logger.error(f"An error occurred: {e}")
```


```python
# Alternative pipeline to construct documents and upload to MongoDB

'''

try: 

    # Extract upper-level information
    title = data.get("title")
    subtitle = data.get("subtitle")
    updated_at = data.get("updated_at")
    logger.info("Extracted source metadata")

    # Extract billionaire rankings data as documents
    billionaires = data.get("data", [])
    logger.info("Extracted billionaires from raw data")

    # Clear MongoDB collection if it is populated with documents
    if collection.count_documents({}) != 0:
        collection.delete_many({})
        logger.info("Deleted contents of existed MongoDB collected")
    
    # Loop through documents and insert into MongoDB
    for billionaire in billionaires:

        # Add metadata to each document
        doc = billionaire.copy()
        doc["source_metadata"] = {
            "title": title,
            "subtitle": subtitle,
            "updated_at": updated_at
            }
        
        # Insert documents into MongoDB individually
        collection.insert_one(doc)
    
    logger.info("Inserted documents into MongoDB")

except Exception as e:
    print(f"Error: {e}")
    logger.error(f"An error occurred: {e}")

'''
```




    '\n\ntry: \n\n    # Extract upper-level information\n    title = data.get("title")\n    subtitle = data.get("subtitle")\n    updated_at = data.get("updated_at")\n    logger.info("Extracted source metadata")\n\n    # Extract billionaire rankings data as documents\n    billionaires = data.get("data", [])\n    logger.info("Extracted billionaires from raw data")\n\n    # Clear MongoDB collection if it is populated with documents\n    if collection.count_documents({}) != 0:\n        collection.delete_many({})\n        logger.info("Deleted contents of existed MongoDB collected")\n    \n    # Loop through documents and insert into MongoDB\n    for billionaire in billionaires:\n\n        # Add metadata to each document\n        doc = billionaire.copy()\n        doc["source_metadata"] = {\n            "title": title,\n            "subtitle": subtitle,\n            "updated_at": updated_at\n            }\n        \n        # Insert documents into MongoDB individually\n        collection.insert_one(doc)\n    \n    logger.info("Inserted documents into MongoDB")\n\nexcept Exception as e:\n    print(f"Error: {e}")\n    logger.error(f"An error occurred: {e}")\n\n'




```python
# Confirm successful insertion

try:
    
    n_docs = collection.count_documents({})

except Exception as e:
    print(f"Error: {e}")


if n_docs == len(billionaires):
    print("All documents inserted to MongoDB successfully")
    logger.info("All documents inserted to MongoDB successfully")

print("Number of documents inserted:", n_docs)
```

    All documents inserted to MongoDB successfully
    Number of documents inserted: 3109



```python
# Extract all top-level fields
field_counts = {}

for document in collection.find():
    for field in document.keys():
        field_counts[field] = field_counts.get(field, 0) + 1

# Output top-level fields and percentage in documents
print("Top-level fields:")
for field, count in field_counts.items():
    percentage = (count / n_docs) * 100
    print(f"{field}: {round(percentage, 2)}%")
```

    Top-level fields:
    _id: 100.0%
    rank: 100.0%
    name: 100.0%
    last_name: 100.0%
    source: 100.0%
    industries: 100.0%
    country: 100.0%
    gender: 100.0%
    age: 100.0%
    current_worth: 100.0%
    previous_worth: 100.0%
    image_url: 100.0%
    source_metadata: 100.0%

