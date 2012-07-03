doppelganger
============
This is a simple app that can be used as a post commit hook in riak to
replicate data.

To use this as a post-commit hook, set the bucket property. This can be set
as a default for all buckets setting the default_bucket_props, which is a 
property of riak_core. Note that the syntax for the postcommit hook entry
is a raw mochijson struct. I imagine that this will change in the future.

    {default_bucket_props, [
       {postcommit, [
         {struct, [{<<"mod">>,<<"doppelganger">>}, {<<"fun">>,<<"replicate">>}] }
       ]},
    ]}

Doppelganger requires some properties to be set in order to replicate data
properly. The only options are to activate the module and set the target
riak host and port. 

    {doppelganger, [
      {active, true},
      {riak_host, "your-doppelganger-host" },
      {riak_port, 8081} % Should be your PB port
    ]},


Future
======
1. Detect network partition and cache updates locally (maybe in a 
   separate riak bucket or in ets)
2. Potentially support multiple riak nodes (but probably leave as is, which
   would be one-for-one)
3. Preserve client id and any other meta data that needs to be preserved
   from the original client request

References
==========
http://wiki.basho.com/Commit-Hooks.html
http://wiki.basho.com/Configuration-Files.html#default_bucket_props
https://github.com/basho/riak-erlang-client



