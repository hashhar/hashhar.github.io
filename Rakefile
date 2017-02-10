require "stringex"

# This rakefile will provide rake tasks to make it easier to manage a Jekyll blog

## -- Misc Configs -- ##
# directory for blog files
posts_dir    = "_posts"
# default new post file extension when using the new_post task
new_post_ext = "md"

desc "Generate the website using Jekyll"
task :build do
  puts "## Generating site with Jekyll"
  system "bundle exec jekyll build"
end

desc "Preview the generated website in a browser and keep watching for changes"
task :preview do
  puts "## Serving the generated site to localhost:4000 and watching for changes"
  jekyllPid = Process.spawn("bundle exec jekyll serve")

  trap("INT") {
    Process.kill(9, jekyllPid) rescue Errno::ESRCH
    exit 0
  }

  Process.wait(jekyllPid)
end

desc "Begin a new blog post in /_posts."
task :new, [:title, :categories] do |t, args|
  if args.title
    title = args.title
  else
    puts("Enter a title for your post: ")
    title = STDIN.gets.chomp
  end

  if args.categories
    categories = args.categories
  else
    puts("Enter comma separated list of categories: ")
    categories = STDIN.gets.chomp
  end

  filename = "#{posts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{new_post_ext}"
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M:%S %z')}"
    post.puts "categories: #{categories}"
    post.puts "---"
  end
end
