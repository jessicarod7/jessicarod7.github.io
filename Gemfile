source "https://rubygems.org"

gem 'jekyll', '~> 4.3', '>= 4.3.3'

# github-pages 232 substitute
group :gh_pages do
  gem 'github-pages-health-check', '~> 1.18', '>= 1.18.2'
  gem 'kramdown', '~> 2.4'
  gem 'kramdown-parser-gfm', '~> 1.1'
  gem 'liquid', '~> 4.0'
  gem 'mercenary', '~> 0.4.0'
  gem 'nokogiri', '~> 1.16', '>= 1.16.7'
  gem 'rouge', '~> 4.3'
  gem 'webrick', '~> 1.8', '>= 1.8.1'
  gem 'terminal-table', '~> 3.0', '>= 3.0.2'
end

group :jekyll_plugins do
  gem 'jekyll-avatar', '~> 0.8.0'
  # gem 'jekyll-commonmark-ghpages', '~> 0.5', '!= 0.5.1' # Not available in Jekyll 4
  gem "jekyll-feed", "~> 0.17.0"
  gem 'jekyll-gist', '~> 1.5'
  gem 'jekyll-github-metadata', '~> 2.16', '>= 2.16.1'
  gem "jekyll-include-cache", "~> 0.2.1"
  gem "jekyll-paginate-v2", "~> 3.0"
  gem "jekyll-redirect-from", "~> 0.16.0"
  gem 'jekyll-relative-links', '~> 0.7.0'
  gem 'jekyll-remote-theme', '~> 0.4.3'
  gem 'jekyll-sass-converter', '~> 3.0'
  gem "jekyll-seo-tag", "~> 2.8"
  gem "jekyll-sitemap", "~> 1.4"
  gem 'jemoji', '~> 0.13.0'
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
