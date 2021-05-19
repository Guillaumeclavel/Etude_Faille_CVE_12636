# Copyright {2017} {Viardot Sebastien}
# Pour créer l'image docker build . -t waf
# Image de base
FROM debian:stretch
# Auteur
MAINTAINER Sebastien Viardot <Sebastien.Viardot@grenoble-inp.fr>
# Installe le nécessaire pour le waf
RUN apt-get update && \
    apt-get -y install  modsecurity-crs libapache2-modsecurity apache2  &&\
    a2enmod proxy_http && \
    cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf &&\
    sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' /etc/modsecurity/modsecurity.conf
COPY waf.conf /etc/apache2/sites-available/waf.conf
RUN a2ensite waf &&\
    a2enmod security2
EXPOSE 80
