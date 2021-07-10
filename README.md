## Cross Build
https://github.com/dockcross/dockcross  
https://rvarago.github.io/2020/10/02/crosscompiling-cpp-dockcross.html

### Cross build using cmake (Basic)
```
docker run --rm dockcross/linux-armv7 > ./dockcross-linux-armv7 && chmod +x ./dockcross-linux-armv7
./dockcross-linux-armv7 cmake -B build/linux-armv7/
./dockcross-linux-armv7 cmake --build build/linux-armv7
```

### Custom docker image build
```
cd dockcross
docker build -t dockcross-linux-x64 -f Dockerfile.linux-x64 .
```

### Cross build using cmake (Custom)
```
docker run --rm -e DEFAULT_DOCKCROSS_IMAGE=dockcross-linux-x64 dockcross-linux-x64 > ./dockcross-linux-x64 && chmod +x ./dockcross-linux-x64
./dockcross-linux-x64 cmake -B build/linux-x64
./dockcross-linux-x64 cmake --build build/linux-x64
```