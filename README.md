To build qfprog run the build from the dockerfiles subdirectory:
```
cd dockerfiles
docker build . -t qfprog
```
The above assumes Docker can find an image called python:3-slim-buster.  

If Docker can't find this python:3-slim-buster locally, it should pull it from hub.docker.com.

The qf_bin directory contains a sample qf_helloworldhw.bin file that flashes the LED red.  
```
~/qfprog/qf_bin$ ls
qf_helloworldhw.bin  qf_helloworldhw.elf  qf_helloworldhw.map
```
To program the hardware from within qf_bin, hit the reset button, and while the quickfeather is 
flashing blue, hit the user button.  It should turn green with a quick flash once per second.

To program the hardware, you may need to run the container privileged. 
```
~/qfprog/qf_bin$ docker run --privileged -it --rm -v $(pwd):/root/work qfprog bash

```
The bash session is running inside the container:

```
root@3efb500ca048:~/work# ls
qf_helloworldhw.bin  qf_helloworldhw.elf  qf_helloworldhw.map

```


Now try something like:
```
root@3efb500ca048:~/work# qfprog --port /dev/ttyACM0 --m4app qf_helloworldhw.bin

```

After programming completes, hit the reset button and wait for the blue flashing to stop.
The quickfeather should flash red about 3 times/sec.