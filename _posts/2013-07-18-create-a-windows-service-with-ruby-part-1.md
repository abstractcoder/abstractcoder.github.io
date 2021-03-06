---
title: Create a Windows Service with Ruby - Part 1
layout: post
last_modified: 2013-08-02
---

Recently I was tasked to create an application that needed to run in the background on a Windows machine, which seemed like the perfect job for a Windows Service. I've created Windows Services with .Net before but I wanted to try to create one with Ruby. It turns out that it was pretty easy to get a Windows Service up and running using Ruby.

The first step is to install Ruby on your Windows machine. I recommend the [Ruby Installer](http://rubyinstaller.org/.) For my project I installed Ruby 1.9.3-p448, and chose to add Ruby to my PATH during installation.

The next step is to install the [win32-service gem](https://github.com/djberg96/win32-service) from the [Win32Utils project](http://win32utils.rubyforge.org/.) The gems in this project expose the Windows APIs in Ruby.

``` bash
gem install win32-service
```

Next I created a script that is responsible for registering a new Windows Service.

### register.rb

``` ruby
require 'rubygems'
require 'win32/service'
include Win32

Service.create({
  service_name: 'testservice',
  host: nil,
  service_type: Service::WIN32_OWN_PROCESS,
  description: 'Test Service',
  start_type: Service::AUTO_START,
  error_control: Service::ERROR_NORMAL,
  binary_path_name: "#{`where ruby`.chomp} -C #{`echo %cd%`.chomp} service.rb",
  load_order_group: 'Network',
  dependencies: nil,
  display_name: 'Test Service'
})

Service.start("testservice")

```

This code creates a Windows Service named 'testservice' that runs a script named service.rb in the current directory. The service is set to run under the Network Service user and it is also set to auto start if the machine is restarted. This means that the service will run on startup without the need for a user to be logged in. Since this script creates a service under the Network Service user you will need to run this script with admin privileges. For more information read the [Win32 Service documentation](http://rubyforge.org/docman/view.php/85/595/service.html)

The following line is responsible for setting up the service to run your script with Ruby.

``` ruby
binary_path_name: "#{`where ruby`.chomp} -C #{`echo %cd%`.chomp} service.rb",
```

`where ruby` uses the Windows command line application 'where' to get the location of Ruby if its in your PATH. `echo %cd%` returns the path to the current directory. The chomp method simply removes any leading or trailing whitespace.

The -C option specifies that Ruby should set the working directory of the script to the directory where the script is located. This allows you to easily access other files in the directory.

The last line actually starts the service. This can also be accomplished by running the following command:

``` bash
sc start testservice
```

Next I created the script that the Windows Service will actually run.

### service.rb

``` ruby
require 'rubygems'
require 'win32/daemon'
include Win32

class TestDaemon < Daemon
  def service_main
      log 'started'
      while running?
        log 'running'
        sleep 10
      end
  end
  
  def service_stop
    log 'ended'
    exit!
  end
  
  def log(text)
    File.open('log.txt', 'a') { |f| f.puts "#{Time.now}: #{text}" }
  end
end

TestDaemon.mainloop
```

This script simply writes to a file named log.txt, which is in the same directory as service.rb. For more information read the [Win32 Daemon documentation](http://rubyforge.org/docman/view.php/85/596/daemon.html.)

Finally, I wrote a script that stops the service if its running, and also deletes it.

### unregister.rb

``` ruby
require 'rubygems'
require 'win32/service'
include Win32

SERVICE = 'testservice'

begin
  Service.stop(SERVICE) if Service.status(SERVICE).controls_accepted.include? "stop" 
rescue
end

Service.delete(SERVICE)
```

In [Part 2](http://abstractcoder.com/2013/08/02/create-a-windows-service-with-ruby-part-2.html,) I will discuss how I used the [ocra gem](https://github.com/larsch/ocra) to bundle the Ruby scripts into executables so they can be run on machines that don't have Ruby installed.

**Update 7/23/2013**

It seems that the Windows API doesn't like to receive an empty array for the service dependencies so I updated the code to pass nil instead.

I have also run into an issue where the line

``` ruby
if Service.stop(SERVICE) if Service.status(SERVICE).controls_accepted.include? "stop" 
```

seems to work in Windows 7, but not in Windows 8.1. I recommend stopping the service with

``` bash
sc stop testservice
```

before running unregister.rb.

Finally, I created a [GitHub repository](https://github.com/abstractcoder/example-ruby-windows-service) for the example code.