FROM redis:6.0.4

# adding the requied sentinel.conf file in docker image
ADD sentinel.conf /etc/redis/sentinel.conf
# chown : an abbreviation of change owner, is used on Unix and Unix-like operating systems to change the owner of file system files, directories.
# with `chown` command we are changing the owner for sentinel.conf file 
RUN chown redis:redis /etc/redis/sentinel.conf

# adding the required entrypoint.sh file in docker image
ADD entrypoint.sh /usr/local/bin/
# chmod : It is the command and system call which is used to change the access permissions of file system objects (files and directories).
# with `chmod` command we are changing the access permission for entrypoint.sh file
RUN chmod +x /usr/local/bin/entrypoint.sh

# REMEMBER : 
# RUN - executes command(s) in a new layer and creates a new image. E.g., it is often used for installing software packages.
# CMD - sets default command and/or parameters, which can be overwritten from command line when docker container runs.
# ENTRYPOINT - configures a container that will run as an executable.
ENTRYPOINT ["entrypoint.sh"]