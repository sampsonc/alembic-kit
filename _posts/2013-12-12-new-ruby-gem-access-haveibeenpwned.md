---
title: New ruby gem to access @haveibeenpwned.
date: 2013-12-12T10:36:48+00:00
author: chs
layout: post
permalink: /new-ruby-gem-access-haveibeenpwned/
categories:
  - Ruby
  - Security
---
So, I decided to figure out how to create a ruby gem and decided to start with a simple gem that checks an email address against [http://haveibeenpwned.com](http://haveibeenpwned.com "http://haveibeenpwned.com").

### Installation

gem install PwnedCheck

### Usage:

<pre lang="ruby" line="1">require 'pwnedcheck'

# The 3 cases.
# foo@bar.com is a valid address on the site
# foo232323ce23ewd@bar.com is a valid address, but not on the site
# foo.bar.com is an invalid format
addresses = ['foo@bar.com', 'foo232323ce23ewd@bar.com', 'foo.bar.com']

addresses.each do |address|
  begin
    sites = PwnedCheck::check(address)
    if sites.length == 0
      puts "#{address} --> Not found on http://haveibeenpwned.com"
    else
      sites.each do |site|
        puts "#{address} --> #{site}"
      end
    end
  rescue PwnedCheck::InvalidEmail => e
    puts "#{address} --> #{e.message}"
  end
end
</pre>

The code is available at <a href="http://github.com/sampsonc/PwnedCheck" title="http://github.com/sampsonc/PwnedCheck" target="_blank">http://github.com/sampsonc/PwnedCheck</a> and the gem page is <a href="http://rubygems.org/gems/PwnedCheck" title="http://rubygems.org/gems/PwnedCheck" target="_blank">http://rubygems.org/gems/PwnedCheck</a>.

Let me know what you think!
