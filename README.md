# Vuepress with self-hosted Netlify CMS

Proof of concept of a Vuepress site connected to a self-hosted Netlify CMS.

## Configuration
### Gotrue
The [Gotrue](https://github.com/netlify/gotrue) microservice is an API service for handling user registration and authentication by issuing JWT tokens.

Configuration has to be done in `conf/.env.gotrue`. See [Gotrue documentation](https://github.com/netlify/gotrue#configuration) for details.

### Git-gateway
[Git-gateway](https://github.com/netlify/git-gateway) is a gataway for hosted Git APIS, working with JWT tokens issued by Gotrue.

Configuration is in `conf/.env.gitgateway`.

### Procedure
1. Due to Git-gateway, it only works with HTTPS domains. Replace all `mysite.com` by your HTTPS domain in `conf/*`files and `src/.vuepress/public/admin/config.yml`.
1. Update password for MariaDB in `.env.gotrue` (in `DATABASE_URL`) and `.envmariadb` (`MYSQL_PASSWORD`).
2. Choose a secret for `GOTRUE_OPERATOR_TOKEN` 
3. Choose a secret for `GOTRUE_JWT_SECRET` and `GITGATEWAY_JWT_SECRET`
4. Configure other parameters in Gotrue config if needed
5. Configure `GITGATEWAY_GITHUB_REPO` with your Github repo
6. Configure `GITGATEWAY_GITHUB_ACCESS_TOKEN` with a Github personal access token


### Install & Deployment
VuePress requires Node.js 10+.
Deployment procedure requires Docker-compose.

First, website has to be built:
```
npm run build
```

Then, run the services:
``` 
docker-compose up -d
```

At first installation or when Gotrue is updated, Gotrue database migrations must be triggered:
```
docker-compose exec gotrue /bin/ash
gotrue migrate
```

### Useful documentation
* https://github.com/hfte/netlify-cms-with-selfhosted-gotrue-and-git-gateway
* https://github.com/netlify/netlify-identity-widget
* https://github.com/petedavisdev/VuePress-with-Netlify-CMS