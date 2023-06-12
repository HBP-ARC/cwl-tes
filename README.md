[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

# GA4GH CWL Task Execution

___cwl-tes___ submits your tasks to a TES server. Task submission is parallelized when possible.


## Requirements

* Python 2.7 / 3.5 / 3.6

* [Docker](https://docs.docker.com/)


## Quickstart

How to run a CWL workflow on the EBRAINS experimental TES server:

1. Clone this repo:

        git clone git@gitlab.ebrains.eu:technical-coordination/private/workflows/cwl-tes.git


2. Install this version of the cwl-tes package:

        # create a virtualenv:
        python3 -m virtualenv cwl-env
        source cwl-env/bin/activate
        
        # or venv:
        python3 -m venv cwl-env
        source cwl-env/bin/activate
        pip install --upgrade pip
        pip install --upgrade wheel

        # and install cwl-tes:
        cd cwl-tes/
        # to ensure that latest version of py-tes is installed:
        pip uninstall py-tes
        pip install .


3. Make sure that you have access to the BSC Swift object storage and that your credentials are stored correctly in the `AWS_{ACCESS_KEY_ID,SECRET_ACCESS_KEY}` environment variables:

        export AWS_ACCESS_KEY_ID=EXAMPLE_KEY_ID
        export AWS_SECRET_ACCESS_KEY=EXAMPLE_ACCESS_KEY

    For info on how to get the access key id and secret access key, see here: https://www.bsc.es/supportkc/docs-ncloud/objectstorage


4. Obtain an EBRAINS token (from the Collaboratory):

        export TOKEN=EXAMPLE_TOKEN

5. Find a CWL workflow and run it using cwl-tes:

        cwl-tes --tes <tes-endpoint> --remote-storage-url <object-storage-endpoint>/<container_name> --token $token <workflow>.cwl <workflow_info>.yml

For example:

        cwl-tes --tes https://tes.apps-dev.hbp.eu/ga4gh/tes --remote-storage-url https://swift.bsc.es/tesk-ebrains-storage --token $TOKEN workflow.cwl workflow_info.yml


## Run the v1.0 conformance tests

To start a funnel server instance automatically and run all of the tests, install [tox](https://github.com/tox-dev/tox/) and run it

```
$ pip install tox
$ tox
```

For running only the conformance tests in python 2.7:

```
$ tox -e py27-unit
```

In a similar way they can be run on any supported python interpreter.

_A more manual approach:_

Download the conformance tests:

```
git submodule update --init --recursive
```

Start the funnel server.

```
funnel server --config /path/to/config.yaml
```

Make sure that TMPDIR is specified in the AllowedDirs of your Local storage configuration.

To run all the tests:

```
./tests/run_conformance.sh
```

To run a specifc test:

```
./tests/run_conformance.sh 10
```