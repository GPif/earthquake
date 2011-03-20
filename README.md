Earthquake
====

Twitter Client on Terminal with Streaming API.

It supports ruby 1.9 only.

Install
----

    gem install earthquake

Usage
----

### Launch

    $ earthquake

Commands
----

### Tweet

    ⚡ Hello World!

### Searth

    ⚡ :search #ruby

### Eval

    ⚡ :eval Time.now

### Exit

    ⚡ :exit

### Restart

    ⚡ :restart

And there are more commands!

Customize
----

The config file is '~/.earthquake/config'.

### Changing the colors

    Earthquake.config[:colors] = (31..36).to_a - [34]

The blue is excluded.

### Running on debug mode

    Earthquake.config[:debug] = true

デバッグモードで動作しているとき、コードの修正は即座に反映される（正確にはコマンドの実行の直前にリロードされる）。

### Defining Plugin

"~/.earthquake/plugin" is the directory for plugins.
At launch, Earthquake try to load files under the directory.

### Defining your commands

#### A command named 'foo':

    Earthquake.init do
      command :foo do
        puts "foo!"
      end
    end

#### Handling the command args:

    Earthquake.init do
      command :hi do |m|
        puts "Hi #{m[1]}!"
      end
    end

The 'm' is a MatchData.

#### Using regexp:

    Earthquake.init do
      # Usage: :add 10 20
      command %r|^:add (\d+)\s+(\d+)|, :as => :add do |m|
        puts m[1].to_i + m[2].to_i
      end
    end

### Handling outputs

#### Keyword notifier:

    Earthquake.init do
      output do |item|
        next unless item["stream"]
        if item["text"] =~ /ruby/i
          notify "#{item["user"]["screen_name"]}: #{item["text"]}"
        end
      end
    end

#### Favorite notifier:

    Earthquake.init do
      output do |item|
        case item["event"]
        when "favorite"
          notify "[favorite] #{item["source"]["screen_name"]} => #{item["target"]["screen_name"]} : #{item["target_object"]["text"]}"
        end
      end
    end

### Defining filters

#### Filtering by keywords

    Earthquake.init do
      filter do |item|
        if item["stream"] && item["text"]
          item["text"] =~ /ruby/i
        else
          true
        end
      end
    end

TODO
----

* keyword tracking
* more intelligent completion
* spec
* typable id

Copyright
----

Copyright (c) 2011 jugyo. See LICENSE.txt for further details.