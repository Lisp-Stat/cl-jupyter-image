
<!-- PROJECT SHIELDS -->

[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MS-PL License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]



<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://github.com/Lisp-Stat/cl-jupyter-image">
    <img src="https://lisp-stat.dev/images/stats-image.svg" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">Common Lisp Jupyter Stack</h3>

  <p align="center">
	An OCI Jupyter Lab image for statistical analysis with Common Lisp</em>
	<br />
    <a href="https://lisp-stat.github.io/IPS9/"><strong>Example analysis »</strong></a>
    <br />
    <br />
    <a href="https://github.com/Lisp-Stat/cl-jupyter-image/issues">Report Bug</a>
    ·
    <a href="https://github.com/Lisp-Stat/cl-jupyter-image/issues">Request Feature</a>
  </p>
</p>



<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary><h2 style="display: inline-block">Table of Contents</h2></summary>
  <ol>
    <li>
      <a href="#about-the-project">About the Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#run-locally-with-docker">Run locally with docker</a></li>
        <li><a href="#run-in-a-devcontainer">Run in a devcontainer</a></li>
        <li><a href="#run-on-the-cloud">Run on the cloud</a></li>
      </ul>
    </li>
    <li><a href="#building-an-image">Building an image</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#resources">Resources</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About the Project

This repository contains the source for an OCI (devcontainer/docker) image of a Jupyter notebook for statistical analysis.

### Built With

