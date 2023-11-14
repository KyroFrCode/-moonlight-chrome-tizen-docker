# Moonlight for Tizen - Dockerfile template
An easy method for building Moonlight for Samsung TV

## Credits
- Moonlight developers: https://moonlight-stream.org
- Samsung developers: https://github.com/SamsungDForum/moonlight-chrome
- This Dockerfile and support files have been adapted from [jellyfin-docker-tizen](https://github.com/babagreensheep/jellyfin-tizen-docker)
- Dockerfile readapted for my repository tizen from [pablojrl123](https://github.com/pablojrl123/moonlight-tizen-docker)

## Usage
1. Enable developer mode on the TV (more information on [official Samsung guide](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html)):
	- Go to Apps.
	- Press `12345` on the remote; a dialog should pop up.
	- Set `Developer mode` to `On`; fill in the IP of the Docker host.
	- Power off and power on the TV as instructed; go once again to Apps.
	- Depending on your model, a "DEVELOP MODE" or similar message might appear.
   
2. Build the application within a Docker image:
	```
	docker build -t moonlight-tizen .
	```
	This will take a while.

 	> Note: If you are running Docker on a Mac with a silicon chip (M1/M2 etc), change the first line in `Dockerfile` to  
	> `FROM --platform=linux/amd64 ubuntu:22.04 as base` before building to ensure compability.

4. Deploy the application to the TV:
	- Run and enter a container; the container will be removed automatically on exit:
	 ```
	 docker run -it --rm moonlight-tizen
	 ```
	- Connect to your TV over Smart Development Bridge:
	 ```sh
	 sdb connect YOUR_TV_IP
	 ```
	- Confirm that you are connected, take note of the device ID:
	 ```
	 sdb devices
	 ```
	 The device ID will be the last column, something like `UE65NU7400`.
	- Install the package:
	 ```sh
	 tizen install -n MoonlightWasm.wgt -t DEVICE_ID
	 ```
	 Moonlight should now appear in your Recent Apps - or similar page - on your TV.
	- Exit the container:
	 ```sh
	 exit
	 ```
	- (Optional) Remove the Docker image:
	 ```sh
	 docker image rm moonlight-tizen
	 ```
