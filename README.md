# About

[![Build Status](https://travis-ci.org/webinv/php-library-datatables.svg?branch=master)](https://travis-ci.org/webinv/php-library-datatables)
[![Latest Stable Version](https://poser.pugx.org/webinv/datatables/v/stable)](https://packagist.org/packages/webinv/datatables)
[![Total Downloads](https://poser.pugx.org/webinv/datatables/downloads)](https://packagist.org/packages/webinv/datatables)
[![Latest Unstable Version](https://poser.pugx.org/webinv/datatables/v/unstable)](https://packagist.org/packages/webinv/datatables)
[![License](https://poser.pugx.org/webinv/datatables/license)](https://packagist.org/packages/webinv/datatables)

Pure PHP models for datatables.net

* https://datatables.net
* https://datatables.net/manual/server-side

# Installation

`composer require webinv/datatables`

# Usage


```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

use Webinv\Datatables\RequestFactory;
use Webinv\Datatables\Response;

// Example data
$data = [
    [
        'first_name' => 'Tiger Nixon',
        'last_name' => 'System Architect',
        'position' => 'Edinburgh',
        'office' => '5421',
        'start_date' => '2011/04/25',
        'salary' => '$320,800'
    ],
    [
        'first_name' => 'Garrett Winters',
        'last_name' => 'Accountant',
        'position' => 'Tokyo',
        'office' => '8422',
        'start_date' => '2011/07/25',
        'salary' => '$170,750'
    ],
    // ...
];

// Create request object from $_GET
$request = (new RequestFactory($_GET))->create();

// Order data
if ($order = $request->getOrder()->current()) {
    usort($data, function ($a, $b) use ($request, $order) {
        $column = $request->getColumnAt($order->getColumn());
        $multiplier = ($order->getDir() === 'asc') ? 1 : -1;
        return strcmp($a[$column->getData()], $b[$column->getData()]) * $multiplier;
    });
}

// Response
header('Content-Type: application/json');

echo json_encode(
    new Response(
        $data,
        count($data),
        count($data),
        $request->getDraw()
    )
);
```