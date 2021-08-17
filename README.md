# MelastiX

![Transformix Results](https://raw.githubusercontent.com/raacampbell13/matlab_elastix/master/MelastiX_examples/transformix/dog_warp_results.png "Transformix Results")

## What is it?
MelastiX is a collection of MATLAB wrappers for the open source image registration suite [Elastix](http://elastix.isi.uu.nl/). Elastix is cross-platform and is normally called from the system command-line (no GUI). MelastiX allows the elastix and transformix binaries to be called from within MATLAB as though they are native MATLAB commands.

## What does it do?
1. The user can feed in MATLAB matrices instead of image file names and get a MATLAB matrix back as a result.
2. Parameters can be passed in as Elastix text files or as an MelastiX YAML file. The latter provides some error-checking options as the type and possible values of the parameters can be checked. 
3. Parameter files can be modified by passing an optional structure as a command-line argument. This makes it easy to explore how changing parameters affects registration accuracy. 
4. A function and example are provided to handle inverse transforms. 
5. Transforms sparse points. 
6. Handles both 2D and 3D data. Examples are only 2D, though.

## What does it not do?

At the moment MelastiX does not provide tools to:

1. Handle the mask option, thread option, and priority arguments for the elastix binary.
2. Handle multiple fixed and moving image in one elastix call.
3. Analyse transform parameters. 


## Getting started

1. Place the [Elastix](http://elastix.isi.uu.nl/) binaries in your *system* path. If you don't know how to do that, there's information here for [Windows](http://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/) and [Linux](http://unix.stackexchange.com/questions/26047/how-to-correctly-add-a-path-to-path). You may need to restart Windows after adding Elastix to the path otherwise MATLAB will not see it. Before restarting, verify that running `elastix --help` in the Windows command line shows the Elastix help text. 
2. Add the MelastiX *code* directory to your MATLAB path. 
3. Add <a href="https://github.com/raacampbell/yamlmatlab">yamlmatlab</a> to your MATLAB path. 
4. Run the examples in MelastiX_examples. 

Installing Elastix via the Linux package manager may lead to errors in MATLAB of the sort: `error while loading shared libraries: libANNlib.so`. 
If so, remove the version installed by the package manager and install manually from the Elastix GitHub page. 
e.g. 
```
$ bzip2 -d elastix-4.9.0-linux.tar.bz2 
$ tar -xvf elastix-4.9.0-linux.tar 
$ sudo cp bin/* /usr/local/bin/
$ sudo cp lib/* /usr/local/lib/
```

If everything is installed correctly, running `elastix('version')` in MATLAB should bring up the Elastix help text. 
If that does not appear and you get errors associated with `libstdc++.so.6` being an incorrect version, then try the suggestions shown here:
https://uk.mathworks.com/matlabcentral/answers/329796-issue-with-libstdc-so-6
A verified solution on Linux is to change the symlink to the system library. e.g.
```bash
$ cd /usr/local/MATLAB/R2021a/sys/os/glnxa64
$ ls -la libstdc++.so.6
lrwxrwxrwx 1 root root 19 Jun 30 11:05 libstdc++.so.6 -> libstdc++.so.6.0.25

$ sudo rm libstdc++.so.6

$ sudo ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6 ./
```

## What to do if the transform fails?
If you get unexpected results, first check whether the transform parameter file was written properly. If you are providing an Elastix parameter file and not modifying it then you should be fine. However, if you use the .yml approach or modify a parameter file using a structure then it's possible you've found a bug in the writing of the paramater file. To diagnose this, look at the written parameter file by calling elastix.m with a user-defined output path (so the files produced are not deleted)  or use the verbose option in <a href="https://github.com/raacampbell/matlab_elastix/blob/master/code/elastix_paramStruct2txt.m">elastix_paramStruct2txt</a>. If you're *still* getting unexpected results then probably you have an issue with Elastix itself: please go the Elastix website for documentation or ask on their forum. 

## Known issues
The paths in the Transform files are absolute so if you have multiple transforms and you run transformix on them, the process will only succeed if the files are in their original locations. If the files have moved, you will need to change the path(s) at `InitialTransformParametersFileName`. 

## MATLAB Versions
You will need at least MATLAB version R2017b.


## Related projects

1. <a href="https://sourcesup.renater.fr/elxfrommatlab/">ElastixFromMatlab toolbox</a>
2. <a href="http://elastix.bigr.nl/wiki/index.php/Matlab_interface">Elastix MATLAB interface</a>
