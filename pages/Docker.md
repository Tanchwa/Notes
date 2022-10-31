-
-
-
- #+BEGIN_TIP
  The thing after the colon is the thing inside the container. For example, in [[Docker Networking]], if you bind port 8080 to port 80, denoted with 8080:targetPort80, the port on the host is 8080 and the port on the container is 80. Similarly, with [[Docker Storage]], it looks like this /path/on/host:/target/path/on/container
  #+END_TIP y
-
- Building Images
  id:: 62efc6e3-e692-4753-865e-6f656c934a57
	- docker images are defined with a [[dockerfile]]
	- run the command: `docker build dockerfile -t docker_user/name-of-app`
	- here, we can specify runtime commands, base image, file directories we need, etc.
	- docker builds images in a layered architecture, i.e., step by step
	- each instruction in the docker file creates a new layer, and includes just the changes from the previous layer
	- docker will reuse similar layers from its cache for like-apps
		- ex: two apps run on the same base image and use the same commands, but have different source code and entry points
	- Image Layers:
		- 5 Entrypoint
		  4 Source Code
		  3 Changes in pip packages
		  2 Changes in apt packages
		  1 Base Image
	- once built, these are read only and can only be modified by rebuilding the image
	- when you run the container, the container becomes the 6th layer
		- this is a writable layer on top of the rest of the image layers
		- the life of this layer runs only as long as the container
		- you CAN "modify" the contents of the files in the lower layers, but this is done by docker first creating a copy of the file that can be edited. You aren't actually modifying the actual file. This is called "copy on write "
		-
-
- Setting Security Standards in a Container
  id:: 62efc6c3-b65e-4c5d-a040-c062a77d2684
	- can add the user id with `--user=1000`
	- can set [[Linux Capabilities]] with --cap-add CAP_NAME ubuntu