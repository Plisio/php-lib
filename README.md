# This is the lightweight Plisio API library

Using this library, you can easily create your own version of the payment plugin for any site, using a minimum of dependencies. <br>
**How to start:**

1. create class

```php
<?php
rquire_once __DIR__ . '/Plisio.php';

$plisio = new PlisioPayment(API_SECRET);
?>
```

# create Invoice

you dont need to use all fields just use require fields and fields you want

```php
<?php
$invoice_array = [

    'currency' => 'TRX',
    'order_name' => 'new order', // require
    'order_number' => '2323', // require
    'amount' => 1,
    'source_currency' => '',
    'source_amount' => '',
    'allowed_psys_cids' => '',
    'description' => '',
    'callback_url' => '',
    'success_callback_url' => '',
    'fail_callback_url' => '',
    'email' => '',
    'language' => '',
    'plugin' => '',
    'version' => '',
    'redirect_to_invoice' => '',
    'api_key' => '',
    'expire_min' => ''
]

$invoice_info $plisio->createInvoice($invoice_array);
// redirect user to invoice page
header('location: ' . $invoice_info['invoice_url']);
?>
```

# verify transactions

```php
<?php

$transaction_status = $plisio->verifyCallbackData()


if ($trasaction_status === true) {
    // transaction verified
}
>


```

**Library compatibility:** <br>
This library is compatible with PHP 5.6 and newer.
