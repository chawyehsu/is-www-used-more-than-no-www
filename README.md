# Is www used more than no-www?

> Just a fun little site to compare the amount of www sites and no-www sites

**WHY** [Google Chrome 69 kills off www in URLs](https://news.ycombinator.com/item?id=17927972)?

![preview](preview.png)

### Getting Started

To run this locally, clone the repo and use Yarn or NPM to install the dependencies. (You’ll also need Node.js installed)

```bash
git clone https://github.com/h404bi/is-www-used-more-than-no-www
cd is-www-used-more-than-no-www
yarn
```

### Development

Start a dev server on `http://127.0.0.1:8080`

```bash
yarn dev
```

### Production

To build for prod, run the following:

```bash
yarn build
```

### Contributions (Add sites)

Edit [`sites.json`](public/sites.json) file, add your site domain to corresponding array.
Please keep the domains array **unique sort**, then open a pull-request and wait. :D

**FAQ: how to see a site is using www or not?**

Use `curl -I <domain>` to see the response header, and your browser to determine it! e.g.:

```bash
$ curl -I https://www.twitter.com
HTTP/1.1 301 Moved Permanently
content-length: 0
date: Sun, 09 Sep 2018 08:50:56 GMT
location: https://twitter.com/
server: tsa_k
...
```

### Resource

- [https://www.yes-www.org/](https://www.yes-www.org/)
- [https://dropwww.com/](https://dropwww.com/)
- [http://no-www.org/](http://no-www.org/)

### Credit

- The original source code is forked from [hasvuepassedreactyet](https://github.com/stursby/hasvuepassedreactyet).
- The icon is from [Flaticon](https://www.flaticon.com).
