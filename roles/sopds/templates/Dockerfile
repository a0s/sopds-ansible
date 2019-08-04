FROM python:3-stretch

ARG DB_ENGINE="django.db.backends.postgresql_psycopg2"
ARG DB_HOST="localhost"
ARG DB_NAME="sopds"
ARG DB_PASSWORD="sopds"
ARG DB_PORT="5432"
ARG DB_USER="sopds"
ARG SOPDS_AUTH="False"
ARG SOPDS_ROOT_LIB="/books"
ARG SUPERUSER_EMAIL="${SUPERUSER_NAME}@sopds.localhost"
ARG SUPERUSER_NAME="sopds"
ARG SUPERUSER_PASSWORD="sopds"

ENV DB_ENGINE=${DB_ENGINE}
ENV DB_HOST=${DB_HOST}
ENV DB_NAME=${DB_NAME}
ENV DB_PASSWORD=${DB_PASSWORD}
ENV DB_PORT=${DB_PORT}
ENV DB_USER=${DB_USER}
ENV SOPDS_AUTH=${SOPDS_AUTH}
ENV SOPDS_ROOT_LIB=${SOPDS_ROOT_LIB}
ENV SUPERUSER_EMAIL=${SUPERUSER_EMAIL}
ENV SUPERUSER_NAME=${SUPERUSER_NAME}
ENV SUPERUSER_PASSWORD=${SUPERUSER_PASSWORD}

COPY _docker_build_1  _docker_build_2 /
WORKDIR /app
RUN \
  git clone https://github.com/mitshel/sopds /app && \
  cd /app && \
  python3 -m pip install -r requirements.txt && \
  python3 manage.py collectstatic --noinput && \
  python3 -m pip install psycopg2 && \
  cat /_docker_build_1 >> /app/sopds/settings.py && \
  python3 manage.py migrate && \
  python3 manage.py sopds_util setconf SOPDS_LANGUAGE ru-RU && \
  python3 manage.py sopds_util setconf SOPDS_ROOT_LIB ${SOPDS_ROOT_LIB} && \
  python3 manage.py sopds_util setconf SOPDS_AUTH ${SOPDS_AUTH} && \
  python3 -m pip install django-createsuperuserwithpassword && \
  cat /_docker_build_2 >> /app/sopds/settings.py && \
  python3 manage.py createsuperuserwithpassword \
    --username "${SUPERUSER_NAME}" \
    --password "${SUPERUSER_PASSWORD}" \
    --email "${SUPERUSER_EMAIL}" \
    --preserve
