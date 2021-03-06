# Documentation for ML UI developers

## Requirements

To use machine learning features, you must have a Platinum or Enterprise license 
or a free 14-day trial. File Data Visualizer requires a Basic license. For more 
info, refer to 
[Set up machine learning features](https://www.elastic.co/guide/en/machine-learning/master/setup.html).

## Setup local environment

### Kibana

1. Fork and clone the [Kibana repo](https://github.com/elastic/kibana).

1. Install `nvm`, `node`, `yarn` (for example, by using Homebrew). See 
   [Install dependencies](https://www.elastic.co/guide/en/kibana/master/development-getting-started.html#_install_dependencies).

1. Make sure that Elasticsearch is deployed and running on localhost:9200.

1. Navigate to the directory of the `kibana` repository on your machine.

1. Fetch the latest changes from the repository.

1. Checkout the branch of the version you want to use. For example, if you want 
   to use a 7.9 version, run `git checkout 7.9`.

1. Run `nvm use`. The response shows the Node version that the environment uses. 
   If you need to update your Node version, the response message contains the 
   command you need to run to do it.

1. Run `yarn kbn bootstrap`. It takes all the dependencies in the code and 
   installs/checks them. It is recommended to use it every time when you switch 
   between branches.

1. Make a copy of `kibana.yml` and save as `kibana.dev.yml`. (Git will not track
   the changes in `kibana.dev.yml` but yarn will use it.)
   
1. Provide the appropriate password and user name in `kibana.dev.yml`.

1. Run `yarn start` to start Kibana.

1. Go to http://localhost:560x/xxx (check the terminal message for the exact 
   path).

For more details, refer to this [getting started](https://www.elastic.co/guide/en/kibana/master/development-getting-started.html) page.

### Adding sample data to Kibana

Kibana has sample data sets that you can add to your setup so that you can test 
different configurations on sample data.

1. Click the Elastic logo in the upper left hand corner of your browser to 
   navigate to the Kibana home page.

1. Click *Load a data set and a Kibana dashboard*.

1. Pick a data set or feel free to click *Add* on all of the available sample 
   data sets.

These data sets are now ready be analyzed in ML jobs in Kibana.


## Running tests

### Jest tests

Run the test following jest tests from `kibana/x-pack`.

New snapshots, all plugins:   

```
node scripts/jest
```
 
Update snapshots for the ML plugin: 

```
node scripts/jest plugins/ml -u
```

Update snapshots for a specific directory only: 

```
node scripts/jest plugins/ml/public/application/settings/filter_lists
```

Run tests with verbose output: 

```
node scripts/jest plugins/ml --verbose
```

### Functional tests

Before running the test server, make sure to quit all other instances of 
Elasticsearch.

1. From one terminal, in the x-pack directory, run:

        node scripts/functional_tests_server.js --config test/functional/config.js

   This command starts an Elasticsearch and Kibana instance that the tests will be run against.

1. In another tab, run the following command to perform API integration tests (from the x-pack directory):

        node scripts/functional_test_runner.js --include-tag mlqa --config test/api_integration/config
        
   ML API integration tests are located in `x-pack/test/api_integration/apis/ml`.

1. In another tab, run the following command to perform UI functional tests (from the x-pack directory):

        node scripts/functional_test_runner.js --include-tag mlqa
        
   ML functional tests are located in `x-pack/test/functional/apps/ml`.

## Shared functions


You can find the ML shared functions in the following files in GitHub:

```
https://github.com/elastic/kibana/blob/master/x-pack/plugins/ml/public/shared.ts
```

```
https://github.com/elastic/kibana/blob/master/x-pack/plugins/ml/server/shared.ts
```

These functions are shared from the root of the ML plugin, you can import them with an import statement. For example:

```
import { MlPluginSetup } from '../../../../ml/server';
```

or 

```
import { ANOMALY_SEVERITY } from '../../ml/common';
```

Functions are shared from the following directories:

```
ml/common
ml/public
ml/server
```
