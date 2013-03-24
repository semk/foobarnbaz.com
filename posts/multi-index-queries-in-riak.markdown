---
title: Multiple Index queries in Riak using Python
date: '25-02-2012'
time: '23:11'
tags: ['Python', 'Riak', 'NoSQL']
layout: 'post.html'
---

![Riak Logo](http://upload.wikimedia.org/wikipedia/en/5/53/Riak_product_logo.png)

[Riak](http://wiki.basho.com/Riak.html) is a [Amazon Dynamo](http://www.allthingsdistributed.com/2007/10/amazons_dynamo.html) inspired masterless Key-Value store written in Erlang. It is one of those [NoSQL](http://en.wikipedia.org/wiki/NoSQL) databases that is rock stable, production ready and promises zero downtime. I have been using Riak at work and was literally blown away by its simplicity (Setting up a three node cluster wouldn't even take ten minutes) and the kind of support the Riak Community provides. And it is amazingly fast too. The Data retrieval operations in Riak are basically using methods like Get (by key), MapReduce and Key Filters.

Riak also supports multiple backends for Key-value store. And Google's own [LevelDB](code.google.com/p/leveldb) is one of them. One of the advantage of using LevelDB with Riak is that they support Secondary Indexes. This is a way to retrieve data faster when you want to use an SQL like Query interface. But the problem is that Riak only supports single index queries. That means, you will be able to query only one field at a time.

I wrote a Python wrapper that allows multiple index queries using Secondary indexes and MapReduce. The basic steps are

1. Query Multiple Indexes and get the associated keys
2. Pass the keys to a MapReduce job where Multiple filters are again evaluated. The map phase applies all the conditions to individual keys.

>*As suggested by Elias Levy, it is ideal to compute the intersection of all the index queries rather than evaluating filters on Map Phase as this involves parsing the data and validating filters. This could become very slow when the number of keys returned by a single index query is larger compared to other indexes. The sources are updated to reflect this change.*

So the new steps are

1. Query Indexes and get the key sets
2. Find the intersection of these key sets
3. Pass the resulting keys to MapReduce where we fetch and sort the data.

Using this, you can write queries like

    :::python
    client = riak.RiakClient('localhost', 8091)
    bucket = client.bucket('test_multi_index')

    bucket.new('sree', {'name': 'Sreejith', 'age': 25}).\
        add_index('name_bin', 'Sreejith').\
        add_index('age_int', 25).store()
    bucket.new('vishnu', {'name': 'Vishnu', 'age': 31}).\
        add_index('name_bin', 'Vishnu').\
        add_index('age_int', 31).store()

    query = RiakMultiIndexQuery(client, 'test_multi_index')
    for res in query.filter('name', '==', 'Sreejith').run():
        print res

    query.reset()
    for res in query.filter('age', '<', 50).filter('name', '==', 'Vishnu').run():
        print res

    query.reset()
    for res in query.filter('age', '<', 50).order('age', 'ASC').run():
        print res

    query.reset()
    for res in query.limit(1).run():
        print res

    query.reset()
    for res in query.order('age', 'ASC').offset(1).limit(1).run():
        print res

You can find the full source code at [GitHub](https://github.com/semk/utils/blob/master/riak_multi_query.py).
