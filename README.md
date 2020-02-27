<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="http://kamilmysliwiec.com/public/nest-logo.png#1" alt="Nest Logo" />   </a>

</p>

<p align="center">pg-promise Module for Nest framework</p>

<p align="center">
<a href="https://img.shields.io/npm/v/nestjs-pgpromise"><img src="https://img.shields.io/npm/v/nestjs-pgpromise" alt="NPM Version" /></a>
<a href="https://img.shields.io/npm/l/nestjs-pgpromise"><img src="https://img.shields.io/npm/l/nestjs-pgpromise" alt="Package License" /></a>
<a href="https://www.npmjs.com/package/nestjs-minio"><img src="https://img.shields.io/npm/dm/nestjs-pgpromise" alt="NPM Downloads" /></a>

</p>


<p align="center">
<a href="https://www.buymeacoffee.com/XbgWxt567" target="_blank"><img src="https://i.imgur.com/CahshSS.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>

</p>


## Description
This's a [nest-pgpromise](https://github.com/rubiin/nest-pgpromise) module for [Nest](https://github.com/nestjs/nest).
This quickstart guide will show you how to install and execute an example nestjs program..

This document assumes that you have a working [nodejs](http://nodejs.org/) setup in place.


## Download from NPM

```sh
npm install --save nestjs-pgpromise
```


## Initialize 

You need five items in order to connect to MinIO object storage server.


| Params     | Description |
| :------- | :------------ |
| host	 | Host address. |
|port| TCP/IP port number.|
| database | The name of db to connect to.   |
| username	| The username to access db.    |
|password | The username's password |

Provide the credentials for minio module by importing it as :

## As Connection object

```javascript
import { Module } from '@nestjs/common';
import { NestPgpromiseClientController } from './nest-pgpromise-client.controller';
import { NestPgpromiseModule } from '../nest-pgpromise.module';

@Module({
  controllers: [NestPgpromiseClientController],
  imports: [
    NestPgpromiseModule.register({
      connection: {
        host: 'localhost',
        port: 5432,
        database: 'cmdbbtbi',
        user: 'cmadbbtbi',
        password: 'cghQZynG0whwtGki-ci2bpxV5Jw_5k6z',
      },
    }),
  ],
})

});
```
## As Connection string

```javascript
import { Module } from '@nestjs/common';
import { NestPgpromiseClientController } from './nest-pgpromise-client.controller';
import { NestPgpromiseModule } from '../nest-pgpromise.module';

@Module({
  controllers: [NestPgpromiseClientController],
  imports: [
    NestPgpromiseModule.register({
      connection:"postgres://YourUserName:YourPassword@YourHost:5432/YourDatabase"
    }),
  ],
})

});
```



Then you can use it in the controller or service by injecting it in the controller as:

```javascript

  constructor(@Inject(NEST_PGPROMISE_CONNECTION) private readonly pg) {

```

## Quick Start Example
This example program connects to postgres on localhost and executes a simple `select` query from table `tasks`.


```js

import { Controller, Get, Inject, Logger } from '@nestjs/common';
import { NEST_PGPROMISE_CONNECTION } from '../constants';

@Controller()
export class NestPgpromiseClientController {
  private logger = new Logger('controller');
  constructor(@Inject(NEST_PGPROMISE_CONNECTION) private readonly pg) {}

  @Get()
  async index() {
    this.pg
      .any('SELECT * FROM task')
      .then(data => {
        // success;
        this.logger.log(data);
      })
      .catch(error => {
        // error;
        this.logger.log(error);
      });
  }
}

```
You can also pass in `initoptions` as supported by pg-promise. 


```javascript
import { Module } from '@nestjs/common';
import { NestPgpromiseClientController } from './nest-pgpromise-client.controller';
import { NestPgpromiseModule } from '../nest-pgpromise.module';

@Module({
  controllers: [NestPgpromiseClientController],
  imports: [
    NestPgpromiseModule.register({
      connection: {
        host: 'localhost',
        port: 5432,
        database: 'cmdbbtbi',
        user: 'cmadbbtbi',
        password: 'cghQZynG0whwtGki-ci2bpxV5Jw_5k6z',
      },
      initOptions:{/* initialization options */};
    }),
  ],
})

});
```

You can find the details about them in the [pg-promise](https://vitaly-t.github.io/pg-promise/index.html) documentation
