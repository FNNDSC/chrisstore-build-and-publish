# :no_entry: Do not use, this was not meant to work :no_entry:

https://github.com/FNNDSC/cookiecutter-chrisapp/issues/25

# ChRIS Store Build and Publish

Automatically builds your ChRIS plugin as a multi-architectural docker image, pushing it to DockerHub.
Next, the plugin is uploaded to the ChRIS store https://chrisstore.co

x86_64 and PowerPC computer architectures are desirable targets since these
are the platforms of the OpenShift clusters within the
[Mass Open Cloud](https://massopen.cloud/).

## Example

```yaml
on: [release]

jobs:
  test:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
      - name: build
        run: docker build -t "${GITHUB_REPOSITORY,,}" .
      - name: nose tests
        run: docker run -w /usr/local/src --entrypoint /bin/sh "${GITHUB_REPOSITORY,,}" -c 'pip install nose && nosetests'
  publish:
    runs-on: ubuntu-20.04
    needs: [test]
    steps:
      - uses: fnndsc/chrisstore-build-and-publish@v1
        with:
          description: "A ChRIS plugin app"
          docker_username: ${{ secrets.DOCKERHUB_USERNAME }}
          docker_password: ${{ secrets.DOCKERHUB_PASSWORD }}
          chrisstore_token: ${{ secrets.CHRIS_STORE_USER }}
          readme-filepath: ./README.rst
```
