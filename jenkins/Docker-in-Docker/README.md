# Jenkins - Docker in Docker

## Problems with this approach

* Security profile of inner Docker will conflict with one of outer Docker
* Incompatible file systems (e.g. AUFS inside Docker container)