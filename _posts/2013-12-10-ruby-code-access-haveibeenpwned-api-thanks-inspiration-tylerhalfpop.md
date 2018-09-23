---
title: '#ruby code to access the @haveibeenpwned api.'
date: 2013-12-10T11:04:43+00:00
author: chs
layout: post
permalink: /ruby-code-access-haveibeenpwned-api-thanks-inspiration-tylerhalfpop/
categories:
  - Code
  - Ruby
---
This is just some ruby I whipped up really quickly to access the API of [havibeenpwned.com](http://haveibeenpwned.com "http://haveibeenpwned.com") which is a cool new site by [Troy Hunt](http://www.troyhunt.com "Troy Hunt") that aggregates password dump information from breaches and allows you to search for your email address.

I think the code is pretty self-explanatory, but comment or send me a line if you have questions/suggestions/criticism/etc!

<pre lang="ruby" line="1">require 'mechanize'
require 'addressable/uri'

agent = Mechanize.new

File.open('addresses.txt').each do |line|
  line = line.chomp
  begin
    target = "http://haveibeenpwned.com/api/breachedaccount/#{line}"
    page = agent.get Addressable::URI.parse(target)
  rescue Mechanize::ResponseCodeError  => e
    case e.response_code
      when '404'
        puts "#{line} => Not Found"
      when '400'
        puts "#{line} => Bad Request"
      else
        puts "#{line} => #{e.message}"
     end
  else
    puts "#{line} => #{page.content}"
  end
end

</pre>
