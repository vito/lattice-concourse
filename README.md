# Concourse on Lattice

These two scripts will get you going from a regular old
[Lattice](https://github.com/cloudfoundry-incubator/lattice) deployment to
a running, scalable [Concourse](https://concourse-ci.org) cluster.

Have fun!

## usage

Grab [Lattice](https://github.com/cloudfoundry-incubator/lattice/releases),
deploy it somewhere (follow their docs), and target it.

Then, run:

```sh
./start-atc 'postgres://user:pass@your-progres-server.com:5432/atc'
./start-worker 'http://atc.your-lattice-ip.xip.io'
```

This will spin up a single, empty ATC server, and then one worker, which will
register with the ATC.

To populate the ATC with a [Concourse pipeline
config](https://concourse-ci.org/pipelines.html), download
[Fly](https://concourse-ci.org/fly-cli.html) from the ATC's web UI, and run
the following:


```sh
export ATC_URL='http://atc.your-lattice-ip.xip.io'
fly configure -c path/to/pipeline.yml
```

Or, just run [one-off
builds](https://concourse-ci.org/fly-cli.html#%28part._fly-execute%29) against
it.

## scaling up

Both the ATC and the workers can be scaled up and down. though it doesn't
really make sense to have any more than you have cells.

## caveats

* Concourse's build scheduling across workers is nowhere near as sophisticated
  as Diego's, but is "good enough" to just throw on Lattice, given that most
  builds are finite and don't have predetermined memory/disk limits.

* Currently the workers are publicly routable. This is bad.

* The PostgreSQL server must run externally. Once Diego does persistence/TCP
  routing this will be easier.

* The workers have no memory/disk limits, and may tend to monopolize their
  host cell.
