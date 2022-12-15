# This is the lightweight Plisio API library
Using this library, you can easily create your own version of the payment plugin for any site, using a minimum of dependencies. <br>
__How to start:__ 
1. Create an instance of a PlisioClient class with a secret key, which is specified in the settings of your store in a Plisio dashboard: 
```php
<?php

require_once(__DIR__ . '/PlisioClient.php');

class PlisioPaymentGatewayExample 
{
    private $plisio;
    private $secretKey; //Your Plisio shop secret key.
    
    public function plisioClientSetup() 
    {
        $this->plisio = new PlisioClient($this->$secretKey);
    }
}
```
2. Create your first invoice using order information:
```php
public function createInvoice($order_info)
{
    $this->plisioClientSetup(); //Initialize Plisio instance.
    
    $invoiceData = array (
        'order_number' => $order_info['number'], //Order number should be uniq for each order.
        'order_name' => 'Order from example shop',
        'source_amount' => number_format($order_info['amount'], 8, '.', ''), //Order amount in source currency.
        'source_currency' => $order_info['currency'], //For example: 'USD'.
        'cancel_url' => 'https://myshop.com/order/failed/' . $order_info['number'], //Url to which Plisio will redirect customer in a case of a cancelled order.
        'callback_url' => 'https://myshop.com/order/callback', //Url to which Plisio will send order related info about status changes.
        'success_url' => 'https://myshop.com/order/success/' . $order_info['number'], //Url to which Plisio will redirect customer in a case of successful order.
        'email' => $order_info['customer_email'], //Customer email. If not specified - customer will be prompted to enter email on Plisio side.
        'plugin' => 'PluginName', //Specify uniq name related to your shop, so it'll be easier to track issues with your invoices if any occurs.
        'version' => 'PluginVersion' //Specify plugin version.
    );
    
    $response = $this->plisio->createTransaction($invoiceData);
}
```
3. Handle API response:
```php
if ($response && $response['status'] !== 'error' && !empty($response['data'])) {
    $invoiceUrl = $response['data']['invoice_url'];
    header('Location: ' . $invoiceUrl); //Redirect to Plisio if invoice's created successfully.
    exit();
} else {
    error_log('Error occurred while processing the payment:  ' . implode(',', json_decode($response['data']['message'], true))); //Log error message from Plisio. You can add error message for a customer to specify the reason of order creation failed (Min sum limit for a cryptocurrency for example).
    header('Location: https://myshop.com/order/new'); //Redirect to invoice creation page.
    exit();
}
```
__API functions overview:__
1. ``getBalances($currency)`` <br>
Get balance for a specified cryptocurrency in your wallet (for example: 'BTC').
2. ``getShopInfo()`` <br>
Get shop info related to an api_key specified when PlisioClient instance created.
3. ``getCurrencies($source_currency)`` <br>
Get cryptocurrencies info related to fiat currency (such as fiat to crypto rate). Only enabled in your shop settings currencies shown.
4. ``createTransaction($orderInfo)`` <br>
Create Plisio invoice with specified order info and get invoice url. 

__Library compatibility:__ <br>
This library is compatible with PHP 5.6 and newer.