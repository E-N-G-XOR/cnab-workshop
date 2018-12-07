# Exporting and Importing Cloud Native Application Bundles

## Export
Consider the case where a user wants to create package that contains the bundle manifest along with the all of the necessary artifacts to execute the install/uninstall/lifecycle actions specified in the invocation images. You can use the `$ duffle export <path>` command to do just that.

Duffle `export` allows a user to create a gzipped tar archive that contains the bundle manifest (signed or unisgned) along with all of the necessary images including the invocation images and the referenced images in the bundle. See example below

### Export Example
Given a directory called `wordpress/` which contains a bundle manifest (`bundle.json` or `bundle.cnab`) file:
```console
$ duffle export wordpress/ -k
$ ls
wordpress/ wordpress-0.2.0.tgz
```

For this example, we're passing in the `-k` flag allowing us to export an unsigned/insecure bundle. The default behavior (without `-k`) of export is to only package signed bundles.

In the example, you'll find the exported artifact, a gzipped tar archive: `wordpress-0.2.0.tgz`. Unpacking that artifact results in the following directory structure:
```
wordpress-0.2.0/
   bundle.json
   artifacts/
      cnab-wordpress-0.2.0.tar
```

Duffle export gives users the ability to package up all the components of their distributed application along with the logic to manage that application leaving users with a portable artifact.

## Import

Duffle import is used to import the exported artifact above along with all of the necessary images to manage the application. It unpacks the artifact and saves all of the images in the `artifacts/` to the local Docker store.

### Import Example
```console
$ duffle import wordpress-0.2.0.tgz -k

$ ls
wordpress-0.2.0.tgz wordpress-0.2.0/

$ ls wordpress-0.2.0/
bundle.json artifacts/

$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
cnab/wordpress      latest              533cfdfba95a        5 hours ago         35MB
