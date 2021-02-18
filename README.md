# docker-upx
Lightweight UPX docker image

## Usage 

```
docker run --rm -w $PWD -v $PWD:$PWD quay.io/kkh913/upx:latest --best --lzma -o app.out ./app
```

## Why do we need to make the container image size smaller?
Reducing the size of the container image allows agile service deployment and spatial-efficient image registry management. To do this, here are two things:

- Use of lightweight base image
  
  It is common to distribute binary files inside a container as static-linked to remove dependencies. In our case, using a [scratch] image is more advantageous than the well-known default images busybox and alpine.
  
- Reduced executable file size

  [UPX] is the most well-known executable file packer. Since the compression ratio is usually significant between 50% ~ 70%.

## Compare container size with other docker-upx projects
```
➜ docker images
REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
quay.io/kkh913/upx   latest    6b16e64aceaa   17 minutes ago   428kB
gruebel/upx          latest    3c533895a64b   3 years ago      1.7MB
lalyos/upx           latest    6fdc9a8475ac   5 years ago      1.15MB
```
## Comments
As a result, there is only a difference in container size - negligible(🤔) . And, each docker-upx has the same compression ratio and performance. But, I think that many drops make a shower in our container clusters.

### [lalyos/upx]
- Single stage Build
  - ADD his own static-linked upx released on GitHub 
  - Packing
- Fixed version 3.91
### [gruebel/upx]
- Multi stage build
  - 1st stage
    - build his own static-linked upx
  - 2nd stage
    - copy upx to busybox
- Dynamic version -> dev = latest but unstable
### [quay.io/kkh913/upx]
- Multi stage build
  - 1st stage
    - Find official latest release upx using [GitHub API] and download it
  - 2nd stage
    - Extract *.tar.xz
  - 3rd stage
    - Copy upx to scratch
- No packing process
  - Official release is already packed
- Dynamic version -> latest and stable


## License 

GPL

[scratch]: https://hub.docker.com/_/scratch/
[UPX]: https://github.com/upx/upx
[lalyos/upx]: https://github.com/lalyos/docker-upx
[gruebel/upx]: https://github.com/gruebel/docker-upx
[GitHub API]: https://docs.github.com/en/rest
[quay.io/kkh913/upx]: https://quay.io/repository/kkh913/upx
