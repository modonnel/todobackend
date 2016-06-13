FROM modonnel/todobackend-base:latest
MAINTAINER Mike O 

# Install dev/build dependancies
RUN apt-get update && \
    apt-get install -qy python-dev libmysqlclient-dev mysql-client

# Activate virtual environment and install whell support
RUN . /appenv/bin/activate && \
    pip install wheel --upgrade

# PIP environment varialbes (NOTE: must be set after intalling wheel)
ENV WHEELHOUSE=/wheelhouse PIP_WHEEL_DIR=/wheelhouse PIP_FIND_LINKS=/wheelhouse XDG_CACHE_HOME=/cache
## remove this after testing
ENV MYSQL_HOST=db MYSQL_USER=root MYSQL_PASSWORD=my-secret-pw TEST_OUTPUT_DIR=/reports


# OUTPUT: Build artifacts (Wheels) are output here
VOLUME /wheelhouse

# OUTPUT: Build cache
VOLUME /build

# OUPUT: Test reports are output here
VOLUME /reports

# add test entrypoint script
COPY scripts/test.sh /usr/local/bin/test.sh
RUN chmod +x /usr/local/bin/test.sh

# Set defaults for entrypoint and command string
ENTRYPOINT ["test.sh"]
#CMD ["python", "manage.py", "test"]
CMD ["python", "manage.py", "test", "--noinput"]

#Add aplication soure
COPY src /application
WORKDIR /application
