
desc "build the vagrant base boxes for all definitions"
task :build do
	
	versions = { 
		:ruby => "1.9.2-p290", 
		:gems => "1.8.16", 
		:chef => "0.10.10"
	}
	
	definitions = Dir.entries('definitions').reject{ |e| e.eql? '.' or e.eql? '..' }
	definitions.each do |definition|
		['bare-os', 'vagrant'].each do |target|
			puts "building '#{definition}-#{target}.box' with #{versions.collect{ |k,v| "#{k}=#{v}"}.join(', ') }"
			template_postinstall(definition, target, versions)
			build_box(definition, target, versions)
		end
	end
end

def template_postinstall(definition, target, versions)
	require 'erb'
	postinstall_file = "./definitions/#{definition}/postinstall.sh"
	custom_install_commands = ERB.new(File.read("./common/custom-install-commands-#{target}.sh.erb")).result(binding)
	File.open("#{postinstall_file}", 'wb') do |outfile|
		outfile.write ERB.new(File.read("#{postinstall_file}.erb")).result(binding)
	end
end

def build_box(name, target, versions)
	require 'bundler/setup'
	require 'fileutils'
	FileUtils.mkdir_p "boxes"
	system "veewee vbox build #{name}"
	system "veewee vbox validate #{name}" unless target.eql? 'bare-os'
	system "vagrant basebox export #{name}"
	FileUtils.mv "#{name}.box", "boxes/#{name}-#{target}.box"
	system "veewee vbox destroy #{name}"
end
