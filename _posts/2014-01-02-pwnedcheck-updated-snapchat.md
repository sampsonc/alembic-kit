---
title: PwnedCheck updated to also check for Snapchat
date: 2014-01-02T19:08:17+00:00
author: chs
layout: post
permalink: /pwnedcheck-updated-snapchat/
categories:
  - Open Source
  - Ruby
  - Security
---
PwnedCheck is a ruby gem that I wrote that checks an email address, phone number, or username against the new site by <a href="http://www.twitter.com/TroyHunt" title="Troy Hunt" target="_blank">Troy Hunt</a> called <a href="http://haveibeenpwned.com" title="haveibeenpwned.com" target="_blank">haveibeenpwned.com</a>. His site aggregates data from breaches and allows you to check to see if your data has been compromised. Use it as follows-

### Installation

<pre lang="ruby" line="0">gem install PwnedCheck
</pre>

### Usage:

<pre lang="ruby" line="1">require 'pwnedcheck'

# The 4 cases.
# foo@bar.com is a valid address on the site
# foo232323ce23ewd@bar.com is a valid address, but not on the site
# foo.bar.com is an invalid format
# mralexgray is a user id in snapchat
list = ['foo@bar.com', 'foo232323ce23ewd@bar.com', 'foo.bar.com', 'mralexgray']

list.each do |item|
  begin
    sites = PwnedCheck::check(item)
    if sites.length == 0
      puts "#{item} --> Not found on http://haveibeenpwned.com"
    else
      sites.each do |site|
        puts "#{item} --> #{site}"
      end
    end
  rescue PwnedCheck::InvalidEmail => e
    puts "#{item} --> #{e.message}"
  end
end
</pre>

### Output:

<pre lang="ruby">foo@bar.com --> Adobe
foo@bar.com --> Gawker
foo@bar.com --> Stratfor
foo232323ce23ewd@bar.com --> Not found on http://haveibeenpwned.com
foo.bar.com --> Not found on http://haveibeenpwned.com
mralexgray --> Snapchat
</pre>

The code is available at <a href="http://github.com/sampsonc/PwnedCheck" title="http://github.com/sampsonc/PwnedCheck" target="_blank">http://github.com/sampsonc/PwnedCheck</a> and the gem page is <a href="http://rubygems.org/gems/PwnedCheck" title="http://rubygems.org/gems/PwnedCheck" target="_blank">http://rubygems.org/gems/PwnedCheck</a>.
