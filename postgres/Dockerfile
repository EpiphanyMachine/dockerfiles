# Run postgres server for local development

# docker volume create -d local --opt o=size=10GB hint-pg-data
# docker run -d \
#     -v hint-pg-data:/var/lib/postgresql
#     -p 5555:5432 \
#     --name hint-pg \
#     EpiphanyMachine/postgres

FROM ubuntu:16.04
MAINTAINER EpiphanyMachine@gmail.com

# Add the PostgreSQL PGP key to verify their Debian packages.
# It should be the same key as https://www.postgresql.org/media/keys/ACCC4CF8.asc
RUN apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8

# Add PostgreSQL's repository. It contains the most recent stable release of PostgreSQL, 9.3.
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list

# Install `python-software-properties`, `software-properties-common` and PostgreSQL 9.3
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    python-software-properties \
    software-properties-common \
    postgresql-9.3 \
    postgresql-client-9.3 \
    postgresql-contrib-9.3

# Run the rest of the commands as the `postgres` user created by the `postgres-9.3` package
USER postgres

# Create a PostgreSQL role named `docker` with `docker` as the password
# and then create a database `docker` owned by the `docker` role.
RUN /etc/init.d/postgresql start &&\
    psql --command "CREATE USER docker WITH SUPERUSER PASSWORD 'docker';" &&\
    createdb -O docker docker

# Adjust PostgreSQL configuration so that remote connections to the
# database are possible.
RUN echo "host all  all    0.0.0.0/0  md5" >> /etc/postgresql/9.3/main/pg_hba.conf

# And add `listen_addresses` to `/etc/postgresql/9.3/main/postgresql.conf`
RUN echo "listen_addresses='*'" >> /etc/postgresql/9.3/main/postgresql.conf

# Expose the PostgreSQL port
EXPOSE 5432

# Set the default command to run when starting the container
CMD ["/usr/lib/postgresql/9.3/bin/postgres", "-D", "/var/lib/postgresql/9.3/main", "-c", "config_file=/etc/postgresql/9.3/main/postgresql.conf"]
