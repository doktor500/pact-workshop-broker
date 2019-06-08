### Creating a pact-broker

### Requirements

[Docker](https://docs.docker.com/)

In this step we are going to see how to integrate everything with a pact-broker.

The consumer will publish the pact contracts to the broker and the provider will validate the contract uploaded in the broker, by doing this, the consumer and the provider can know when an integration is broken and put on hold a release process to avoid failures in a production environment.

There are two ways to work with a pact-broker using a "Pact broker as a service" that it is available on [pactflow.io](https://pactflow.io/), or setup our own pact-broker and hosted in our preferred cloud provider.

For the purpose of this workshop we are going to use a pact-broker on [pactflow.io](https://pactflow.io/)

#### Creating a pact-broker based on the official docker image

This step is not needed for the workshop and it is just here for reference, you can skip this step.

The pact-broker uses a postgresql database to keep track of the contracts and their states.

Create the directory for the pact-broker

`mkdir broker && cd broker`

Create a docker-compose file

`touch docker-compose.yml`

With the following content:

```yaml
version: '2'

services:
  pactbroker:
    image: dius/pact-broker
    environment:
      PACT_BROKER_DATABASE_ADAPTER: postgres
      PACT_BROKER_DATABASE_USERNAME: pactuser
      PACT_BROKER_DATABASE_PASSWORD: pass
      PACT_BROKER_DATABASE_HOST: postgres
      PACT_BROKER_DATABASE_NAME: pactbroker
    ports:
      - 8000:80
    depends_on:
      - postgres

  postgres:
    image: postgres:10.4
    environment:
      POSTGRES_USER: pactuser
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: pactbroker
    ports:
      - 5432:5432
```

Execute `docker-compose up`

Navigate to `localhost:8000` to see the Web interface of the pact-broker that you have just created.

Now set the following environment variable in your `~/.basrc`, `~/.zshrc`, `~/.fishrc` etc.

```bash
export PACT_BROKER_BASE_URL=http://localhost:8000
```

Don't add a final slash to the URL in the `PACT_BROKER_BASE_URL` environment variable

Restart your terminal or source the file in all your active tabs `source ~/.basrc`, `source ~/.zshrc`, `source ~/.fishrc` etc.

You should be able to execute

```bash
echo $PACT_BROKER_BASE_URL
```

Congratulations, you have a pact-broker up and running, now follow the instructions in the `pact-workshop-consumer repository` readme file

#### Creating a pact-broker account on pactflow.io

Creating a pact-broker account on pactflow.io is need it for the second part of the workshop. You don't need to do this now if you have just created a broker using docker compose.

If you are in the second part of the workshop, navigate to [pactflow.io](https://pactflow.io/) and click on the "Sign up" link

![Pactflow Step 1](https://github.com/doktor500/pact-workshop-broker/blob/master/resources/pactflow-step1.png "Pactflow Step 1")

Choose the free plan

![Pactflow Step 2](https://github.com/doktor500/pact-workshop-broker/blob/master/resources/pactflow-step2.png "Pactflow Step 2")

And fill in the form to create a new account, choose a subdomain that is meaningful to you

![Pactflow Step 3](https://github.com/doktor500/pact-workshop-broker/blob/master/resources/pactflow-step3.png "Pactflow Step 3")

Click on the settings icon that can be found in the top right corner

![Pactflow Step 4](https://github.com/doktor500/pact-workshop-broker/blob/master/resources/pactflow-step4.png "Pactflow Step 4")

And copy the "Read/write token" to your clipboard

![Pactflow Step 5](https://github.com/doktor500/pact-workshop-broker/blob/master/resources/pactflow-step5.png "Pactflow Step 5")

Now set the following environment variables in your `~/.basrc`, `~/.zshrc`, `~/.fishrc` etc.

```bash
export PACT_BROKER_BASE_URL=https://${YOUR_DOMAIN}.pact.dius.com.au
export PACT_BROKER_TOKEN=${YOUR_TOKEN}
```

Don't add a final slash to the URL in the `PACT_BROKER_BASE_URL` environment variable

Replacing the domain and the token with the pactflow.io URL of your account and the "Read/write token" value that you fetched from the settings page.

Restart your terminal or source the file in all your active tabs `source ~/.basrc`, `source ~/.zshrc`, `source ~/.fishrc` etc.

You should be able to execute

```bash
echo $PACT_BROKER_BASE_URL
echo $PACT_BROKER_TOKEN
```
