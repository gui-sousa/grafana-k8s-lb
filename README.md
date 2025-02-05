# grafana-k8s-lb



<a name="readme-top"></a>

<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project

A simplified way to provide a monitoring structure with Grafana within a k8s cluster (preferably local) with 2 replicas running on different Nodes. 
Using nginx as a load balancer, distributing load among the pods through the NodePort of the cluster

<p align="right">(<a href="#readme-top">back to top</a>)</p>



### Built With

* [![Nginx][Nginx]][Nginx-url]
* [![K3S][K3S]][K3S-url]
* [![Docker][Docker]][Docker-url]

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- GETTING STARTED -->
### Prerequisites

* Having a local (or cloud provider) Kubernetes infrastructure with at least two worker nodes;
* Set your IP's;
* Make sure you have kubernetes and load balancer ports relesead in your network;
* Make sure you have docker installed.

### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/gui-sousa/grafana-k8s-lb.git
   ```
2. Build docker image
   ```sh
   docker run -d -p 80:80 --name lb-grafana gsousa/grafana-bwg:latest
   ```
3. Deploy k8s statements
   ```sh
    kubectl apply -f pv.yaml && \
    kubectl apply -f pvc.yaml && \
    kubectl apply -f service.yaml && \
    kubectl apply -f deployment.yaml
   ```
4. Access you grafana initial setup page in _http://localhost_

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- USAGE EXAMPLES -->
## Usage

In _pv.yaml_ change the nfs server ip adress for your environment
```yaml
    nfs:
    server: <YOUR STORAGE IP>
    path: "/mnt/apps/grafana-vol"
```

The same in upstream block in nginx.conf
```conf
      upstream grafana {
        server <K3S-NODE-1>:32009 weight=2 max_fails=3 fail_timeout=10;
        server <K3S-NODE-2>:32009 backup;
    }
```

This file, there is a load balancing configuration between the two Nodes running Grafana. All traffic is prioritized to *NODE-1* while *NODE-2* remains as a backup. 
In this scenario, if Grafana on *NODE-1* experiences 2 connection failures within 10 seconds, all traffic is redirected to the Grafana hosted on *NODE-2*.


<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- CONTACT -->
## Contact

Gui Sousa - https://www.linkedin.com/in/guilherme-sousa-rodrigues/

Project Link: [https://github.com/github_username/repo_name](https://github.com/gui-sousa/Adfs-Packer-Terraform)

<p align="right">(<a href="#readme-top">back to top</a>)</p>


<!-- ACKNOWLEDGMENTS -->



<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/github_username/repo_name.svg?style=for-the-badge
[contributors-url]: https://github.com/github_username/repo_name/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/github_username/repo_name.svg?style=for-the-badge
[forks-url]: https://github.com/github_username/repo_name/network/members
[stars-shield]: https://img.shields.io/github/stars/github_username/repo_name.svg?style=for-the-badge
[stars-url]: https://github.com/github_username/repo_name/stargazers
[issues-shield]: https://img.shields.io/github/issues/github_username/repo_name.svg?style=for-the-badge
[issues-url]: https://github.com/github_username/repo_name/issues
[license-shield]: https://img.shields.io/github/license/github_username/repo_name.svg?style=for-the-badge
[license-url]: https://github.com/github_username/repo_name/blob/master/LICENSE.txt
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/linkedin_username
[product-screenshot]: images/screenshot.png
[Next.js]: https://img.shields.io/badge/next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white
[Next-url]: https://nextjs.org/
[React.js]: https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB
[Terraform]: https://img.shields.io/badge/Terraform-20232A?style=for-the-badge&logo=terraform&logoColor=7B42BC
[Packer]: https://img.shields.io/badge/packer-20232A?style=for-the-badge&logo=packer&logoColor=02A8EF
[Ansible]: https://img.shields.io/badge/Ansible-20232A?style=for-the-badge&logo=ansible&logoColor=EE0000
[Nginx]: https://img.shields.io/badge/NGNIX-20232A?style=for-the-badge&logo=nginx&logoColor=%23009639
[Powershell]: https://img.shields.io/badge/Powershell-20232A?style=for-the-badge&logo=powershell&logoColor=5391FE
[K3S]: https://img.shields.io/badge/K3s-20232A?style=for-the-badge&logo=k3s&logoColor=%23FFC61C
[Docker]: https://img.shields.io/badge/DOCKER-20232A?style=for-the-badge&logo=docker&logoColor=%232496ED
[React-url]: https://reactjs.org/
[Vue.js]: https://img.shields.io/badge/Vue.js-35495E?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D
[Vue-url]: https://vuejs.org/
[Angular.io]: https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white
[Angular-url]: https://angular.io/
[Svelte.dev]: https://img.shields.io/badge/Svelte-4A4A55?style=for-the-badge&logo=svelte&logoColor=FF3E00
[Svelte-url]: https://svelte.dev/
[Laravel.com]: https://img.shields.io/badge/Laravel-FF2D20?style=for-the-badge&logo=laravel&logoColor=white
[Laravel-url]: https://laravel.com
[Bootstrap.com]: https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white
[Bootstrap-url]: https://getbootstrap.com
[JQuery.com]: https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white
[JQuery-url]: https://jquery.com 
[Nginx-url]: https://nginx.org/en/docs/
[K3S-url]: https://docs.k3s.io/
[Docker-url]: https://docs.docker.com/
[Linkedin-url]: https://www.linkedin.com/in/guilherme-sousa-rodrigues/

