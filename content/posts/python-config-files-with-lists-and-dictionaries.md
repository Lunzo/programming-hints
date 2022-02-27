---
title: "Python Config Files with Lists and Dictionaries"
date: 2014-11-16T01:25:00
tags: [config, json, python]
---
A common problem with configuration files is that they only allow simple values like strings, integers, doubles. It's not uncommon to want to include more complex values such as a list or dictionary in your config.

The most elegant approach I've seen for doing this in python is to make use of the json library to parse the values. First I'll load an example config file using python's [SafeConfigParser](https://docs.python.org/3/library/configparser.html). Typically your config would be in a separate file, however for the purposes of this blog post I'll use a multi-line string.

```
>>> from configparser import SafeConfigParser
>>> import json
>>> config_text = """
[examples]
list_example=[1,2,3,4]
dict_example={"foo":1, "bar":2}
"""
>>> parser = SafeConfigParser()
>>> parser.read_string(config_text)
>>> parser.items("examples")
[('list_sample', '[1,2,3,4]'), ('map_example', '{"foo":1, "bar":2}')]
```

The list of tuples returned by `ConfigParser.items(section)` can be tricky to work with, unless you were planning to iterate through all values in the config file. I find it easier to convert the list into a dictionary, which is simple in python:

`config = dict(parser.items("examples"))`

Now that we have a dictionary which maps keys in the config file to their values, you can use `json.loads(str)` to convert values from strings to a list or dictionary. The operations after calling `json.loads` in the next part of my interpreter session are to demonstrate that these objects are actually a real python list and dictionary.

```
>>> config
{'list_example': '[1,2,3,4]', 'dict_example': '{"foo":1, "bar":2}'}

>>> list_example = json.loads(config['list_example'])
>>> list_example
[1, 2, 3, 4]
>>> list_example[2]
3
>>> list_example[:3]
[1, 2, 3]
>>> 5 in list_example
False

>>> dict_example = json.loads(config['dict_example'])
>>> dict_example
{'bar': 2, 'foo': 1}
>>> dict_example['foo']
1
>>> 'bar' in dict_example
True
```