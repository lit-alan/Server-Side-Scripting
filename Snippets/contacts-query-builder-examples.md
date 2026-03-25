## CodeIgniter Query Builder Examples for the Contacts Table



**1. Get latest contacts (you already have this from )**

```php
$data['contacts'] = $contactModel
    ->orderBy('id', 'DESC')
    ->findAll();

````


**2. Limit results (e.g., show recent 5 contacts)**

```php
$data['contacts'] = $contactModel
    ->orderBy('created_at', 'DESC')
    ->findAll(5);
```


**3. Filter by City**

```php
$data['contacts'] = $contactModel
    ->where('city', 'Dublin')
    ->findAll();

```

**4. Search by last name**
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
    
