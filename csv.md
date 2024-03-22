# CSV files

CSV stands for "comma separated values". It is mostly used for exporting data 
from databases or spreadsheets, like Excel. CSV standard can have different
"dialects" which means that different applications use different standards 
for formatting those files. Most use commas to separate values, but others 
can use, for example, semicolons. 

In Python, we can read or write those files using standard file `read` and `write` 
procedures, and then use `.split` on strings to split them by commas or other 
characters. But it is recommended to use the built-in library "csv" which can handle 
different standards (so-called "dialects") of CSV files, or just different 
ways of writing them (separating by comma, semicolons, tabs etc, using single or 
double quotes for strings and so on).

## Reading from CSV file

Example of reading CSV file using `csv` library:

```python
import csv

with open("csv_sales_data.csv", newline="") as csvfile:
    sales_data = csv.reader(csvfile)

    for row in sales_data:
        for item in row:
            print(item, end=" ")
        print()
```

`csv.reader` returns a stream which we can iterate using the for loop. 
Each row in the iterator is a list of strings.

## writing to a CSV file

If we want to write to a CSV file, we need to use `csv.writer`:

```python
import csv

with open("csv_people_data.csv", "w", newline="") as csvfile:
    data_writer = csv.writer(csvfile)
    data_writer.writerow(["Tom", "Kaunas", "25"])
    data_writer.writerow(["John", "London", "46"])
```

If we want to use something else to separate our values than 
commas, we can add them as parameters to our reader or writer (`delimiter` parameter):

```python
import csv

with open("csv_people_data.csv", "w", newline="") as csvfile:
    data_writer = csv.writer(csvfile, delimiter=" ", quotechar='|')
    data_writer.writerow(["Tom", "Kaunas", "25"])
    data_writer.writerow(["John", "London", "46"])
```

We can write and read dictionaries as CSV files. For this we can use a special class DictReader:

Reading a dictionary from file:

```python
import csv

with open('names.csv', newline='') as csvfile:

    reader = csv.DictReader(csvfile)

    for row in reader:

        print(row['first_name'], row['last_name'])


print(row)
# {'first_name': 'John', 'last_name': 'Cleese'}
```


Writing a dictionary to a file:

```python

import csv

with open('names.csv', 'w', newline='') as csvfile:
    fieldnames = ['first_name', 'last_name']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

    writer.writeheader()
    writer.writerow({'first_name': 'Baked', 'last_name': 'Beans'})
    writer.writerow({'first_name': 'Lovely', 'last_name': 'Lentils'})
    writer.writerow({'first_name': 'Wonderful', 'last_name': 'Peas'})
```
