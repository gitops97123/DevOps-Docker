# Reusing Existing Dockerfiles

One method of creating container images has been covered so far: create a container, modify
it to meet the requirements of the application to run in it, and then commit the changes to an
image. This option, although straightforward, is only suitable for using or testing very specific
changes. It does not follow best software practices, like maintainability, automation of building, and
repeatability.

Dockerfiles are another option for creating container images, addressing these limitations.
Dockerfiles are easy to share, version control, reuse, and extend.

Dockerfiles also make it easy to extend one image, called a child image, from another image, called
a parent image. A child image incorporates everything in the parent image and all changes and
additions made to create it.

To share and reuse images, many popular applications, languages, and frameworks are already
available in public image registries such as Quay.io. It is not trivial to customize an application
configuration to follow recommended practices for containers, and so starting from a proven
parent image usually saves a lot of work.

Using a high-quality parent image enhances maintainability, especially if the parent image is kept
updated by its author to account for bug fixes and security issues.

Typical scenarios in creating a Dockerfile for building a child image from an existing container
image include:

• Add new runtime libraries, such as database connectors.
• Include organization-wide customization such as SSL certificates and authentication providers.
• Add internal libraries to be shared as a single image layer by multiple container images for
different applications.

Changing an existing Dockerfile to create a new image can also be a sensible approach in other
scenarios. For example:

• Trim the container image by removing unused material (such as man pages, or documentation
found in /usr/share/doc).
• Lock either the parent image or some included software package to a specific release to lower
risk related to future software updates.

# Building Custom Container Images with Dockerfiles

## Building Base Containers

A Dockerfile is a mechanism to automate the building of container images. Building an image
from a Dockerfile is a three-step process:

1. Create a working directory
2. Write the Dockerfile
3. Build the image with Podman

## Create a Working Directory

The working directory is the directory containing all files needed to build the image. Creating an
empty working directory is good practice to avoid incorporating unnecessary files into the image.
For security reasons, the root directory, /, should never be used as a working directory for image
builds.

## Write the Dockerfile Specification

A **Dockerfile** is a text file that must exist in the working directory. This file contains the
instructions needed to build the image. The basic syntax of a **Dockerfile** follows:

    # Comment
    INSTRUCTION arguments

Lines that begin with a hash, or pound, symbol (#) are comments. INSTRUCTION states for any
Dockerfile instruction keyword. Instructions are not case-sensitive, but the convention is to make
instructions all uppercase to improve visibility.

The first non-comment instruction must be a **FROM** instruction to specify the base image.
Dockerfile instructions are executed into a new container using this image and then committed to
a new image. The next instruction (if any) executes into that new image. The execution order of
instructions is the order of their appearance in the Dockerfile.

The following is an example Dockerfile for building a simple Apache web server container.

    # This is a comment line 
    FROM ubi7/ubi:7.7 
    LABEL description="This is a custom httpd container image" 
    MAINTAINER John Doe <jdoe@xyz.com> 
    RUN yum install -y httpd 
    EXPOSE 80 
    ENV LogLevel "info" 
    ADD http://someserver.com/filename.pdf /var/www/html 
    COPY ./src/ /var/www/html/ 
    USER apache 
    ENTRYPOINT ["/usr/sbin/httpd"] 
    CMD ["-D", "FOREGROUND"]

1. Lines that begin with a hash, or pound, sign (#) are comments.

2. The **FROM** instruction declares that the new container image extends **ubi7/ubi:7.7**
container base image. Dockerfiles can use any other container image as a base image, not
only images from operating system distributions. Red Hat provides a set of container images
that are certified and tested and highly recommends using these container images as a base.

3. The **LABEL** is responsible for adding generic metadata to an image. A **LABEL** is a simple keyvalue pair.

4. **MAINTAINER** indicates the **Author** field of the generated container image's metadata. You
can use the **docker inspect** command to view image metadata.

5. **RUN** executes commands in a new layer on top of the current image. The shell that is used to
execute commands is /bin/sh.

6. **EXPOSE** indicates that the container listens on the specified network port at runtime. The
**EXPOSE** instruction defines metadata only; it does not make ports accessible from the host.
The -p option in the **docker run** command exposes container ports from the host.

7. **ENV** is responsible for defining environment variables that are available in the container. You
can declare multiple **ENV** instructions within the **Dockerfile.** You can use the **env** command
inside the container to view each of the environment variables.

8. **ADD** instruction copies files or folders from a local or remote source and adds them to the
container's file system. If used to copy local files, those must be in the working directory. **ADD**
instruction unpacks local **.tar** files to the destination image directory.

9. **COPY** copies files from the working directory and adds them to the container's file system. It
is not possible to copy a remote file using its URL with this Dockerfile instruction.

10. **USER** specifies the username or the UID to use when running the container image for the
 **RUN,** **CMD,** and **ENTRYPOINT** instructions. It is a good practice to define a different user other
than **root** for security reasons.

11. **ENTRYPOINT** specifies the default command to execute when the image runs in a container.
If omitted, the default **ENTRYPOINT** is **/bin/sh -c.**

12. **CMD** provides the default arguments for the **ENTRYPOINT** instruction. If the default
**ENTRYPOINT** applies **(/bin/sh -c),** then **CMD** forms an executable command and
parameters that run at container start.

## CMD and ENTRYPOINT

**ENTRYPOINT** and **CMD** instructions have two formats:

- Exec form (using a JSON array):

    ENTRYPOINT ["command", "param1", "param2"]
    CMD ["param1","param2"] 

- Shell form:

    ENTRYPOINT command param1 param2
    CMD param1 param2

## ADD and COPY

The ADD and COPY instructions have two forms:

- The Shell form
    ADD <source>... <destination>
    COPY <source>... <destination> 

- The Exec form:
    ADD ["<source>",... "<destination>"]
    COPY ["<source>",... "<destination>"] 

If the source is a file system path, it must be inside the working directory.
The **ADD** instruction also allows you to specify a resource using a URL:

    ADD http://someserver.com/filename.pdf /var/www/html

If the source is a compressed file, then the **ADD** instruction decompresses the file to the
destination folder. The **COPY** instruction does not have this functionality.

