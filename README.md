# Logstash-Output-Sentry Plugin

This is a plugin for [Logstash](https://github.com/elasticsearch/logstash).

This plugin gives you the possibility to send your output parsed with Logstash to a Sentry host.

This plugin is fully free and fully open source. The license is Apache 2.0, meaning you are pretty much free to use it however you want in whatever way.

But keep in mind that this is not an official plugin, and this plugin is not supported by the Logstash community. 


## Documentation

### Installation 

You must have the [Logstash](https://github.com/elasticsearch/logstash) folder installed on your computer if you want to use this plugin.

As this plugin will be no longer supported by me in the future, you can install the plugin this way :  

* [Download](https://github.com/antho31/logstash-output-sentry/archive/master.zip) and extract or [clone](https://github.com/antho31/logstash-output-sentry.git) the project. 

* Open the Gemfile in your logstash-X.X.X folder with your favorite editor. 

* Add this line at the end of the Gemfile (don't forget to replace "[the_absolute_..._project]" with the correct path) : 

```ruby
gem "logstash-output-sentry", :path => "[the_absolute_path_where_you_put_the_project]/logstash-output-sentry"
```

* Install the plugin with this command (don't forget to replace "[logstash_path_folder]" with the correct path.) :
```sh
cd [logstash_path_folder]
bin/plugin install --no-verify
```

* You can now run this output plugin (see how below). Feel free to modify the file ```logstash-output-sentry/lib/logstash/output/sentry.rb``` if you need. 

### Usage 

[Sentry](https://getsentry.com/) is a modern error logging and aggregation platform.
It’s important to note that Sentry should not be thought of as a log stream, but as an aggregator. 
It fits somewhere in-between a simple metrics solution (such as Graphite) and a full-on log stream aggregator (like Logstash).

* In Sentry, generate and get your client key (Settings -> Client key). The client key has this form : 
```
[http|https]://[key]:[secret]@[host]/[project_id]
```

* In your Logstash configuration file, inform your client key : 
```ruby
output {
  sentry {
    'key' => "yourkey"
    'secret' => "yoursecretkey"
   'project_id' => "yourprojectid"
  }
 }
```

* Note that all your fields (incluing the Logstash field "message") will be in the "extra" field in Sentry. But be careful : by default , the host is set to "app.getsentry.com". If you have installed Sentry on your own machine, please change the host (change "http://localhost:9000" with the correct value according your configuration) :   
```ruby
output {
  sentry {
    'key' => "yourkey"
    'secret' => "yoursecretkey"
   'project_id' => "yourprojectid"
   'host' => "http://localhost:9000"
   'use_ssl' => false
  }
 }
```

* You can change the "message" field  (default : "Message from logstash"), or optionally specify a field to use from your event. In case the message field doesn't exist, it'll be used as the actual message.
```ruby
sentry {
   'key' => "87e60914d35a4394a69acc3b6d15d061"
   'secret' => "596d005d20274474991a2fb8c33040b8"
   'project_id' => "1"
   'msg' => "msg_field"
}
```

* You can indicate the level (default : "error"), and decide if all your Logstash fields will be tagged in Sentry. If you use the protocole HTTPS, please enable "use_ssl" (default : true), but if you use http you MUST disable ssl. 
```ruby
sentry {
   'key' => "87e60914d35a4394a69acc3b6d15d061"
   'secret' => "596d005d20274474991a2fb8c33040b8"
   'project_id' => "1"
   'host' => "http://192.168.56.102:9000"
   'msg' => "Message you want"
   'level_tag' => "fatal"
   'use_ssl' => false
   'fields_to_tags' => true 
}
```

## Contributing

All contributions are welcome: ideas, patches, documentation, bug reports, complaints, and even something you drew up on a napkin.

Note that this plugin has been written from [this Dave Clark's Gist](https://gist.github.com/clarkdave/edaab9be9eaa9bf1ee5f)

