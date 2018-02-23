This is the source for the cardstack/chrome docker image. We use it in CI testing. 

    docker build -t cardstack/chrome .
    docker push cardstack/chrome

If you already have your application in a docker image, and you want to layer headless browsing testing capability on top, a fast way to do it is to make a new Dockerfile with:

```Dockerfile
FROM cardstack/chrome as chrome

FROM your-application-image

COPY --from=chrome /opt/google /opt/google
COPY --from=chrome /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu
COPY --from=chrome /lib/x86_64-linux-gnu /lib/x86_64-linux-gnu
RUN ln -s /opt/google/chrome/google-chrome /usr/bin/google-chrome

CMD your-tests-here

```

This copies all the bits you need out of this image and into a new test-oriented image that extends your application's image.


