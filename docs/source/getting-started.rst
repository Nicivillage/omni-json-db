Getting Started
===============


Installating **omni-json-db**
-------------------------------

.. code-block:: bash

   pip install omni-json-db


Quick Start
------------

.. code-block:: python

   from omni_json_db import JDb

   # Initialize the database
   jdb = JDb("example.jdb")

   # Store data
   jdb["user:1"] = {"name": "Ryan", "role": "Developer"}

   # Retrieve data
   print(jdb["user:1"]["name"]) # Output: Ryan

   # Bulk Update
   jdb += {
       "user:2": {"name": "Alice", "role": "Admin"},
       "user:3": {"name": "Bob", "role": "Developer"}
   }

   # Query data
   matches = jdb.find(ANY="Alice")
   print(matches["user:2"]["name"]) # Output: Alice
