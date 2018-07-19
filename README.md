**mac-webpagetest-server**

This Docker image inherits from webpagetest/server and adds a custom `locations.ini` file which adds a connectivity parameter to avoid traffic-shaping bugs on MacOS. The solution was taken from [this post](https://medium.com/@francis.john/local-webpagetest-using-docker-90441d7c2513).


**Instructions**
1. `docker pull amsimpson/mac-webpagetest-server:latest` (or any [tag](https://github.com/asimpson/mac-webpagetest-server/releases) instead of `latest`).
2. `docker run -d -p 4000:80 mac-webpagetest-server`
The server will now be available on `localhost:4000`.
3. To run webpagetest locally you will also need `webpagetest/agent`.
4. `docker pull webpagetest/agent`
5. `docker run -d -p --network="host" -e "SERVER_URL=http://localhost:4000/work/" -e "LOCATION=Test" -e "SHAPER=none" webpagetest/agent`.
The only note on running the agent is that on macOS you need to specify the environment variable `SHAPER=none` to avoid traffic-shaping issues. Also to surface custom development domains such as `.dev` domains that are defined in `/etc/hosts` on your Mac you'll need to pass those mappings in as well, e.g.
`--add-host customdomain.dev:localMachineIP`.
