FROM reallibrephotos/librephotos-base:dev
# actual project

ARG USER_ID=root
ARG GROUP_ID=root
USER ${USER_ID}

ARG DEBUG
ARG MLVALIDATION
ARG IMAGE_TAG
ENV IMAGE_TAG ${IMAGE_TAG}

WORKDIR /code
RUN git clone --depth 1 https://github.com/LibrePhotos/librephotos .
USER root
RUN pip install --break-system-packages --no-cache-dir -r requirements.txt
RUN if [ "$DEBUG" = 1 ] ; then \
  pip install --break-system-packages -r requirements.dev.txt; \
  fi
RUN if [ "$MLVALIDATION" = 1 ] ; then \
  apt-get update && apt-get install default-jre -y; \
  pip install --break-system-packages -r requirements.mlval.txt; \
  fi
USER ${USER_ID}
EXPOSE 8001

COPY --chown=${USER_ID}:${GROUP_ID} entrypoint.sh /entrypoint.sh
CMD ["/entrypoint.sh"]
