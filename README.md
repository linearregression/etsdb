#ETSDB

An Erlang Time Series Database built with LevelDB and Riak Core.


# Developing

Install erlang R16 (limited by Riak Core)

    $ make shell_1

Brings you into a shell

You can now send metrics in via the graphite/carbon protocol to port 2003: http://graphite.readthedocs.org/en/1.0/feeding-carbon.html

To bring up a new node on the same box:

    $ make shell_2

When in the shell you can join it to the previous shell with

    (etsdb_2@127.0.0.1)1> riak_core:join("etsdb@127.0.0.1").

Which will move the appropriate vnodes across.

# Querying

To get the list of metrics available:

    http://localhost:8080/metrics

To get data for a metric

    http://localhost:8080/metrics/{metric}?from={timestamp}&until={timestamp}

Other fields available

    ?bucket_size=How many seconds of data to aggregate into a single datapoint returned

    ?aggregation=How to aggregate the data points (min/avg)


# Making a Release

On ubuntu

    apt-get install erlang libleveldb-dev libsnappy-dev build-essential
    gem install fpm --no-rdoc
    make deps apps release package


# Installing the release

Build a package on another machine with the same OS/architecture. Install
that package on another machine, it will include Erlang and ETSDB.

# Running

    # /etc/init.d/etsdb start

# Configuration

The package will install a `/etc/etsdb/etsdb.config`. Follow the contents
of `rel/sys.config` for details`

You may one to set the carbon port (which this defaults to 2008) to 2003, the
standard port. This would look like

   [{etsdb, [{graphite_port, 2003}]}].
