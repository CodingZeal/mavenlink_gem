guard :rspec, cmd: 'bundle exec rspec' do
  watch(%r{^spec/.+_spec\.rb$})
  watch(%r{^lib/(.+)\.rb$})     { |m| "spec/lib/#{m[1]}_spec.rb" }
  watch(%r{^lib/config/(.+)\.yml$}) { "spec" }
  watch('lib/mavenlink/errors.rb') { "spec" }
  watch('spec/spec_helper.rb')  { "spec" }
  watch('spec/support/shared_examples.rb')  { "spec" }
end
