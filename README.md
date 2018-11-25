# How to insert your frozen-module into the bbcmicropython project and compile a .hex file 

I think I have found the solution.
The whole procedure to make your own frozen module should be like this:

1. Get the [micropython/micropython](https://github.com/micropython/micropython)
2. In its tool folder, find the make-frozen.py. Make sure you are on the master branch, not some old version of it.
3. Write your own .py file and put it in a folder
4. Transform .py to .c, `python  make-frozen.py  your_folder_path  >  frozen_module.c`
5. Open `frozen_module.c` and check its the module name, you could probably change the module name in the file.
6. Put your `frozen_module.c` to the `micropython/source/py`. `**CAUTION: make sure you only put one .c file of your own. Or a compile error will pop out**`
7. Build your compile tool chain. But [with a few tricks](https://github.com/bbcmicrobit/micropython/issues/514), if you are on ubuntu.
Thank you shenki for analyse the issue and dpgeorge, rojer, and especially temporaryaccount for cracking the nut. 
In case you can't find it. This is the solution given by temporaryaccount.

> You could try to install the version of libnewlib-arm-* from Ubuntu cosmic?
> Yes, I can confirm fixing the issue by installing the newer packages.
> Just get the [libnewlib-dev (2.4.0.20160527-4)](https://packages.ubuntu.com/cosmic/all/libnewlib-dev/download) and [libnewlib-arm-none-eabi (2.4.0.20160527-4) *.deb](https://packages.ubuntu.com/cosmic/all/libnewlib-arm-none-eabi/download) files, then install them:
>`sudo dpkg -i libnewlib-dev_2.4.0.20160527-4_all.deb libnewlib-arm-none-eabi_2.4.0.20160527-4_all.deb`

> As of today, @temporaryaccount's instructions worked for Ubuntu 18.04.1 (but the versions provided at the links have been updated).
> `sudo dpkg -i libnewlib-arm-none-eabi_3.0.0.20180802-2_all.deb libnewlib-dev_3.0.0.20180802-2_all.deb`


I think on mac, I've met the same problem(not so sure, the similar part is success on compiling but fail on linking). If you have any solution to it, please please give a hint. Thank you!

8. Follow front page [README.md](https://github.com/bbcmicrobit/micropython) to compile the project. If no ['ninja' error] occurs, then you made it.

Ubuntu users can install the needed packages using:
```
sudo add-apt-repository -y ppa:team-gcc-arm-embedded
sudo add-apt-repository -y ppa:pmiller-opensource/ppa
sudo apt-get update
sudo apt-get install cmake ninja-build gcc-arm-none-eabi srecord libssl-dev
pip3 install yotta
```

Once all packages are installed, use yotta and the provided Makefile to build.
You might need need an Arm Mbed account to complete some of the yotta commands,
if so, you could be prompted to create one as a part of the process.

- Use target bbc-microbit-classic-gcc-nosd:

  ```
  yt target bbc-microbit-classic-gcc-nosd
  ```

- Run yotta update to fetch remote assets:

  ```
  yt up
  ```

- Start the build:

  ```
  make all
  ```

If you found it not working, remove yotta_modules, yotta_targets, .yotta.json and build/ and repeat the procedure.

10. Find your .hex file in `micropython/build/xxxxxxxxxx/source/`

11. Send it to microbit. Then open the REPL and type: `help('modules')`, you should see your module listed below if you do it right.

12. Cheers!
