# Create a custom image 

Here you will learn how to leverage your own image (libraries, code etc) to execute batch and interactive analyses.

## Example: Add some packages on top of the default image

- First, you have to create a file named `Dockerfile`, with the series of instructions required to install all the required packages.

  :::{warning}
  The only requirement is that the image has to be derived from 
  ```bash 
  ghcr.io/icsc-spoke2-repo/jlab:wp5-alma9-highrate-v0.1.1
  ```
  :::

  As an example:

  ```dockerfile
  FROM ghcr.io/icsc-spoke2-repo/jlab:wp5-alma9-highrate-v0.1.1
  
  RUN yum install -y texlive dvipng ripgrep && dnf install -y texlive-type1cm
  ```

- To make the image visible on the high-rate platform, there are several options available:
  - Publish the image on a container registry (dockerhub, quay, ...);
  - Use Github CI/CD to publish the image directly on GitHub.

  For the second option, you have to create a public GitHub repository, containing your `Dockerfile` and an additional file under the folder `.github/workflows` named `docker-publish.yml`. 
  A minimal working examples is:
  ```yaml
  name: docker-publish

  on:
    push:
      branches: [ "main" ]
      tags:
      - "*"
  jobs:
    build-and-push-image:
      runs-on: ubuntu-latest
      steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v1
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
      - name: Login to GitHub Container Registry
        uses: docker/login-action@v1
        with:
            registry: ghcr.io
            username: ${{ github.repository_owner }}
            password: ${{ secrets.GITHUB_TOKEN }}
      - name: Get Repo Owner
        id: get_repo_owner
        run: echo ::set-output name=repo_owner::$(echo ${{ github.repository_owner }} | tr '[:upper:]' '[:lower:]')
      - name: Build container image
        uses: docker/build-push-action@v2
        with:
          outputs: "type=registry,push=true"
          tags: |
            ghcr.io/${{ steps.get_repo_owner.outputs.repo_owner }}/custom-jlab:latest
          platforms: linux/amd64
  ```

- After committing (and pushing) all these files to the repository, a workflow will be triggered (as shown with a yellow dot near your last commit in GitHub).
  Clicking on that dot will show you the execution log (in case of debugging).

  :::{warning}
  Make sure the repository has **read/write permissions** for actions. To can change that under Settings -> Actions -> Workflow permissions
  :::

- After a successful execution (green tick), your image will be visible on the right bar of the repository (under `Packages`).

- If you click on the image name, you will find a link to the image (starting with `ghcr.io/...`), that you can copy/paste in the high-rate platform home page (under `Select your desired image`).