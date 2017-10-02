# Arise

Arise is a next generation crypto-currency and decentralized application platform, written entirely in JavaScript. For more information please refer to our website: https://arise.io/.

[![Build Status](https://travis-ci.org/arisebank/arise.svg?branch=development)](https://travis-ci.org/arisebank/arise)
[![Coverage Status](https://coveralls.io/repos/github/arisebank/arise/badge.svg?branch=development)](https://coveralls.io/github/arisebank/arise?branch=development)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](http://www.gnu.org/licenses/gpl-3.0)
[![Join the chat at https://gitter.im/arisebank/arise](https://badges.gitter.im/arisebank/arise.svg)](https://gitter.im/arisebank/arise?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

**NOTE:** The following information is applicable to: **Ubuntu 14.04, 16.04 (LTS) or 16.10 - x86_64**.

## Prerequisites - In order

- Tool chain components -- Used for compiling dependencies

  `sudo apt-get install -y python build-essential curl automake autoconf libtool`

- Git (<https://github.com/git/git>) -- Used for cloning and updating Arise

  `sudo apt-get install -y git`

- Node.js (<https://nodejs.org/>) -- Node.js serves as the underlying engine for code execution.

  System wide via package manager:

  ```
  curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
  sudo apt-get install -y nodejs
  ```

  Locally using [nvm](https://github.com/creationix/nvm):

  ```
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
  nvm install v6.10.1
  ```

- Install PostgreSQL (version 9.6.2):

  ```
  curl -sL "https://downloads.arise.io/scripts/setup_postgresql.Linux" | bash -
  sudo -u postgres createuser --createdb $USER
  createdb arise_test
  createdb arise_main
  sudo -u postgres psql -d arise_test -c "alter user "$USER" with password 'password';"
  sudo -u postgres psql -d arise_main -c "alter user "$USER" with password 'password';"
  ```

- Bower (<http://bower.io/>) -- Bower helps to install required JavaScript dependencies.

  `npm install -g bower`

- Grunt.js (<http://gruntjs.com/>) -- Grunt is used to compile the frontend code and serves other functions.

  `npm install -g grunt-cli`

- PM2 (<https://github.com/Unitech/pm2>) -- PM2 manages the node process for Arise (Optional)

  `npm install -g pm2`

## Installation Steps

Clone the Arise repository using Git and initialize the modules.

```
git clone https://github.com/arisebank/arise.git
cd arise
npm install
```

Install Arise Node, a specialized version of Node.js used to execute dapps within a virtual machine:

```
wget https://downloads.arise.io/arise-node/arise-node-Linux-x86_64.tar.gz
tar -zxvf arise-node-Linux-x86_64.tar.gz
```

Arise Node has to be in `[ARISE_DIR]/nodejs/node`.

Load git submodules ([arise-ui](https://github.com/arisebank/arise-ui) and [arise-js](https://github.com/arisebank/arise-js)):

```
git submodule init
git submodule update
```

Build the user-interface:

```
cd public
npm install
bower install
grunt release
```

## Managing Arise

To test that Arise is built and configured correctly, run the following command:

`node app.js`

In a browser navigate to: <http://localhost:9012> (for the mainnet) or <http://localhost:7000> (for the testnet). If Arise is running on a remote system, switch `localhost` for the external IP Address of the machine.

Once the process is verified as running correctly, `CTRL+C` and start the process with `pm2`. This will fork the process into the background and automatically recover the process if it fails.

`pm2 start --name arise app.js`

After the process is started, its runtime status and log location can be retrieved by issuing the following command:

`pm2 show arise`

To stop Arise after it has been started with `pm2`, issue the following command:

`pm2 stop arise`

**NOTE:** The **port**, **address** and **config-path** can be overridden by providing the relevant command switch:

```
pm2 start --name arise app.js -- -p [port] -a [address] -c [config-path]
```

## Tests

Before running any tests, please ensure Arise is configured to run on the same testnet that is used by the test-suite.

Replace **config.json** and **genesisBlock.json** with the corresponding files under the **test** directory:

```
cp test/config.json test/genesisBlock.json .
```

**NOTE:** If the node was started with a different genesis block previous, trauncate the database before running tests.

```
dropdb arise_test
createdb arise_test
```

**NOTE:** The master passphrase for this genesis block is as follows:

```
wagon stock borrow episode laundry kitten salute link globe zero feed marble
```

Launch Arise (runs on port 4000):

```
node app.js
```

Run the test suite:

```
npm test
```

Run individual tests:

```
npm test -- test/lib/accounts.js
npm test -- test/lib/transactions.js
```

## Authors

- Boris Povod <boris@crypti.me>
- Pavel Nekrasov <landgraf.paul@gmail.com>
- Sebastian Stupurac <stupurac.sebastian@gmail.com>
- Oliver Beddows <oliver@lightcurve.io>
- Isabella Dell <isabella@lightcurve.io>
- Marius Serek <mariusz@serek.net>
- Maciej Baj <maciej@lightcurve.io>

## License

Copyright © 2016-2017 Arise Foundation

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the [GNU General Public License](https://github.com/arisebank/arise/tree/master/LICENSE) along with this program.  If not, see <http://www.gnu.org/licenses/>.

***

This program also incorporates work previously released with arise `0.7.0` (and earlier) versions under the [MIT License](https://opensource.org/licenses/MIT). To comply with the requirements of that license, the following permission notice, applicable to those parts of the code only, is included below:

Copyright © 2016-2017 Arise Foundation  
Copyright © 2015 Crypti

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.