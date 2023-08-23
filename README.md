# 'Eclipse Keypop' OPS   

This is the repository for the Ops settings of the '[Eclipse Keypop](https://keypop.org/)' project.

## java-builder

This is a Docker container used to build all Keypop java-based modules.

To build locally, use:
`docker build -t eclipsekeypop/java-builder:1.0 .`

### Usage
To simulate Eclipse's usage when using Jenkins containers, use this command:
`docker run -it --rm -u $((1000100000 + RANDOM % 100000)):0 eclipsekeypop/java-builder:1.0 bash`
