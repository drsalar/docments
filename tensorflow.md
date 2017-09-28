1. Download the source code


```
go get -d github.com/tensorflow/tensorflow/tensorflow/go 
```

2. Install bazel

```
git clone https://github.com/bazelbuild/bazel.git
```

Need jdk

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

3. Download tenserflow from:
https://storage.googleapis.com/tensorflow/libtensorflow/libtensorflow-cpu-linux-x86_64-1.3.0.tar.gz

4. Tar

```
tar -C /usr/local -xf libtensorflow-cpu-linux-x86_64-1.3.0.tar.gz
```

5. Test

```
go test libtensorflow-cpu-linux-x86_64-1.3.0.tar.gz
```

