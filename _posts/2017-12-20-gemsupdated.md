---
title: Two Gems Updated
date: 2017-12-20T07:37:06+00:00
author: chs
layout: post
permalink: /gemsupdated/
categories:
  - Ruby
  - Security
---
## PwnedCheck

PwnedCheck is a gem that checks <a href="http://haveibeenpwned.com" target="_blank" rel="noopener">http://haveibeenpwned.com</a> to see if an email address or user handle has been involved in a breach.

### How to Install

<pre>gem install PwnedCheck</pre>

### How to Use

<pre>require 'pwnedcheck'

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
      puts "#{item} --&gt; Not found on http://haveibeenpwned.com"
    else
      sites.each do |site|
        #site is a hash of data returned
        puts item
        puts "\tTitle=#{site['Title']}"
        puts "\tBreach Date=#{site['BreachDate']}"
        puts "\tDescription=#{site['Description']}"
      end
    end
  rescue PwnedCheck::InvalidEmail =&gt; e
    puts "#{item} --&gt; #{e.message}"
  end
end
</pre>

<pre>require 'pwnedcheck'

# The 4 cases to check for pastes.
# foo@bar.com is a valid address on the site
# foo232323ce23ewd@bar.com is a valid address, but not on the site
# foo.bar.com is an invalid format
# mralexgray is a user id in snapchat
list = ['foo@bar.com', 'foo232323ce23ewd@bar.com', 'foo.bar.com', 'mralexgray']

list.each do |item|
  begin
    sites = PwnedCheck::check_pastes(item)
    if sites.length == 0
      puts "#{item} --&gt; Not found on http://haveibeenpwned.com"
    else
      sites.each do |site|
        #site is a hash of data returned
        puts item
        puts "\tSource=#{site['Source']}"
        puts "\tTitle=#{site['Title']}"
        puts "\tDate=#{site['Date']}"
        puts "\tEmail Count=#{site['EmailCount']}"
      end
    end
  rescue PwnedCheck::InvalidEmail =&gt; e
    puts "#{item} --&gt; #{e.message}"
  end
end
</pre>

## Jekyll-Clicky

Jekyll-clicky is a gem to add clicky analytics to a site generated with <a href="https://jekyllrb.com/" rel="noopener" target="_blank">Jekyll</a>.

### Installation

Add this line to your application&#8217;s Gemfile:

And then execute:

<pre>$ bundle</pre>

Or install it yourself as:

<pre>$ gem install jekyll-clicky</pre>

### Usage Add-

<pre>jekyll_clicky:              #Add this if you want to track with Clicky analytics
  site:
    id: ###          # Required - replace with your tracking id
</pre>

to _config.yml in your jekyll site directory. Replace ### with the id of your clicky site.
