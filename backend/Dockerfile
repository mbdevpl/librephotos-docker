FROM reallibrephotos/librephotos-base:dev
# actual project

ARG USER_ID=0
ARG GROUP_ID=0
ARG GIT_REPOSITORY="https://github.com/LibrePhotos/librephotos"
ARG GIT_VERSION="HEAD"

RUN addgroup --gid "${GROUP_ID}" librephotos \
  || echo "group ${GROUP_ID} already exists, so not adding it"
RUN adduser --disabled-password --gecos "LibrePhotos" --uid "${USER_ID}" --gid "${GROUP_ID}" librephotos \
  || echo "user ${USER_ID} already exists, so not adding it"
USER ${USER_ID}

ARG DEBUG
ARG MLVALIDATION
ARG IMAGE_TAG
ENV IMAGE_TAG ${IMAGE_TAG}

WORKDIR /code
RUN git clone ${GIT_REPOSITORY} .
RUN git checkout ${GIT_VERSION}
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
