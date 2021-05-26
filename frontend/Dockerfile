FROM node:14.15.5-alpine as build
WORKDIR /frontend
ENV PATH /frontend/node_modules/.bin:$PATH
COPY package.json /frontend/package.json
RUN npm install --silent
RUN npm install @vue/cli@3.7.0 -g
COPY . /frontend
RUN npm run build

# production environment
#EXPOSE 80
#CMD ["npm", "run", "serve"]

FROM library/nginx:1.17-alpine
COPY /nginx/nginx-proxy.conf /etc/nginx/nginx.conf
RUN rm -rf /usr/share/nginx/html/*
COPY --from=build /frontend/dist /usr/share/nginx/html
EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]