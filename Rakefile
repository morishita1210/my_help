require "bundler/gem_tasks"
require 'yard'
require "rake/testtask"
require 'fileutils'
p base_path = File.expand_path('..', __FILE__)
p basename = File.basename(base_path)

task :default do
  system 'rake -T'
end

desc "make documents by yard"
task :yard => [:hiki2md] do
  YARD::Rake::YardocTask.new
end

desc "transfer hikis/*.hiki to wiki"
task :hiki2md do
  files = Dir.entries('hikis')
  files.each{|file|
    name=file.split('.')
    case name[1]
    when 'hiki'
      p command="hiki2md hikis/#{name[0]}.hiki > #{basename}.wiki/#{name[0]}.md"
      system command
    when 'gif','png','pdf'
      p command="cp hikis/#{file} #{basename}.wiki/#{file}"
#      system command
      FileUtils.cp("hikis/#{file}","#{basename}.wiki/#{file}",:verbose=>true)
      FileUtils.cp("hikis/#{file}","doc/#{file}",:verbose=>true)
    end
  }
  readme_en="#{basename}.wiki/README_en.md"
  readme_ja="#{basename}.wiki/README_ja.md"
  FileUtils.cp(readme_en,"./README_en.md",:verbose=>true)
  FileUtils.cp(readme_ja,"./README.md",:verbose=>true)
  FileUtils.cp(readme_ja,"#{basename}.wiki/Home.md",:verbose=>true)
end

desc "transfer hikis/*.hiki to latex"
task :latex do
  target = 'handout_sample'
  command = "hiki2latex --pre latexes/handout_pre.tex hikis/#{target}.hiki > latexes/#{target}.tex"
  system command
  command = "open latexes/#{target}.tex"
  system command
end


desc "make own help from lib/daddygongon/files"
task :my_help do
  user_name = 'daddygongon'
  p entries=Dir.entries(File.join('.','lib',user_name))[2..-1]
  entries.each{|file|
    p file
    next if file[0]=='#' or file[-1]=='~'
    exe_cont="#!/usr/bin/env ruby\n"
    p file_name=file.split('_')
    target_files = [file, file_name[0][0]+"_"+file_name[1][0]]
    p cont_name = File.join('lib',user_name,file)
    exe_cont << "require 'specific_help'\n"
    exe_cont << "help_file = File.expand_path(\"../../#{cont_name}\", __FILE__)\n"
    exe_cont << "SpecificHelp::Command.run(help_file, ARGV)\n"
    target_files.each{|name|
      print "\n"
      p target=File.join('exe',name)
      File.open(target,'w'){|file|
        print exe_cont
        file.print exe_cont
      }
      FileUtils.chmod('a+x', target, :verbose => true)
    }
  }
end

desc "make hiki documents"
task :mk_hiki do
  p entries=Dir.entries(File.join('.','hikis'))[2..-1]
  hiki_dir='/Users/bob/Sites/nishitani0/Internal/data/text'
  entries.each{|file|
    next if file[0]=='#' or file[-1]=='~'
    p src=File.join('.','hikis',file)
    p file_name='MyHelp_'+File.basename(file,'.hiki')
    p des=File.join(hiki_dir,file_name)
    FileUtils::Verbose.cp(src,des)
    FileUtils::Verbose.chmod('a+w',des)
  }
end
