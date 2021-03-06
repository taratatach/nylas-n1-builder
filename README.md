### DEPRECATED
Latest release of N1 let you specify a custom sync-engine URL with no need to change sources and rebuild the package.
So this image is not necessary anymore. Simply edit your config.cson with "custom" as `env` value and specify the URL like this :
```cson
"*":
  env: "custom"
  syncEngine:
    APIRoot: "http://your-hostname.com:5555"
  nylas:
    accounts: [
      {
        server_id: "{ACCOUNT_ID_1}"
        ...
```

You can still see https://hub.docker.com/r/nhurel/nylas-sync-engine/ to start a sync-engine container

### N1 Builder
This image lets you build a custom package for the N1 email client.
It gives you the possibility to customize N1 and make it point to a self-hosted instance of the sync engine.
For more information about N1 email client and all nylas production, go to https://www.nylas.com
and look at their repositories https://github.com/nylas/sync-engine and https://github.com/nylas/N1

### Usage
This image is designed to run a one-shot container and destroy it.
Assuming your sync engine is available at `http://your-hostname.com:5555`,  run :
```bash
docker run -e ENGINE_URL=http://your-hostname.com:5555 --name n1 nhurel/nylas-n1-builder
docker cp n1:/tmp/nylas-build/nylas-0.4.9-amd64.deb .
docker rm n1
```

### Install N1
Once your package is built, simply run the following command to install it :
```bash
dpkg -i nylas-0.4.9-amd64.deb
```

### Build your RPM
This image now supports also building the RPM package of N1.
After your docker run, get it simply by running :
```bash
docker cp n1:/tmp/nylas-build/nylas-0.4.9-0.1.x86_64.rpm .
```

### Confugure N1 to use your self-hsoted sync engine
Edit or create the `~/.nylas/config.cson` file and define `selfhosted` for the `env` attribute. For example :
```cson
"*":
  env: "selfhosted"
  nylas:
    accounts: [
      {
        server_id: "{ACCOUNT_ID_1}"
        object: "account"
        account_id: "{ACCOUNT_ID_1}"
        name: "{YOUR NAME}"
        provider: "{PROVIDER_NAME}"
        email_address: "{YOUR_EMAIL_ADDRESS}"
        organization_unit: "{folder or label}"
        id: "{ACCOUNT_ID_1}"
      }
      {
        server_id: "{ACCOUNT_ID_2}"
        object: "account"
        account_id: "{ACCOUNT_ID_2}"
        name: "{YOUR_NAME}"
        provider: "{PROVIDER_NAME}"
        email_address: "{YOUR_EMAIL_ADDRESS}"
        organization_unit: "{folder or label}"
        id: "{ACCOUNT_ID_2}"
      }
    ]
    accountTokens:
      "{ACCOUNT_ID_1}": "{ACCOUNT_ID_1}"
      "{ACCOUNT_ID_2}": "{ACCOUNT_ID_2}"
```

More information about running N1 against a self hosted sync engine can be found here : https://github.com/nylas/N1/blob/master/CONTRIBUTING.md#running-against-open-source-sync-engine

### Need help to run your sync-engine ?
See https://hub.docker.com/r/nhurel/nylas-sync-engine/ to start a sync-engine container
