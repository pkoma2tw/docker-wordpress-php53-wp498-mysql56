FROM centos:6

RUN set -ex; \
    yum -y update; \
    yum -y install httpd php php-mbstring php-mysql php-gd php-xml wget unzip

ENV WORDPRESS_VERSION 4.9.8-ja
ENV WORDPRESS_SHA1 7680095050699294e660cf537968f12f28a21e47

RUN set -ex; \
	curl -o wordpress.tar.gz -fSL "https://ja.wordpress.org/wordpress-${WORDPRESS_VERSION}.tar.gz"; \
	echo "$WORDPRESS_SHA1 *wordpress.tar.gz" | sha1sum -c -; \
# upstream tarballs include ./wordpress/ so this gives us /usr/src/wordpress
	tar -xzf wordpress.tar.gz -C /usr/src/; \
	rm -f wordpress.tar.gz

WORKDIR /var/www/html
VOLUME /var/www/html

# WP プラグイン (zip) ダウンロード
WORKDIR /tmp/wp-plugins
RUN wget https://downloads.wordpress.org/plugin/custom-field-template.2.4.6.zip
RUN wget https://downloads.wordpress.org/plugin/p3-profiler.1.5.3.9.zip
#RUN wget https://downloads.wordpress.org/plugin/contact-form-7.4.6.zip
#RUN wget https://downloads.wordpress.org/plugin/syntaxhighlighter.zip
#RUN wget https://downloads.wordpress.org/plugin/wp-emmet.zip

# Zip ファイルを解凍してインストール
RUN unzip './*.zip' -d /usr/src/wordpress/wp-content/plugins

# 不要になった一時ファイルを削除
RUN yum clean packages
RUN rm -rf /tmp/*

# サーバが読めるように wp-content 以下の所有者を変更
RUN chown -R apache:apache /usr/src/wordpress/wp-content

COPY docker-entrypoint.sh /usr/local/bin/

ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["/usr/sbin/apachectl", "-DFOREGROUND"]

