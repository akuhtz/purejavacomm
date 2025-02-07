Version 1.0.5
* Upgraded maven pom to use JNA 5.11.0
* Added OSGi headers

Version 1.0.4
* Upgraded maven pom to use JNA 5.10.0

Version 1.0.3.2

* Upgraded maven pom to use JNA 5.6.0
* Added JTermios OpenBSD support
* Some other fixes
* Changed groupId to 'org.bidib.com.github.purejavacomm'

Version 1.0.2

* Changed PureJavaSerialPort input stream to throw IOException if the port is closed from an other thread or otherwise

Version 1.0.1

* Fixed a missing timeout that caused hangs with in pollmode
* Upgraded maven pom to use JNA 4.2.2
* Some other minor fixes

Version 1.0.0

* Fixed some logging bugs that prevented use of logging
* Improved timeout handling when opening a serial port

Version 0.0.29

* Added missing 'currentOwner' field for PortInUseException

Version 0.0.28

* As a precaution all the relevant WinAPI calls are wrapped in try/catch LastErrorException

Version 0.0.27

* Hopefully fixes the final issues caused by the change over to JNA Direct Mapping
  
  This version should work with Java 1.5 or higher
  
  Reverted windows backend to commit a6d51cd879ba4d828c6c45fc1b48db18fabdf67b
  to get rid of the  problems caused by the introduction of
  LastErrorException to a code base that was based on using GetLastError.
  
  Re-implemented a fix for WaitCommEvent returning false but GetLastError
  return zero.
  
  This version now runs the TestSuite cleanly on Mac OS X and Windows,
  and presumably on Linux too, BSD/Solaris have not been tested. 

Version 0.0.26

* Hopefully fixes the final issues caused by the change over to JNA Direct Mapping
  
  This version requires Java 1.7 or higher. 
  
  Contributed by Pete Loveall.
  
  
Version 0.0.25

* Fixed some issues on Windows backend that sometimes
  caused problems on Windows.
  
  This version requires Java 1.7 or higher. 
  
  Contributed by Pete Loveall.
  
Version 0.0.24

* All backends now uses JNA Direct Mapping for less CPU
  loading and faster operation. 
  
  Contributed by Pete Loveall.
  
  
Version 0.0.23

* Windows backend now uses JNA Direct Mapping for less CPU
  loading and faster operation. 
  
  Contributed by Pete Loveall.
  

Version 0.0.22

* Windows backend increased QueryDosDevice buffer max size to 
  1 mega chars to support systems with thousands of COM ports


Version 0.0.21

* Reverted the overzealous checking for successful tcsetattr


Version 0.0.20

* Fixed a bug that caused setting of control lines (CTS,RTS etc) 
  to throw an exception with JNA 4.0.0 on Mac OS X

* Reverted the use of CancelIoEx as it is not available on Windows XP

* Re-worked and simplified file handle closing logic in Windows
  to prevent hangups and work around the lack of CancelIoEx 

* Added tests to the test suite for successful closing of ports 
 

Version 0.0.19

* Changed to the use of CancelIoEx for canceling all I/O in close() to prevent 
  hanging when I/O is in progress in an other thread.
  
* Ignore all errors when trying to clear pending wait in select()

  Thanks for Colin Leitner for contributing following! 

* Handle free form device paths correctly, by returning a
  CommPortIdentifier for any existing device path, even if opening that
  port fails
 
* Changed access to test port name to use getPortName

* Added check for port ownership and fixed test case to handle a
  non-enumerated test port gracefully

* Added TwoPortSerialTest for tests that can't be run without two
  independent serial ports

* Increased maximum timeout value to 1100, which was necessary to reliably
  run this test on a windows machine

* Added check for valid device path in CommPortIdentifier getPortIdentifier(String portName)

* Removed port detection feature from the TestBase. It's been preventing a
  proper test setup
  
* Let the test suite return an exit value of 1 on failure
  
  
 Version 0.0.18

* Fixed problems in the Windows backend that rendered it dysfunctional

