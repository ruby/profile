# Profile

Profile provides a way to Profile your Ruby application.

Profiling your program is a way of determining which methods are called and
how long each method takes to complete.  This way you can detect which
methods are possible bottlenecks.

Profiling your program will slow down your execution time considerably,
so activate it only when you need it.  Don't confuse benchmarking with
profiling.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'profile'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install profile

## Usage

There are two ways to activate Profiling:

### Command line

Run your Ruby script with <code>-rprofile</code>:

```bash
$ ruby -rprofile example.rb
```

If you're profiling an executable in your <code>$PATH</code> you can use
<code>ruby -S</code>:

```bash
$ ruby -rprofile -S some_executable
```

### From code

Just require 'profile':

```ruby
require 'profile'

def slow_method
  5000.times do
    9999999999999999*999999999
  end
end

def fast_method
  5000.times do
    9999999999999999+999999999
  end
end

slow_method
fast_method
```

The output in both cases is a report when the execution is over:

```
$ ruby -rprofile example.rb

  %   cumulative   self              self     total
 time   seconds   seconds    calls  ms/call  ms/call  name
 68.42     0.13      0.13        2    65.00    95.00  Integer#times
 15.79     0.16      0.03     5000     0.01     0.01  Fixnum#*
 15.79     0.19      0.03     5000     0.01     0.01  Fixnum#+
  0.00     0.19      0.00        2     0.00     0.00  IO#set_encoding
  0.00     0.19      0.00        1     0.00   100.00  Object#slow_method
  0.00     0.19      0.00        2     0.00     0.00  Module#method_added
  0.00     0.19      0.00        1     0.00    90.00  Object#fast_method
  0.00     0.19      0.00        1     0.00   190.00  #toplevel
```

#### It is also possible to profile only a piece of code

```
require 'profiler'

def method_0
  Array.new
end

def method_1
  Hash.new
end

Profiler__::start_profile
method_0
Profiler__::print_profile(STDERR)

method_1
```

```
$ ruby example.rb

  %   cumulative   self              self     total
 time   seconds   seconds    calls  ms/call  ms/call  name
 44.02     0.00      0.00        1     0.66     0.66  TracePoint#enable
  0.54     0.00      0.00        1     0.01     0.01  Class#new
  0.54     0.00      0.00        1     0.01     0.02  Object#method_0
  0.07     0.00      0.00        1     0.00     0.00  TracePoint#disable
  0.07     0.00      0.00        1     0.00     0.00  Array#initialize
  0.00     0.00      0.00        1     0.00     1.49  #toplevel
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/ruby/profile.
