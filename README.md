# BML
Compiling materials here for how to use the Bayesian Multilevel Modeling functions in AFNI (a la Gang Chen's RBA) on the neuron sever!

## Configuring your environment for using these tools

1 - Add the following lines to to your .cshrc.mine files so that these environmental variables are set every time you log on to neuron: 

```
setenv PATH /software/neuron/afni/v21.2.04/abin:$PATH
setenv PATH /usr/local/bin:$PATH
setenv PATH /software/neuron/R-3.6.3/bin:$PATH
```


The first line is so you are using the right version of AFNI., and the only reason I’m saying to use AFNI v21.2.04 because there were few bugs that I’d found in Gang’s RBA.R script and the older BayesianGroupAna.py script that I have fixed and added to that AFNI folder on neuron – some the bugs prevented all the output from being produced, so now you can get all the goodies from these programs!

The last 2 lines are a bit more technical: the major dependency issue is that the gcc compiler on neuron is too old, and I'd installed a new one that went to /usr/local/bin, but when I log off and on again, it goes back to the old one. So it is necessary to change the environment to find the new one before launching the program (ie., setenv PATH /usr/local/bin:$PATH). However, now that it is searching in /usr/local/bin first, it gets the wrong version of R (version 3.5.* is in /usr/local/bin), so the second line directs it to find R in the right place before R calls on gcc compiler (this is a very hacky workaround). All the required packages for the Bayesian function have been installed/updated for R 3.6.3, and are not compatible with 3.5


2 - You might need to edit the Makevars file for R so it finds the right C++. To read your Makevars file, use the following command: 

```
more ~/.R/Makevars
```

This is what mine has, and if yours is different, edit this file so it matches mine:
```
CXX14FLAGS=-O3 -march=native -mtune=native -fPIC
CXX14=g++ -std=c++11
CXX14STD='-std=c++1y'
```


a) It is possible that you don’t already have a Makevars file created (i.e., if you haven’t used R on neuron before), in which case you might get an error that says ‘No such file or directory’ when you run the command above.  If that is the case, you can create it by doing the following:

```
mkdir ~/.R
touch ~/.R/Makevars
echo CXX14FLAGS=-O3 -march=native -mtune=native -fPIC >> ~/.R/Makevars
echo CXX14=g++ -std=c++11 >> ~/.R/Makevars
echo CXX14STD='-std=c++1y' >> ~/.R/Makevars
```

b) When attempting to make the Makevars file using the steps above, you might get an error saying something like ‘cannot create directory … Disk quota exceeded’

This happens because your home directory has very limited storage space available, so it fills up quickly. You’ll need to figure out if you have some large files that you can delete to clear up space to create the Makevars file. It is a tiny file, so you won’t need to clear up much space!


3 - When connecting to neuron, be sure to use the -Y option. This enables the use of graphic libraries typically used by GUIs and graphics softwares. Even though the bayesian programs are not going to launch a graphic, it still needs to use the graphics libraries to generate the plots. For example: 
 ```
 ssh -Y diana@neuron.umd.edu
 ```
