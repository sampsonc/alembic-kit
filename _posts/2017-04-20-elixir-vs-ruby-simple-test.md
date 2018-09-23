---
title: 'Elixir vs Ruby - A Simple Test'
date: 2017-04-20T20:48:07+00:00
author: chs
layout: post
permalink: /elixir-vs-ruby-simple-test/
categories:
  - Ruby
---
A coworker of mine is really excited about Elixir and it&#8217;s performance.   So, I created a little program in both Ruby and Elixir to return an array containing the squares of numbers from 1 to 1000.

This is the Ruby version.   It uses a gem called [Parallel](https://github.com/grosser/parallel) that I found quickly from searching. It uses all available processors and uses threads.

<pre>require 'parallel'
Parallel.map(1..1000) do |i|
  i * i
end
</pre>

Running this a few times gives results like this-

<pre>real	0m0.118s
user	0m0.090s
sys	0m0.059s
</pre>

This is the Elixir version. It uses no external libraries. It uses all available processors and uses threads.

<pre>defmodule Parallel do
  def pmap(collection, func) do
    collection
    |> Enum.map(&(Task.async(fn -> func.(&1) end)))
    |> Enum.map(&Task.await/1)
  end
end

Parallel.pmap 1..1000, &(&1 * &1)
</pre>

Running this a few times gives results like this-

<pre>real	0m0.271s
user	0m0.258s
sys	0m0.176s
</pre>

Ruby is considerably faster on this simple example.  Definitely going to experiment more&#8230;
