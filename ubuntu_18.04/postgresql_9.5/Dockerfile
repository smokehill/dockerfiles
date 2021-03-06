FROM ubuntu:18.04

RUN groupadd -r postgres && \
    useradd -r -g postgres postgres

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    locales \
    tzdata \
    lsb-release \
    wget

RUN locale-gen en_US.UTF-8 && \
    ln -fs /usr/share/zoneinfo/Europe/Moscow /etc/localtime && \
    dpkg-reconfigure -f noninteractive tzdata

RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - && \
    sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y -q postgresql-9.5 postgresql-contrib-9.5

RUN sed -i -e "s/data_directory = '\/var\/lib\/postgresql\/9.5\/main'/data_directory = '\/var\/lib\/postgresql\/9.5\/data'/g" /etc/postgresql/9.5/main/postgresql.conf && \
    sed -i -e "s/#listen_addresses = 'localhost'/listen_addresses = '*'/g" /etc/postgresql/9.5/main/postgresql.conf && \
    echo "host all all all md5" >> /etc/postgresql/9.5/main/pg_hba.conf

VOLUME ["/var/lib/postgresql/9.5/data"]

COPY ./entrypoint.sh /sbin/entrypoint.sh
RUN chmod +x /sbin/entrypoint.sh

EXPOSE 5432

ENTRYPOINT ["/sbin/entrypoint.sh"]