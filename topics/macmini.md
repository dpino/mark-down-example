h1. Mac Mini

{{toc}}

h2. Accessing

h3. Account

* IP: @macmini.local.igalia.com@
* User: @igalia@
* Password: @WebKit@

h3. SSH

<pre><code class="bash">
$ ssh igalia@macmini.local.igalia.com
</code></pre>

h3. VNC (via ssh tunneling)

1. Open a tunnel

<pre><code class="bash">
$ ssh -p 6789 -L 5900:macmini.local.igalia.com:5900 igalia.com
</code></pre>

If everything is OK, you should get into igalia.com after executing this command.

2. Open VNC session

<pre><code class="bash">
$ vinagre &
</code></pre>

!https://forge.igalia.com/attachments/download/1495/vinagre-with-vnc-tunnel-conf.png!

h3. Comments

  * _ssh server_ is installed
  * _vnc server_ is enabled
  * _sudo access_ is enabled
  * If you want to create your own account you can login as @igalia@ user and follow the next steps: https://support.apple.com/kb/PH11468
  * If the machine is offline try to ping macmini.local.igalia.com from maestria. If it doesn't reply call by phone to the Coruña office and ask somebody to power it on. The Mac Mini is located where Joaquim's desktop was.

h2. Mac port

h3. Building

* First, create your own folder in _/home/igalia_.
* Clone a WebKit repository. There are other users with a cloned repository in the same folder (for instance: calvaris, cgarcia, dpino, etc), so simply copy their WebKit repository and update your master.
* To build WebKit, simply run:
<pre><code class="bash">
$ Tools/Scripts/build-webkit
</code></pre>

h3. Launching WebKit

* Once it has been built, run the minibrowser:
<pre><code class="bash">
$ Tools/Scripts/run-minibrowser
</code></pre>

* It's also possible to launch safari with the new fresh WebKit built:
<pre><code class="bash">
$ Tools/Scripts/run-safari
</code></pre>

h3. Debugging:

The Mac port uses _xcode_ to build the sources. Because of that, customizing the debug is slightly different from the _GTK_ port.

* There is quite a lot of good information for debugging the Mac port at: https://trac.webkit.org/wiki/WikiStart#Bugsanddebugging

h3. Logging support

Logging and other output/behaviors support is activated by default in a _Debug_ build only.

In addition to having the logging support activated in the compilation, we need also to turn on the proper logging channels for specific domains when running.

The domains are:
* @WebCoreLogging@
* @WebKitLogging@
* @WebKit2Logging@

These channels are defined in the "​Source/WebCore/platform/Logging.h":http://trac.webkit.org/browser/trunk/Source/WebCore/platform/Logging.h, ​"Source/WebKit2/Platform/Logging.h":http://trac.webkit.org/browser/trunk/Source/WebKit2/Platform/Logging.h and  "Source/WebKit/mac/Misc/WebKitLogging.h":http://trac.webkit.org/browser/trunk/Source/WebKit/mac/Misc/WebKitLogging.h headers.

For passing the wanted channels to the running process we need to set some of _default_ values for the process we want to be affected. In this case, we will want them to affect the processes identified as _com.apple.Safari_ and _com.apple.WebProcess_ .

This is an example for turning *on* the logging int he Network channel. Notice that the channels are case insensitive.
<pre><code class="bash">
$ defaults write com.apple.WebProcess WebCoreLogging network
$ defaults write com.apple.Safari WebCoreLogging network
$ Tools/Scripts/run-safari
</code></pre>

It is also possible to turn *on* the logging and other output/behaviors support in a Release build by setting the proper C Macros. You may want to check the ​"Source/WTF/wtf/Assertions.h":http://trac.webkit.org/browser/trunk/Source/WTF/wtf/Assertions.h header.

This is a Release build example in which we want to turn on the logging support
<pre><code class="bash">
$ Tools/Scripts/build-webkit GCC_PREPROCESSOR_DEFINITIONS='${inherited} LOG_DISABLED=0'
</code></pre>

h4. Network logging

Turning *on* the network logging may not give us all the information about the HTTP traffic.

Tools like ​"tcpdump":http://www.tcpdump.org/ or "​WireShark":https://www.wireshark.org/ can be handy in this case and both are already installed in the Mac Mini.

For further information you may want to check https://trac.webkit.org/wiki/WebKitGTK/Debugging#Networkanalysis

h3. Other info:

* Building WebKit: https://www.webkit.org/building/build.html
* Running WebKit: https://www.webkit.org/building/run.html

h2. Transfer a patch from the Mac Mini

If you have coded a patch in the Mac Mini and would like to transfer it to your local machine, you can _scp_ the patch from the __mac mini__ to __maestria__ and again from __maestria__ to your local machine.

The Mac Mini has a *filetea* command line installed which can make the process straight forward.

1. Create your patch.

<pre><code class="bash">
$ git format-patch HEAD~
</code></pre>

2. Transfer it with filetea.

<pre><code class="bash">
$ filetea 0001-XXX.patch
</code></pre>

3. Open the given URL as a result of step 2 and paste it in your browser.