* Ignore TIOCMGET fail on port open to be compatible Linux virtual ports

* Lots of source code clean up by Colin Leitner, thanks.

* Added support for mark and space parity.

* Added support for 1.5 stop bits 
  No hardware actually supports it but now you don't get an exception


Version 0.0.17

* Ignore error for TIOCGSERIAL/TIOCSSERIAL when setting standard
  baudrates
  
* Improved port enumeration on Linux

* Fixed sendBreak "timeout value is negative" error

* Fixed WinAPI.SetCommBreak mistakenly called CloseHandle on Windows

* Fixed random "port not locked" caused by problem in JTermiosImpl.lock()

* Fixed WaitCommEvent returns an 87: bad parameter, error code on Windows


Version 0.0.16

* Just bumped the version number because what was supposed to 
  be release 0.0.15 actually reported version number 0.0.15.rc2.
  
  No code changes.
  

Version 0.0.15

* Improved the Windows back end ie fixed bugs revealed by the 
  changes in the front end that were done to improve Linux
  performance.
  

Version 0.0.14 *** EXPERIMENTAL ***

* More re-factoring to improve read performance and code

* Fixes to numerous corner cases to conform to specification

* Added getNativeFileDescriptor() to allow mixing and matching JTermios
  raw calls with PureJavaComm functionality. Caveat emptor!
  
* In raw read mode (purejavacomm.rawreadmode == true) the maximum
  allowed enableReceiveThreshold(value) is 255. An attempt to set
  a too large value will result in UnsupportedCommOperationException.
  
* The maximum enableReceiveTimeout(value) is now 255000 msec
  ie 25.5 seconds. An attempt to set a too large value will 
  result in UnsupportedCommOperationException.


Version 0.0.13 *** EXPERIMENTAL ***

* Removed some debugging output from SerialPort.close() that
  was left there by mistake
  
* Added a new property that disables the internal use of
  a pipe to get nudge select()/poll() out of coma. The
  pipe uses two extra file descriptors so in a huge
  system this maybe a critical resource. To disable
  the use of the pipe set 'purejavacomm.usenudgepipe' to 'false'
  

Version 0.0.12 *** EXPERIMENTAL ***

* This version includes a major re-factoring of receive timeout and
  threshold handling to improve performance on low end platforms
  like RaspberryPI and to improve conformance to the JavaComm spec.
  
  This version has had limited testing so it may unintentionally
  break existing applications, proceed with caution. This
  version has not been tested on Windows for the lack
  of suitable setup.

* New faster DirectMapping for poll() in Linux backend

* Linux backend now defaults to poll() instead of select()
  to default to better performance. This can be overridden
  by setting the system property 'purejavacomm.usepoll' to 'false'.

* Re-factored receive timeout and threshold handling that
  exploits the platform native VMIN and VTIME to reduce
  CPU loading
  
* Added property named 'purejavacomm.rawreadmode' that further
  reduces overhead in InputStream.read(...) with the
  risk of indefinite blocking on reads without timeout.

* Improved event handling to not to poll at all if control
  line notifications are not requested, to improve performance
  on low end platforms.  Currently this feature is not available 
  on the Windows backend, where it is typically not needed.

* A blocking read() can now be interrupted by close() 
  
* A blocking read() with timeout can no longer block indefinitely
  even if the serial line is indefinitely idle

* Added equals() and hashCode() methods to CommPortIdentfier
  to fix ownership mechanism
  
* PureJavaSeriaPort.getInputStream()/getOutputStream() now
  return same stream instances if called multiple times.
  
* Number of potential multithreading issues fixed


Version 0.0.11 

* Moved over to JNA DiretMapping in Linux back end to squeeze
  out better performance on low end Linux system like
  RaspberryPI
  
* Added property named 'purejavacomm.pollperiod' to allow tuning of
  polling period for port control lines. Default is 10 (msec)
  if the property is not set, this is compatible with previous
  practice and RXTX.

