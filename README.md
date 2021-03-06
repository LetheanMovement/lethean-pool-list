# cryptonote-universal-pool-list

> Lethean Pool List, based on [cryptonote-universal-pool].

[cryptonote-universal-pool]: https://github.com/fancoder/cryptonote-universal-pool

## Basic features

- Customizable per currency
- Server configuration based on file system, one file === one server
- coinmarket.com integration
- Exchange integrations
- GitHub issue integration
- Google Analytic support (optional)
- Donation list (optional)
- docker image

## Customization

In order to run you own `cryptonote-universal-pool-list`, you have to fork this repository.
Then from your own repository, follow the documentation below and override stuff which sould be overridden.

### Main configuration

The file `src/config/main.json` is containing the main configuration.
The configuration is split by domains.

#### currency

It is about the mined currency. i.e. the name, symbol etc.

```json
{
  "currency": {
    "name": "Lethean",
    "short_name": "Lethean",
    "tech_name": "lethean",
    "symbol": "LTHN",
    "difficultyTarget": 120
  }
}
```

`tech_name` is used to locate the config directory (`src/config/<tech_name>`) as well as the domain name (`.circleci/config.yml`).
`difficultyTarget` is used to estimate the network hashrate.

#### links

From the front page, links can be displayed just below the site's tile.
Those links are configurable under the *links domain*, like below:

```json
{
  "links": {
    "lethean.io": {
      "icon": "fas fa-home",
      "url": "https://lethean.io"
    }
  }
}
```

The icon value is in fact the content of a `class` attribute related to an empty tag `<i></i>`.
So, the content is dedicated for library like [fontawesome.io].

This version of cryptonote-universal-pool-list has been updated to FontAwesome v5. Note that icons using [fontawesome.io] classes (eg. `fa`) now require `fas` or `fab` according to FontAwesome version 5 specs.

[fontawesome.io]: http://fontawesome.io/ 

#### exchanges

From the front page, there is a block dedicated to exchanges web-site.
The content of this block is related to the *exchanges domain*.
Each exchanges is be described with a logo and a URL.
The key, there `stocks.exchange`, will be the `alt` attribute of the `img` element.

```json
{
  "exchanges": {
    "stex": {
      "logo": "images/logo.stex.png",
      "url": "https://app.stex.com/en/basic-trade/pair/BTC/LTHN/1D"
    }
  }
}
```

#### coinmarket

From the front page, on the right of the site's title, a coinmarket widget can be displayed according to the *coinmarket domain*.
Please refer to [coinmarketcap's documentation] for more information.

```json
{
  "coinmarket": {
    "currency": "lethean",
    "base": "USD",
    "secondary": "BTC",
    "ticker": "true",
    "rank": "true",
    "marketcap": "true",
    "volume": "true",
    "stats": "USD",
    "statsticker": "false"
  }
}
```
[coinmarketcap's documentation]:https://coinmarketcap.com/widget

#### donations

Before the footer of the front page, a donations block can be displayed according to the *donations domain*.

```json
{
  "donations": {
    "ITNS": "iz4 ...",
    "BTC": "13 ..."
  }
}
```

#### administrator

The *administrator domain* is there to setup the administrator details like the email.
`request_user` and `request_repo` are used to configure the form creating the GitHub issues. 

```json
{
  "administrator": {
    "email": "admin@example.com",
    "request_user": "tmorin",
    "request_repo": "cryptonote-universal-pool-list"
  }
}
```

#### analytic

To activate Google Analytic, the 

```json
{
  "analytic": {
    "ga": "XX-XXXXXXXX-X"
  }
}
```

#### worker

The *domain worker* is dedicated to the back part.
`interval_ms` is interval between to refresh of the stats. By default the value is 5 minutes.

```json
{
  "worker": {
    "interval_ms": 300000
  }
}
```

#### Icons

The icons of the front page has to be customized there: `src/front/images/icons`.

### Servers

The server definitions are located in `src/config/servers`.
There is one file per servers.
The name of the file is the key of the server from the front ent point of view.
The content of a file is a JSON document describing:
- the name of the server
- the URL of the front
- the URL of the back
- the location
- the implementation: `nodejs-pool` or `cryptonote-universal-pool`, by default it's `cryptonote-universal-pool`
- the boolean disabled

For instance:
```json
{
  "name": "lethean.io",
  "front": "https://lethean.io/pool",
  "back": "https://lethean.io/pool/api",
  "location": "US"
}
```

### Deployment

`cryptonote-universal-pool-list` is ready to be deployed over docker.
There is script doing the stuff: `scripts/deploy.sh`.
The script is expecting to be run in a docker environment where [nginx-proxy] and [docker-letsencrypt-nginx-proxy-companion] are ready.
To customize the domain name and SSL configuration, the following environment variables have to be customized: 
- ENV_NAME
- VIRTUAL_HOST
- LETSENCRYPT_HOST
- LETSENCRYPT_EMAIL

[nginx-proxy]: https://hub.docker.com/r/jwilder/nginx-proxy
[docker-letsencrypt-nginx-proxy-companion]: https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion

## Development tasks

Install the dependencies
```bash
npm install
```

Start the back in development mode
```bash
npm run w:back:<currency symbol lower case>
```

Start the front in development mode
```bash
npm run w:front
```

Build the production sources
```bash
npm run build
```

Build local docker container
```bash
docker build -t lethean-pool-list .
```

Call the deploy script to build and start the updated container.
```bash
scripts/deploy.sh <tech_name> <GITHUB_CLIENT_ID> <GITHUB_CLIENT_SECRET>
```

## License

Released under the GNU General Public License v3
