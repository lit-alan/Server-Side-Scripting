The following are some important code snippets for your project.

1. [Database script](#DB-Script)
2. [Code for the Controller](#controller)
3. [Code for the Model](#model)
4. [Code for the Views](#views)
5. [Set up the routes](#routes)
6. [Config Settings](#config-settings)


### DB Script

```sql

-- MariaDB dump 10.18  Distrib 10.4.17-MariaDB, for Win64 (AMD64)
--
-- Host: localhost    Database: address_book
-- ------------------------------------------------------
-- Server version	10.4.17-MariaDB

create database address_book;
use address_book;

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

--
-- Table structure for table `contacts`
--

DROP TABLE IF EXISTS `contacts`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `contacts` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(50) NOT NULL,
  `last_name` varchar(50) NOT NULL,
  `phone` varchar(20) DEFAULT NULL,
  `email` varchar(100) DEFAULT NULL,
  `address_line` varchar(255) DEFAULT NULL,
  `city` varchar(100) DEFAULT NULL,
  `postcode` varchar(20) DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=22 DEFAULT CHARSET=utf8mb4;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `contacts`
--

LOCK TABLES `contacts` WRITE;
/*!40000 ALTER TABLE `contacts` DISABLE KEYS */;
INSERT INTO `contacts` VALUES (1,'Marian','Kiely','0851234567','mk@email.com','12 Main Street','Dublin','D01 AB12','2025-12-21 07:33:14'),
(2,'Mary','Smith','0862345678','mary.smith@email.com','45 Green Road','Cork','T12 CD34','2026-02-19 01:04:35'),
(3,'Alex','Johnson','0873456789','alex.johnson@email.com','78 Lake View','Galway','H91 EF56','2025-12-18 02:21:36'),
(4,'Sara','Connor','0859876543','sara.connor@email.com','9 Seaview Terrace ','Dublin','D04 GH78','2026-02-13 13:02:53'),
(5,'Tom','Brown','0868765432','tom.brown@email.com','22 Service Street','Limerick','V94 IJ90','2025-10-03 11:33:55'),
(6,'Emma','Wilson','0851112233','emma.wilson@email.com','10 River Lane','Dublin','D03 AA11','2025-10-24 02:12:04'),
(7,'Liam','Murphy','0862223344','liam.murphy@email.com','22 Oak Drive','Cork','T12 BB22','2026-03-02 21:37:46'),
(8,'Sophie','Kelly','0873334455','sophie.kelly@email.com','5 Hillcrest','Galway','H91 CC33','2026-01-04 00:02:24'),
(9,'Noah','O’Brien','0854445566','noah.obrien@email.com','18 Park View','Limerick','V94 DD44','2025-09-28 22:59:29'),
(10,'Ava','Byrne','0865556677','ava.byrne@email.com','3 Seaside Ave','Waterford','X91 EE55','2026-01-17 01:22:04'),
(11,'Jack','Doyle','0876667788','jack.doyle@email.com','44 Main Street','Dublin','D08 FF66','2025-10-11 02:57:33'),
(12,'Grace','Ryan','0857778899','grace.ryan@email.com','12 Elm Park','Cork','T23 GG77','2025-11-10 16:48:39'),
(13,'Daniel','McCarthy','0868889900','daniel.mccarthy@email.com','7 Brookfield','Galway','H91 HH88','2025-12-09 21:39:54'),
(14,'Chloe','Flynn','0879990011','chloe.flynn@email.com','9 Rose Court','Limerick','V94 II99','2026-03-14 22:13:55'),
(15,'Sean','Quinn','0851212121','sean.quinn@email.com','25 Market Street','Dublin','D01 JJ10','2025-12-21 13:18:48'),
(16,'Ella','Walsh','0862323232','ella.walsh@email.com','6 Meadow Lane','Cork','T12 KK21','2025-09-27 05:45:37'),
(17,'Cian','Hughes','0873434343','cian.hughes@email.com','14 Woodside','Galway','H91 LL32','2025-10-08 09:31:03'),
(18,'Mia','Kennedy','0854545454','mia.kennedy@email.com','2 Sunset Blvd','Waterford','X91 MM43','2026-01-20 20:53:04'),
(19,'Owen','Reilly','0865656565','owen.reilly@email.com','30 Harbour Road','Dublin','D04 NN54','2025-10-08 15:02:18'),
(20,'Lucy','Carroll','0876767676','lucy.carroll@email.com','11 Forest Hill','Cork','T23 OO65','2026-02-17 22:39:45');
/*!40000 ALTER TABLE `contacts` ENABLE KEYS */;
UNLOCK TABLES;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

-- Dump completed on 2026-03-18 12:29:44

```

## Controller

### `app/controllers/Contacts.php`
This controller handles the core functionality of the address book by retrieving and displaying contacts, showing the form to add a new contact, and saving submitted contact data to the database.
```php
<?php

namespace App\Controllers;

use App\Models\ContactModel;

class Contacts extends BaseController
{
    public function index()
    {
        $contactModel = new ContactModel();

        $data['contacts'] = $contactModel->orderBy('id', 'DESC')->findAll();

        return view('contacts/index', $data);
    }

    public function create()
    {
        return view('contacts/create');
    }

    public function store()
    {
        $contactModel = new ContactModel();

        $contactModel->save([
            'first_name'   => $this->request->getPost('first_name'),
            'last_name'    => $this->request->getPost('last_name'),
            'phone'        => $this->request->getPost('phone'),
            'email'        => $this->request->getPost('email'),
            'address_line' => $this->request->getPost('address_line'),
            'city'         => $this->request->getPost('city'),
            'postcode'     => $this->request->getPost('postcode'),
            'created_at'   => date('Y-m-d H:i:s'),
        ]);

        return redirect()->to('/contacts');
    }
}
```

## Model

### `app/model/ContactModel.php`

This model represents the contacts table and provides an interface for interacting with it, allowing the application to perform operations such as retrieving, inserting, and updating contact records while controlling which fields can be written to the database.

```php

<?php

namespace App\Models;

use CodeIgniter\Model;

class ContactModel extends Model
{
    protected $table = 'contacts';
    protected $primaryKey = 'id';

    protected $allowedFields = [
        'first_name',
        'last_name',
        'phone',
        'email',
        'address_line',
        'city',
        'postcode',
        'created_at'
    ];

    protected $useTimestamps = false;
}
```

## Views

### `app/views/index.php`

This view displays a list of all contacts retrieved from the database in a table format.

It loops through the $contacts array provided by the controller and outputs each contact’s details, using esc() to safely display the data, while also providing a link to add a new contact.

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Contacts</title>
</head>
<body>

    <h1>Contacts</h1>

    <p><a href="<?= base_url('contacts/create') ?>">Add New Contact</a></p>

    <?php if (!empty($contacts)): ?>
        <table border="1" cellpadding="8" cellspacing="0">
            <tr>
                <th>ID</th>
                <th>First Name</th>
                <th>Last Name</th>
                <th>Phone</th>
                <th>Email</th>
                <th>City</th>
            </tr>

            <?php foreach ($contacts as $contact): ?>
                <tr>
                    <td><?= esc($contact['id']) ?></td>
                    <td><?= esc($contact['first_name']) ?></td>
                    <td><?= esc($contact['last_name']) ?></td>
                    <td><?= esc($contact['phone']) ?></td>
                    <td><?= esc($contact['email']) ?></td>
                    <td><?= esc($contact['city']) ?></td>
                </tr>
            <?php endforeach; ?>
        </table>
    <?php else: ?>
        <p>No contacts found.</p>
    <?php endif; ?>

</body>
</html>
```

### `app/views/create.php`

This view displays a simple HTML form that allows the user to enter contact details and submit them to the application.

When the form is submitted, the data is sent via a POST request to the contacts/store route (generated using base_url()), where it is processed and saved to the database. Each input field corresponds to a column in the contacts table.

```php
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Add Contact</title>
</head>
<body>

    <h1>Add Contact</h1>

    <p><a href="<?= base_url('contacts') ?>">Back to Contacts</a></p>

    <form action="<?= base_url('contacts/store') ?>" method="post">
        <?= csrf_field() ?>

        <p>
            <label>First Name</label><br>
            <input type="text" name="first_name">
        </p>

        <p>
            <label>Last Name</label><br>
            <input type="text" name="last_name">
        </p>

        <p>
            <label>Phone</label><br>
            <input type="text" name="phone">
        </p>

        <p>
            <label>Email</label><br>
            <input type="text" name="email">
        </p>

        <p>
            <label>Address</label><br>
            <input type="text" name="address_line">
        </p>

        <p>
            <label>City</label><br>
            <input type="text" name="city">
        </p>

        <p>
            <label>Postcode</label><br>
            <input type="text" name="postcode">
        </p>

        <p>
            <button type="submit">Save Contact</button>
        </p>
    </form>

</body>
</html>

```

## Routes

### `app/Config/routes.php`

```php
$routes->get('/contacts', 'Contacts::index');
$routes->get('/contacts/create', 'Contacts::create');
$routes->post('/contacts/store', 'Contacts::store');
```



## Config Settings
To get the project running correctly in CodeIgniter, there are a few configuration steps that must be completed before the application will work properly. 
These settings ensure that the framework can generate the correct URLs, connect to the database, and use the required PHP features.

1. Set the Base URL in the `.env` file

In the project root, open the .env file and set:

```ini
CI_ENVIRONMENT = development
app.baseURL = 'http://localhost/address-book/public/'
```

2. Configure the Database in the .env file

CodeIgniter also needs the correct database settings so it can connect to MySQL.

Add or update these lines in .env:

```ini
database.default.hostname = localhost
database.default.database = address_book
database.default.username = root
database.default.password =
database.default.DBDriver = MySQLi
database.default.port = 3306
```

_This assumes the database is called address_book, the MySQL username is root, and there is no password, which is typical in a local XAMPP setup._

3. Enable the required PHP extensions

Some PHP extensions must be enabled. In the active php.ini file for the PHP version Apache is using, make sure the following are enabled:

```ini
extension=mysqli
extension=intl
```
_The php.ini file is part of the PHP installation, not the CodeIgniter project._

4. Make sure the URL helper is loade

The URL helper must be loaded. This can be done in `app/Controllers/BaseController.php`

```php
protected $helpers = ['url'];
```

5. Restart Apache after changes

Whenever you change .env, php.ini, or Apache/PHP configuration, restart Apache from XAMPP before testing again. Otherwise, changes may not take effect.


