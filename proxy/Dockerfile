FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf

ARG USERNAME="nginx"
ARG USER_ID=101
ARG GROUPNAME="nginx"
ARG GROUP_ID=101

RUN addgroup --gid "${GROUP_ID}" "${USERNAME}" \
  || echo "group ${GROUP_ID} already exists, so not adding it"
RUN adduser --disabled-password --gecos "Nginx" --uid "${USER_ID}" --gid "${GROUP_ID}" "${USERNAME}" \
  || echo "user ${USER_ID} already exists, so not adding it"

RUN sed -i "s|user  nginx;|user  ${USERNAME} ${GROUPNAME};|" /etc/nginx/nginx.conf
