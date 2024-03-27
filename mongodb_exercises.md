1. Basic CRUD Operations:
    - Create a new database called "School" and within it, create a collection called "Students".
    - Insert a few documents into the "Students" collection, each containing fields like "name", "age", "grade", and "subjects" (nested array).
    - Update the age of a specific student.
    - Delete a student from the collection.

2. Querying with Comparison Operators:
    - Find all students who are 18 years old or older.
    - Retrieve students who are in grades higher than or equal to 10.
    - Fetch students who are studying subjects that start with the letter 'M'.

3. Nested Queries:
    - Insert documents with nested records representing the courses each student is enrolled in.
    - Retrieve students who are studying a specific subject (e.g., "Mathematics").
    - Find students who are studying more than one subject.

4. Aggregation:
    - Calculate the average age of all students.
    - Group students by grade and count the number of students in each grade level.
    - Find the maximum and minimum age among all students.

5. Indexing:
    - Create an index on the "age" field to improve query performance.
    - Check the execution plan of a query before and after adding an index.

6. Advanced Queries:
    - Retrieve students sorted by age in descending order.
    - Find students whose age is between 15 and 20.
    - Retrieve the oldest student in the collection.

7. Data Modeling:
    - Design a schema for a new collection called "Courses" and create sample documents.
    - Create a reference between the "Students" collection and the "Courses" collection to represent student-course enrollment.
    - Perform queries to find students enrolled in a specific course.

8. Advanced Updates:
    - Add a new subject to the subjects array of a specific student.
    - Update the grades of all students in a particular grade level.

9. Data Validation:
    - Define a schema for the "Students" collection to enforce data validation.
    - Attempt to insert documents with missing required fields and observe the results.
