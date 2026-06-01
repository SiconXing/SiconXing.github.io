# frozen_string_literal: true

source "https://rubygems.org"

gem "jekyll-theme-chirpy", "~> 7.2"

group :test do
  gem "html-proofer", "~> 5.0"
end

# Jekyll plugins
group :jekyll_plugins do
  gem "jekyll-archives"
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
end

# Windows and JRuby does not include zoneinfo files
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.2.0", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock jekyll-sass-converter for Ruby < 3
gem "jekyll-sass-converter", "~> 3.0"
