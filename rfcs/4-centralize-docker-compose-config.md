# RFC 4 - Centralize `docker-compose` configuration
* Comments: [#4](https://api.github.com/repos/mozilla-conduit/developer-productivity-rfcs/issues/4)
* Proposed by: @mitchhentges

# Summary

* The `docker-compose` definitions of how to start each service for development should only exist 
in [`suite`](https://github.com/mozilla-conduit/suite/)
* For each service that has automated tests that require docker, they should have a `docker-file.test.yml` in their 
repository.

## Motivation

Right now, we define the development environment for each of our services multiple 
times. For example, configuration for starting a local copy of BMO exists in the following places: 
* in [`bmo`](https://github.com/mozilla-bteam/bmo/blob/master/docker-compose.yml) as `docker-compose`,
* in [`bmo`](https://github.com/mozilla-bteam/bmo/blob/master/Vagrantfile) as `vagrant` 
* in [`phabricator-extensions`](https://github.com/mozilla-services/phabricator-extensions/blob/master/docker-compose.bmo.yml) as `docker-compose`
* and in [`suite`](https://github.com/mozilla-conduit/suite/blob/master/docker-compose.bmo.yml) as `docker-compose`

This makes maintaining and working on BMO harder:

* If you change BMO's expected environment, you need to find all the potential `docker-compose `"development 
environments" and update them
* If you're a developer who is working on Phabricator-to-BMO integration and you use the `docker-compose.bmo.yml` in the 
`phabricator-extensions` repo, you have to hope that it's up-to-date with BMO

By centralizing the service definitions for development, then we have two benefits:
1. When a service's expected environment changes, you only have a single "development environment" to update2. As a
developer working on a service (or something that depends on that service) it's less likely to be broken since everyone 
is using the same `docker-compose` development environment

# Details

1. For each service, ensure that it's definition in `suite` captures all development requirements
    * For each redundant configuration that is going to be thrown away, ensure the setup in `suite` doesn't regress in performance
2. Remove all development `docker-compose` files that aren't in `suite` (leave `docker-compose.test.yml` files since
they're needed for CI, of course)
3. Update documentation in each repository to recommend using `suite` and its directory structure (top-level `conduit` 
folder with all projects underneath)

# Open Questions

* One main assumption here is that each service can be defined with "one true development environment™". Is there a case
in which we need multiple different development configurations for the same service?
* Is this worth the tradeoff of forcing devs to have the `suite` repository to work on any project?

# Implementation



