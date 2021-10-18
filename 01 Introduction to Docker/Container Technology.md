# Objectives

# Containerized Applications

Software applications typically depend on other libraries, configuration files, or services that
are provided by the runtime environment. The traditional runtime environment for a software
application is a physical host or virtual machine, and application dependencies are installed as part
of the host.<br>

For example, consider a Python application that requires access to a common shared library that
implements the TLS protocol. Traditionally, a system administrator installs the required package
that provides the shared library before installing the Python application.<br>

The major drawback to traditionally deployed software application is that the application's
dependencies are entangled with the runtime environment. An application may break when any
updates or patches are applied to the base operating system (OS).<br>

For example, an OS update to the TLS shared library removes TLS 1.0 as a supported protocol.
This breaks the deployed Python application because it is written to use the TLS 1.0 protocol for
network requests. This forces the system administrator to roll back the OS update to keep the
application running, preventing other applications from using the benefits of the updated package.
Therefore, a company developing traditional software applications may require a full set of tests to
guarantee that an OS update does not affect applications running on the host.<br>

Furthermore, a traditionally deployed application must be stopped before updating the associated
dependencies. To minimize application downtime, organizations design and implement complex
systems to provide high availability of their applications. Maintaining multiple applications on a
single host often becomes cumbersome, and any deployment or update has the potential to break
one of the organization's applications.<br>

Figure 1.1 describes the difference between applications running as containers and applications
running on the host operating system.


# 