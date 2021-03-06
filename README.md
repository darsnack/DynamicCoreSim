# DynamicCoreSim
A research based project for ECE597: Adv. Computer Architecture at Rose-Hulman Inst. of Tech. Attempts to analyze the performance benefits of a dynamic processor core.


## Pre-Requisites
- You must have gcc
- You must create a file "splash2/codes/Makefile.dir" that includes the line "BASEDIR := your base directory" (this file is in the .gitignore)
- Include "SYS := X86" or whatever derivative ISA you are using in Makefile.dir (options include X86, ARM, or out)

## Compiling with Alpha toolchain
1. Change directories to the folder of interest (ex. mediabench/jpeg/jpeg-6a)
2. Execute the appropriate lines:
 a. For jpeg: ./configure CC='../../../alpha-unknown-linux-gnu/bin/alpha-unknown-linux-gnu-gcc-3.4.3 -static -static-libgcc' && make
 b. For g721: make CC=../../../alpha-unknown-linux-gnu/bin/alpha-unknown-linux-gnu-gcc-3.4.3
 c. For gsm: make CC=../../alpha-unknown-linux-gnu/bin/alpha-unknown-linux-gnu-gcc-3.4.3
 d. For epic: make CC=../../../alpha-unknown-linux-gnu/bin/alpha-unknown-linux-gnu-gcc-3.4.3
 e. For adpcm: make CC=../../../alpha-unknown-linux-gnu/bin/alpha-unknown-linux-gnu-gcc-3.4.3
4. Don't forget to make clean before committing

---------------------------------------
David's Findings as of Febuary 6, 2015
---------------------------------------
Using X86 successfully run benchmarks and how to run them.
1. ./BARNES.X86 < input
2. ./FMM.X86 < inputs/"input filename"
3. ./OCEAN.X86
4. ./WATER-NSQUARED.X86 < input

---------------------------------------
David's Findings as of January 19, 2015
---------------------------------------
1. Tried using the "arm-linux-gnueabihf-gcc" compiler to compile OCEA to arm code was unable to get it working throws following error:
/usr/lib/gcc-cross/arm-linux-gnueabihf/4.8/../../../../arm-linux-gnueabihf/bin/ld: cannot find crt1.o: No such file or directory
/usr/lib/gcc-cross/arm-linux-gnueabihf/4.8/../../../../arm-linux-gnueabihf/bin/ld: cannot find crti.o: No such file or directory
/usr/lib/gcc-cross/arm-linux-gnueabihf/4.8/../../../../arm-linux-gnueabihf/bin/ld: cannot find -lm
/usr/lib/gcc-cross/arm-linux-gnueabihf/4.8/../../../../arm-linux-gnueabihf/bin/ld: cannot find -lpthread
/usr/lib/gcc-cross/arm-linux-gnueabihf/4.8/../../../../arm-linux-gnueabihf/bin/ld: cannot find -lc
collect2: error: ld returned 1 exit status
make: *** [OCEAN] Error 1

2. Help for se.py is very usefull and provides many parameters: build/X86/gem5.opt configs/example/se.py -h

3. The available CPU types such as InOrder or OutOfOrder can be seen with: build/X86/gem5.opt configs/example/se.py --list-cpu-types

4. Additional cpus can be specified when the simulator is being built with the CPU_MODELS flag

5. Usefull options to se.py
    --cpu-type=CPU_MODEL
    -o "String of parameters for benchmark"
        ->Pass paramaters to the benchmark code for ocean -p# specifies number of cores where # is a power of two
    -n "specify number of CPUS to be used by simulator"
    Example.) build/X86/gem5.opt configs/example/se.py --cpu-type=AtomicSimpleCPU -n 4 -o "-p4" -c ../DynamicCoreSim/splash2/codes/apps/ocean/non_contiguous_partitions/OCEAN

---------------------------------------
Kyle's Findings as of January 20, 2015
---------------------------------------

1. Managed to compile OCEAN using arm-linux-gnueabihf-gcc. My compiler is globally installed as a package from the Arch User Repositories, so maybe that is why the linker can find the right files for me.

2. I, too, found the same useful options for se.py and run.py (under splash).

3. I cannot run any simulations, since I get an error "Class L1 Cache has no attribute latency".

---------------------------------------
David's Findings as of January 23, 2015
---------------------------------------
The following are directions are running gem5 in full system mode with X86 and ARM:

1. Uncompress the full_system_images.zip file place and copy it to wherever

2. First add a varible which conatains the path to the full_system_images directory. Add this line to your .bashrc file: export M5_PATH=path/to/full_system_images

3. Note that "build/X86/gem5.opt configs/example/fs.py --help" provides many usefull options specifically at apperars that you can set flags before you start the simulator to tell it where specifically you want to start collecting benchmark data and you can simple tell it to run a benchmark with the -b option

4. The following two instructions start the simulator for their respective ISA:
	build/X86/gem5.opt configs/example/fs.py --disk-image=linux-x86.img
	build/ARM/gem5.opt configs/example/fs.py --disk-image=aarch64-ubuntu-trusty-headless.img --machine-type=VExpress_EMM --kernel=vmlinux-3.3-arm-vexpress-emm-pcie
	Note: If the arm version throws errors about memory size force it to use 356MB by adding the option --mem-size=256MB

5. After starting the simulator connect to it via telnet with: telnet localhost 3456

6. After awhile a linux prompt should appear.

---------------------------------------
Ben's Findings as of January 25, 2015
---------------------------------------
https://docs.google.com/document/d/1B7nZSqMLwkwoVNEj_58tMPTk4bKWvoEMbokOAjqeC-k/preview

---------------------------------------
Kyle's Findings as of 25 January 2015
---------------------------------------
- Could not get Ben's image to open ("panic: Error opening /opt/gem5/full_system/disks/x86root-parsec.img").
- I modified the Makefile.config to use a variable called SYS. This means that it will generate binaries called OCEAN.X86, OCEAN.ARM or OCEAN.out. Use Makefile.dir to specify SYS := X86 or whatever ISA you need. The .out is the standard ISA for our local machines.

