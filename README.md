# Spree on Heroku

This is an extension for Spree, allowing the e-commerce system to run on Heroku - http://heroku.com.

The major constraint on Heroku is that we can't write files to disk, so this extension disables all disk caching, fixes a few issues and changes Spree to store on Amazon S3.

This is an update of http://github.com/RSpace/spree-heroku that works with Rails >= 3.1.3 and Spree >= 1.0.0

# Requirements 

A Heroku account and an Amazon S3 account with a bucket.

# Installation and configuration

Add this to your project Gemfile (before spree gem):

<pre>
gem 'aws-s3'
gem 'aws-sdk'
</pre>

And this, somewhere after spree gem
<pre>
gem 'spree_heroku', '1.0.0', :git => 'git://github.com/biow0lf/spree-heroku.git'
</pre>

Install the new gems with bundler:
<pre>
bundle install
</pre>

Specify the S3 credentials:

<pre>

Create under RAILS_ROOT/config/s3.yml

development:
  bucket: yourapp_dev
  access_key_id: AAAAAAAAAAAAAAAAAAA
  secret_access_key: AJHKHJKKJHKHkjsdf+EuXQu5xvingQXY0M+gyYnFGqUJ

test:
  bucket: yourapp_test
  access_key_id: AAAAAAAAAAAAAAAAAAA
  secret_access_key: AJHKHJKKJHKHkjsdf+EuXQu5xvingQXY0M+gyYnFGqUJ

production:
  bucket: yourapp_prod
  access_key_id: AAAAAAAAAAAAAAAAAAA
  secret_access_key: AJHKHJKKJHKHkjsdf+EuXQu5xvingQXY0M+gyYnFGqUJ

</pre>

Create a Heroku application and deploy it:

<pre>
git init
git add .
git commit -m 'Initial create'
heroku create
git push heroku master
</pre>

Enable SSL, since Spree uses SSL for administration and payment flow in its standard setup:

<pre>
heroku addons:add "Piggyback SSL"
</pre>

Bootstrap the database locally (not possible in Heroku, because the rake task attempts to copy files), and transfer it to Heroku:

<pre>
rake db:bootstrap
heroku db:push
</pre>

Please note that if you choose to load sample data, images will be missing for all products. Spree's bootstrap task copies the images locally, but it doesn't put them on S3, where this extension configures Spree to look for images.

That's it - you're done! :)

# Troubleshooting

This extension has been tested with Spree 1.0.0 and Rails 3.1.3. If you have problems using the extension with a newer version of Spree, it could be due to Spree's gem dependencies having changed.

# Copyright and license

Copyright (c) 2011 Casper Fabricius, released under the New BSD License

Contributors:

*   Pavel Chipiga
*   Andrey Voronkov
*   Amed Rodriguez