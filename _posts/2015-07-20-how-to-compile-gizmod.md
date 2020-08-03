---
layout: post
title: "How To Compile Gizmod"
date:   2015-07-20 21:00:26 +0200
categories: ["Retro Gaming"]
featured-img: gizmod
---
The Gizmo Daemon is a very powerfull little utility to control applications and devices based on input events. I am using Gizmod to control my DIY [Arcade Cabinet](/2013/07/22/arcade-cabinet-scratch-build/) with USB Controllers to navigate through the menus and start or stop games. When I updated my Arcade Cabinet with new hardware I found out that Gizmod does not compile on newer Ubuntu versions. It seems like Gizmod is not well maintained at the moment. 
<center>
<figure>
<img src="/images/2015/gizmod.jpg"><figcaption>I'm using the Gizmo Daemon to create input events from USB gamepads</figcaption>
</figure>
</center>

### As described in the documentation...

1) Checkout the Subversion code of Gizmod:
{% highlight bash %}
svn checkout svn://svn.code.sf.net/p/gizmod/code/ gizmod-code
{% endhighlight %}

2) Install some dependencies neccessary for the compile:
{% highlight bash %} 
sudo apt-get install libboost-all-dev
sudo apt-get install libx11-dev
sudo apt-get install libvisual-0.4-dev
sudo apt-get install libasound2
{% endhighlight %}

3) Create the build directory:
{% highlight bash %} 
mkdir build
cd build
{% endhighlight %}

4) And call cmake:
{% highlight bash %} 
cmake -DCMAKE_INSTALL_PREFIX=/usr -DSYSCONF_INSTALL_DIR=/etc ..
CMake Warning (dev) at CMakeLists.txt:55 (include):
  Policy CMP0011 is not set: Included scripts do automatic cmake_policy PUSH
  and POP.  Run "cmake --help-policy CMP0011" for policy details.  Use the
  cmake_policy command to set the policy and suppress this warning.

  The included script

    /home/user/Software/gizmod/gizmod-code/gizmod3/cmake/Modules/
DefineProjectDefaults.cmake

  affects policy settings.  CMake is implying the NO_POLICY_SCOPE option for
  compatibility, so the effects are applied to the including context.
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Found Boost: /usr/lib/x86_64-linux-gnu/libboost_date_time.so;/usr/lib/
x86_64-linux-gnu/libboost_filesystem.so;/usr/lib/x86_64-linux-gnu/
libboost_iostreams.so;/usr/lib/x86_64-linux-gnu/libboost_prg_exec_monitor.so;
/usr/lib/x86_64-linux-gnu/libboost_program_options.so;/usr/lib/
x86_64-linux-gnu/libboost_python.so;/usr/lib/x86_64-linux-gnu/
libboost_regex.so;/usr/lib/x86_64-linux-gnu/libboost_serialization.so;
/usr/lib/x86_64-linux-gnu/libboost_signals.so;/usr/lib/x86_64-linux-gnu/
libboost_test_exec_monitor.a;
/usr/lib/x86_64-linux-gnu/libboost_thread.so;/usr/lib/x86_64-linux-gnu/
libboost_unit_test_framework.so;/usr/lib/x86_64-linux-gnu/
libboost_wserialization.so
-- Installation Variables:
   Build Type       (CMAKE_BUILD_TYPE):     Release
   Install Prefix   (CMAKE_INSTALL_PREFIX): /usr
   Data Dir         (DATA_INSTALL_DIR):     /usr/share/Gizmod
   Lib Dir          (LIB_INSTALL_DIR):      /usr/lib
   Plugin Dir       (PLUGIN_INSTALL_DIR):   /usr/lib/Gizmod
   Config Dir       (SYSCONF_INSTALL_DIR):  /usr/etc
   Build vis plugin (BUILD_VIS_PLUGIN):     True
-- ALSA not found
-- Configuring done
-- Generating done
-- Build files have been written to: /home/user/Software/gizmod/gizmod-code/
gizmod3/build
{% endhighlight %}

5) Compile:
{% highlight bash %} 
make
Scanning dependencies of target H
[  1%] Building CXX object libH/CMakeFiles/H.dir/Average.o
In file included from /home/user/Software/gizmod/gizmod-code/gizmod3/libH/
Average.cpp:31:0:
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.hpp:74:5: 
error: ‘size_t’ does not name a type
     size_t        mHeadIdx;   ///&lt;  Current location in the circular buffer
     ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.hpp:76:5: error: 
