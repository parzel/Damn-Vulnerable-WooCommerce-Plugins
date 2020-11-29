# Damn Vulnerable WooCommerce Plugins

This is a docker environment ready set up for multiple WooCommerce Plugin vulnerabilities. [@vinulium](https://twitter.com/vinulium) and [me](https://twitter.com/parzel2) created it to practice writing exploits from vulnerability descriptions.

The environment contains the following vulnerabilites that can be exploited:

* [PHP Object Injection Vulnerability in Booster for WooCommerce <= 3.0.1](https://www.acunetix.com/vulnerabilities/web/wordpress-plugin-booster-for-woocommerce-php-object-injection-3-0-1/)
* [LFI in WOOF – Products Filter for WooCommerce <= 1.1.9](https://www.acunetix.com/vulnerabilities/web/wordpress-plugin-woocommerce-products-filter-multiple-vulnerabilities-1-1-9/)
* [XSS Woocomerce Currency Switcher <= 1.1.5.1](https://www.acunetix.com/vulnerabilities/web/wordpress-plugin-woocommerce-currency-switcher-cross-site-scripting-1-1-5-1/)
* [WooCommerce Checkout Manager Arbitrary File Upload](https://wpscan.com/vulnerability/9262)
* [LFI vulnerability in MailChimp for WooCommerce <= 2.1.1](https://www.acunetix.com/vulnerabilities/web/wordpress-plugin-mailchimp-for-woocommerce-local-file-inclusion-2-1-1/)
* [YITH WooCommerce Compare <= 2.0.9 - Unauthenticated PHP Object injection](https://www.acunetix.com/vulnerabilities/web/wordpress-plugin-yith-woocommerce-compare-php-object-injection-2-0-9/)
* [CVE-2018-20966: XSS in Booster for WooCommerce < 3.8.0](https://nvd.nist.gov/vuln/detail/CVE-2018-20966)

The wordpress installation is ready to be exploited, some of the plugins need further setup as stated below. Each plugin needs to be activated for exploitation. It is better to stick to only one activated plugin as otherwise there can be some compatibility issues.

## WriteUp
We did writeups for all of the vulnerabilites in [this](https://parzelsec.de/static/index.html) blogpost.

## Setup
```
docker-compose build && docker-compose up
```

Instance should be here ```http://localhost/```

## Credentials:
admin:admin

## Additional Setup Instructions:

### CVE-2018-20966: XSS in Booster for WooCommerce < 3.8.0
* Add at least one product here: ```http://localhost/wp-admin/post-new.php?post_type=product&tutorial=true```
* Go to ```http://localhost/wp-admin/admin.php?page=wc-settings&tab=jetpack```, enable "Products per Page" and save changes

### PHP Object Injection Vulnerability in Booster for WooCommerce <= 3.0.1
* Go to ```http://localhost/wp-admin/admin.php?page=wc-settings&tab=jetpack```, enable "Email Verification" and save changes
* Now this plugin is ready for exploitation

### WooCommerce Checkout Manager Arbitrary File Upload to RCE
* Go to http://localhost/wp-admin/admin.php?page=woocommerce-checkout-manager and activate "Allow Customers to Upload Files" and "Categorize Uploaded Files"
* Run the WooCommerce Setup http://localhost/wp-admin/admin.php?page=wc-setup , select only digital goods and activate "Cash on delivery". Skip plugins and recommendations in between.
* Set up a product with price ```http://localhost/wp-admin/post-new.php?post_type=product```
* Order the product
* Now this plugin is ready for exploitation