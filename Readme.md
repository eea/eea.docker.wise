# Docker Orchestration for WISE Portal

## Installation

1. Install [Docker](https://docs.docker.com/engine/installation/linux/centos/).

2. Install [Docker Compose](https://docs.docker.com/compose/install/).

3. Install [Rancher CLI](http://www.rancher.com)

## Usage

### Development

In order to be able to edit source-code on your machine using your favorite editor, without having to do it inside a Docker container, you'll have to create a new user on your laptop with `uid=500` and use this user for development:

    $ useradd -u 500 zope-www
    $ usermod -a -G docker zope-www
    $ sudo su - zope-www

1. Get the source code:

        $ git clone https://github.com/eea/eea.docker.wise.git
        $ cd eea.docker.wise/wise-devel

2. Update development stack:

        $ git pull
        $ docker-compose -f source-code.yml up
        $ docker-compose pull

3. Start development stack:

        $ docker-compose up -d

4. Start Plone in foreground:

        $ docker-compose exec plone bin/instance fg

5. Within your favorite browser head to http://localhost:8080, add a Plone site and install the following add-ons:
    * `EEA Plone buildout profile`
    * `wise.theme`
    * `wise.content`

6. Now you are ready to develop Plone Add-ons within `src` folder:

        $ ls -l src/

7. You can do normal Plone development, including setting breakpoints with `ipdb`


### Deployment

#### Pre-requirements:

* [Rancher CLI](https://docs.rancher.com/rancher/v1.2/en/cli/)
* Within Rancher UI register min 1 hosts with label: `wise=yes`

On your laptop:

    $ git clone https://github.com/eea/eea.docker.wise.git
    $ cd eea.docker.wise

#### Access rights

To enable Rancher CLI to launch services in a Rancher instance, you’ll need to configure it
See related [Rancher documentation](http://docs.rancher.com/rancher/v1.3/en/api/v2-beta/access-control/)
on how to obtain your Rancher API Keys. Thus:

1. Via Rancher UI:

    * Go to **API Tab** add an **Account API Key**

2. On your laptop configure Rancher CLI:

        $ rancher config

3. Make sure that you're deploying within the right environment:

        $ rancher config -p

#### Deploy in production:

    $ rancher up -d

#### Upgrade:

    $ rancher up -d --upgrade --batch-size 1

...and confirm that the upgrade went well:

    $ rancher up -d --confirm-upgrade

...or roll-back:

    $ rancher up -d --roll-back
