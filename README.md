<p align="center">
    <a href="https://www.3brs.com" target="_blank">
        <img src="https://3brs1.fra1.cdn.digitaloceanspaces.com/3brs/logo/3BRS-logo-sylius-200.png"/>
    </a>
</p>
<h1 align="center">
    Payment Restrictions Plugin
    <br />
    <a href="https://packagist.org/packages/3brs/sylius-payment-restrictions-plugin" title="License" target="_blank">
        <img src="https://img.shields.io/packagist/l/3brs/sylius-payment-restrictions-plugin.svg" />
    </a>
    <a href="https://packagist.org/packages/3brs/sylius-payment-restrictions-plugin" title="Version" target="_blank">
        <img src="https://img.shields.io/packagist/v/3brs/sylius-payment-restrictions-plugin.svg" />
    </a>
    <a href="https://circleci.com/gh/3BRS/sylius-payment-restrictions-plugin" title="Build status" target="_blank">
        <img src="https://circleci.com/gh/3BRS/sylius-payment-restrictions-plugin.svg?style=shield" />
    </a>
</h1>

## Features

 - Restrict payment method by zone. This enables to limit selected payment methods to specific zones or areas from the delivery address.
 - Restrict payment method by shipping method - this means that it can disable specific shipment-payment combinations.

<p align="center">
	<img src="https://raw.githubusercontent.com/3BRS/sylius-payment-restrictions-plugin/master/doc/admin.png"/>
</p>

## Installation

1. Run `$ composer require 3brs/sylius-payment-restrictions-plugin`.
2. Add plugin class to your `config/bundles.php`:
 
   ```php
   return [
      ...
      ThreeBRS\SyliusPaymentRestrictionPlugin\ThreeBRSSyliusPaymentRestrictionPlugin::class => ['all' => true],
   ];
   ```
   
3. Your Entity `PaymentMethod` has to implement `\ThreeBRS\SyliusPaymentRestrictionPlugin\Model\PaymentMethodRestrictionInterface`. You can use Trait `ThreeBRS\SyliusPaymentRestrictionPlugin\Model\PaymentMethodRestrictionTrait`.
 
   ```php
   <?php 
   
   declare(strict_types=1);
   
   namespace App\Entity\Payment;
   
   use Doctrine\Common\Collections\ArrayCollection;
   use Doctrine\ORM\Mapping as ORM;
   use Sylius\Component\Core\Model\Payment as BasePayment;
   use ThreeBRS\SyliusPaymentRestrictionPlugin\Model\PaymentMethodRestrictionInterface;
   use ThreeBRS\SyliusPaymentRestrictionPlugin\Model\PaymentMethodRestrictionTrait;

    #[ORM\Entity]
    #[ORM\Table(name: 'sylius_payment_method')]
   class PaymentMethod extends BasePayment implements PaymentMethodRestrictionInterface
   {
       use PaymentMethodRestrictionTrait;
   
       public function __construct()
       {
           parent::__construct();
       
           $this->shippingMethods = new ArrayCollection();
       }
   }
   ```

4. Change `@SyliusAdmin/PaymentMethod/_form.html.twig`.
 
    ```twig
    ...
	<div class="two fields">
		{{ form_row(form.zone) }}
		{{ form_row(form.shippingMethods) }}
	</div>
    ...
    ```

5. Create and run doctrine database migrations.

For guide to use your own entity see [Sylius docs - Customizing Models](https://docs.sylius.com/en/1.6/customization/model.html)

## Development

### Usage

- Develop your plugin in `/src`
- See `bin/` for useful commands

### Testing

After your changes you must ensure that the tests are still passing.

#### Prerequisites
```bash
composer install
yarn --cwd tests/Application install
yarn --cwd tests/Application build
tests/Application/bin/console doctrine:schema:create --env test
```


```bash
composer checks
```

License
-------
This library is under the MIT license.

Credits
-------
Developed by [3BRS](https://3brs.com)<br>
Forked from [manGoweb](https://github.com/mangoweb-sylius/SyliusPaymentRestrictionsPlugin).
