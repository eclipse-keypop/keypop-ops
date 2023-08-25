# 'Eclipse Keypop' OPS   

This is the repository for the Ops settings of the '[Eclipse Keypop](https://keypop.org/)' project.

## java-builder

This is a Docker container used to build all Keypop java-based modules.

To build locally, use:
`docker build -t eclipsekeypop/java-builder:1.0 .`

:warning: **Line Ending Warning**:

If you're cloning or editing files from this repository on a Windows machine, be aware of potential line ending issues
with the bash scripts included to docker image (`import_gpg`, `publish_packaging`, `uid_entrypoint`). 
Windows uses \r\n (CR+LF) as a line ending, whereas Linux uses \n (LF). 
A discrepancy in line endings can lead to unexpected behaviors, especially when executing scripts in a Linux-based 
Docker container.

### Usage
To simulate Eclipse's usage when using Jenkins containers, use this command:
`docker run -it --rm -u $((1000100000 + RANDOM % 100000)):0 eclipsekeypop/java-builder:1.0 bash`
