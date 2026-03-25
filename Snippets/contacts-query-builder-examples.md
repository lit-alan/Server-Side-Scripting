## CodeIgniter Query Builder Examples for the Contacts Table

_In the following examples `$contactModel` is an instance of your model class that represents the `contacts` table and provides methods to interact with the database (e.g. querying, inserting, updating, deleting records)._

_Remember that `$data['contacts']` is an associative array, where each item represents a single contact and each key corresponds to a column name (e.g. first_name, email)._

**1. Get latest contacts (you already have this)**

```php
//Retrieves all contacts from the database, ordered by id in descending order.
//This means the most recently added contacts (highest id) appear first.
//findAll() executes the query and returns all results as an array.
$data['contacts'] = $contactModel
    ->orderBy('id', 'DESC')
    ->findAll();

````


**2. Limit results (e.g., show recent 5 contacts)**

```php
//Retrieves the 5 most recently created contacts.
//orderBy('created_at', 'DESC') sorts records so the newest entries appear first.
//findAll(5) limits the result to 5 records.
//The results are returned as an array and stored in $data['contacts'].
$data['contacts'] = $contactModel
    ->orderBy('created_at', 'DESC')
    ->findAll(5);
```


**3. Filter by City**

```php
//Retrieves all contacts where the city is "Dublin".
//where('city', 'Dublin') filters the results to only matching records.
//findAll() executes the query and returns the filtered results as an array.
$data['contacts'] = $contactModel
    ->where('city', 'Dublin')
    ->findAll();

```

**4. Search by last name**
```php
//Retrieves all contacts where the last name contains "Murphy".
//like('last_name', 'Murphy') performs a partial match search (e.g. "Murphy", "O'Murphy").
//findAll() executes the query and returns the matching results as an array.
$data['contacts'] = $contactModel
    ->like('last_name', 'Murphy')
    ->findAll();

```

**5. Multiple conditions (city + postcode)**

```php
$data['contacts'] = $contactModel
    ->where([
        'city' => 'Dublin',
        'postcode' => 'D06'
    ])
    ->findAll();
```


**6. Find a single contact by ID**

```php
$contact = $contactModel->find($id);
```

**7. Contacts created recently (e.g., last 7 days)**

```php
//Retrieves all contacts created within the last 7 days.
//The WHERE clause filters records where `created_at` is greater than or equal
//to a timestamp representing "now minus 7 days".
//date('Y-m-d H:i:s') formats the timestamp into MySQL-compatible DATETIME format:
//  Y = 4-digit year, m = month, d = day,
//  H = 24-hour, i = minutes, s = seconds (e.g. 2026-03-25 14:37:05).
//
//findAll() executes the query and returns the matching results as an array.
$data['contacts'] = $contactModel
    ->where('created_at >=', date('Y-m-d H:i:s', strtotime('-7 days')))
    ->findAll();
```

**8. Count contacts in a city**

```php
$count = $contactModel
    ->where('city', 'Dublin')
    ->countAllResults();
```

**9. Select specific columns only (performance optimisation)**

```php
$data['contacts'] = $contactModel
    ->select('first_name, last_name, email')
    ->findAll();
```

**10. Order alphabetically by last name**
```php
$data['contacts'] = $contactModel
    ->orderBy('last_name', 'ASC')
    ->findAll();
```

**11. Pagination (important for scaling)**
Pagination splits large result sets into smaller pages, reducing load time and improving usability by limiting how many records are shown at once. It also supports scaling by loading only a small subset of data at a time, keeping the application fast as data or user numbers grow; scaling means a system can handle increased demand without slowing down or failing.

```php
//Retrieves contacts ordered by newest first (highest id first),
//but limits the results to 10 per page using pagination.
//paginate(10) applies SQL LIMIT and OFFSET under the hood:
//   LIMIT = how many records to return per page (here, 10)
//   OFFSET = how many records to skip based on the current page
//(e.g. page 2 skips the first 10 records).
//The pager object is then used to generate navigation links (e.g. next/previous pages).

$data['contacts'] = $contactModel
    ->orderBy('id', 'DESC')
    ->paginate(10);

$pager = $contactModel->pager;

```

**12. Search across multiple fields**

```php
//Retrieves contacts where either the first name or last name contains "John".
//groupStart() and groupEnd() wrap conditions in brackets, grouping them together.
//This ensures the OR logic is applied correctly.
$data['contacts'] = $contactModel
    ->groupStart()
        ->like('first_name', 'John')
        ->orLike('last_name', 'John')
    ->groupEnd()
    ->findAll();

```

**13. Get contacts with missing emails**

```php
$data['contacts'] = $contactModel
    ->where('email IS NULL')
    ->findAll();
```

**14. Delete a contact by their ID**
```php
$contactModel->delete($id);
```

**15. Update a contacts phone number (partial update)**

```php
$contactModel->update($id, [
    'phone' => '123456789'
]);
```


**16. Update an entire contact**

```php
$contactModel->update($id, [
    'first_name'   => 'Jane',
    'last_name'    => 'Doe',
    'phone'        => '0871234567',
    'email'        => 'jane.doe@example.com',
    'address_line' => '123 Main Street',
    'city'         => 'Dublin',
    'postcode'     => 'D02 XY76',
]);

//Updates all editable fields for a specific contact (identified by $id).
//Each key in the array corresponds to a column in the database.
//The values replace the existing data in that row.
//Note: 'id' is not included because it is the primary key,
//and 'created_at' is typically not updated after creation.
```

_all of the above code snippets belong in the Controller_


