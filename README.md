# Permanent-Recall-Web-Suite
A web application for memorizing text passages.
Several years ago I got into memorization and I found a program designed to help you memorize a passage every day. It claimed that if you followed the program for two years then you would develop a photographic memory. I didn't put much stock in that claim but that it would be a good challenge regardless.
The problem with memorizing so many passages is that you need to review the passages multiple times so you don't forget them. However when you add a new passage every day they really start to pile up.  I was using index cards to store my memorized passages and I had a filing system for reviewing. But it was tedious and not mobile. So I decided to make it into a web app.

### Review Schedule
After a passages is memorized it will be reviewed:
<ul>
    <li>Once a day for a week</li>
    <li>Once a week for a month</li>
    <li>Once a month for a year</li>
</ul>

## Development

This development environment uses Docker Compose to set up 4 servers on your development machines. These servers Are in the form of Docker containers. You can think of them as virtual machines. They are self contained Linux installations with their own IP addresses within their own private network. There are containers (or servers) defined in the `docker-compose.yml`:

- **nginx:** A webserver used to separate frontend traffic and backend traffic on the same virtual host. This runs on port 80, which by default is mapped to port 8080 on your development machine (the host). Any traffic it receives at `/api` will be routed to the **backend** server, and all other traffic will be routed to the **frontend** server.
- **frontend:** A Nuxt development server running on port 8080. It will have the `frontend/` folder in this repository mapped to its own `/app` directory.
- **backend:** A Typescript node server running on port 3000. It will have the `backend/` folder mapped to its own `/app` directory.
- **mongo:** A mongoDB instance used by **backend** for database storage.

The `frontend/` and `backend/` folders in this repository are git submodules and must be initialized as such. Since they are mapped to filesystem locations on the Docker containers, any changes you make in these local folders will be immediately reflected on the Docker container. In the case of the **frontend**, this should result in things like live reload.

## Setup
You can clone this repository and the submodules by using:
```bash
git clone --recursive {repository url}
```
after cloning the repositories make sure you install the dependencies by using `yarn install`. To ensure that you get the correct versions of the dependencies for the docker containers that they will be running in you will want to install them using docker. Simply use the following commands:
```bash
docker-compose run --rm frontend yarn install
docker-compose run --rm backend yarn install
```
this will spin up the frontend and backend containers respectively and run the yarn install command to install the dependencies. Because it is using the `--rm` flag the containers will be removed after the command is complete that way you do not collect a bunch of unused docker containers.

With the dependencies installed you are ready to spin up your docker images. simply run:

```bash
docker-compose up
```
this will bring up all 4 docker images and they will be accessible at `localhost:8080`.
To stop the servers you can simply use `Ctr-c` or if you are in another terminal window you can use `docker-compose stop`


## Deployment
There is a CI/CD pipline in the works to deploy the code to a server hosted by AWS. This will be triggered and run automatically when you push to gitlab.
