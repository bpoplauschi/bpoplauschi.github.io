source "https://rubygems.org"

gem "jekyll"
gem "minima"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem 'jekyll-sitemap'
  gem 'jekyll-seo-tag'
end

install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :install_if => Gem.win_platform?
