# Home Assistant

Home Assistant is running in Docker. The configuration is managed in [this repo](https://github.com/jaredjensen/home-assistant-config) and the container mounts ~/apps/home-assistant-config. Host network mode is used, so port mapping isn't required.

If editing config files is required:

```bash
# Connect to the NUC
ssh -l jared 192.168.208

# Shell into the HA container
sudo docker exec -it home_assistant bash
```
