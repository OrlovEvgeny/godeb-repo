
# godeb-repo (deb pkg repository)

A lightweight apt repository server. 


# General Usage:

This project is now using the native Go vendoring feature so you will need to build with Go >1.7

Once it is running POST a file to the `/upload` endpoint:

`curl -XPOST 'http://localhost:9090/upload?arch=amd64&distro=stable&section=main' -F "file=@myapp.deb"`

Or delete an existing file:

`curl -XDELETE 'http://localhost:9090/delete' -d '{"filename":"myapp.deb","distroName":"stable","arch":"amd64", "section":"main"}'`

To use your new repo you will have to add a line like this to your sources.list file:

`deb http://my-hostname:listenPort/ stable main`

`my-hostname` should be the actual hostname/IP where you are running deb-simple and `listenPort` will be whatever you set in the config. By default deb-simple puts everything into the `stable` distro and `main` section but these can be changed in the config. If you have enabled SSL you will want to swap `http` for `https`.


# Using API keys:

deb-simple supports the idea of an API key to limit who can upload and delete packages. To use the API keys feature you first need to enable it in the config file by setting `enableAPIKeys` to `true`. Once that is done you'll need to generate at least one API key. To do that just run `godeb-repo -g` and an API key will be printed to stdout. 

Now that you have a key you'll need to include it in your `POST` and `DELETE` requests by simply adding on the `key` URL parameter. An example for an upload might look like:

`curl -XPOST 'http://localhost:9090/upload?arch=amd64&distro=stable&section=main&key=MY_BIG_API_KEY' -F "file=@myapp.deb"`

A delete would look like:

`curl -XDELETE 'http://localhost:9090/delete?key=MY_BIG_API_KEY' -d '{"filename":"myapp.deb","distroName":"stable","arch":"amd64", "section":"main"}'`
# License:

[MIT](LICENSE.txt) so go crazy
