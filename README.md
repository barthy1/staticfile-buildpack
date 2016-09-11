#Cloud Foundry Static Buildpack
[![CF Slack](https://s3.amazonaws.com/buildpacks-assets/buildpacks-slack.svg)](http://slack.cloudfoundry.org)

A Cloud Foundry [buildpack](http://docs.cloudfoundry.org/buildpacks/) for static stites (HTML/JS/CSS).

### Buildpack User Documentation

Official buildpack documentation can be found at http://docs.cloudfoundry.org/buildpacks/staticfile/index.html.

### Building the Buildpack

1. Make sure you have fetched submodules

  ```bash
  git submodule update --init
  ```

1. Get latest buildpack dependencies

  ```shell
  BUNDLE_GEMFILE=cf.Gemfile bundle
  ```

1. Build the buildpack

  ```shell
  BUNDLE_GEMFILE=cf.Gemfile bundle exec buildpack-packager [ --cached | --uncached ]
  ```

1. Use in Cloud Foundry

  Upload the buildpack to your Cloud Foundry and optionally specify it by name

  ```bash
  cf create-buildpack custom_node_buildpack node_buildpack-offline-custom.zip 1
  cf push my_app -b custom_node_buildpack
  ```

### Update to ppc64le
1. Build cloudfoundry/cflinuxfs2 Docker image

   ``` bash
   git clone https://github.com/cloudfoundry/stacks.git 
   cd stacks
   make
   ```

1. Build nginx blob [using Docker](https://github.com/cloudfoundry/binary-builder#running-within-docker). 

   ``` bash
   sudo docker run -w /binary-builder -v `pwd`:/binary-builder -it cloudfoundry/cflinuxfs2 ./bin/binary-builder --name=nginx --version=1.11.3 --md5=18275c1daa39c5fac12e56c34907d45b
   ```

1. Make nginx-1.11.3-linux-x64.tgz blob available to download

1. Update manifest.yml with link to blob and md5

### Testing
Buildpacks use the [Machete](https://github.com/cloudfoundry/machete) framework for running integration tests.

To test a buildpack, run the following command from the buildpack's directory:

```
BUNDLE_GEMFILE=cf.Gemfile bundle exec buildpack-build
```

More options can be found on Machete's [Github page.](https://github.com/cloudfoundry/machete)

### Contributing

Find our guidelines [here](./CONTRIBUTING.md).

### Help and Support

Join the #buildpacks channel in our [Slack community] (http://slack.cloudfoundry.org/) if you need any further assistance.

### Reporting Issues

Open a GitHub issue on this project [here](https://github.com/cloudfoundry/staticfile/issues/new)

### Active Development

The project backlog is on [Pivotal Tracker](https://www.pivotaltracker.com/projects/1042066)

### Acknowledgements

This buildpack is based heavily upon Jordon Bedwell's Heroku buildpack and the modifications by David Laing for Cloud Foundry [nginx-buildpack](https://github.com/cloudfoundry-community/nginx-buildpack). It has been tuned for usability (configurable with `Staticfile`) and to be included as a default buildpack (detects `Staticfile` rather than the presence of an `index.html`). Thanks for the buildpack Jordon!
