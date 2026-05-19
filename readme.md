# direkt.bahn.guru

**[direkt.bahn.guru](https://direkt.bahn.guru/)** All direct long-distance railway connections from a given city.

[![License](https://img.shields.io/github/license/juliuste/direkt.bahn.guru.svg?style=flat)](license)
[![Contact me](https://img.shields.io/badge/contact-email-turquoise)](mailto:mail@juliustens.eu)

[![Screenshot of direkt.bahn.guru](assets/screenshot.png)](https://direkt.bahn.guru)

## Setup

See [SETUP.md](SETUP.md) for local development and server deployment instructions.

## API Migration

In late 2024, Deutsche Bahn discontinued the unofficial HAFAS API that this project was originally based on. The station search has been migrated to the new [v6.db.transport.rest](https://v6.db.transport.rest/) API, which uses [db-vendo-client](https://github.com/public-transport/db-vendo-client) as its backend. See [SETUP.md](SETUP.md) for details.

## See also

- [bahn.guru](https://github.com/juliuste/bahn.guru) - Find the cheapest Deutsche Bahn "Sparpreise" (low-cost tickets) for the next month.
- [pricemap.eu](https://github.com/juliuste/travel-price-map) - Map of cheapest railway and bus (coach) travel prices between several european cities.
- [api.direkt.bahn.guru](https://github.com/juliuste/api.direkt.bahn.guru) - Backend for this service

## Contributing

If you found a bug or want to propose a feature, feel free to visit [the issues page](https://github.com/juliuste/direkt.bahn.guru/issues).
