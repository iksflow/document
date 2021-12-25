# Rails Application Autoload

```ruby
# application.rb

require "rails"
require "rails/all"

module MyModule
  class Application < Rails::Application
    # config.autoload_paths 의 경로에 있는 파일들 autoload
    config.autoload_paths += %W(#{config.root}/lib #{config.root}/lib/modules)
  end
end

```

config 값 확인하기
1. rails console 실행
2. Rails.application.config 