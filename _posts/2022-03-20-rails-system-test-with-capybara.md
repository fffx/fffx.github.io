---
layout: post
title: rails-system-test-with-capybara
---

### Config capybara download  path

```
require 'capybara/rails'
class ApplicationSystemTestCase < ActionDispatch::SystemTestCase
  driven_by :selenium,
            using: ENV['HEADLESS'] == 'false' ? :chrome : :headless_chrome,
            screen_size: [1400, 1400] do |driver_option|

    FileUtils.mkdir_p "#{Capybara.save_path}/downloads"
    driver_option.add_preference 'download.default_directory', "#{Capybara.save_path}/downloads"
    driver_option.add_preference 'download', {default_directory: "#{Capybara.save_path}/downloads"}
    driver_option.add_preference 'download.prompt_for_download', false
  end
```


Change wait time temporarily
```
using_wait_time 3 do
  # ...
end
```
Wanna known how capybara wait works? Check [Capybara::Node::Base#synchronize](https://www.rubydoc.info/gems/capybara/Capybara%2FNode%2FBase:synchronize)


### [Selectors](https://www.rubydoc.info/gems/capybara/Capybara/Selector) special cases
- select option
```
select "option_name_here", :from => "organizationSelectId"
// or
find('#tran_action').find(:xpath, 'option[1]').select_option
```

- find element without raising exception if it's not exists.
Find the first element on the page matching the given selector and options. By default #first will wait up to default_max_wait_time seconds for matching elements to appear and then raise an error if no matching element is found, or nil if the provided count options allow for empty results
```
modal = first ".modal", between: 0..1
click_on "Close" if modal&.visible?
```
- skip(ignore) within scope, this is useful when you test inside a `within`, but you still ned to check if a global element(like datepicker) exists.
```
 assert_selector page.document, '.datepicker-dropdown'
```



### [Scroll](https://www.rubydoc.info/gems/capybara/Capybara%2FNode%2FElement:scroll_to)

```
# scroll to el
el = find("#el_id")
scroll_to el

# scroll by distance 
scroll_to x, y
```


### Debug

use `binding.pry` instead of `byebug` , as `byebug` cause thread blocking https://github.com/deivid-rodriguez/byebug/issues/193



### Run system test in CI

run system test in gitlab / github CI require  selenium/standalone-chrome
[check this gist](https://gist.github.com/julianrubisch/7a96e4778302c1cb9911b6f9db2cb75f)



references:
* https://thoughtbot.com/blog/write-reliable-asynchronous-integration-tests-with-capybara