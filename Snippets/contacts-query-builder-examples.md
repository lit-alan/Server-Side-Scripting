## CodeIgniter Query Builder Examples for the Contacts Table


1. Get latest contacts (you already have this from )

```php
$data['contacts'] = $contactModel
    ->orderBy('id', 'DESC')
    ->findAll();

````


2. Limit results (e.g., show recent 5 contacts)

```php
$data['contacts'] = $contactModel
    ->orderBy('created_at', 'DESC')
    ->findAll(5);
```


3. Filter by City

```php
$data['contacts'] = $contactModel
    ->where('city', 'Dublin')
    ->findAll();

```

4. Search by last name
$data['contacts'] = $contactModel
    ->like('last_name', 'Murphy')
    ->findAll();

```

5. Multiple conditions (city + postcode)

```php
$data['contacts'] = $contactModel
    ->where([
        'city' => 'Dublin',
        'postcode' => 'D06'
    ])
    ->findAll();
```
