require 'safe_yaml'
require 'fileutils'

task :refresh_tags do

  FileUtils.rm_rf(Dir.glob('blog/tag/*.md'))
  yaml = File.open('_data/tags.yaml').read

  YAML.load(yaml, :safe => true).each do |tag|

    header = <<EOS
---
layout: blog_by_tag
tag: #{tag}
permalink: /blog/tag/#{tag}/
---
EOS

    File.write("blog/tag/#{tag}.md", header)
  end

end
