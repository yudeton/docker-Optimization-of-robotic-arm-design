# 這個映像檔內包含
1. Ubuntu 18.04
2. ROS melodic
3. ROS package: moveit ....
4. Cuda 10.2

# Now create a script to run the image called run_my_image.bash

```
XAUTH=/tmp/.docker.xauth
if [ ! -f $XAUTH ]
then
    xauth_list=$(xauth nlist :0 | sed -e 's/^..../ffff/')
    if [ ! -z "$xauth_list" ]
    then
        echo $xauth_list | xauth -f $XAUTH nmerge -
    else
        touch $XAUTH
    fi
    chmod a+r $XAUTH
fi

nvidia-docker run -it \
    --user=root \
    --net=host \
    --rm --ipc=host \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --env="XAUTHORITY=$XAUTH" \
    --volume="$XAUTH:$XAUTH" \
    --runtime=nvidia \
    --privileged \
    -e LANG=C.UTF-8 \
    --volume=/dev:/dev \
    samkaiyang/opt_dynamic_design:($tag_version))\
    /bin/bash
```
# Make the script executable
```
chmod a+x run_my_image.bash
```
# Execute the script
```
sudo ./run_my_image.bash
```
# If you want to add multiple terminals, you can enter the following commands in the new terminal
```
docker exec -it --user root ($CONTAINER ID) /bin/bash
```

# Open VS code
```
sudo code /directory-to-open --user-data-dir='.' --no-sandbox
```