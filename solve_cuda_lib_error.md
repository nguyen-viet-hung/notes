# libcublas.so.x.0: cannot open shared object file
"LD_LIBRARY_PATH cannot be set in $HOME/.profile, /etc/profile, nor /etc/environment files" in http://stackoverflow.com/questions/13428910/how-to-set-the-environmental-variable-ld-library-path-in-linux.
To solve it, do as follow

```
sudo echo "/usr/local/cuda/lib64" > /etc/ld.so.conf.d/cuda.conf
sudo ldconfig
```
