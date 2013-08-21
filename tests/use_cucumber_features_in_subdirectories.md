# Use Cucumber features in subdirectories
You can orginaize cucumber features in their own subdirectories. However when you run cucumber, by default it only loads the *.rb files whitin the directory passed as argument.

```bash
$ cucumber # default directory "features"
$ cucumber features # so will this
```

This shouldn't be a problem if your run the whole testsuite. In that cause cucumber will automatically load all the subdirectories as well.

But when you run a cucumber feature in a subdirectory it will only look in that subdirectory.

## Solution 
To solve this you can pass an extra argument to cucumber.

```bash
$ cucumber -r features
$ cucumber -r features features/all_sub_dir # so will do this
```

This is a bit tedious so you can also pass this option in the cucumber config file (end of the `std_opts`). 

./config/cucumber.yml
```ruby
std_opts = "--format #{ENV['CUCUMBER_FORMAT'] || 'pretty'} --strict --tags ~@wip -r features"
```