* Changed property name 'purejavacomm.use_poll' to 'purejavacomm.usepoll'
  for consistency. Old name is supported but deprecated.

* Added pipe() to JTermios API, not implemented on Windows

* Fixed getReceiveThreshold() and isReceiveThresholdEnabled() to
  return correct values
  
* Fixed a bug that caused read(...) to block if the requested
  number of bytes was less that the set receive threshold value

* Re-ordered logging initialization so that setting the log level/mask in
  code always works and overrides any properties regardless of when/where
  the setting is set in code.

* Fixed a logging format typo in Linux/poll code path 

* Removed flushing of the serial port while opening it as this is not
  required by JavaComm spec, RXTX does not do it, and it may loose
  data if the device starts send something as soon as the port is open.
  
* Fixed PureJavaSerialPort to set FLOWCONTROL_NONE in hardware when opening the port
  (previously the hardware was untouched which was confusing because it looked like
  the default was no flow control.)
  
* Fixed checkReturnCode() to return the correct class/line where the error code was checked  


Version 0.0.10

* Fixed a missing field in getFieldOrder() for Linux 
 

Version 0.0.9

* Added support for JNA 3.5.0 which require getFieldOrder() for all Struct derived classes

* Changed Linux backend to ignore EINVAL error in ioctl/TIOCGSERIAL/TIOCSSERIAL calls
  as all USB CDC ACM ack USB/Serial Dongles don't support it
  

Version 0.0.8

-  Mostly thanks to Alexander Salgado 

* Added support for internally switching to poll() instead of select()

- poll is useful if the operating system uses file descriptors that overflow 'fd_set'
- to use poll() set system property purejavacomm.use_poll = true
- only available for Linux at the moment, for other platforms 
   purejavacomm.use_poll is ignored

* Fixed poll() in jtermios.linux.JTermiosImpl.java

- poll() can now be used instead of select() , which has the benefit of 
  marginally less overhead and support file descriptors that overdflow
  what select() can handle, rare but useful if it happens
  
* Fixed a few wrong constants in jtermios.linux.JTermiosImpl.java

- constant for baudrate 1200 was wrong (resulted in 600)
- some error codes were wrong (probably had no effect)


Version 0.0.7

* On Linux ignore failure of TIOCGSERIAL in when trying to use standard baudrates

- not every serial port driver supports TIOCGSERIAL, so when we set standard baudrate
  we try to turn of custom divisor using TIOCGSERIAL, but if it fails we just ignore it
  
- related to above logging for JTermiosImpl.setspeed() was improved to help debugging


Version 0.0.6

* Added support for FreeBSD

- thanks to the persistent efforts of Denver Hull

* Fixed a synchronization issue with control line state change detection

- this bug often caused failure in Test1.java

* Moved baudrate setting to each individual backend to support non standard baudrates

- setting of non standard baudrates is very specific to OS and thus best
  handled separately for each platform

* Changed the way baudrate setting is handled in Mac OS X 

- the code now first tries the standard (POSIX) way of setting the baudate
  and if that fails then it tries the Mac OS X specific ioctl IOSSIOSPEED
  
* Change port enumeration matching to use a regular expression.

- each backend defines its own rules for matching port names during enumeration,
  see implementation of each jtermios.JTermios.JTermiosInterface.getPortNamePattern().
  
- for each backend the pattern can be overridden by setting the system property
  purejavacomm.portnamepattern.<backend-fully-qualified-classname>.
  
- the pattern can be globally overriden by setting the system property
  purejavacomm.portnamepattern
  
- the current default backend patterns, as returned by getPortNamePattern(),
  are as follows:
  
    OS          Regular expression        Port name...
    -------------------------------------------------------------------
    Linux       ^tty*                     Starts with 'tty'
    Mac OS      ^(tty\.|cu\.).*           Starts with 'cu.' or 'tty.'
    FreeBSD     ^(tty\.|cu\.).*           Starts with 'cu.' or 'tty.'
    Solaris     .*                        Anything goes
    Windows     ^COM.*                    Starts with 'COM'
  
  
  