* [Lisp-Stat](https://github.com/Lisp-Stat/lisp-stat)
* [common-lisp-jupyter](https://github.com/yitzchak/common-lisp-jupyter)
* [Jupyter Docker Stacks](https://jupyter-docker-stacks.readthedocs.io/en/latest/)

<!-- GETTING STARTED -->
## Getting Started

A prebuilt version of this image can be found at:

`ghcr.io/lisp-stat/cl-jupyter`

This is the image URL you will use in a devcontainer.json or Docker file.

To run the image you need tooling to run OCI images. These include:

* [Docker](https://github.com/docker/docker-ce) - Most popular container platform
* [Podman](https://github.com/containers/podman) - Daemonless, rootless alternative to Docker
* [containerd](https://github.com/containerd/containerd) - Industry-standard container runtime
* [Lima](https://github.com/lima-vm/lima) - Linux VMs on macOS with container support
* [Rancher Desktop](https://github.com/rancher-sandbox/rancher-desktop) - Desktop container management
* [Colima](https://github.com/abiosoft/colima) - Container runtimes on macOS with minimal setup
* [VS Code Dev Containers](https://github.com/microsoft/vscode-dev-containers) - Development environments in containers
* [GitHub Codespaces](https://github.com/features/codespaces) - Cloud-based

### Run locally with docker

This image is based on a [Jupyter Docker Stacks](https://jupyter-docker-stacks.readthedocs.io/en/latest/) and you can find full documentation on [running a jupyter docker stack](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/running.html) there. Be certain to use the right image.

```
docker run -it -p 8888:8888 ghcr.io/lisp-stat/cl-jupyter

# Entered start.sh with args: jupyter lab

# ...

#     To access the server, open this file in a browser:
#         file:///home/jovyan/.local/share/jupyter/runtime/jpserver-7-open.html
#     Or copy and paste one of these URLs:
#         http://eca4aa01751c:8888/lab?token=d4ac9278f5f5388e88097a3a8ebbe9401be206cfa0b83099
#         http://127.0.0.1:8888/lab?token=d4ac9278f5f5388e88097a3a8ebbe9401be206cfa0b83099
```

Pressing Ctrl-C twice shuts down the Server but leaves the container intact on disk for later restart or permanent deletion.  At the time of this writing (July 2025) the upstream (Jupyter Docker Stacks) images are migrating to Notebook 7 format and you may see warnings.  They can be ignored.

### Run in a devcontainer

A devcontainer.json file for working with CL-Jupyter looks like this: 

```json
{   
	"name": "IPS9",
  "image": "ghcr.io/lisp-stat/cl-jupyter",
  // Use base images default CMD.
	"overrideCommand": false,
	// Forward Jupyter port locally, mark required.
	"forwardPorts": [8888],
	"portsAttributes": {
		"8888": {
			"label": "Jupyter",
			"requireLocalPort": true,
			"onAutoForward": "ignore"
		}
	},
	// Configure tool-specific properties.
	"customizations": {
		// Configure properties specific to VS Code.
		"vscode": {
			// Set *default* container specific settings.json values on container create.
			"settings": {
				"python.defaultInterpreterPath": "/opt/conda/bin/python"
			},
			// Add the IDs of extensions you want installed when the container is created.
			"extensions": ["ms-python.python", "ms-toolsai.jupyter"]
		}
    }
}
```

To see an example in a production environment (modified from the above for some real-world constraints) see the [IPS9 worked examples repository](https://github.com/Lisp-Stat/IPS9).


### Run on the cloud
To run the image *from this respository* on Github code spaces you must also build it (otherwise there would be a circular reference, the repository can't both provide the source of the image and use the image).  The examples above used the prebuilt image at `ghcr.io/lisp-stat/cl-jupyter` and therefore load quickly.  If you want to *use* a cloud based notebook rather than build one, modify your devcontainer.json file to use the image (instead of the individual features that comprise the image) and follow the instructions below.  See the examples of [Introduction to the Practice of Statistics](https://github.com/Lisp-Stat/IPS9/) or the devcontainer example above for working examples of using (rather than building) this image for cloud use.

Building the base image will take some time, so be patient.

Click on the 'Code' button, as if you were going to clone the repo:

<img width="1281" height="1000" alt="select-code" src="https://github.com/user-attachments/assets/45adebf6-bdc7-469e-825b-c0ae0edd2b03" />

and select "Codespaces"

<img width="1281" height="1000" alt="codespaces" src="https://github.com/user-attachments/assets/e81f83d6-3c13-4b21-8f3f-e04d6328c54a" />

from the Dropdown menu, select ‘Open in Codespaces’. This will open the code in a cloud-based development environment.

<img width="1281" height="1000" alt="create-codespace" src="https://github.com/user-attachments/assets/e97cf540-979e-44a7-917b-8ddc56a22197" />

Wait for the code space to load.  This may take some time. Be sure to wait until the process is complete.  When it is finished, you will have a remote VS Code window.  If you like you can work with Jupyter notebooks from within, or you can launch Jupyter Lab to edit.  To use Jupyter lab, close the VS Code *tab* and go back to the repository page you were using above.  Now refresh the page (just to be sure) and click on the code -> codespaces button again.  This time you'll see your active codespaces and be able to select the editor by clicking on the three dots next to the codespace name:

<img width="1380" height="1000" alt="select-dots" src="https://github.com/user-attachments/assets/57d73b6d-ea03-4b5e-860e-da6483c82b86" />

From the Dropdown menu, select ‘Open in JupyterLab’ to start working an analysis in your the JupyterLab environment.

<img width="1380" height="1000" alt="open-jupyter-lab" src="https://github.com/user-attachments/assets/e62fe433-2915-4373-a8b0-b198875f2bb3" />

You will briefly see a 'setting up' screen.

<img width="1380" height="1000" alt="setting-up-codespace" src="https://github.com/user-attachments/assets/4b7f3924-121f-4fd7-bb06-9e2a46e510a7" />

And then the Jupyter Lab will open.

<img width="1380" height="1000" alt="jupyter-lab" src="https://github.com/user-attachments/assets/757ee55c-329c-426e-b230-719199aa2060" />




<!-- BUILDING EXAMPLES -->
## Building an image

To build this image you will need to use the [devcontainer cli](https://github.com/devcontainers/cli).  Note that this image uses devcontainer features, and therefore cannot be built using standard OCI tools.  See [pre-building dev container images](https://code.visualstudio.com/docs/devcontainers/containers#_prebuilding-dev-container-images) for more information.

The commands to build this image are:

```shell
cd <this directory>
devcontainer build --workspace-folder . --image-name my-cl-jupyter:latest
```

This builds a local image `my-cl-jupyter:latest` you can use. You'll want to replace the `--image-name` parameter with your own to avoid confusion.

If you want to tag and push your custom image to a registry for others to use:

```shell
docker tag my-cl-jupyter:latest ghcr.io/username/cl-jupyter:latest
docker push ghcr.io/username/cl-jupyter:latest
```

Or build and push directly to a registry:

```shell
devcontainer build --workspace-folder . --image-name ghcr.io/username/cl-jupyter:latest --push
```



<!-- ROADMAP -->
## Roadmap

See the [open issues](https://github.com/Lisp-Stat/IPS/issues) for a
list of proposed features (and known issues). We will include
additional examples in the chapter order of the book

## Resources

This system is part of the [Lisp-Stat](https://lisp-stat.dev/)
project; that should be your first stop for information. Also see the
<!-- [resources](https://lisp-stat.dev/resources) and -->
[community](https://lisp-stat.dev/community) page for more
information.

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing
place to be learn, inspire, and create. Any contributions you make are
greatly appreciated.  Please see [CONTRIBUTING](CONTRIBUTING.md) for
details on the code of conduct, and the process for submitting pull
requests.

<!-- LICENSE -->
## License

Distributed under the MS-PL License. See [LICENSE](LICENSE) for more information.



<!-- CONTACT -->
## Contact

Project Link: [https://github.com/lisp-stat/cl-jupyter-image](https://github.com/Lisp-Stat/cl-jupyter-image)



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/lisp-stat/cl-jupyter-image.svg?style=for-the-badge
[contributors-url]: https://github.com/lisp-stat/cl-jupyter-image/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/lisp-stat/cl-jupyter-image.svg?style=for-the-badge
[forks-url]: https://github.com/lisp-stat/cl-jupyter-image/network/members
[stars-shield]: https://img.shields.io/github/stars/lisp-stat/cl-jupyter-image.svg?style=for-the-badge
[stars-url]: https://github.com/lisp-stat/cl-jupyter-image/stargazers
[issues-shield]: https://img.shields.io/github/issues/lisp-stat/cl-jupyter-image.svg?style=for-the-badge
[issues-url]: https://github.com/lisp-stat/cl-jupyter-image/issues
[license-shield]: https://img.shields.io/github/license/lisp-stat/cl-jupyter-image.svg?style=for-the-badge
[license-url]: https://github.com/lisp-stat/cl-jupyter-image/blob/master/LICENSE
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://www.linkedin.com/company/symbolics/
