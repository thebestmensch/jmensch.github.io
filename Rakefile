# encoding: UTF-8
require "rubygems"
require "tmpdir"
require "bundler/setup"
require "jekyll"

# Change your GitHub reponame
GITHUB_REPONAME    = "jmensch/jmensch.github.io"
GITHUB_REPO_BRANCH = "master"

SOURCE = "."
DEST   = "_site"
CONFIG = {
  'layouts' => File.join(SOURCE, "_layouts"),
  'posts' => File.join(SOURCE, "_posts"),
  'post_ext' => "md",
  'categories' => File.join(SOURCE, "categories"),
  'tags' => File.join(SOURCE, "tags")
}

task default: %w[publish]

desc "Update submodules"
task :updateSubmodules do
  system "git submodule foreach git pull -q origin master"
end

# desc "Generate blog files"
# task :generate do
#   Jekyll::Site.new(Jekyll.configuration({
#     "source"      => ".",
#     "destination" => "_site",
#     "config"      => "_config.yml"
#   })).process
# end

desc "Generate and publish blog to gh-pages"
task :publish => [:updateSubmodules] do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp

    pwd = Dir.pwd
    Dir.chdir tmp

    system "git init"
    system "git checkout --orphan #{GITHUB_REPO_BRANCH}"
    system "git add ."
    message = "Site updated at #{Time.now.utc}"
    system "git commit -am #{message.inspect}"
    system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
    system "git push origin #{GITHUB_REPO_BRANCH} --force"

    Dir.chdir pwd
  end
end

