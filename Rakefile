task :install do
  ln_s "#{Dir.pwd}/bash_airbnb", "#{ENV["HOME"]}/.bash_airbnb", force: true
  ln_s "#{Dir.pwd}/bash_profile", "#{ENV["HOME"]}/.bash_profile", force: true
  ln_s "#{Dir.pwd}/bash_prompt", "#{ENV["HOME"]}/.bash_prompt", force: true
  ln_s "#{Dir.pwd}/bashrc", "#{ENV["HOME"]}/.bashrc", force: true
end

task default: :install
