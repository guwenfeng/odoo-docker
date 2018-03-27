FROM debian:stretch
MAINTAINER ZhangJie <zhangjie@bankcall.net>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; apt-get update

RUN apt-get install -y --no-install-recommends \
            ca-certificates \
            curl \
            node-less \
            python3-pip \
            python3-setuptools \
            python3-renderpm \
            libssl1.0-dev \
            xz-utils

COPY ./wkhtmltox-0.12.4.tar.xz /wkhtmltox.tar.xz
RUN tar xvf /wkhtmltox.tar.xz \
        && cp wkhtmltox/lib/* /usr/local/lib/ \
        && cp wkhtmltox/bin/* /usr/local/bin/ \
        && cp -r wkhtmltox/share/man/man1 /usr/local/share/man/

# Install Odoo
COPY ./odoo_11.0.deb /odoo.deb

RUN dpkg --force-depends -i /odoo.deb \
        && apt-get -y install -f --no-install-recommends

# Copy entrypoint script and Odoo configuration file
RUN pip3 install num2words xlwt
COPY ./entrypoint.sh /
COPY ./odoo.conf /etc/odoo/
RUN chown odoo /etc/odoo/odoo.conf

# Mount /var/lib/odoo to allow restoring filestore and /mnt/extra-addons for users addons
RUN mkdir -p /mnt/extra-addons \
        && chown -R odoo /mnt/extra-addons
VOLUME ["/var/lib/odoo", "/mnt/extra-addons"]

# Expose Odoo services
EXPOSE 8069 8071

# Set the default config file
ENV ODOO_RC /etc/odoo/odoo.conf

## extra python dependences
#---
RUN pip3 install pycryptodome suds -i https://pypi.douban.com/simple
#---
##

RUN rm -rf /var/lib/apt/lists/* /odoo.deb /wkhtmltox*

# Set default user when running the container
USER odoo

ENTRYPOINT ["/entrypoint.sh"]
CMD ["odoo"]
