# leet haxorz

Take a simple dockerfile:

```dockerfile
FROM alpine

ARG MY_SECRET=mysupersecretpassword

ARG MY_BUILD_TIME_SECRET

RUN echo "${MY_SECRET}"
```

Then build the image
```sh
docker build -t expose \
             --build-arg MY_BUILD_TIME_SECRET=weReallyShouldntExposeTh1s \
             --progress=plain .
```

Trying to run `docker history expose`
```
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
4006d8fcf946   2 minutes ago   RUN |2 MY_SECRET=mysupersecretpassword MY_BU…   0B        buildkit.dockerfile.v0
<missing>      2 minutes ago   ARG MY_BUILD_TIME_SECRET                        0B        buildkit.dockerfile.v0
<missing>      2 minutes ago   ARG MY_SECRET=mysupersecretpassword             0B        buildkit.dockerfile.v0
<missing>      3 months ago    /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>      3 months ago    /bin/sh -c #(nop) ADD file:9233f6f2237d79659…   5.59MB
```

It doesn't provide the image ids for the previous layers ..
