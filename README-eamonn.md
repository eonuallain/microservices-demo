# microservices-demo

# Run docker-compose (with error)

In order to fix the error below I had to remove the reference to json 1.8.3

```
Attaching to microservices-demo-jekyll-1
microservices-demo-jekyll-1  | /usr/local/lib/ruby/site_ruby/2.3.0/bundler/runtime.rb:319:in `check_for_activated_spec!': You have already activated json 1.8.3.1, but your Gemfile requires json 1.8.3. Since json is a default gem, you can either remove your dependency on it or try updating to a newer version of bundler that supports json as a default gem. (Gem::LoadError)
microservices-demo-jekyll-1  |  from /usr/local/lib/ruby/site_ruby/2.3.0/bundler/runtime.rb:31:in `block in setup'
microservices-demo-jekyll-1  |  from /usr/local/lib/ruby/2.3.0/forwardable.rb:202:in `each'
microservices-demo-jekyll-1  |  from /usr/local/lib/ruby/2.3.0/forwardable.rb:202:in `each'
microservices-demo-jekyll-1  |  from /usr/local/lib/ruby/site_ruby/2.3.0/bundler/runtime.rb:26:in `map'
microservices-demo-jekyll-1  |  from /usr/local/lib/ruby/site_ruby/2.3.0/bundler/runtime.rb:26:in `setup'
microservices-demo-jekyll-1  |  from /usr/local/lib/ruby/site_ruby/2.3.0/bundler.rb:107:in `setup'
microservices-demo-jekyll-1  |  from /usr/local/bundle/gems/jekyll-3.3.1/lib/jekyll/plugin_manager.rb:36:in `require_from_bundler'
microservices-demo-jekyll-1  |  from /usr/local/bundle/gems/jekyll-3.3.1/exe/jekyll:9:in `<top (required)>'
microservices-demo-jekyll-1  |  from /usr/local/bundle/bin/jekyll:23:in `load'
microservices-demo-jekyll-1  |  from /usr/local/bundle/bin/jekyll:23:in `<main>'
microservices-demo-jekyll-1 exited with code 1
```

I then create a docker-compose file which uses the image I created, and ran as follows:
```
sudo docker-compose -f docker-compose-eamonn.yml up --build
```

# Build and Push image to docker
```
sudo docker build . -t eonuallain/microservices-demo
sudo docker login
sudo docker push eonuallain/microservices-demo
```

The image can now be pulled (from docker) and run with
```
sudo docker run -v $(pwd):/srv/jekyll -p 4000:4000 eonuallain/microservices:latest
```

Note that the above will not work correctly if srv/jekyll is not mounted correctly from the current working directly.


# microk8s install (Ubuntu)

```
sudo snap install microk8s --classic
```

microk8s failed to start for me initially. I ran 'microk8s inspect' and was informed that the following needed to be executed:
```
File "/etc/docker/daemon.json" does not exist.
You should create it and add the following lines:
{
    "insecure-registries" : ["localhost:32000"]
}
and then restart docker with: sudo systemctl restart docker
```
