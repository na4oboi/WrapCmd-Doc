# Run Wrap4D project within docker container.

To run wrap cmd directly from terminal you can use this massive command.

```shell
docker run --mount " 'type=volume,target=/nfs/common,volume-driver=local,volume-opt=type=cifs,volume-opt=device=//192.168.0.30/Public,\"volume-opt=o=addr=192.168.0.30,username=user,password=password,file_mode=0777,dir_mode=0777\"'-e WRAP4DCMD_COMMON_DIR=/nfs/common -e WRAP4DCMD_LICENSE=7307@192.168.0.13 -e XDG_RUNTIME_DIR=/tmp/runtime-root -i --entrypoint /usr/bin/xvfb-run debug-r3dsnode:2022.6.1 -s '+extension GLX +render' '/opt/R3DS/Node/WrapCmd.sh' compute -s $start_frame -e $end_frame /nfs/common/WrapProjects/opengl.wrap
```
Let's see what is going on step-by-step.

## Mount Volume
In our office we use nfs to storage all the data, which connected to all nodes.
You can create custom docker volume or use this flag.
```shell
--mount " \
         'type=volume,target=/nfs/common, \
          volume-driver=local, \
          volume-opt=type=cifs, \
          volume-opt=device=//nfs_ip/Public, \
          \"volume-opt=o=addr=nfs_ip, \
          username=user,password=password, \
          file_mode=0777,dir_mode=0777\"'\ 
```

## Environment variables
Only last variable is required all of other variables are optional and can be configured manually if you are using Rush. 
```shell
  	-e WRAP4DCMD_GALLERY_DIR=/gallery_path \
	-e WRAP4DCMD_COMMON_DIR=/common_dir_path \
	-e WRAP4DCMD_LICENSE=7307@lic_server_ip \ 
	-e RUSH_ADDRESS=http://rush_address:7308 \
  	-e XDG_RUNTIME_DIR=/tmp/runtime-root -i \
```

## Entrypoint
This entrypoint creates opengl context before wrap have been started.
```shell
--entrypoint /usr/bin/xvfb-run debug-r3dsnode:2022.6.1 -s '+extension GLX +render' \
```

## Start Wrap project
```shell
'/opt/R3DS/Node/WrapCmd.sh' compute -s $start_frame -e $end_frame /nfs/common/WrapProjects/opengl.wrap
```
Here it's just WrapCmd command in the format:
```shell
  $wrap_cmd_path compute {-s $start_frame -e $end_frame} $wrap_project_path
```