‘size_t’ does not name a type
     size_t    mFillState;   ///&lt;  Current fill state
     ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp: In constructor 
‘H::Average::Average(int)’:
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp:53:2: error: 
‘mHeadIdx’ was not declared in this scope
  mHeadIdx = 0;
  ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp:55:2: error: 
‘mFillState’ was not declared in this scope
  mFillState = 0;
  ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp: In member 
function ‘void H::Average::push(double)’:
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp:73:6: error: 
‘mFillState’ was not declared in this scope
  if (mFillState == mValues.size()) {
      ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp:74:27: error: 
‘mHeadIdx’ was not declared in this scope
   mSum += Value - mValues[mHeadIdx];
                           ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp:79:11: error: 
‘mHeadIdx’ was not declared in this scope
   mValues[mHeadIdx] = Value;
           ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp:81:2: error: 
‘mHeadIdx’ was not declared in this scope
  mHeadIdx = (mHeadIdx + 1) % mValues.size();
  ^
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp: In member 
function ‘double H::Average::average()’:
/home/user/Software/gizmod/gizmod-code/gizmod3/libH/Average.cpp:89:8: error: 
‘mFillState’ was not declared in this scope
    if (mFillState == 0)
        ^
make[2]: *** [libH/CMakeFiles/H.dir/Average.o] Fehler 1
make[1]: *** [libH/CMakeFiles/H.dir/all] Fehler 2
make: *** [all] Fehler 2
{% endhighlight %}

### Fix the errors
The compiler throws some errors. To fix the size_t errors add the following include in the files <i>libH/Average.hpp</i> and <i>libH/DynamicBuffer.hpp</i>
{% highlight cpp %}
#include <stdlib>
{% endhighlight %}
If we run make again we get:
{% highlight bash %}
make
/home/user/Software/gizmod/gizmod-code/gizmod3/libGizmod/Processes.cpp:157:27: 
error: ‘class boost::filesystem::directory_entry’ has no member named ‘string’
   string StatPath = iter->string() + "/stat";
                           ^
make[2]: *** [libGizmod/CMakeFiles/Gizmod.dir/Processes.o] Fehler 1
make[1]: *** [libGizmod/CMakeFiles/Gizmod.dir/all] Fehler 2
make: *** [all] Fehler 2
{% endhighlight %}
To fix these errors we have to change the line 157 in file <i>/gizmod-3.5/libGizmod/Proceses.cpp</i>:
{% highlight cpp %}
string StatPath = iter->string() + "/stat"; 
{% endhighlight %}
to
{% highlight cpp %}
string StatPath = iter->path().string() + "/stat";
{% endhighlight %}
and in the file <i>/gizmod-3.5/gizmod/GizmoDaemon.cpp</i> on line 1109 we have to change:
{% highlight cpp %}
ret += "sys.path.insert(0, \"" + iter->string() + "\")\n";
{% endhighlight %}
to:
{% highlight cpp %}
ret += "sys.path.insert(0, \"" + iter->path().string() + "\")\n";
{% endhighlight %}
On line 1567 we have to change:
{% highlight cpp %}
UserScripts.push_back(iter->path().leaf());
{% endhighlight %}
to:
{% highlight cpp %}
UserScripts.push_back(iter->path().filename().string());
{% endhighlight %}
On line 2192:
{% highlight cpp %}
if (iter->path().leaf().find("event") != 0)
{% endhighlight %}
to:
{% highlight cpp %}
if (iter->path().filename().string().find("event") != 0) 
{% endhighlight %}
And on line 2194, change:
{% highlight cpp %}
eventsFiles.push_back(mEventsDir + "/" + iter->path().leaf());
{% endhighlight %}
to:
{% highlight cpp %}
eventsFiles.push_back(mEventsDir + "/" + iter->path().filename().string());
{% endhighlight %}

Let's call make again:
{% highlight bash %}
make
[ 29%] Built target H
[ 87%] Built target Gizmod
Scanning dependencies of target gizmod
[ 88%] Building CXX object gizmod/CMakeFiles/gizmod.dir/GizmoDaemon.o
[ 90%] Building CXX object gizmod/CMakeFiles/gizmod.dir/GizmoUtils.o
[ 92%] Building CXX object gizmod/CMakeFiles/gizmod.dir/
GizmodEventHandlerInterface.o
[ 94%] Building CXX object gizmod/CMakeFiles/gizmod.dir/Main.o
/home/user/Software/gizmod/gizmod-code/gizmod3/gizmod/Main.cpp: In function 
‘int main(int, char**)’:
/home/user/Software/gizmod/gizmod-code/gizmod3/gizmod/Main.cpp:57:2: error: 
‘default_name_check’ is not a member of ‘boost::filesystem::path’
  path::default_name_check(native);
  ^
make[2]: *** [gizmod/CMakeFiles/gizmod.dir/Main.o] Fehler 1
make[1]: *** [gizmod/CMakeFiles/gizmod.dir/all] Fehler 2
make: *** [all] Fehler 2
{% endhighlight %}
Still not there! To fix this error, we have to remove line 57 from <i>gizmod-3.5/gizmod/Main.cpp</i>
{% highlight cpp %}
    // set filesystem to native filesystem checking
    // path::default_name_check(native);
{% endhighlight %}
We call make once again:
{% highlight bash %}
make:
[ 29%] Built target H
Scanning dependencies of target Gizmod
[ 31%] Building CXX object libGizmod/CMakeFiles/Gizmod.dir/Processes.o
Linking CXX shared library libGizmod.so
[ 87%] Built target Gizmod
Scanning dependencies of target gizmod
[ 88%] Building CXX object gizmod/CMakeFiles/gizmod.dir/Main.o
Linking CXX executable gizmod
/usr/bin/ld: CMakeFiles/gizmod.dir/GizmoDaemon.o: undefined reference to 
symbol '_ZN5boost6system15system_categoryEv'
//usr/lib/x86_64-linux-gnu/libboost_system.so.1.54.0: error adding symbols: 
DSO missing from command line
collect2: error: ld returned 1 exit status
make[2]: *** [gizmod/gizmod] Fehler 1
make[1]: *** [gizmod/CMakeFiles/gizmod.dir/all] Fehler 2
make: *** [all] Fehler 2
{% endhighlight %}
To fix this error add <i>-lboost_system</i> to the arguments in file <i>/gizmod-3.5/build/gizmod/CMakeFiles/gizmod.dir/link.txt</i>
{% highlight bash %}
/usr/bin/c++ -O3 -DNDEBUG CMakeFiles/gizmod.dir/GizmoDaemon.o 
CMakeFiles/gizmod.dir/GizmoUtils.o CMakeFiles/gizmod.dir/
GizmodEventHandlerInterface.o CMakeFiles/gizmod.dir/Main.o 
-o gizmod -rdynamic ../libH/libH.a ../libGizmod/libGizmod.so.3.4.0 
-lboost_program_options -lpython2.7 ../libH/libH.a -lboost_filesystem 
-lboost_system -lboost_thread -lboost_serialization -lboost_python 
-lasound -lSM -lICE -lX11 -lXext -lutil -lpython2.7 
-Wl,-rpath,/home/user/tmp/gizmod/gizmod-3.5/build/libGizmod:
{% endhighlight %}
And finally edit the file <i>/gizmod-3.5/libH/FileEventWatcher.cpp</i> in line 232-236 to fix the 100% CPU usage problem:
{% highlight cpp %}
    case WATCH_INOUT:
        flags = O_RDWR;
        //events = POLLIN | POLLOUT; // This line causes 100% CPU usage on 
newer kernels
        events = POLLIN;// | POLLOUT;
        ModeString = "Read / Write";
        break;
{% endhighlight %}
That's it, Gizmod should compile and run fine.

<span itemprop="author" itemscope itemtype="https://schema.org/Person">
    <meta itemprop="name" content="Sebastian Glahn" />
    <meta itemprop="url" content="http://blog.tldnr.org" />
    <meta itemprop="sameAs" content="https://twitter.com/sglahn" />
    <meta itemprop="sameAs" content="https://github.com/sglahn" />
    <meta itemprop="sameAs" content="https://www.thingiverse.com/sglahn" />
    <meta itemprop="sameAs" content="https://stackoverflow.com/users/1453205/sglahn" />
</span>
