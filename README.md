# Introducing Voilk (beta)

Voilk is a Delegated Proof of Stake blockchain that uses a "Proof of Brain" social consensus algorithm for token allocation.

  - Currency symbol VOILK and VSD.
  - VOILK is created via the social consensus algorithm and users are free to trade their stable VSD currency with it via the `internal market` also participate in mining/minting process of it.
  - Create VSD on demand, allows the company to peg VSD with USD, and manage it (only @voilk can create VSD)
  - Create accounts only via the @voilk, no other account can create accounts.
  - VOILK is however, created via the social consensus algorithm and users are free to trade their stable VSD currency with it via the `internal market` also participate in mining/minting process of it.
  - 1.032 VOILK block reward which narrows down to 0.100 VOILK over 20 years
  - 85% of the reward goes to "Proof of Brain" social consensus algorithm.
    - 25% to curators (people who choose with their likes/upvotes which content is good)
    - 75% to content creators (people who create the content)
  - 15% of the reward goes to the block producers. (called witnesses)
  - Author rewards linear
  - Curation rewards quadratic
  
Premine option available
  - 10 Million VOILK premine
  - 10 Million VSD premine (the amount does not matter here, since it can be produced on demand)
  - Premine of VSD does not matter, since it always will be pegged with the USD, you can get 1 USD against your 1 VSD and vice versa from the company/person/organization who runs the project.

# No Support & No Warranty

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.


# Quickstart

Just want to get up and running quickly? We have pre-built docker images for your convenience. More details are in our [quickstart guide](https://github.com/voilknetwork/voilk/blob/master/doc/exchangequickstart.md).

# Building

We strongly recommend using the latest binaries that we release, the latest ones, You can find them on the releases tab/section. 

However if you would still like to build from the source, we do have [build instructions](https://github.com/voilknetwork/voilk/blob/master/doc/building.md) for Linux (Ubuntu LTS) and macOS.


# Environment variables

There are quite a few environment variables that can be set to run voilkd in different ways:

* `USE_WAY_TOO_MUCH_RAM` - if set to true, voilkd starts a 'full node'
* `USE_FULL_WEB_NODE` - if set to true, a default config file will be used that enables a full set of API's and associated plugins.
* `USE_NGINX_FRONTEND` - if set to true, this will enable an NGINX reverse proxy in front of voilkd that proxies websocket requests to voilkd. This will also enable a custom healthcheck at the path '/health' that lists how many seconds away from current blockchain time your node is. It will return a '200' if it's less than 60 seconds away from synced.
* `USE_MULTICORE_READONLY` - if set to true, this will enable voilkd in multiple reader mode to take advantage of multiple cores (if available). Read requests are handled by the read-only nodes, and write requests are forwarded back to the single 'writer' node automatically. NGINX load balances all requests to the reader nodes, 4 per available core. This setting is still considered experimental and may have trouble with some API calls until further development is completed.
* `HOME` - set this to the path where you want voilkd to store it's data files (block log, shared memory, config file, etc). By default `/var/lib/voilkd` is used and exists inside the docker container. If you want to use a different mountpoint (like a ramdisk, or a different drive) then you may want to set this variable to map the volume to your docker container.

# PaaS mode

Voilkd now supports a PaaS mode (platform as a service) that currently works with Amazon's Elastic Beanstalk service. It can be launched using the following environment variables:

* `USE_PAAS` - if set to true, voilkd will launch in a format that works with AWS EB. Containers will exit upon failure so that they can be relaunched automatically by ECS. This mode assumes `USE_WAY_TOO_MUCH_RAM` and `USE_FULL_WEB_NODE`, they do not need to be also set.
* `S3_BUCKET` - set this to the name of the S3 bucket where you will store shared memory files for voilkd in Amazon S3. They will be stored compressed in bz2 format with the file name `blockchain-$VERSION-latest.tar.bz2`, where $VERSION is the release number followed by the git short commit hash stored in each docker image at `/etc/voilkdversion`.
* `SYNC_TO_S3` - if set to true, the node will function to only generate shared memory files and upload them to the specified S3 bucket. This makes fast deployments and autoscaling for voilkd possible.

# Config File

Run `voilkd` once to generate a data directory and config file. The default location is `.voilkd`. Kill `voilkd`. It won't do anything without seed nodes. If you want to modify the config to your liking, we have two example configs used in the docker images. [full node](contrib/fullnode.config.ini) ) All options will be present in the default config file and there may be more options needing to be changed from the docker configs (some of the options actually used in images are configured via command line).

# Seed Nodes

With latest build, we actually added the seed nodes in the config file,  but if you still need to add for some reason

`seed.voilk.com:3331`

This same file is baked into the docker images and can be overridden by
setting `VOILKD_SEED_NODES` in the container environment at `docker run`
time to a whitespace delimited list of seed nodes (with port).

# CLI Wallet

We provide a basic cli wallet for interfacing with `voilkd`. The wallet is self documented via command line help. The node you connect to via the cli wallet needs to be running the `account_by_key_api`, `condenser_api`, and needs to be configured to accept websocket connections via `webserver-ws-endpoint`.

# Building

See [doc/building.md](doc/building.md) for detailed build instructions, including
compile-time options, and specific commands for Linux (Ubuntu LTS) or macOS.

# Testing

See [doc/testing.md](doc/testing.md) for test build targets and info
on how to use lcov to check code test coverage.

# System Requirements

  - Linux Ubuntu 16.04
  - 10 GB Ram
  - 20 GB Disk Storage

On Linux use the following Virtual Memory configuration for the initial sync and subsequent replays. It is not needed for normal operation.

```
echo    75 | sudo tee /proc/sys/vm/dirty_background_ratio
echo  1000 | sudo tee /proc/sys/vm/dirty_expire_centisecs
echo    80 | sudo tee /proc/sys/vm/dirty_ratio
echo 30000 | sudo tee /proc/sys/vm/dirty_writeback_centisecs
```

# Credits

Even though the code was changed (it was re-purposed) but it was originally created by the `STEEM` developers, so most of the credit goes to them :)
