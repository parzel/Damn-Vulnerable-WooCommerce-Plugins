# use the official wordpress Docker image
FROM wordpress:5.1.1

# install curl and jq
RUN apt-get update
RUN apt clean && apt update && apt install -y jq curl unzip wget vim nano
RUN echo "upload_max_filesize = 40M" > /usr/local/etc/php/php.ini && echo "post_max_size = 40M" >>   /usr/local/etc/php/php.ini

# install wp cli
RUN curl -o /bin/wp https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && chmod +x /bin/wp

# install woocommerce
RUN wget https://downloads.wordpress.org/plugin/woocommerce.4.0.0.zip && unzip *.zip -d /usr/src/wordpress/wp-content/plugins/

###
# install vulnerable plugins
###

# PHP Object Injection Vulnerability in Booster for WooCommerce <= 3.0.1
RUN wget https://downloads.wordpress.org/plugin/woocommerce-jetpack.3.0.1.zip && unzip *.zip -d /usr/src/wordpress/wp-content/plugins/

# LFI in WOOF – Products Filter for WooCommerce <= 1.1.9.
RUN apt install -y subversion
RUN cd /usr/src/wordpress/wp-content/plugins && svn checkout -r r1819087 https://plugins.svn.wordpress.org/woocommerce-products-filter/trunk woocommerce-products-filter

# XSS Woocomerce Currency Switcher <= 1.1.5.1
RUN cd /usr/src/wordpress/wp-content/plugins && svn checkout -r r1327673 http://plugins.svn.wordpress.org/woocommerce-currency-switcher/trunk/ woocommerce-currency-switcher

# WooCommerce Checkout Manager Arbitrary File Upload
RUN wget https://downloads.wordpress.org/plugin/woocommerce-checkout-manager.4.2.6.zip && unzip *.zip -d /usr/src/wordpress/wp-content/plugins/

# LFI vulnerability in MailChimp for WooCommerce <= 2.1.1
RUN wget https://downloads.wordpress.org/plugin/mailchimp-for-woocommerce.2.1.1.zip && unzip *.zip -d /usr/src/wordpress/wp-content/plugins/

# YITH WooCommerce Compare <= 2.0.9 - Unauthenticated PHP Object injection
RUN wget https://downloads.wordpress.org/plugin/yith-woocommerce-compare.2.0.9.zip && unzip *.zip -d /usr/src/wordpress/wp-content/plugins/

###
# setup wordpress
###

# this auto creates the site and enables all plugins
RUN sed -i 's/exec "$@"/wp --allow-root core install --path="\/var\/www\/html" --url=localhost --title="WordPress site" --admin_user="admin" --admin_password="admin" --admin_email="admin@abc.com"/g' /usr/local/bin/docker-entrypoint.sh
RUN echo 'wp --allow-root plugin activate woocommerce' >> /usr/local/bin/docker-entrypoint.sh
RUN echo 'chown -R www-data:www-data /var/www/html/' >> /usr/local/bin/docker-entrypoint.sh
RUN echo 'exec "$@"' >> /usr/local/bin/docker-entrypoint.sh
