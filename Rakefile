require 'date'

task :default => :deploy

task :build do
  pids = [
  	spawn("scss --watch _source/assets:_source/stylesheets"),
  	spawn("jekyll build -s ./_source ./website"),
    # spawn("coffee -b -w -o javascripts -c _source/_assets/*.coffee")
  ]

  trap "INT" do
    Process.kill "INT", *pids
    exit 1
  end

  loop do
    sleep 1
  end
end

desc "clean"
task :clean do
	rm_rf 'website/*'
end

desc "add changes to git repo"
task :add do
	sh "git add ."
	message = ENV['MESSAGE'] || begin
		puts "commit message: "
		STDIN.gets.strip
	end
	sh "git commit -m '#{message}'"
end

desc "push to git repo"
task :push do
	# ensure_committed
	sh "git push origin master"
end

desc "build site, upload to NearlyFreeSpeech, then deploy to github"
task :deploy => [ :add, :build, :upload, :sitemap, :ping, :push ] do
	sh "working..."
end


def ensure_committed
	system "git diff --quiet HEAD"
	raise "uncommitted changes detected. commit changes first!" unless ($?.success? || ENV['FORCE'])
end

desc 'Generate and publish the entire site, and send out pings'
task :publish => [:build, :push] do
end

